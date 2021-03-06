---
title: Como retomar um aplicativo (DirectX e C++)
description: Este tópico mostra como restaurar dados importantes do aplicativo quando o sistema retoma o aplicativo UWP (Plataforma Universal do Windows) DirectX.
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, retomar, directx
ms.localizationpriority: medium
ms.openlocfilehash: b1506351dd06563386154ac35938cbd17f5ced32
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368610"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>Como retomar um aplicativo (DirectX e C++)



Este tópico mostra como restaurar dados importantes do aplicativo quando o sistema retoma o aplicativo UWP (Plataforma Universal do Windows) DirectX.

## <a name="register-the-resuming-event-handler"></a>Registrar o manipulador de eventos de retomada


Registre para manipular o evento [**CoreApplication::Resuming**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.resuming) que indica que o usuário alternou para outro aplicativo que não o seu e, depois, retornou.

Adicione este código à sua implementação do método [**IFrameworkView::Initialize**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) de seu provedor de exibição:

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## <a name="refresh-displayed-content-after-suspension"></a>Atualizar o conteúdo exibido após a suspensão


Quando seu aplicativo manipula o evento Resuming, ele tem a oportunidade de atualizar o conteúdo exibido. Restaure qualquer aplicativo que você tenha salvo com seu manipulador para [**CoreApplication::Suspending**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.suspending) e reinicie o processamento. Desenvolvedores de jogos: se você suspendeu seu mecanismo de áudio, agora é o momento de reiniciá-lo.

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

Essa chamada de retorno ocorre como uma mensagem de evento processada pelo [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) para o [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) do aplicativo. Essa chamada de retorno não será solicitada se você não chamar o [**CoreDispatcher::ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) no loop principal do aplicativo (implementado no método [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) de seu provedor de visualização).

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

## <a name="remarks"></a>Comentários


O sistema suspende o aplicativo sempre que o usuário alterna para outro aplicativo ou para a área de trabalho. O sistema retoma o seu aplicativo sempre que o usuário alterna de volta para ele. Quando o sistema retoma o aplicativo, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do aplicativo pelo sistema. O sistema restaura o aplicativo exatamente como ele havia parado, de maneira que o usuário tem impressão de que ele estava sendo executado em tela de fundo. Porém, o aplicativo pode ter sido suspenso durante um período de tempo significativo, assim ele deve atualizar qualquer conteúdo exibido que pode ter alterado enquanto o aplicativo estava suspenso,e reinicie quaisquer threads de renderização ou processamento de áudio. Se você salvou quaisquer dados de estado de jogo durante um evento suspenso anterior, restaure-o agora.

## <a name="related-topics"></a>Tópicos relacionados

* [Como suspender um aplicativo (DirectX e C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Como ativar um aplicativo (DirectX e C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




