---
author: mtoepke
title: Como ativar um aplicativo (DirectX e C++)
description: "Este tópico mostra como definir a experiência de ativação para um aplicativo UWP (Plataforma Universal do Windows) DirectX."
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 14859d03c7af45a17772c76f8c79b3c1bc56272c

---

# Como ativar um aplicativo (DirectX e C++)


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico mostra como definir a experiência de ativação para um aplicativo UWP (Plataforma Universal do Windows) DirectX.

## Registrar o manipulador de eventos de ativação do aplicativo


Primeiro, registre para manipular o evento [**CoreApplicationView::Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) que é acionado quando seu aplicativo é iniciado e inicializado pelo sistema operacional.

Adicione este código à sua implementação do método [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) de seu provedor de visualização (nomeado **MyViewProvider** no exemplo):

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

## Ativar a instância do CoreWindow para o aplicativo


Quando seu aplicativo é iniciado, você deve obter uma referência ao [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para ele. **O CoreWindow** inclui o dispatcher de mensagem de eventos de janela que seu aplicativo usa para processar os eventos da janela. Obtenha essa referência no retorno de chamada do evento de ativação do aplicativo chamando [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589). Depois que você obtiver essa referência, ative a janela principal do aplicativo chamando [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## Iniciar a mensagem do evento de processamento para a janela do aplicativo principal


Seus retornos de chamada ocorrem como mensagens de evento que são processadas pelo [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) para o [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) do aplicativo. Essa chamada de retorno não será solicitada se você não chamar o [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) no loop principal do aplicativo (implementado no método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) de seu provedor de visualização).

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

## Tópicos relacionados


* [Como suspender um aplicativo (DirectX e C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Como retomar um aplicativo (DirectX e C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 







<!--HONumber=Jun16_HO4-->


