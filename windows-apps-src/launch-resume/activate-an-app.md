---
title: Manipular a ativação do aplicativo
description: Aprenda a manipular ativação de aplicativo substituindo o método OnLaunched.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
---

# Manipular a ativação do aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Aprenda a manipular ativação de aplicativo substituindo o método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## Substitua o manipulador de inicialização

Quando um aplicativo está ativado por algum motivo, o sistema envia o evento [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018). Para obter uma lista de tipos de ativação, consulte a enumeração [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

A classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) define métodos que você pode substituir para manipular os vários tipos de ativação. Muitos tipos de ativação têm um método específico que você pode substituir. Para os outros tipo de ativação, substitua o método [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

Defina a classe do seu aplicativo.

```xaml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
             x:Class="AppName.App" >
```

Substitua o método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). Esse método é chamado sempre que o usuário abre o aplicativo. O parâmetro [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contém o estado anterior do seu aplicativo e os argumentos de ativação.

**Observação**  Para aplicativos Loja do Windows Phone, esse método é chamado cada vez que o usuário abre o aplicativo no Bloco iniciar ou na Lista de aplicativos, mesmo quando ele está suspenso na memória. No Windows, abrir um aplicativo suspenso no Bloco iniciar ou na Lista de aplicativos chama esse método.

> [!div class="tabbedCodeSnippets"]
```cs
using System;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace AppName
{
   public partial class App
   {
      async protected override void OnLaunched(LaunchActivatedEventArgs args)
      {
         EnsurePageCreatedAndActivate();
      }
      
      // Creates the MainPage if it isn't already created.  Also activates
      // the window so it takes foreground and input focus.
      private MainPage EnsurePageCreatedAndActivate()
      {
         if (Window.Current.Content == null)
         {
             Window.Current.Content = new MainPage();
         }
         
         Window.Current.Activate();
         return Window.Current.Content as MainPage;
      }
   }
}
```
```vb
Class App
   Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
      Window.Current.Content = New MainPage()
      Window.Current.Activate()
   End Sub
End Class
```
```cpp
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;
void App::OnLaunched(LaunchActivatedEventArgs^ args)
{
   EnsurePageCreatedAndActivate();
}

// Creates the MainPage if it isn't already created.  Also activates
// the window so it takes foreground and input focus.
void App::EnsurePageCreatedAndActivate()
{
    if (_mainPage == nullptr)
    {
        // Save the MainPage for use if we get activated later
        _mainPage = ref new MainPage();
    }
    Window::Current->Content = _mainPage;
    Window::Current->Activate();
}
```

## Restaurar dados do aplicativo se ele for suspenso e encerrado


Quando o usuário muda para o seu aplicativo encerrado, o sistema manda o evento [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018), com [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) definido como **Abrir** e [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) definido como **Encerrado** ou **ClosedByUser**. O aplicativo deve carregar seus dados salvos e atualizar o conteúdo exibido.

> [!div class="tabbedCodeSnippets"]
```cs
async protected override void OnLaunched(LaunchActivatedEventArgs args)
{
   if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
       args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }
   
   EnsurePageCreatedAndActivate();
}
```
```vb
Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
   Dim restoreState As Boolean = False

   Select Case args.PreviousExecutionState
      Case ApplicationExecutionState.Terminated
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case ApplicationExecutionState.ClosedByUser
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case Else
         ' TODO: Populate the UI with defaults
   End Select

   Window.Current.Content = New MainPage(restoreState)
   Window.Current.Activate()
End Sub
```
```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
   if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
       args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

Se o valor de [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) estiver como **NotRunning**, o aplicativo não conseguiu salva seus dados com sucesso e ele deve iniciar novamente como se estivesse sido aberto inicialmente.

## Comentários

> **Observação**  Para aplicativos da Loja do Windows Phone, o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) é sempre seguido por [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), mesmo quando o seu aplicativo está suspenso no momento e o usuário reabri-lo em um bloco principal ou lista de aplicativos. Os aplicativos podem pular a inicialização se já houver conteúdo definido na janela atual. Você pode verificar a propriedade [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar se o aplicativo foi aberto de um bloco principal ou secundário e, com base nisso, decidir se você deve ou não retomar a experiência de aplicativo a partir daquele ponto ou apresentar uma nova.

## Tópicos relacionados

* [Manipular a suspensão do aplicativo](suspend-an-app.md)
* [Manipular a retomada do aplicativo](resume-an-app.md)
* [Diretrizes para suspensão e retomada de aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Ciclo de vida do aplicativo](app-lifecycle.md)

**Referência**

* [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br242324)

 

 





<!--HONumber=Mar16_HO1-->


