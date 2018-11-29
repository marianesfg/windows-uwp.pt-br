---
title: Como suspender um aplicativo (DirectX e C++)
description: Este tópico mostra como salvar dados importantes do aplicativo e do estado do sistema quando o sistema suspende o aplicativo UWP (Plataforma Universal do Windows) DirectX.
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, suspensão, directx
ms.localizationpriority: medium
ms.openlocfilehash: 0b588d6bf6e7cbf43651d94a7fd46e9a767c6f09
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7981750"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>Como suspender um aplicativo (DirectX e C++)



Este tópico mostra como salvar dados importantes do aplicativo e do estado do sistema quando o sistema suspende o aplicativo UWP (Plataforma Universal do Windows) DirectX.

## <a name="register-the-suspending-event-handler"></a>Registrar o manipulador de eventos de suspensão


Primeiro, registre para manipular o evento [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) que é acionado quando seu aplicativo é movido para um estado suspenso por uma ação do usuário ou do sistema.

Adicione este código à sua implementação do método [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) de seu provedor de exibição:

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>Salve todos os dados do aplicativo antes de suspender


Ao manipular o evento [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860), seu aplicativo tem a oportunidade de salvar seus dados importantes na função do manipulador. O aplicativo deve usar a API de armazenamento [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) para salvar dados simples do aplicativo de maneira síncrona. Se você estiver desenvolvendo um jogo, salve quaisquer informações de estado de jogo crítico. Não se esqueça de suspender o processamento de áudio!

Agora, implemente a chamada de retorno. Salve os dados do aplicativo neste método.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}
```

Esta chamada de retorno deve ser concluída dentro de 5 segundos. Durante esta chamada de retorno, você deve solicitar um adiamento chamando [**SuspendingOperation::GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) que inicia a contagem regressiva. Quando seu aplicativo concluir a operação de salvamento, chame o [**SuspendingDeferral::Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) para informar o sistema que seu aplicativo agora está pronto para ser suspenso. Se você não solicitar um adiamento ou se seu aplicativo demorar mais que cinco segundos para salvar os dados, ele será suspenso automaticamente.

Essa chamada de retorno ocorre como uma mensagem de evento processada pelo [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) para o [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) do aplicativo. Essa chamada de retorno não será solicitada se você não chamar o [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) no loop principal do aplicativo (implementado no método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) de seu provedor de visualização).

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="call-trim"></a>Chamar Trim()


A partir do Windows 8.1, todos os aplicativos UWP DirectX devem chamar [**Idxgidevice3**](https://msdn.microsoft.com/library/windows/desktop/dn280346) suspensão. Essa chamada pede para o driver gráfico liberar todos os buffers temporários alocados para o aplicativo evitando que o aplicativo seja encerrado para recuperar recursos de memória durante o estado de suspensão. Isso é um requisito de certificação para Windows 8.1.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}

// Call this method when the app suspends. It provides a hint to the driver that the app 
// is entering an idle state and that temporary buffers can be reclaimed for use by other apps.
void DX::DeviceResources::Trim()
{
    ComPtr<IDXGIDevice3> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    dxgiDevice->Trim();
}
```

## <a name="release-any-exclusive-resources-and-file-handles"></a>Liberar identificadores de arquivos e recursos exclusivos


Quando seu aplicativo manipula o evento [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860), ele também tem a oportunidade de liberar indicadores de arquivos e recursos exclusivos. Liberar explicitamente os indicadores de arquivos e recursos exclusivos ajuda a garantir que outros aplicativos possam acessá-los quando não estiverem sendo usados pelo seu aplicativo. Quando o aplicativo for ativado após o encerramento, ele deverá abrir seus indicadores de arquivos e recursos exclusivos.

## <a name="remarks"></a>Comentários


O sistema suspende o aplicativo sempre que o usuário alterna para outro aplicativo ou para a área de trabalho. O sistema retoma o seu aplicativo sempre que o usuário alterna de volta para ele. Quando o sistema retoma o aplicativo, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do aplicativo pelo sistema. O sistema retoma o aplicativo exatamente de onde parou, o que faz parecer ao usuário que ele estava sendo executado em tela de fundo.

O sistema tenta manter o aplicativo e seus dados na memória enquanto ele está suspenso. Entretanto, caso não tenha recursos suficientes para manter o aplicativo na memória, o sistema encerra o aplicativo. Quando o usuário alterna de volta para um aplicativo suspenso que foi encerrado, o sistema envia um evento [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) e deve restaurar os dados do aplicativo em seu manipulador para o evento **CoreApplicationView::Activated**.

O sistema não notifica um aplicativo quando ele está encerrado, por isso seu aplicativo deve salvar seus dados de aplicativo e liberar identificadores de arquivo e recursos exclusivos quando suspenso e restaurá-los quando ativado após o encerramento.

## <a name="related-topics"></a>Tópicos relacionados

* [Como retomar um aplicativo (DirectX e C++)](how-to-resume-an-app-directx-and-cpp.md)
* [Como ativar um aplicativo (DirectX e C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




