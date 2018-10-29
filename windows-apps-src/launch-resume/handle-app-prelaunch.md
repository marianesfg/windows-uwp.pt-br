---
author: TylerMSFT
title: Tratar a pré-inicialização do aplicativo
description: Saiba como manipular a pré-inicialização de aplicativos substituindo o método OnLaunched e chamando CoreApplication.EnablePrelaunch(true).
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a13ec942080d7fe517a10b837bea9ae8fae27750
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5740090"
---
# <a name="handle-app-prelaunch"></a>Tratar a pré-inicialização do aplicativo

Saiba como manipular a pré-inicialização de aplicativos substituindo o método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## <a name="introduction"></a>Introdução

Quando os recursos do sistema disponíveis permitirem, o desempenho de inicialização de aplicativos UWP em dispositivos da família de dispositivos de desktop é melhorado iniciando-se proativamente os aplicativos mais usados do usuário em segundo plano. Um aplicativo pré-lançado é colocado no estado suspenso pouco depois de ser iniciado. Depois, quando o usuário invoca o aplicativo, o aplicativo é retomado com a saída do estado suspenso para o estado em execução – que é mais rápido do que iniciar o aplicativo a frio. A experiência do usuário é que o aplicativo simplesmente iniciou muito rapidamente.

Antes do Windows 10, os aplicativos não usufruíam automaticamente de pré-inicialização. No Windows 10, versão 1511, todos os aplicativos da plataforma Universal do Windows (UWP) eram candidatos para serem pré-inicializados. No Windows 10, versão 1607, você deve ativar o comportamento de pré-inicialização chamando [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx). Um bom lugar para colocar essa chamada é em `OnLaunched()`, perto do local em que a verificação `if (e.PrelaunchActivated == false)` feita.

A pré-inicialização do aplicativo depende de recursos do sistema. Se o sistema estiver enfrentando pressão dos recursos, os aplicativos não serão pré-inicializados.

Alguns tipos de aplicativo talvez precisem alterar o comportamento de inicialização para funcionarem bem com o pré-lançamento. Por exemplo, um aplicativo que reproduz música quando é iniciado, um jogo que pressupõe que o usuário está presente e exibe elementos visuais elaborados quando o aplicativo é inicializado, um aplicativo de mensagens que altera a visibilidade online do usuário durante a inicialização, pode identificar quando o aplicativo foi pré-inicializado e pode alterar o comportamento de inicialização conforme descrito nas seções a seguir.

Os modelos padrão de projetos XAML (C#, VB, C++) e WinJS aceitam pré-inicialização no Visual Studio 2015 Atualização 3.

## <a name="prelaunch-and-the-app-lifecycle"></a>Pré-lançamento e ciclo de vida do aplicativo

Assim que é pré-inicializado, um aplicativo entra no estado suspenso. (Consulte [Manipular a suspensão de aplicativo](suspend-an-app.md).)

## <a name="detect-and-handle-prelaunch"></a>Detectar e manipular a pré-inicialização

Os aplicativos recebem o sinalizador [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) durante a ativação. Use esse sinalizador para executar o código que deve executar somente quando o usuário inicia explicitamente o aplicativo, conforme mostrado na seguinte a modificação [**Application. OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // CoreApplication.EnablePrelaunch was introduced in Windows 10 version 1607
    bool canEnablePrelaunch = Windows.Foundation.Metadata.ApiInformation.IsMethodPresent("Windows.ApplicationModel.Core.CoreApplication", "EnablePrelaunch");

    // NOTE: Only enable this code if you are targeting a version of Windows 10 prior to version 1607
    // and you want to opt-out of prelaunch.
    // In Windows 10 version 1511, all UWP apps were candidates for prelaunch.
    // Starting in Windows 10 version 1607, the app must opt-in to be prelaunched.
    //if ( !canEnablePrelaunch && e.PrelaunchActivated == true)
    //{
    //    return;
    //}

    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        // On Windows 10 version 1607 or later, this code signals that this app wants to participate in prelaunch
        if (canEnablePrelaunch)
        {
            TryEnablePrelaunch();
        }

        // TODO: This is not a prelaunch activation. Perform operations which
        // assume that the user explicitly launched the app such as updating
        // the online presence of the user on a social network, updating a
        // what's new feed, etc.

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
}

/// <summary>
/// Encapsulates the call to CoreApplication.EnablePrelaunch() so that the JIT
/// won't encounter that call (and prevent the app from running when it doesn't
/// find it), unless this method gets called. This method should only
/// be called when the caller determines that we are running on a system that
/// supports CoreApplication.EnablePrelaunch().
/// </summary>
private void TryEnablePrelaunch()
{
    Windows.ApplicationModel.Core.CoreApplication.EnablePrelaunch(true);
}
```

Observe o `TryEnablePrelaunch()` funcionar, acima. O motivo pelo qual a chamada para `CoreApplication.EnablePrelaunch()` é acrescentado fora para essa função é como quando um método é chamado, o JIT (apenas na compilação de time) tentará compilar todo o método. Se seu aplicativo é executado em uma versão do Windows 10 que não dá suporte a `CoreApplication.EnablePrelaunch()`, em seguida, o JIT falhará. Por fatoração a chamada para um método que é chamado apenas quando o aplicativo determina que a plataforma oferece suporte a `CoreApplication.EnablePrelaunch()`, podemos evitar esse problema.

Há também código no exemplo acima que você remova se seu aplicativo precisa recusar de pré-inicialização quando em execução no Windows 10, versão 1511. Na versão 1511, todos os aplicativos UWP foram aceitou automaticamente em pré-inicialização, que pode não ser apropriado para seu aplicativo.

## <a name="use-the-visibilitychanged-event"></a>Usar o evento VisibilityChanged

Os aplicativos ativados pela pré-inicialização não são visíveis ao usuário. Eles ficam visíveis quando o usuário os alterna. Você poderá adiar determinadas operações até que a janela principal do aplicativo esteja visível. Por exemplo, se o aplicativo exibir uma lista de itens de novidades de um feed, você poderá atualizar a lista durante o evento [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458), em vez de usar a lista que foi criada quando o aplicativo foi pré-inicializado, pois ela pode estar obsoleta no momento em que o usuário ativa o aplicativo. O código a seguir manipula o evento **VisibilityChanged** de **MainPage**:

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## <a name="directx-games-guidance"></a>Diretrizes de jogos em DirectX

Os jogos em DirectX geralmente não devem permitir pré-inicialização porque muitos jogos em DirectX fazem a inicialização antes de a pré-inicialização ser detectada. A partir do Windows 1607, a edição de aniversário, seu jogo não será pré-inicializado por padrão.  Se você quiser que seu jogo tire proveito da pré-inicialização, chame [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx).

Se seu jogo direcionar uma versão anterior do Windows 10, você poderá manipular a condição de pré-inicialização para sair do aplicativo:

```cppwinrt
void ViewProvider::OnActivated(CoreApplicationView const& /* appView */, Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Launch)
    {
        auto launchArgs{ args.as<Windows::ApplicationModel::Activation::LaunchActivatedEventArgs>()};
        if (launchArgs.PrelaunchActivated())
        {
            // Opt-out of Prelaunch.
            CoreApplication::Exit();
        }
    }
}

