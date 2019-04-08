---
title: Exibir uma tela inicial por mais tempo
description: Exiba uma tela inicial por mais tempo criando uma tela inicial estendida para o seu aplicativo. Essa tela estendida imita a tela inicial exibida quando o aplicativo é iniciado, mas pode ser personalizada.
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bed81def33eedb79619b49ff698a3f45f31bdb62
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615891"
---
# <a name="display-a-splash-screen-for-more-time"></a>Exibir uma tela inicial por mais tempo

**APIs importantes**

-   [Classe SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763)
-   [Evento Window.SizeChanged](https://msdn.microsoft.com/library/windows/apps/br209055)
-   [Método Application.OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335)

Exiba uma tela inicial por mais tempo criando uma tela inicial estendida para o seu aplicativo. Essa tela estendida imita a tela inicial exibida quando o aplicativo é iniciado, mas pode ser personalizada. Seja para mostrar informações de carregamento em tempo real ou para simplesmente proporcionar ao aplicativo mais tempo para preparar a interface do usuário inicial, uma tela inicial estendida permite definir a experiência de inicialização.

> [!NOTE]
> A frase "estendido tela inicial" neste tópico se refere a uma tela inicial que permanecerá na tela por um longo período de tempo. Isso não significa que uma subclasse que deriva de [SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763) classe.

Verifique se sua tela inicial estendida imita com precisão a tela inicial padrão seguindo estas recomendações:

-   Sua página de tela inicial estendida deve usar uma imagem de 620 x 300 pixels consistente com a imagem especificada para a sua tela inicial no manifesto do aplicativo (imagem da tela inicial do aplicativo). No Microsoft Visual Studio 2015, configurações de tela de abertura são armazenadas na **tela inicial** seção o **ativos visuais** guia em seu manifesto de aplicativo (arquivo Package. appxmanifest).
-   Sua tela inicial estendida deve usar uma cor de tela de fundo consistente com a cor de tela de fundo especificada para a sua tela inicial no manifesto do aplicativo (tela de fundo da tela inicial do seu aplicativo).
-   Seu código deve usar o [SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763) coordenadas de classe para posicionar a imagem da tela inicial do seu aplicativo na mesma tela como a tela inicial padrão.
-   Seu código deve responder a eventos de redimensionamento de janela (como quando a tela é girada ou seu aplicativo é movido ao lado do outro aplicativo na tela) usando o [SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763) classe para reposicionar os itens em sua tela inicial estendido.

Use as etapas a seguir para criar uma tela inicial estendida que imita efetivamente a tela inicial padrão.

## <a name="add-a-blank-page-item-to-your-existing-app"></a>Adicione um item **Página em Branco** ao aplicativo existente


Este tópico pressupõe que você deseja adicionar uma tela inicial estendida a um projeto de aplicativo existente da Plataforma Universal do Windows (UWP) em C#, Visual Basic ou C++.

-   Abra o aplicativo no Visual Studio.
-   Pressione ou abra **Projeto** na barra de menus e clique em **Adicionar Novo Item**. Uma caixa de diálogo **Adicionar Novo Item** será aberta.
-   Nessa caixa de diálogo, adicione uma nova **Página em Branco** ao aplicativo. Esse tópico nomeia a página de tela inicial estendida como "ExtendedSplash".

Adicionar um item **Página em Branco** gera dois arquivos, um para marcação (ExtendedSplash.xaml) e outro para código (ExtendedSplash.xaml.cs).

## <a name="essential-xaml-for-an-extended-splash-screen"></a>XAML essencial para uma tela inicial estendida


Siga estas etapas para adicionar uma imagem e um controle de progresso à sua tela inicial estendida.

No arquivo ExtendedSplash.xaml:

-   Alterar o [plano de fundo](https://msdn.microsoft.com/library/windows/apps/br209396) propriedade padrão do [grade](https://msdn.microsoft.com/library/windows/apps/br242704) elemento para coincidir com a cor de fundo que você definir para a tela inicial do seu aplicativo em seu manifesto de aplicativo (no **ativos visuais**seção do arquivo Package. appxmanifest). A cor da tela inicial padrão é cinza-claro (valor hexadecimal \#464646). Observe que esse elemento **Grid** é fornecido por padrão ao criar uma nova **Página em Branco**. Não é necessário usar **Grid**; ele é apenas uma base conveniente para criar uma tela inicial estendida.
-   Adicionar um [Canvas](https://msdn.microsoft.com/library/windows/apps/br209267) elemento para o [grade](https://msdn.microsoft.com/library/windows/apps/br242704). Você usará essa **Canvas** para posicionar a imagem da tela inicial estendida.
-   Adicionar um [imagem](https://msdn.microsoft.com/library/windows/apps/br242752) elemento para o [tela](https://msdn.microsoft.com/library/windows/apps/br209267). Use a mesma imagem de 600 x 320 pixels para sua tela inicial estendida que escolheu para a tela inicial padrão.
-   (Opcional) Adicione um controle de progresso para mostrar aos usuários que o aplicativo está sendo carregado. Este tópico adiciona uma [ProgressRing](https://msdn.microsoft.com/library/windows/apps/br227538), em vez de uma determinada ou deixar indeterminada [ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529).

O exemplo a seguir demonstra uma [grade](https://msdn.microsoft.com/library/windows/apps/br242704) com essas adições e alterações.

```xaml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

> [!NOTE]
> Este exemplo define a largura do [ProgressRing](https://msdn.microsoft.com/library/windows/apps/br227538) a 20 pixels. Você pode definir manualmente a largura com um valor que funciona para o seu aplicativo, porém o controle não renderizará em larguras de menos de 20 pixels.

## <a name="essential-code-for-an-extended-splash-screen-class"></a>Código essencial para uma classe de tela inicial estendida


A tela inicial estendida precisa responder sempre que o tamanho (somente Windows) ou a orientação da janela muda. A posição da imagem que você usa deve ser atualizada de maneira que a tela inicial estendida tenha boa aparência independentemente de como a janela mudar.

Use estas etapas de forma a definir os métodos para a exibição correta da sua tela inicial estendida.

1.  **Adicionar namespaces necessários**

    Você precisará adicionar os seguintes namespaces a serem **ExtendedSplash.xaml.cs** para acessar o [SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763) classe, o [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) struct e o [ Window.SizeChanged](https://msdn.microsoft.com/library/windows/apps/br209055) eventos.

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.Foundation;
    using Windows.UI.Core;
    ```

2.  **Criar uma classe parcial e declarar variáveis de classe**

    Inclua o código a seguir em ExtendedSplash.xaml.cs para criar uma classe parcial para representar uma tela inicial estendida.

    ```cs
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

       // Define methods and constructor
    }
    ```

    Estas variáveis de classe são usadas por vários métodos. A variável `splashImageRect` armazena as coordenadas em que o sistema exibiu a imagem de tela inicial para o aplicativo. O `splash` variável armazena uma [SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763) objeto e o `dismissed` variável controla se a tela inicial que é exibida pelo sistema foram ignorada.

3.  **Definir um construtor para sua classe que posiciona corretamente a imagem**

    O código a seguir define um construtor para a classe de tela inicial estendida que escuta eventos de redimensionamento de janelas, posiciona a imagem e o controle de progresso (opcional) na tela inicial estendida, cria um quadro para navegação e chama um método assíncrono para restaurar um estado de sessão salvo.

    ```cs
    public ExtendedSplash(SplashScreen splashscreen, bool loadState)
    {
        InitializeComponent();

        // Listen for window resize events to reposition the extended splash screen image accordingly.
        // This ensures that the extended splash screen formats properly in response to window resizing.
        Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

        splash = splashscreen;
        if (splash != null)
        {
            // Register an event handler to be executed when the splash screen has been dismissed.
            splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

            // Retrieve the window coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            PositionRing();
        }

        // Create a Frame to act as the navigation context
        rootFrame = new Frame();            
    }
    ```

    Certifique-se de registrar seu [Window.SizeChanged](https://msdn.microsoft.com/library/windows/apps/br209055) manipulador (`ExtendedSplash_OnResize` no exemplo) em seu construtor de classe, de modo que seu aplicativo posiciona a imagem corretamente em sua tela inicial estendido.

4.  **Defina um método de classe para posicionar a imagem em sua tela inicial estendido**

    Esse código demonstra como posicionar a imagem na página de tela inicial estendida com a variável de classe `splashImageRect`.

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **(Opcional) Defina um método de classe para posicionar um controle de progresso em sua tela inicial estendido**

    Se você optar por adicionar um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/br227538) para a tela inicial estendido, posicione-o em relação a imagem da tela inicial. Adicione o código a seguir a ExtendedSplash.xaml.cs para centralizar **ProgressRing** 32 pixels abaixo da imagem.

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **Dentro da classe, definir um manipulador para o evento descartado**

    Em ExtendedSplash.xaml.cs, responder quando o [SplashScreen.Dismissed](https://msdn.microsoft.com/library/windows/apps/br224764) ocorre um evento, definindo o `dismissed` variável de classe como true. Se o aplicativo tiver operações de instalação, adicione-as a esse manipulador de eventos.

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    Depois que a instalação do aplicativo estiver concluída, saia da tela inicial estendida. O código a seguir define um método chamado `DismissExtendedSplash` que navega até a `MainPage` definida no arquivo MainPage.xaml do aplicativo.

    ```cs
    async void DismissExtendedSplash()
      {
         await Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,() =>            {
              rootFrame = new Frame();
              rootFrame.Content = new MainPage(); Window.Current.Content = rootFrame;
            });
      }
      ```

7.  **Dentro da classe, definir um manipulador de eventos Window.SizeChanged**

    Prepare a tela inicial estendida para reposicionar seus elementos se um usuário redimensionar a janela. Esse código responde quando um [Window.SizeChanged](https://msdn.microsoft.com/library/windows/apps/br209055) evento ocorre, capturando as novas coordenadas e reposicionamento a imagem. Se você adicionou um controle de progresso à tela inicial estendida, reposicione-a também no manipulador de eventos.

    ```cs
    void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
    {
        // Safely update the extended splash screen image coordinates. This function will be executed when a user resizes the window.
        if (splash != null)
        {
            // Update the coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            // PositionRing();
        }
    }
    ```

    > [!NOTE]
    > Antes de tentar obter o imagem local, verifique se a variável de classe (`splash`) contém válido [SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763) do objeto, conforme mostrado no exemplo.

     

8.  **(Opcional) Adicione um método de classe para restaurar um estado de sessão salva**

    O código adicionado para o [OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335) método na etapa 4: [Modifique o manipulador de ativação de inicialização](#modify-the-launch-activation-handler) faz com que seu aplicativo exibir uma tela de abertura estendida quando ele é iniciado. Você pode considerar para consolidar todos os métodos relacionados à inicialização de aplicativo em sua classe de tela de abertura estendido, adicionando um método ao seu arquivo ExtendedSplash.xaml.cs para restaurar o estado do aplicativo.

    ```cs
    void RestoreState(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    Quando você modifica o manipulador de ativação de inicialização em App.xaml.cs, você também definirá `loadstate` como true se o anterior [ApplicationExecutionState](https://msdn.microsoft.com/library/windows/apps/br224694) de seu aplicativo foi **encerrado**. Nesse caso, o método `RestoreState` restaura o aplicativo ao estado anterior. Para obter uma visão geral de inicialização, suspensão e encerramento do aplicativo, consulte o [ciclo de vida do aplicativo](app-lifecycle.md).

## <a name="modify-the-launch-activation-handler"></a>Modificar o manipulador de ativação de inicialização


Quando o aplicativo é iniciado, o sistema passa informações da tela inicial ao manipulador de eventos de ativação de inicialização desse aplicativo. Você pode usar essas informações para posicionar corretamente a imagem na sua página de tela inicial estendida. Você pode obter essas informações de tela inicial da ativação argumentos de evento que são passados para seu aplicativo [OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335) manipulador (consulte a `args` variável no código a seguir).

Se você já não tiver substituído o [OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335) manipulador para seu aplicativo, consulte [ciclo de vida do aplicativo](app-lifecycle.md) para aprender a lidar com eventos de ativação.

Em App.xaml.cs, adicione o código a seguir para criar e exibir uma tela inicial estendida.

```cs
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    if (args.PreviousExecutionState != ApplicationExecutionState.Running)
    {
        bool loadState = (args.PreviousExecutionState == ApplicationExecutionState.Terminated);
        ExtendedSplash extendedSplash = new ExtendedSplash(args.SplashScreen, loadState);
        Window.Current.Content = extendedSplash;
    }

    Window.Current.Activate();
}
```

## <a name="complete-code"></a>Código completo

O código a seguir difere um pouco dos trechos de código mostrados nas etapas anteriores.
-   ExtendedSplash.xaml inclui um botão `DismissSplash`. Quando esse botão é clicado, um manipulador de eventos, `DismissSplashButton_Click`, chama o método `DismissExtendedSplash`. No seu aplicativo, chame `DismissExtendedSplash` quando o aplicativo concluir o carregamento de recursos ou a inicialização da interface do usuário.
-   Esse aplicativo também usa um modelo de projeto de aplicativo UWP, que usa [quadro](https://msdn.microsoft.com/library/windows/apps/br242682) navegação. Como resultado, no App.xaml.cs, o manipulador de ativação de inicialização ([OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335)) define um `rootFrame` e o utiliza para definir o conteúdo da janela do aplicativo.

### <a name="extendedsplashxaml"></a>ExtendedSplash.xaml

Este exemplo inclui um `DismissSplash` botão porque ele não tem recursos de aplicativo para carregar. Em seu aplicativo, ignore a tela inicial estendida automaticamente quando o aplicativo tiver concluído o carregamento de recursos ou a preparação de sua interface do usuário inicial.

```xml
<Page
    x:Class="SplashScreenExample.ExtendedSplash"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SplashScreenExample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"/>
        </Canvas>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Bottom">
            <Button x:Name="DismissSplash" Content="Dismiss extended splash screen" HorizontalAlignment="Center" Click="DismissSplashButton_Click" />
        </StackPanel>
    </Grid>
</Page>
```

### <a name="extendedsplashxamlcs"></a>ExtendedSplash.xaml.cs

Observe que o `DismissExtendedSplash` método é chamado do manipulador de eventos para o `DismissSplash` botão. No aplicativo, você não precisará de um botão `DismissSplash`. Em vez disso, chame `DismissExtendedSplash` quando o aplicativo tiver concluído o carregamento de recursos e você quiser navegar até a página principal.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

using Windows.ApplicationModel.Activation;
using SplashScreenExample.Common;
using Windows.UI.Core;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace SplashScreenExample
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

        public ExtendedSplash(SplashScreen splashscreen, bool loadState)
        {
            InitializeComponent();

            // Listen for window resize events to reposition the extended splash screen image accordingly.
            // This is important to ensure that the extended splash screen is formatted properly in response to snapping, unsnapping, rotation, etc...
            Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

            splash = splashscreen;

            if (splash != null)
            {
                // Register an event handler to be executed when the splash screen has been dismissed.
                splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

                // Retrieve the window coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();

                // Optional: Add a progress ring to your splash screen to show users that content is loading
                PositionRing();
            }

            // Create a Frame to act as the navigation context
            rootFrame = new Frame();

            // Restore the saved session state if necessary
            RestoreState(loadState);
        }

        void RestoreState(bool loadState)
        {
            if (loadState)
            {
                // TODO: write code to load state
            }
        }

        // Position the extended splash screen image in the same location as the system splash screen image.
        void PositionImage()
        {
            extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
            extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
            extendedSplashImage.Height = splashImageRect.Height;
            extendedSplashImage.Width = splashImageRect.Width;

        }

        void PositionRing()
        {
            splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
            splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
        }

        void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
        {
            // Safely update the extended splash screen image coordinates. This function will be fired in response to snapping, unsnapping, rotation, etc...
            if (splash != null)
            {
                // Update the coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();
                PositionRing();
            }
        }

        // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
        void DismissedEventHandler(SplashScreen sender, object e)
        {
            dismissed = true;

            // Complete app setup operations here...
        }

        void DismissExtendedSplash()
        {
            // Navigate to mainpage
            rootFrame.Navigate(typeof(MainPage));
            // Place the frame in the current Window
            Window.Current.Content = rootFrame;
        }

        void DismissSplashButton_Click(object sender, RoutedEventArgs e)
        {
            DismissExtendedSplash();
        }
    }
}
```

### <a name="appxamlcs"></a>App.XAML.cs

Este projeto foi criado usando o aplicativo UWP **aplicativo em branco (XAML)** modelo de projeto no Visual Studio. Os manipuladores de eventos `OnNavigationFailed` e `OnSuspending` são gerados automaticamente e não precisam ser modificados para implementar uma tela inicial estendida. Esse tópico só modifica `OnLaunched`.

Se você não usou um modelo de projeto para seu aplicativo, consulte a etapa 4: [Modifique o manipulador de ativação de inicialização](#modify-the-launch-activation-handler) para obter um exemplo de uma modificação `OnLaunched` que não usam [quadro](https://msdn.microsoft.com/library/windows/apps/br242682) navegação.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Application template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234227

namespace SplashScreenExample
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : Application
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Microsoft.ApplicationInsights.WindowsAppInitializer.InitializeAsync(
            Microsoft.ApplicationInsights.WindowsCollectors.Metadata |
            Microsoft.ApplicationInsights.WindowsCollectors.Session);
            this.InitializeComponent();
            this.Suspending += OnSuspending;
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="e">Details about the launch request and process.</param>
        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
#if DEBUG
            if (System.Diagnostics.Debugger.IsAttached)
            {
                this.DebugSettings.EnableFrameRateCounter = true;
            }
#endif

            Frame rootFrame = Window.Current.Content as Frame;

            // Do not repeat app initialization when the Window already has content,
            // just ensure that the window is active
            if (rootFrame == null)
            {
                // Create a Frame to act as the navigation context and navigate to the first page
                rootFrame = new Frame();
                // Set the default language
                rootFrame.Language = Windows.Globalization.ApplicationLanguages.Languages[0];

                rootFrame.NavigationFailed += OnNavigationFailed;

                //  Display an extended splash screen if app was not previously running.
                if (e.PreviousExecutionState != ApplicationExecutionState.Running)
                {
                    bool loadState = (e.PreviousExecutionState == ApplicationExecutionState.Terminated);
                    ExtendedSplash extendedSplash = new ExtendedSplash(e.SplashScreen, loadState);
                    rootFrame.Content = extendedSplash;
                    Window.Current.Content = rootFrame;
                }
            }

            if (rootFrame.Content == null)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(typeof(MainPage), e.Arguments);
            }
            // Ensure the current window is active
            Window.Current.Activate();
        }

        /// <summary>
        /// Invoked when Navigation to a certain page fails
        /// </summary>
        /// <param name="sender">The Frame which failed navigation</param>
        /// <param name="e">Details about the navigation failure</param>
        void OnNavigationFailed(object sender, NavigationFailedEventArgs e)
        {
            throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
        }

        /// <summary>
        /// Invoked when application execution is being suspended.  Application state is saved
        /// without knowing whether the application will be terminated or resumed with the contents
        /// of memory still intact.
        /// </summary>
        /// <param name="sender">The source of the suspend request.</param>
        /// <param name="e">Details about the suspend request.</param>
        private void OnSuspending(object sender, SuspendingEventArgs e)
        {
            var deferral = e.SuspendingOperation.GetDeferral();
            // TODO: Save applicaiton state and stop any background activity
            deferral.Complete();
        }
    }
}
```

## <a name="related-topics"></a>Tópicos relacionados


* [Ciclo de vida do aplicativo](app-lifecycle.md)

**Referência**

* [Namespace ApplicationModel](https://msdn.microsoft.com/library/windows/apps/br224766)
* [Classe Windows.ApplicationModel.Activation.SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763)
* [Propriedade Windows.ApplicationModel.Activation.SplashScreen.ImageLocation](https://msdn.microsoft.com/library/windows/apps/br224765)
* [Evento Windows.ApplicationModel.Core.CoreApplicationView.Activated](https://msdn.microsoft.com/library/windows/apps/br225018)

 

 
