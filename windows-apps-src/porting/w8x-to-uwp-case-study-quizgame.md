---
author: mcleblanc
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: "Este tópico apresenta um estudo de caso de portabilidade de uma amostra de aplicativo do WinRT 8.1 de um jogo de teste ponto a ponto em funcionamento para um aplicativo da Plataforma Universal do Windows (UWP) do Windows 10."
title: Estudo de caso do Windows Runtime 8.x para UWP, amostra de aplicativo QuizGame ponto a ponto
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: cd05c3edbc254cceb00c55caba698d21998f5594

---

# Estudo de caso do Windows Runtime 8.x para UWP, amostra de aplicativo QuizGame ponto a ponto


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este tópico apresenta um estudo de caso de portabilidade de uma amostra de aplicativo do WinRT 8.1 de um jogo de teste ponto a ponto em funcionamento para um aplicativo da Plataforma Universal do Windows (UWP) do Windows 10.

Um aplicativo Universal 8.1 é aquele que cria duas versões do mesmo aplicativo: um pacote do aplicativo para Windows 8.1 e outro pacote do aplicativo para Windows Phone 8.1. A versão WinRT 8.1 do QuizGame usa a disposição de projeto de um Aplicativo Universal do Windows, mas adota uma abordagem diferente e compila um aplicativo funcionalmente distinto para as duas plataformas. O pacote do aplicativo do Windows 8.1 serve como host para uma sessão de jogo de teste, enquanto o pacote do aplicativo do Windows Phone 8.1 cumpre a função do cliente para o host. As duas metades da sessão do jogo de teste se comunicam por meio da rede ponto a ponto.

Faz sentido personalizar as duas metades ao PC e ao telefone, respectivamente. Mas não seria ainda melhor se você pudesse executar o host e o cliente em praticamente qualquer dispositivo de sua escolha? Neste estudo de caso, portaremos os dois aplicativos para o Windows 10 em que cada um será compilado em um único pacote do aplicativo que os usuários podem instalar em uma grande variedade de dispositivos.