void ViewProvider::Initialize(CoreApplicationView const & appView)
{
    appView.Activated({ this, &App::OnActivated });
}
```

```cpp
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## <a name="winjs-app-guidance"></a>Diretrizes de aplicativos WinJS

Se seu aplicativo WinJS direciona uma versão anterior do Windows 10, você pode manipular a pré-inicialização condição no manipulador [onactivated](https://msdn.microsoft.com/library/windows/apps/br212679.aspx):

```javascript
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## <a name="general-guidance"></a>Orientações gerais

-   Os aplicativos não devem executar operações de longa duração durante a pré-inicialização porque o aplicativo será encerrado se não puder ser suspenso rapidamente.
-   Os aplicativos não devem iniciar a reprodução de áudio de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) quando o aplicativo for pré-inicializado, pois o aplicativo não estará visível e ele não ficará aparente porque o áudio está sendo reproduzido.
-   Os aplicativos não devem executar operações durante a inicialização que pressuponham que o aplicativo esteja visível para o usuário ou que o aplicativo tenha sido iniciado explicitamente pelo usuário. Como um aplicativo agora pode ser iniciado em segundo plano sem ação explícita do usuário, os desenvolvedores devem levar em consideração a privacidade, a experiência do usuário e as implicações sobre o desempenho.
    -   Um exemplo de consideração de privacidade acontece quando um aplicativo de rede social deve alterar o estado do usuário para online. Ele deve aguardar até que o usuário alterne para o aplicativo, em vez de alterar o status quando o aplicativo é pré-inicializado.
    -   Um exemplo de consideração de experiência do usuário é que, se você tiver um aplicativo, como um jogo, que exiba uma sequência introdutória quando é iniciado, você poderá atrasar a sequência introdutória até o usuário alternar para o aplicativo.
    -   Um exemplo de implicação de desempenho é que você poderá aguardar até o usuário alternar para o aplicativo a fim de recuperar as informações de clima atuais, em vez de carregá-las quando o aplicativo é pré-inicializado e, depois, recarregá-las quando o aplicativo ficar visível, para garantir que as informações estejam atualizadas.
-   Caso o aplicativo limpe o Bloco Dinâmico quando iniciado, adie isso até o evento de visibilidade alterada.
-   A telemetria do aplicativo deve distinguir ativações de bloco normais e ativações de pré-lançamento para facilitar a limitação do cenário se problemas ocorrerem.
-   Se você tiver o Microsoft Visual Studio2015 atualização 1 e o Windows 10, versão 1511, você pode simular a pré-inicialização do seu aplicativo no Visual Studio2015 escolhendo **Depurar** &gt; **Outros destinos de depuração** &gt; **Depurar aplicativo Universal do Windows Pré-inicialização**.

## <a name="related-topics"></a>Tópicos relacionados

* [Ciclo de vida do aplicativo](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)
