---
author: martinekuan
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: Criar um aplicativo Hello, world (XAML)
description: "Este tutorial ensina a usar XAML (Extensible Application Markup Language) com C# para criar um aplicativo Hello, world simples destinado à UWP (Plataforma Universal do Windows) no Windows 10."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 0a524d51f713c37ce2069b4e750bf3ed20fe19ab

---

# Criar um aplicativo "Hello, world" (XAML)

Este tutorial ensina a usar XAML (Extensible Application Markup Language) com C# para criar um aplicativo "Hello, world" simples destinado à UWP (Plataforma Universal do Windows) no Windows 10. Com um único projeto no Microsoft Visual Studio, você pode compilar um aplicativo que seja executado em qualquer dispositivo do Windows 10. Aqui, nosso foco é criar um aplicativo que seja executado igualmente bem em dispositivos móveis e desktops.


              **Importante**   Este tutorial deve ser usado com o Microsoft Visual Studio 2015 e o Windows 10. Ele não funcionará corretamente com versões anteriores.

Aqui, você aprenderá a:

-   Criar um novo projeto do Visual Studio direcionado ao Windows 10 e à UWP.
-   Adicionar conteúdo XAML à sua página inicial.
-   Manipular a entrada por toque, à caneta e de mouse.
-   Executar o projeto na área de trabalho local e no emulador do telefone no Visual Studio.
-   Adaptar a interface do usuário a diferentes tamanhos de tela.

## Antes de começar...


