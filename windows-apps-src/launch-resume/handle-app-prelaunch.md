---
author: mcleblanc
title: Manipular a pré-inicialização do aplicativo
description: Saiba como manipular a pré-inicialização de aplicativos substituindo o método OnLaunched.
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
---

# Manipular a pré-inicialização do aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Saiba como manipular a pré-inicialização de aplicativos substituindo o método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## Introdução


Quando os recursos do sistema disponíveis permitirem, o desempenho de inicialização dos aplicativos da Windows Store é melhorado iniciando-se proativamente os aplicativos mais usados do usuário em segundo plano. Um aplicativo pré-lançado é colocado no estado suspenso pouco depois de ser iniciado. Quando o usuário invoca o aplicativo, o aplicativo é retomado com a saída do estado suspenso para o estado em execução – que é mais rápido do que iniciar o aplicativo a frio.

Antes do Windows 10, os aplicativos não usufruíam automaticamente de pré-inicialização. A partir do Windows 10, todos os aplicativos UWP (Plataforma Universal do Windows) usufruem automaticamente de pré-inicialização.

A maioria dos aplicativos deve funcionar com o pré-lançamento sem nenhuma alteração. No entanto, alguns tipos de aplicativos talvez precisem alterar o comportamento de inicialização para funcionarem bem com o pré-lançamento. Por exemplo, um aplicativo de sistema de mensagens que altera a visibilidade online do usuário durante a inicialização ou um jogo que pressupõe que o usuário esteja presente e exibe elementos visuais elaborados quando o aplicativo é iniciado.

## Pré-lançamento e ciclo de vida do aplicativo


Assim que é pré-inicializado, um aplicativo entra no estado suspenso. (Consulte [Manipular a suspensão de aplicativo](suspend-an-app.md).)

## Detectar e manipular a pré-inicialização


Os aplicativos recebem o sinalizador [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) durante a ativação. Use esse sinalizador para determinar se é necessário realizar operações que devem ser feitas somente quando o usuário iniciar o aplicativo explicitamente conforme mostrado no trecho a seguir de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a 
            // what's new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Dica**  Se você quiser recusar a pré-inicialização, marque o sinalizador [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740). Se ele estiver definido, retorne de OnLaunched() antes de fazer qualquer trabalho para criar um quadro ou ativar a janela.

 

## Usar o evento VisibilityChanged


Os aplicativos ativados pela pré-inicialização não são visíveis ao usuário. Eles ficam visíveis quando o usuário os alterna. Você poderá adiar determinadas operações até que a janela principal do aplicativo esteja visível. Por exemplo, se o aplicativo exibir uma lista de itens de novidades de um feed, você poderá atualizar a lista durante o evento [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458), em vez de se basear em uma lista que foi criada quando o aplicativo foi pré-inicializado e pode estar obsoleta no momento em que o usuário ativa o aplicativo. O código a seguir manipula o evento **VisibilityChanged** de **MainPage**:

```cs
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

## Diretrizes


-   Os aplicativos não devem executar operações de longa duração durante a pré-inicialização porque o aplicativo será encerrado se não puder ser suspenso rapidamente.
-   Os aplicativos não devem iniciar a reprodução de áudio de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) quando o aplicativo for pré-inicializado, pois o aplicativo não estará visível e ele não ficará aparente porque o áudio está sendo reproduzido.
-   Os aplicativos não devem executar operações durante a inicialização que pressuponham que o aplicativo esteja visível para o usuário ou que o aplicativo tenha sido iniciado explicitamente pelo usuário. Como um aplicativo agora pode ser iniciado em segundo plano sem ação explícita do usuário, os desenvolvedores devem levar em consideração a privacidade, a experiência do usuário e as implicações sobre o desempenho.
    -   Um exemplo de consideração de privacidade acontece quando um aplicativo de rede social deve alterar o estado do usuário para online. Ele deve aguardar até que o usuário alterne para o aplicativo, em vez de alterar o status quando o aplicativo é pré-inicializado.
    -   Um exemplo de consideração de experiência do usuário é que, se você tiver um aplicativo, como um jogo, que exiba uma sequência introdutória quando é iniciado, você poderá atrasar a sequência introdutória até o usuário alternar para o aplicativo.
    -   Um exemplo de implicação de desempenho é que você poderá aguardar até o usuário alternar para o aplicativo a fim de recuperar as informações de clima atuais, em vez de carregá-las quando o aplicativo é pré-inicializado e, depois, recarregá-las quando o aplicativo ficar visível, para garantir que as informações estejam atualizadas.
-   Caso o aplicativo limpe o Bloco Dinâmico quando iniciado, adie isso até o evento de visibilidade alterada.
-   A telemetria do aplicativo deve distinguir ativações de bloco normais e ativações de pré-lançamento de maneira que seja possível identificar o cenário em que problemas ocorrem.
-   Se você tiver o Microsoft Visual Studio 2015 Atualização 1 e o Windows 10, versão 1511, poderá simular a pré-inicialização do seu aplicativo no Visual Studio 2015 escolhendo **Depurar**&gt;**Outros Destinos de Depuração**&gt;**Debug Windows Universal App PreLaunch**.

## Tópicos relacionados

* [Ciclo de vida do aplicativo](app-lifecycle.md)

 

 





<!--HONumber=May16_HO2-->


