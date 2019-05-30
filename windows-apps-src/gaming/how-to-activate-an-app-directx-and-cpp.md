---
title: Como ativar um aplicativo (DirectX e C++)
description: Este tópico mostra como definir a experiência de ativação para um aplicativo UWP (Plataforma Universal do Windows) DirectX.
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, ativação
ms.localizationpriority: medium
ms.openlocfilehash: 4aeba58af61cffa33626c64cebcbade272af109b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368598"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>Como ativar um aplicativo (DirectX e C++)



Este tópico mostra como definir a experiência de ativação para um aplicativo UWP (Plataforma Universal do Windows) DirectX.

## <a name="register-the-app-activation-event-handler"></a>Registrar o manipulador de eventos de ativação do aplicativo


Primeiro, registre para manipular o evento [**CoreApplicationView::Activated**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) que é acionado quando seu aplicativo é iniciado e inicializado pelo sistema operacional.

Adicione este código à sua implementação do método [**IFrameworkView::Initialize**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) de seu provedor de visualização (nomeado **MyViewProvider** no exemplo):

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## <a name="activate-the-corewindow-instance-for-the-app"></a>Ativar a instância do CoreWindow para o aplicativo


Quando seu aplicativo é iniciado, você deve obter uma referência ao [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) para ele. **O CoreWindow** inclui o dispatcher de mensagem de eventos de janela que seu aplicativo usa para processar os eventos da janela. Obtenha essa referência no retorno de chamada do evento de ativação do aplicativo chamando [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread). Depois que você obtiver essa referência, ative a janela principal do aplicativo chamando [**CoreWindow::Activate**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.activate).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>Iniciar a mensagem do evento de processamento para a janela do aplicativo principal


Seus retornos de chamada ocorrem como mensagens de evento que são processadas pelo [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) para o [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) do aplicativo. Essa chamada de retorno não será solicitada se você não chamar o [**CoreDispatcher::ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) no loop principal do aplicativo (implementado no método [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) de seu provedor de visualização).

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

## <a name="related-topics"></a>Tópicos relacionados


* [Como suspender um aplicativo (DirectX e C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Como reiniciar um aplicativo (DirectX e C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 




