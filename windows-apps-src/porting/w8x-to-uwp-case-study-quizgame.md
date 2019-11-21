---
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: This topic presents a case study of porting a functioning peer-to-peer quiz game WinRT 8.1 sample app to a Windows 10 Universal Windows Platform (UWP) app.
title: Estudo de caso do Windows Runtime 8.x para UWP, exemplo de aplicativo QuizGame ponto a ponto
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b03e13352717c5e414dda60fe413b00edc9ba827
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260116"
---
# <a name="windows-runtime-8x-to-uwp-case-study-quizgame-sample-app"></a>Estudo de caso do Windows Runtime 8.x para UWP, exemplo de aplicativo QuizGame




This topic presents a case study of porting a functioning peer-to-peer quiz game WinRT 8.1 sample app to a Windows 10 Universal Windows Platform (UWP) app.

A Universal 8.1 app is one that builds two versions of the same app: one app package for Windows 8.1, and a different app package for Windows Phone 8.1. A versão WinRT 8.1 do QuizGame usa a disposição de projeto de um Aplicativo Universal do Windows, mas adota uma abordagem diferente e compila um aplicativo funcionalmente distinto para as duas plataformas. The Windows 8.1 app package serves as the host for a quiz game session, while the Windows Phone 8.1 app package plays the role of the client to the host. As duas metades da sessão do jogo de teste se comunicam por meio da rede ponto a ponto.

Faz sentido personalizar as duas metades ao PC e ao telefone, respectivamente. Mas não seria ainda melhor se você pudesse executar o host e o cliente em praticamente qualquer dispositivo de sua escolha? In this case study, we'll port both apps to Windows 10 where they will each build into a single app package that users can install onto a wide range of devices.

O aplicativo utiliza padrões que usam modos e modelos de exibição. Como resultado dessa separação clara, o processo de portabilidade para esse aplicativo é muito simples, como você verá.

**Note**  This sample assumes your network is configured to send and receive custom UDP group multicast packets (most home networks are, although your work network may not be). O exemplo também envia e recebe pacotes TCP.

 

