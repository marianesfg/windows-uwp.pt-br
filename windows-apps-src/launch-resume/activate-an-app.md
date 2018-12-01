---
title: Manipular a ativação do aplicativo
description: Aprenda a tratar ativação de app substituindo o método OnLaunched.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: a75136f26aa6cfa330e4118e6709b0b4d4be4054
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8342267"
---
# <a name="handle-app-activation"></a>Tratar a ativação do app

Aprenda a manipular ativação de aplicativo substituindo o método [**Application. OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) .

## <a name="override-the-launch-handler"></a>Substitua o manipulador de inicialização

Quando um aplicativo é ativado, por qualquer motivo, o sistema envia o evento [**Coreapplicationview**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) . Para obter uma lista de tipos de ativação, consulte a enumeração [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

A classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) define métodos que você pode substituir para manipular os vários tipos de ativação. Muitos tipos de ativação têm um método específico que você pode substituir. Para os outros tipo de ativação, substitua o método [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

Defina a classe do seu aplicativo.

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

Substitua o método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). Esse método é chamado sempre que o usuário abre o aplicativo. O parâmetro [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contém o estado anterior do seu aplicativo e os argumentos de ativação.

> [!NOTE]
> No Windows, iniciar um aplicativo suspenso do iniciar a lista de bloco ou o aplicativo não chama esse método.

```csharp
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

```cppwinrt
...
#include "MainPage.h"
#include "winrt/Windows.ApplicationModel.Activation.h"
#include "winrt/Windows.UI.Xaml.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
...
using namespace winrt;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    /// <summary>
    /// Invoked when the application is launched normally by the end user.  Other entry points
    /// will be used such as when the application is launched to open a specific file.
    /// </summary>
    /// <param name="e">Details about the launch request and process.</param>
    void OnLaunched(LaunchActivatedEventArgs const& e)
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
        }

        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active
        if (rootFrame == nullptr)
        {
            // Create a Frame to act as the navigation context and associate it with
            // a SuspensionManager key
            rootFrame = Frame();

            rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

            if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
            {
                // Restore the saved session state only when appropriate, scheduling the
                // final launch steps after the restore is complete
            }

            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Place the frame in the current Window
                Window::Current().Content(rootFrame);
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
        else
        {
            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
    }
};
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

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>Restaurar os dados de aplicativo se o aplicativo foi suspenso e depois terminado

Quando o usuário muda para o seu aplicativo encerrado, o sistema manda o evento [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018), com [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) definido como **Abrir** e [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) definido como **Encerrado** ou **ClosedByUser**. O aplicativo deve carregar seus dados salvos e atualizar o conteúdo exibido.

```csharp
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

```cppwinrt
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs const& e)
{
    if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated ||
        e.PreviousExecutionState() == ApplicationExecutionState::ClosedByUser)
    {
        // Populate the UI with the previously saved application data.
    }
    else
    {
        // Populate the UI with defaults.
    }
    ...
}
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

## <a name="remarks"></a>Comentários

> [!NOTE]
> Os aplicativos podem pular a inicialização se já houver conteúdo definido na janela atual. Você pode verificar a propriedade [**Tileid**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar se o aplicativo foi iniciado em principal ou um bloco secundário e, com base nisso, decidir se deve apresentar uma nova ou não retomar a experiência de aplicativo.

## <a name="important-apis"></a>APIs Importantes
* [Windows.ApplicationModel.Activation](https://msdn.microsoft.com/library/windows/apps/br224766)
* [Windows.UI.Xaml.Automation](https://msdn.microsoft.com/library/windows/apps/br242324)

## <a name="related-topics"></a>Tópicos relacionados
* [Manipular a suspensão do aplicativo](suspend-an-app.md)
* [Manipular a retomada do aplicativo](resume-an-app.md)
* [Diretrizes para suspensão e retomada de aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Ciclo de vida do aplicativo](app-lifecycle.md)