-   Vamos passar diretamente para as etapas usadas para a criação de um aplicativo universal simples. É altamente recomendável que você leia e entenda as informações da visão geral em [Novidades no Windows 10](https://dev.windows.com/whats-new-windows-10-dev-preview) e [O que é um Aplicativo Universal do Windows](whats-a-uwp.md) antes de começar este tutorial.
-   Para concluir este tutorial, você precisa do Windows 10 e do Visual Studio 2015. Consulte [Prepare-se para começar](get-set-up.md) para saber mais.
-   Pressupomos que você tenha uma compreensão básica de XAML e dos conceitos na [Visão geral de XAML](https://msdn.microsoft.com/library/windows/apps/Mt185595).
-   Também pressupomos que você esteja usando o layout de janela padrão no Visual Studio. Se você alterar o layout padrão, poderá redefini-lo no menu **Janela** usando o comando **Redefinir Layout da Janela**.

##  Etapa 1: crie um novo projeto no Visual Studio.


1.  Inicie o Visual Studio 2015.

   A página inicial do Visual Studio 2015 é exibida. (De agora em diante, chamaremos o Visual Studio 2015 simplesmente de Visual Studio.)

2.  No menu **Arquivo**, selecione **Novo** > **Projeto**.

   A caixa de diálogo **Novo Projeto** será exibida. O painel esquerdo da caixa de diálogo permite que você selecione o tipo de modelos a exibir.

3.  No painel esquerdo, expanda **Instalado > Modelos > Visual C# > Windows** e escolha o grupo de modelos **Universal**. O painel central da caixa de diálogo exibe uma lista de modelos de projetos para aplicativos UWP (Plataforma Universal do Windows).

   ![A janela Novo Projeto ](images/newproject-cs.png)
   
   (Se você não vir essas opções, verifique se tem as ferramentas de desenvolvimento de aplicativo Universal do Windows instaladas. Consulte [Prepare-se para começar](get-set-up.md) para saber mais.)

4.  No painel central, selecione o modelo **Aplicativo em Branco (Universal do Windows)**.

   O modelo **Aplicativo em Branco** cria um aplicativo UWP básico que é compilado e executado, mas não contém controles de interface do usuário ou dados. Você adicionará controles ao aplicativo ao longo deste tutorial.

5.  Na caixa de texto **Nome**, digite "HelloWorld".
6.  Clique em **OK** para criar o projeto.

   O Visual Studio criará seu projeto e o exibirá no **Gerenciador de Soluções**.

   ![Visual Studio Solution Explorer para o projeto HelloWorld](images/solutionexplorer-cs.png)

Apesar de ser um modelo básico, **Aplicativo em Branco** contém vários arquivos:

-   Um arquivo de manifesto (Package.appxmanifest) que descreve o aplicativo (nome, descrição, bloco, página inicial etc.) e lista os arquivos que ele contém.
-   Um conjunto de imagens de logotipo (Assets/Square150x150Logo.scale-200.png, Assets/Square44x44Logo.scale-200.png e Assets/Wide310x150Logo.scale-200.png) para exibir no menu Iniciar.
-   Uma imagem (Assets/StoreLogo.png) para representar o aplicativo na Windows Store.
-   Uma tela inicial (Assets/SplashScreen.scale-200.png) para ser mostrada quando o aplicativo é iniciado.
-   Arquivos de código e XAML para o aplicativo (App.xaml e App.xaml.cs).
-   Uma página inicial (MainPage.xaml) e um arquivo de código correspondente (MainPage.xaml.cs) que são executados quando o aplicativo é iniciado.

Esses arquivos são essenciais para todos os aplicativos UWP em C#. Eles fazem parte de todos os projetos criados no Visual Studio.

## Etapa 2: modifique a página inicial


### O que os arquivos incluem?

Para exibir e editar um arquivo no projeto, clique duas vezes no arquivo no **Gerenciador de Soluções**. Por padrão, você pode expandir um arquivo XAML como se fosse uma pasta para ver o arquivo de código associado. Os arquivos XAML são abertos em um modo divisão que mostra a área de design e o editor de XAML.

Neste tutorial, você trabalhará com apenas alguns dos arquivos mencionados anteriormente: App.xaml, MainPage.xaml e MainPage.xaml.cs.

### App.xaml e App.xaml.cs

App.xaml é onde você declara os recursos que serão usados em todo o aplicativo. App.xaml.cs é o arquivo code-behind de App.xaml. Code-behind é o código unido à classe parcial da página XAML. Juntos, o XAML e o code-behind compõem uma classe completa. App.xaml.cs é o ponto de entrada do aplicativo. Como todas as páginas code-behind, ele contém um construtor que chama o método `InitializeComponent`. Não é você quem cria o método `InitializeComponent`. Ele é gerado pelo Visual Studio e sua principal finalidade é inicializar os elementos declarados no arquivo XAML. App.xaml.cs também contém métodos para manipular a ativação e a suspensão do aplicativo.

### MainPage.xaml

Em MainPage.xaml, você definirá a interface do usuário do aplicativo. É possível adicionar elementos usando diretamente a marcação XAML ou usar as ferramentas de design fornecidas pelo Visual Studio. MainPage.xaml.cs é a página code-behind de MainPage.xaml. É onde você adiciona a lógica e os manipuladores de eventos do aplicativo.

Juntos, esses dois arquivos definem uma nova classe chamada `MainPage`, que herda de [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503), no namespace `HelloWorld`.

MainPage.xaml

```xml
    <Page
    x:Class="HelloWorld.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorld"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    </Grid>
</Page>
```

MainPage.xaml.cs

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace HelloWorld
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }
    }
}
```

### Modificar a página inicial

Agora, vamos adicionar algum conteúdo para o aplicativo.

**Para modificar a página inicial**

1.  Clique duas vezes em MainPage.xaml no **Gerenciador de Soluções** para abri-lo.
2.  No editor XAML, adicione os controles da interface do usuário.

   Na [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) raiz, adicione esse XAML. Ele contém um [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635) com um título [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652), um **TextBlock** que solicita o nome do usuário, um elemento [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) para aceitar o nome do usuário, um [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) e outro **TextBlock** para mostrar uma saudação. Alguns desses controles têm nomes para que você possa se referir a eles posteriormente em seu código.

```xml    
    <StackPanel x:Name="contentPanel" Margin="8,32,0,0">
        <TextBlock Text="Hello, world!" Margin="0,0,0,40"/>
        <TextBlock Text="What' s your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="280" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