**Note**   When opening QuizGame10 in Visual Studio, if you see the message "Visual Studio update required", then follow the steps in [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

 

## <a name="downloads"></a>Downloads

[Baixe o aplicativo Universal 8.1 QuizGame](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame). Esse é o estado inicial do aplicativo antes da portabilidade. 

[Download the QuizGame10 Windows 10 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame10). Esse é o estado do aplicativo depois da portabilidade. 

[Veja a versão mais recente deste exemplo no GitHub](https://github.com/microsoft/Windows-appsample-networkhelper).

## <a name="the-winrt-81-solution"></a>A solução do WinRT 8.1


Esta é a aparência do QuizGame, o aplicativo que vamos portar.

![o aplicativo host quizgame em execução no windows](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

O aplicativo host QuizGame em execução no Windows

 

![o aplicativo cliente quizgame em execução no windows phone](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

O aplicativo cliente QuizGame em execução no Windows Phone

## <a name="a-walkthrough-of-quizgame-in-use"></a>Um passo a passo do QuizGame em uso

Esta é uma breve descrição hipotética do aplicativo em uso, mas que fornece informações úteis caso você queira experimentar o aplicativo em sua rede sem fio.

Um divertido jogo de teste está ocorrendo em um bar. Lá, já uma grande TV que todos possam ver. O apresentador tem um computador cuja saída está sendo mostrada na TV. Esse computador está executando "o aplicativo host". Qualquer pessoa que queira participar do teste precisa apenas instalar "o aplicativo cliente" no telefone ou no Surface.

O aplicativo host está no modo de lobby, e na TV grande, anunciando que está pronto para a conexão de aplicativos cliente. Joan inicia o aplicativo cliente em seu dispositivo móvel. Ela digita o nome na caixa de texto **Nome do jogador** e toca em **Entrar no jogo**. O aplicativo host reconhece que Joan ingressou, exibindo seu nome, e o aplicativo de cliente de Joan indica que está aguardando o início do jogo. Em seguida, Maxwell passa pelas mesmas etapas em seu dispositivo móvel.

O apresentador clica em **Iniciar jogo** e o aplicativo host mostra uma pergunta e as possíveis respostas (além de uma lista dos jogadores participantes em fonte com espessura normal, cinza). Ao mesmo tempo, as respostas aparecem nos botões dos dispositivos cliente participantes. Joan toca no botão com a resposta "1975" e então todos os botões dela ficam desabilitados. No aplicativo host, o nome de Joan fica verde (e aparece em negrito), indicando o recebimento de sua resposta. Maxwell também responde. O apresentador, ao notar que os nomes de todos os jogadores estão verdes, clica em **Próxima pergunta**.

As perguntas continuam sendo feitas e respondidas nesse mesmo ciclo. Quando a última pergunta é mostrada no aplicativo host, o botão apresenta **Mostrar resultados** e não **Próxima pergunta**. Ao clicar em **Mostrar resultados**, os resultados são mostrados. Ao clicar em **Retornar ao lobby**, o jogo volta ao início do ciclo de vida, exceto pelos jogadores participantes que continuam conectados. Mas, voltar ao lobby dá a novos jogadores uma chance de entrar, e esse também é um momento prático para os participantes saírem (embora um jogador possa sair a qualquer momento, tocando em **Sair do jogo**).

## <a name="local-test-mode"></a>Modo de teste local

Para experimentar o aplicativo e as interações em um único computador, e não distribuído em vários dispositivos, você pode compilar o aplicativo host no modo de teste local. Esse modo ignora totalmente o uso da rede. Em vez disso, a interface do usuário do aplicativo host exibe a parte do host à esquerda da janela e, à direita, duas cópias da interface do usuário do aplicativo cliente, empilhadas verticalmente (observe que, nessa versão, a interface do usuário do modo teste local é fixa para um vídeo do computador; ela não se adapta a dispositivos pequenos). Esses segmentos da interface do usuário, todos no mesmo aplicativo, comunicam-se entre si por meio de um comunicador cliente fictício, que simula as interações que, de outra forma, ocorreriam na rede.

Para ativar o modo de teste local, defina **LOCALTESTMODEON** (nas propriedades do projeto) como um símbolo de compilação condicional e recompile.

## <a name="porting-to-a-windows10-project"></a>Porting to a Windows 10 project

O QuizGame inclui as seguintes partes.

-   P2PHelper. Esta é uma biblioteca de classes portátil que contém a lógica de rede ponto a ponto.
-   QuizGame.Windows. This is the project that builds the app package for the host app, which targets Windows 8.1.
-   QuizGame.WindowsPhone. Este é o projeto que compila o pacote do aplicativo para o aplicativo cliente, que visa o Windows Phone 8.1.
-   QuizGame.Shared. Este é o projeto que contém o código-fonte, os arquivos de marcação e outros ativos e recursos usados pelos outros dois projetos.

Para este estudo de caso, temos as opções usuais descritas em [Se você tiver um aplicativo Universal 8.1](w8x-to-uwp-root.md) em relação a quais dispositivos dar suporte.

Based on those options, we'll port QuizGame.Windows to a new Windows 10 project called QuizGameHost. And, we'll port QuizGame.WindowsPhone to a new Windows 10 project called QuizGameClient. Esses projetos serão segmentados para a família de dispositivos universais; logo, eles serão executados em qualquer dispositivo. Manteremos os arquivos de origem de QuizGame.Shared etc. na própria pasta, e vincularemos esses arquivos compartilhados aos dois projetos novos. Como antes, vamos manter tudo em uma única solução e chamá-la de QuizGame10.

**The QuizGame10 solution**

-   Create a new solution (**New Project** &gt; **Other Project Types** &gt; **Visual Studio Solutions**) and name it QuizGame10.

**P2PHelper**

-   In the solution, create a new Windows 10 class library project (**New Project** &gt; **Windows Universal** &gt; **Class Library (Windows Universal)** ) and name it P2PHelper.
-   Exclua Class1.cs do novo projeto.
-   Copie P2PSession.cs, P2PSessionClient.cs e P2PSessionHost.cs para a pasta do novo projeto e inclua os arquivos copiados no novo projeto.
-   O projeto será compilado sem precisar de outras alterações.

**Shared files**

-   Copy the folders Common, Model, View, and ViewModel from \\QuizGame.Shared\\ to \\QuizGame10\\.
-   Quando nos referirmos às pastas compartilhadas no disco, estaremos falando de Common, Model, View e ViewModel.

**QuizGameHost**

-   Create a new Windows 10 app project (**Add** &gt; **New Project** &gt; **Windows Universal** &gt; **Blank Application (Windows Universal)** ) and name it QuizGameHost.
-   Add a reference to P2PHelper (**Add Reference** &gt; **Projects** &gt; **Solution** &gt; **P2PHelper**).
-   No **Gerenciador de Soluções**, crie uma nova pasta para cada uma das pastas compartilhadas no disco. In turn, right-click each folder you just created and click **Add** &gt; **Existing Item** and navigate up a folder. Abra a pasta compartilhada apropriada, selecione todos os arquivos e clique em **Adicionar como Link**.
-   Copy MainPage.xaml from \\QuizGame.Windows\\ to \\QuizGameHost\\ and change the namespace to QuizGameHost.
-   Copy App.xaml from \\QuizGame.Shared\\ to \\QuizGameHost\\ and change the namespace to QuizGameHost.
-   Em vez de substituir app.xaml.cs, vamos manter a versão no novo projeto e fazer apenas uma alteração direcionada a ela para permitir o modo de teste local. Em app.xaml.cs, substitua esta linha de código:

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

por:

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   In **Properties** &gt; **Build** &gt; **conditional compilation symbols**, add LOCALTESTMODEON.
-   Agora você poderá voltar ao código adicionado a app.xaml.cs e resolver o tipo de TestView.
-   Em package.appxmanifest, altere o nome do recurso de internetClient para internetClientServer.

**QuizGameClient**

-   Create a new Windows 10 app project (**Add** &gt; **New Project** &gt; **Windows Universal** &gt; **Blank Application (Windows Universal)** ) and name it QuizGameClient.
-   Add a reference to P2PHelper (**Add Reference** &gt; **Projects** &gt; **Solution** &gt; **P2PHelper**).
-   No **Gerenciador de Soluções**, crie uma nova pasta para cada uma das pastas compartilhadas no disco. In turn, right-click each folder you just created and click **Add** &gt; **Existing Item** and navigate up a folder. Abra a pasta compartilhada apropriada, selecione todos os arquivos e clique em **Adicionar como Link**.
-   Copy MainPage.xaml from \\QuizGame.WindowsPhone\\ to \\QuizGameClient\\ and change the namespace to QuizGameClient.
-   Copy App.xaml from \\QuizGame.Shared\\ to \\QuizGameClient\\ and change the namespace to QuizGameClient.
-   Em package.appxmanifest, altere o nome do recurso de internetClient para internetClientServer.

Agora você poderá compilar e executar.

## <a name="adaptive-ui"></a>Interface do usuário adaptável

The QuizGameHost Windows 10 app looks fine when the app is running in a wide window (which is only possible on a device with a large screen). Porém, quando a janela do aplicativo é estreita (o que acontece em dispositivos pequenos e também pode acontecer em dispositivos grandes), a interface do usuário é tão espremida que fica ilegível.

Podemos usar o recurso adaptável do Gerenciador de Estado Visual para corrigir isso, conforme explicado em [Estudo de caso: Bookstore2](w8x-to-uwp-case-study-bookstore2.md). Primeiro, defina as propriedades nos elementos visuais de forma que, por padrão, a interface do usuário seja disposta em estado estreito. All of these changes take place in \\View\\HostView.xaml.

-   Na **Grid** principal, altere **Height** da primeira **RowDefinition** de "140" para "Auto".
-   Na **Grid** que contém o **TextBlock** chamado `pageTitle`, defina `x:Name="pageTitleGrid"` e `Height="60"`. Essas duas primeiras etapas possibilitam o controle efetivo da altura dessa **RowDefinition** por meio de um setter em um estado visual.
-   Em `pageTitle`, defina `Margin="-30,0,0,0"`.
-   Na **Grid** indicada pelo comentário `<!-- Content -->`, defina `x:Name="contentGrid"` e `Margin="-18,12,0,0"`.
-   No **TextBlock** imediatamente acima do comentário `<!-- Options -->`, defina `Margin="0,0,0,24"`.
-   No estilo de **TextBlock** padrão (o primeiro recurso no arquivo), altere o valor do setter **FontSize** para "15".
-   Em `OptionContentControlStyle`, altere o valor do setter **FontSize** para "20". Essa etapa e a anterior fornecerão uma boa rampa de tipos, que funcionará bem em todos os dispositivos. These are much more flexible sizes than the "30" we were using for the Windows 8.1 app.
-   Por fim, adicione a marcação apropriada do Gerenciador de Estado Visual à raiz **Grid**.

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## <a name="universal-styling"></a>Estilo universal


You'll notice that in Windows 10, the buttons don't have the same touch-target padding in their template. Duas pequenas alterações corrigirão isso. Primeiro, adicione esta marcação a app.xaml em QuizGameHost e QuizGameClient.

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

And second, add this setter to `OptionButtonStyle` in \\View\\ClientView.xaml.

```xml
<Setter Property="Margin" Value="6"/>
```

Com esse último ajuste, o aplicativo se comportará e parecerá como antes da portabilidade, mas agora será executado em todos os lugares.

## <a name="conclusion"></a>Conclusão

O aplicativo que portamos este estudo de caso era relativamente complexo, envolvendo vários projetos, uma biblioteca de classes e um volume relativamente grande de código e interface do usuário. Mesmo assim, a portabilidade foi simples. Some of the ease of porting is directly attributable to the similarity between the Windows 10 developer platform and the Windows 8.1 and Windows Phone 8.1 platforms. Além disso, também à forma como o aplicativo original foi projetado para manter os modelos, os modelos de exibição e os modos de exibição separados.