O aplicativo utiliza padrões que usam modos e modelos de exibição. Como resultado dessa separação clara, o processo de portabilidade para esse aplicativo é muito simples, como você verá.


            **Observação**   Esta amostra pressupõe que a rede esteja configurada para enviar e receber pacotes UDP multicast em grupo personalizados (a maioria das redes domésticas está, embora sua rede de trabalho possa não estar). A amostra também envia e recebe pacotes TCP.

 


            **Observação**   Ao abrir QuizGame10 no Visual Studio, caso você veja a mensagem "Atualização do Visual Studio necessária", siga as etapas em [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md#targetplatformversion).

 

## Downloads


            [Baixe o aplicativo Universal 8.1 QuizGame](http://go.microsoft.com/fwlink/?linkid=532953). Esse é o estado inicial do aplicativo antes da portabilidade. 


            [Baixe o aplicativo QuizGame10 do Windows 10](http://go.microsoft.com/fwlink/?linkid=532954). Esse é o estado do aplicativo depois da portabilidade. 


            [Veja a versão mais recente deste exemplo no GitHub](https://github.com/Microsoft/Windows-appsample-quizgame).

## A solução do WinRT 8.1


Esta é a aparência do QuizGame, o aplicativo que vamos portar.

![o aplicativo host quizgame em execução no windows](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

O aplicativo host QuizGame em execução no Windows

 

![o aplicativo cliente quizgame em execução no windows phone](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

O aplicativo cliente QuizGame em execução no Windows Phone

## Um passo a passo do QuizGame em uso

Esta é uma breve descrição hipotética do aplicativo em uso, mas que fornece informações úteis caso você queira experimentar o aplicativo em sua rede sem fio.

Um divertido jogo de teste está ocorrendo em um bar. Lá, já uma grande TV que todos possam ver. O apresentador tem um computador cuja saída está sendo mostrada na TV. Esse computador está executando "o aplicativo host". Qualquer pessoa que queira participar do teste precisa apenas instalar "o aplicativo cliente" no telefone ou no Surface.

O aplicativo host está no modo de lobby, e na TV grande, anunciando que está pronto para a conexão de aplicativos cliente. Joan inicia o aplicativo cliente em seu dispositivo móvel. Ela digita o nome na caixa de texto **Nome do jogador** e toca em **Entrar no jogo**. O aplicativo host reconhece que Joan ingressou, exibindo seu nome, e o aplicativo de cliente de Joan indica que está aguardando o início do jogo. Em seguida, Maxwell passa pelas mesmas etapas em seu dispositivo móvel.

O apresentador clica em **Iniciar jogo** e o aplicativo host mostra uma pergunta e as possíveis respostas (além de uma lista dos jogadores participantes em fonte com espessura normal, cinza). Ao mesmo tempo, as respostas aparecem nos botões dos dispositivos cliente participantes. Joan toca no botão com a resposta "1975" e então todos os botões dela ficam desabilitados. No aplicativo host, o nome de Joan fica verde (e aparece em negrito), indicando o recebimento de sua resposta. Maxwell também responde. O apresentador, ao notar que os nomes de todos os jogadores estão verdes, clica em **Próxima pergunta**.

As perguntas continuam sendo feitas e respondidas nesse mesmo ciclo. Quando a última pergunta é mostrada no aplicativo host, o botão apresenta **Mostrar resultados** e não **Próxima pergunta**. Ao clicar em **Mostrar resultados**, os resultados são mostrados. Ao clicar em **Retornar ao lobby**, o jogo volta ao início do ciclo de vida, exceto pelos jogadores participantes que continuam conectados. Mas, voltar ao lobby dá a novos jogadores uma chance de entrar, e esse também é um momento prático para os participantes saírem (embora um jogador possa sair a qualquer momento, tocando em **Sair do jogo**).

## Modo de teste local

Para experimentar o aplicativo e as interações em um único computador, e não distribuído em vários dispositivos, você pode compilar o aplicativo host no modo de teste local. Esse modo ignora totalmente o uso da rede. Em vez disso, a interface do usuário do aplicativo host exibe a parte do host à esquerda da janela e, à direita, duas cópias da interface do usuário do aplicativo cliente, empilhadas verticalmente (observe que, nessa versão, a interface do usuário do modo teste local é fixa para um vídeo do computador; ela não se adapta a dispositivos pequenos). Esses segmentos da interface do usuário, todos no mesmo aplicativo, comunicam-se entre si por meio de um comunicador cliente fictício, que simula as interações que, de outra forma, ocorreriam na rede.

Para ativar o modo de teste local, defina **LOCALTESTMODEON** (nas propriedades do projeto) como um símbolo de compilação condicional e recompile.

## Portando para um projeto do Windows 10

O QuizGame inclui as seguintes partes.

-   P2PHelper. Esta é uma biblioteca de classes portátil que contém a lógica de rede ponto a ponto.
-   QuizGame.Windows. Este é o projeto que compila o pacote do aplicativo para o aplicativo host, que visa o Windows 8.1.
-   QuizGame.WindowsPhone. Este é o projeto que compila o pacote do aplicativo para o aplicativo cliente, que visa o Windows Phone 8.1.
-   QuizGame.Shared. Este é o projeto que contém o código-fonte, os arquivos de marcação e outros ativos e recursos usados pelos outros dois projetos.

Para este estudo de caso, temos as opções usuais descritas em [Se você tiver um aplicativo Universal 8.1](w8x-to-uwp-root.md#if-you-have-an-81-universal-windows-app) em relação a quais dispositivos dar suporte.

Com base nessas opções, portaremos QuizGame.Windows para um novo projeto do Windows 10 chamado QuizGameHost. E portaremos QuizGame.WindowsPhone para um novo projeto do Windows 10 chamado QuizGameClient. Esses projetos serão segmentados para a família de dispositivos universais; logo, eles serão executados em qualquer dispositivo. Manteremos os arquivos de origem de QuizGame.Shared etc. na própria pasta, e vincularemos esses arquivos compartilhados aos dois projetos novos. Como antes, vamos manter tudo em uma única solução e chamá-la de QuizGame10.

**A solução QuizGame10**

-   Crie uma nova solução (**Novo Projeto**&gt;**Outros Tipos de Projetos**&gt;**Soluções Visual Studio**) e chame-a de QuizGame10.

**P2PHelper**

-   Na solução, crie um novo projeto de biblioteca de classes do Windows 10 (**Novo Projeto**&gt;**Universal do Windows**&gt;**Biblioteca de Classes (Universal do Windows)**) e chame-o de P2PHelper.
-   Exclua Class1.cs do novo projeto.
-   Copie P2PSession.cs, P2PSessionClient.cs e P2PSessionHost.cs para a pasta do novo projeto e inclua os arquivos copiados no novo projeto.
-   O projeto será compilado sem precisar de outras alterações.

**Arquivos compartilhados**

-   Copie as pastas Common, Model, View e ViewModel de \\QuizGame.Shared\\ para \\QuizGame10\\.
-   Quando nos referirmos às pastas compartilhadas no disco, estaremos falando de Common, Model, View e ViewModel.

**QuizGameHost**

-   Crie um novo projeto de aplicativo do Windows 10 (**Adicionar**&gt;**Novo Projeto**&gt;**Universal do Windows**&gt;**Aplicativo em Branco (Universal do Windows)**) e chame-o de QuizGameHost.
-   Adicione uma referência ao P2PHelper (**Adicionar Referência**&gt;**Projetos**&gt;**Solução**&gt;**P2PHelper**).
-   No **Gerenciador de Soluções**, crie uma nova pasta para cada uma das pastas compartilhadas no disco. Em seguida, clique com o botão direito do mouse em cada pasta recém-criada e clique em **Adicionar**&gt;**Item Existente** e navegue até uma pasta. Abra a pasta compartilhada apropriada, selecione todos os arquivos e clique em **Adicionar como Link**.
-   Copie MainPage.xaml de \\QuizGame.Windows\\ para \\QuizGameHost\\ e altere o namespace para QuizGameHost.
-   Copie App.xaml de \\QuizGame.Shared\\ para \\QuizGameHost\\ e altere o namespace para QuizGameHost.
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

-   Em **Propriedades**&gt;**Compilar**&gt;**símbolos de compilação condicional**, adicione LOCALTESTMODEON.
-   Agora você poderá voltar ao código adicionado a app.xaml.cs e resolver o tipo de TestView.
-   Em package.appxmanifest, altere o nome do recurso de internetClient para internetClientServer.

**QuizGameClient**

-   Crie um novo projeto de aplicativo do Windows 10 (**Adicionar**&gt;**Novo Projeto**&gt;**Universal do Windows**&gt;**Aplicativo em Branco (Universal do Windows)**) e chame-o de QuizGameClient.
-   Adicione uma referência ao P2PHelper (**Adicionar Referência**&gt;**Projetos**&gt;**Solução**&gt;**P2PHelper**).
-   No **Gerenciador de Soluções**, crie uma nova pasta para cada uma das pastas compartilhadas no disco. Em seguida, clique com o botão direito do mouse em cada pasta recém-criada e clique em **Adicionar**&gt;**Item Existente** e navegue até uma pasta. Abra a pasta compartilhada apropriada, selecione todos os arquivos e clique em **Adicionar como Link**.
-   Copie MainPage.xaml de \\QuizGame.WindowsPhone\\ para \\QuizGameClient\\ e altere o namespace para QuizGameClient.
-   Copie App.xaml de \\QuizGame.Shared\\ para \\QuizGameClient\\ e altere o namespace para QuizGameClient.
-   Em package.appxmanifest, altere o nome do recurso de internetClient para internetClientServer.

Agora você poderá compilar e executar.

## Interface do usuário adaptável

O aplicativo do Windows 10 QuizGameHost tem boa aparência quando é executado em uma janela larga (o que é possível somente em dispositivos com tela grande). Porém, quando a janela do aplicativo é estreita (o que acontece em dispositivos pequenos e também pode acontecer em dispositivos grandes), a interface do usuário é tão espremida que fica ilegível.

Podemos usar o recurso adaptável do Gerenciador de Estado Visual para corrigir isso, conforme explicado em [Estudo de caso: Bookstore2](w8x-to-uwp-case-study-bookstore2.md). Primeiro, defina as propriedades nos elementos visuais de forma que, por padrão, a interface do usuário seja disposta em estado estreito. Todas essas alterações ocorrem em \\View\\HostView.xaml.

-   Na **Grid** principal, altere **Height** da primeira **RowDefinition** de "140" para "Auto".
-   Na **Grid** que contém o **TextBlock** chamado `pageTitle`, defina `x:Name="pageTitleGrid"` e `Height="60"`. Essas duas primeiras etapas possibilitam o controle efetivo da altura dessa **RowDefinition** por meio de um setter em um estado visual.
-   Em `pageTitle`, defina `Margin="-30,0,0,0"`.
-   Na **Grid** indicada pelo comentário `<!-- Content -->`, defina `x:Name="contentGrid"` e `Margin="-18,12,0,0"`.
-   No **TextBlock** imediatamente acima do comentário `<!-- Options -->`, defina `Margin="0,0,0,24"`.
-   No estilo de **TextBlock** padrão (o primeiro recurso no arquivo), altere o valor do setter **FontSize** para "15".
-   Em `OptionContentControlStyle`, altere o valor do setter **FontSize** para "20". Essa etapa e a anterior fornecerão uma boa rampa de tipos, que funcionará bem em todos os dispositivos. Eles são tamanhos muito mais flexíveis que "30", que estávamos usando para o aplicativo do Windows 8.1.
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

## Estilo universal


Você notará que, no Windows 10, os botões não têm o mesmo preenchimento da área de toque nos modelos. Duas pequenas alterações corrigirão isso. Primeiro, adicione esta marcação a app.xaml em QuizGameHost e QuizGameClient.

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

Depois, adicione este setter a `OptionButtonStyle` em \\View\\ClientView.xaml.

```xml
<Setter Property="Margin" Value="6"/>
```

Com esse último ajuste, o aplicativo se comportará e parecerá como antes da portabilidade, mas agora será executado em todos os lugares.

## Conclusão

O aplicativo que portamos este estudo de caso era relativamente complexo, envolvendo vários projetos, uma biblioteca de classes e um volume relativamente grande de código e interface do usuário. Mesmo assim, a portabilidade foi simples. Parte da facilidade de portabilidade está diretamente relacionada à semelhança entre a plataforma de desenvolvimento do Windows 10 e as plataformas do Windows 8.1 e do Windows Phone 8.1. Além disso, também à forma como o aplicativo original foi projetado para manter os modelos, os modelos de exibição e os modos de exibição separados.



<!--HONumber=Jun16_HO4-->