```    

    The controls that you added in the XAML editor show up in the design view.

## Etapa 3: inicie o aplicativo


Neste ponto, você criou um aplicativo muito simples. Este é um bom momento para compilar, implantar e iniciar seu aplicativo e verificar sua aparência. Você pode depurar o aplicativo no computador local, em um simulador ou emulador, ou em um dispositivo remoto. Aqui está o menu do dispositivo de destino no Visual Studio.

![Lista suspensa de destinos de dispositivo para depuração do aplicativo](images/uap-debug.png)

### Inicie o aplicativo em um dispositivo da área de trabalho

Por padrão, o aplicativo é executado no computador local. O menu do dispositivo de destino fornece várias opções para depurar seu aplicativo em dispositivos da família de dispositivos da área de trabalho.

-   **Simulador**
-   **Computador local**
-   **Computador remoto**

**Para iniciar a depuração no computador local.**

1.  No menu do dispositivo de destino (![Start debugging menu](images/startdebug-full.png)) na barra de ferramentas **Padrão**, verifique se **Computador Local** está selecionado. (Esta é a seleção padrão.)
2.  Clique no botão **Iniciar depuração** (![botão Iniciar depuração](images/startdebug-sm.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Iniciar Depuração**.

   –ou–

   Pressione F5.

O aplicativo é aberto em uma janela, e uma tela inicial padrão aparece primeiro. A tela inicial é definida por uma imagem (SplashScreen.png) e uma cor da tela de fundo (especificada no arquivo de manifesto do aplicativo).

A tela inicial desaparecerá, e o aplicativo será exibido em seguida. Ele terá a aparência a seguir.

![Tela inicial do aplicativo](images/helloworld-1-cs.png)

Pressione a tecla Windows para abrir o menu **Iniciar** e exibir todos os aplicativos. Observe que implantar o aplicativo localmente adiciona seu bloco ao menu **Iniciar**. Para executar o aplicativo novamente (não no modo de depuração), toque ou clique no bloco no menu **Iniciar**.

Ele ainda não faz muita coisa, mas parabéns! Você criou seu primeiro aplicativo UWP!

**Para parar a depuração**

-   Clique no botão **Parar Depuração** (![botão Parar depuração](images/stopdebug.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Parar depuração**.

   –ou–

   Feche a janela do aplicativo.

### Iniciar o aplicativo em um emulador de dispositivo móvel

Seu aplicativo é executado em qualquer dispositivo do Windows 10, portanto vamos ver sua aparência em um Windows Phone.

Além das opções para depurar em um dispositivo da área de trabalho, o Visual Studio fornece opções para implantar e depurar seu aplicativo em um dispositivo móvel físico conectado ao computador, ou em um emulador de dispositivo móvel. Você pode escolher entre emuladores para dispositivos com diferentes configurações de memória e exibição.

-   **Dispositivo**
-   **Emulador <SDK version> WVGA de 4 polegadas e 512 MB**
-   **Emulador <SDK version> WVGA de 4 polegadas e 1 GB**
-   etc. (Diversos emuladores em outras configurações)

(Se você não vir os emuladores, verifique se tem as ferramentas de desenvolvimento de aplicativo Universal do Windows instaladas. Consulte [Prepare-se para começar](get-set-up.md) para saber mais.)

É recomendável testar o aplicativo em um dispositivo com tela pequena e memória limitada, portanto use a opção **Emulator 10.0.10240.0 WVGA 4 inch 512MB**.
**Para iniciar a depuração em um emulador de dispositivo móvel**

1.  No menu do dispositivo de destino (![Menu Iniciar depuração](images/startdebug-full.png)) na barra de ferramentas **Padrão**, escolha **Emulador 10.0.0.0 WVGA de 4 polegadas e 512 MB**.
2.  Clique no botão **Iniciar depuração** (![botão Iniciar depuração](images/startdebug-sm.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Iniciar Depuração**.

   –ou–

   Pressione F5.

O Visual Studio inicia o emulador selecionado e, em seguida, implanta e inicia o aplicativo. No emulador do dispositivo móvel, o aplicativo tem a seguinte aparência.

![Tela inicial do aplicativo no dispositivo móvel](images/helloworld-1-cs-phone.png)

A primeira coisa que você observará é que o botão é pressionado fora da tela menor de um dispositivo móvel. Mais para frente neste tutorial, você aprenderá a adaptar a interface do usuário aos diferentes tamanhos de tela para que seu aplicativo sempre tenha uma boa aparência.

Você também perceberá que é possível digitar na [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683), mas, no momento, clicar ou tocar no [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) não faz nada. Nas próximas etapas, você criará um manipulador de eventos para o evento [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) do botão para exibir uma saudação personalizada. Você adicionará o código do manipulador de eventos ao arquivo MainPage.xaml.cs.

## Etapa 4: crie um manipulador de eventos


Elementos XAML podem enviar mensagens quando determinados eventos ocorrem. Essas mensagens de evento permitem executar uma ação em resposta ao evento. Você coloca seu código para responder ao evento em um método do manipulador de eventos. Um dos eventos mais comuns em vários aplicativos é o usuário clicar em um [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265).

Agora, criaremos um manipulador de eventos para o evento [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) do botão. O manipulador de eventos obterá o nome do usuário por meio do controle `nameInput` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) e o usará para gerar uma saudação no `greetingOutput` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652).

### Usando eventos que funcionam com a entrada de toque, mouse e caneta

Quais eventos devem ser manipulados? Como seu aplicativo da Windows Store poderá ser executado em diversos dispositivos, projete-o tendo em mente a entrada por toque. O aplicativo também deverá manipular a entrada de um mouse ou uma caneta. Felizmente, eventos como [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) e [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/BR208922) são independentes de dispositivo. Se você tiver conhecimento de programação no Microsoft .NET, talvez já tenha visto eventos separados para a entrada de mouse, toque e caneta, como **MouseMove**, **TouchMove** e **StylusMove**. Nos aplicativos da Windows Store, esses eventos separados são substituídos por um evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/BR208970) único que funciona igualmente para a entrada de toque, mouse e caneta.

**Para adicionar um manipulador de eventos**

1.  Na exibição de XAML ou de design, selecione o [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) "Say Hello" que você adicionou a MainPage.xaml.
2.  Na **janela Propriedades**, clique no botão Eventos (![botão Eventos](images/eventsbutton.png)).
3.  Localize o evento [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) na parte superior da lista de eventos. Na caixa de texto do evento, digite o nome da função que manipula o evento **Click**. Para este exemplo, digite "Button\_Click".

   ![Lista de eventos na janela Propriedades](images/xaml-hw-event.png)

4.  Pressione Enter. O método do manipulador de eventos será criado e aberto no editor de códigos para que você possa adicionar o código que será executado quando o evento ocorrer.

    No editor XAML, o XAML do [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) é atualizado para declarar o manipulador de eventos [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737), deste modo.

```xml   
   <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
```    

5.  Adicione código ao manipulador de eventos criado na página code-behind. No manipulador de eventos, recupere o nome do usuário por meio do controle `nameInput` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) e use-o para criar uma saudação. Use o `greetingOutput` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) para exibir o resultado.
    
```csharp    
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        greetingOutput.Text = "Hello, " + nameInput.Text + "!";
    }
```    

6.  Depure o aplicativo no computador local. Quando você inserir seu nome na caixa de texto e clicar no botão, o aplicativo exibirá uma saudação personalizada.

## Etapa 5: adapte a interface do usuário a diferentes tamanhos de janela


Agora faremos a interface do usuário se adaptar a diferentes tamanhos de tela para que ela tenha uma boa aparência em dispositivos móveis. Para isso, adicione um [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) e defina as propriedades aplicadas a diferentes estados visuais.

**Para ajustar o layout da interface do usuário**

1.  No editor XAML, adicione este bloco de XAML após a marca de abertura do elemento [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) raiz.

```xml    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
```    

2.  Depure o aplicativo no computador local. Observe que a interface do usuário terá a mesma aparência de antes, a menos que a janela seja mais estreita do que 641 pixels.
3.  Depure o aplicativo no emulador do dispositivo móvel. Observe que a interface do usuário usa as propriedades definidas no `narrowState` e é exibida corretamente na tela pequena.

![Tela do aplicativo móvel](images/helloworld-2-cs-phone.png)

Se você usou um [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) nas versões anteriores do XAML, poderá notar que o XAML aqui usa uma sintaxe simplificada.

O [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) denominado `wideState` tem um [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382) com a propriedade [**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) definida como 641. Isso significa que o estado deverá ser aplicado somente quando a largura da janela não for menor que o mínimo de 641 pixels. Se você não definir objetos [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) para esse estado, ele usará as propriedades de layout definidas no XAML para o conteúdo da página.

O segundo [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007), `narrowState`, tem um [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382) com a propriedade [**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) definida como 0. Esse estado é aplicado quando a largura da janela for maior que 0, mas menor que 641 pixels. (Em 641 pixels, o `wideState` é aplicado.) Nesse estado, você define alguns objetos [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) para alterar as propriedades de layout dos controles da interface do usuário:

-   Você altera a [**Orientação**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.orientation) do elemento `inputPanel` de **Horizontal** para **Vertical**.
-   Adicione uma margem superior de 4 ao elemento `inputButton`.

## Resumo


Parabéns, você criou seu primeiro aplicativo para o Windows 10 e a UWP!



<!--HONumber=Jul16_HO2-->


