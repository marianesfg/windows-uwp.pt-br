---
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: Saiba como criar um aplicativo "Olá, mundo" (XAML)
description: Use XAML (Extensible Application Markup Language) com C# para criar um aplicativo Olá, Mundo simples destinado à UWP (Plataforma Universal do Windows) no Windows 10.
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, uwp, primeiro aplicativo, olá mundo
ms.localizationpriority: medium
ms.openlocfilehash: 93c78845a218620a8a46fc4439733734099b9853
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "73847609"
---
# <a name="create-a-hello-world-app-xaml"></a>Criar um aplicativo "Hello, world" (XAML)

Este tutorial ensina a usar XAML e C# para criar um aplicativo "Olá, Mundo" simples para a UWP (Plataforma Universal do Windows) no Windows 10. Com um único projeto no Microsoft Visual Studio, você pode compilar um aplicativo que pode ser executado em qualquer dispositivo com Windows 10.

Aqui, você aprenderá a:

-   Criar um novo projeto do **Visual Studio** direcionado ao **Windows 10** e à **UWP**.
-   Escreva o XAML para alterar a interface do usuário em sua página inicial.
-   Execute o projeto na área de trabalho local no Visual Studio.
-   Use um SpeechSynthesizer para fazer o aplicativo falar quando você pressiona um botão.


## <a name="before-you-start"></a>Antes de começar...

-   [O que é um aplicativo universal do Windows?](universal-application-platform-guide.md)
-   [Baixar o Visual Studio 2017 (e o Windows 10)](https://developer.microsoft.com/windows/downloads). Se você precisar de ajuda, saiba como [preparar-se](get-set-up.md).
-   Também pressupomos que você esteja usando o layout de janela padrão no Visual Studio. Se você alterar o layout padrão, poderá redefini-lo no menu **Janela** usando o comando **Redefinir Layout da Janela**.

> [!NOTE]
> Este tutorial usa o Visual Studio Community 2017. Se você estiver usando uma versão diferente do Visual Studio, talvez ela seja um pouco diferente.

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Writing-Your-First-Windows-10-App/player]

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Etapa 1: Criar um novo projeto no Visual Studio.

1.  Inicie o Visual Studio.

2.  No menu **Arquivo**, selecione **Novo > Projeto** para abrir a caixa de diálogo *Novo Projeto*.

3.  Na lista de modelos no lado esquerdo, escolha **Instalado > Visual C# > Universal do Windows** para ver a lista de modelos de projeto da UWP.

    (Se você não vir modelos de Universal, talvez não tenha os componentes para a criação de aplicativos UWP. Repita o processo de instalação e adicione suporte a UWP clicando em **Abrir instalador do Visual Studio** na caixa de diálogo *Novo Projeto*. Confira [Preparar-se](get-set-up.md)).

    ![Como repetir o processo de instalação](images/win10-cs-install.png)

4.  Escolha o modelo **Aplicativo em branco (Universal Windows)** e insira "HelloWorld" como **nome**. Selecione **OK**.

    ![A janela Novo projeto](images/win10-cs-01.png)

> [!NOTE]
> Se esta for a primeira vez que usa o Visual Studio, talvez veja uma caixa de diálogo Configurações solicitando a habilitação do **Modo de desenvolvedor**. O Modo de desenvolvedor é uma configuração especial que habilita determinados recursos, como a permissão para executar aplicativos diretamente, em vez de apenas na Store. Para saber mais, leia [Habilitar seu dispositivo para desenvolvimento](enable-your-device-for-development.md). Para continuar com este guia, selecione **Modo de desenvolvedor**, clique em **Sim** e feche a caixa de diálogo.

 ![Ativar a caixa de diálogo Modo de desenvolvedor](images/win10-cs-00.png)

5.  A caixa de diálogo de versão pretendida/versão mínima é exibida. As configurações padrão são adequadas para este tutorial, então selecione **OK** para criar o projeto.

    ![Janela Gerenciador de soluções](images/win10-cs-02.png)

6.  Quando o seu novo projeto é aberto, seus arquivos são exibidos no painel do **Gerenciador de soluções** à direita. Talvez seja necessário escolher a guia **Gerenciador de soluções** em vez da guia **Propriedades** para ver seus arquivos.

    ![Janela Gerenciador de soluções](images/win10-cs-03.png)

Apesar de ser um modelo básico, **Aplicativo em Branco (Universal do Windows)** contém vários arquivos. Esses arquivos são essenciais para todos os aplicativos UWP em C#. Eles fazem parte de todos os projetos criados no Visual Studio.


### <a name="whats-in-the-files"></a>O que os arquivos incluem?

Para exibir e editar um arquivo no projeto, clique duas vezes no arquivo no **Gerenciador de Soluções**. Expanda um arquivo XAML como se fosse uma pasta para ver o arquivo de código associado. Os arquivos XAML são abertos em um modo divisão que mostra a área de design e o editor de XAML.
> [!NOTE]
> O que é XAML? Extensible Application Markup Language (XAML) é a linguagem usada para definir a interface do usuário do seu aplicativo. Ela pode ser inserida manualmente ou criada usando as ferramentas de design do Visual Studio. Um arquivo .xaml tem um arquivo .xaml.cs code-behind que contém a lógica. Juntos, o XAML e o code-behind compõem uma classe completa. Para saber mais, consulte [Visão geral de XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview).

*App.xaml e App.xaml.cs*

-   App.xaml é onde você declara os recursos que serão usados em todo o aplicativo.
-   App.xaml.cs é o arquivo code-behind de App.xaml. Como todas as páginas code-behind, ele contém um construtor que chama o método `InitializeComponent`. Não é você quem cria o método `InitializeComponent`. Ele é gerado pelo Visual Studio e sua principal finalidade é inicializar os elementos declarados no arquivo XAML.
-   App.xaml.cs é o ponto de entrada do aplicativo.
-   App.xaml.cs também contém métodos para tratar da [ativação](../launch-resume/activate-an-app.md) e da [suspensão](../launch-resume/suspend-an-app.md) do aplicativo.

*MainPage.xaml*

-   Em MainPage.xaml, você define a interface do usuário do aplicativo. É possível adicionar elementos usando diretamente a marcação XAML ou usar as ferramentas de design fornecidas pelo Visual Studio.
-   MainPage.xaml.cs é a página code-behind de MainPage.xaml. É onde você adiciona a lógica e os manipuladores de eventos do aplicativo.
-   Juntos, esses dois arquivos definem uma nova classe chamada `MainPage`, que herda de [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page), no namespace `HelloWorld`.

*Package.appxmanifest*
-   Um arquivo de manifesto que descreve seu aplicativo: nome, descrição, bloco, página de início, etc.
-   Inclui uma lista de dependências, recursos e arquivos contidos em seu aplicativo.

*Um conjunto de imagens de logotipo*
-   Assets/Square150x150Logo.scale-200.png e Wide310x150Logo.scale-200.png representam seu aplicativo (de tamanho Médio ou Largo) no menu Iniciar.
-   Assets/Square44x44Logo.png representa seu aplicativo na lista de aplicativos do menu Iniciar, na barra de tarefas e no gerenciador de tarefas.
-   Assets/StoreLogo.png representa seu aplicativo na Microsoft Store.
-   Assets/SplashScreen.scale-200.png é a tela inicial que será exibida quando o aplicativo iniciar.
-   Assets/LockScreenLogo.scale-200.png pode ser usado para representar o aplicativo na tela de bloqueio, quando o sistema estiver bloqueado.

## <a name="step-2-adding-a-button"></a>Etapa 2: Adicionar um botão

### <a name="using-the-designer-view"></a>Usando o modo de exibição de designer

Vamos adicionar um botão à nossa página. Neste tutorial, você trabalhará apenas com alguns dos arquivos mencionados anteriormente: App.xaml, MainPage.xaml e MainPage.xaml.cs.

1.  Clique duas vezes em **MainPage.xaml** para abri-lo no modo de exibição de Design.

    Você notará que há uma exibição gráfica na parte superior da tela e o modo de exibição de código XAML abaixo. Você pode fazer alterações em qualquer uma delas, mas, por enquanto, vamos usar o modo de exibição gráfico.

    ![Janela Gerenciador de soluções](images/win10-cs-04.png)

2.  Clique na guia vertical **Caixa de Ferramentas** à esquerda para abrir a lista de controles de interface do usuário. (Você pode clicar no ícone fixar na sua barra de título para manter visível).

    ![Janela Gerenciador de soluções](images/win10-cs-05.png)

3.  Expanda **Controles XAML comuns**e arraste o **Botão** para o meio da tela de design.

    ![Janela Gerenciador de soluções](images/win10-cs-06.png)

    Se você olhar para a janela de código XAML, verá que o botão foi adicionado lá também:

 ```XAML
<Button x:Name="button" Content="Button" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
 ```

4.  Altere o texto do botão.

    Clique no modo de exibição de código XAML e altere o conteúdo de "Botão" para "Hello, world!".

```XAML
<Button x:Name="button" Content="Hello, world!" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
```

Observe como o botão é exibido nas atualizações de telas de design para exibir o novo texto.

![Janela Gerenciador de soluções](images/win10-cs-07.png)

## <a name="step-3-start-the-app"></a>Etapa 3: Iniciar o aplicativo


Neste ponto, você criou um aplicativo muito simples. Este é um bom momento para compilar, implantar e iniciar seu aplicativo e verificar sua aparência. Você pode depurar o aplicativo no computador local, em um simulador ou emulador, ou em um dispositivo remoto. Aqui está o menu do dispositivo de destino no Visual Studio.

![Lista suspensa de destinos de dispositivo para depuração do aplicativo](images/uap-debug.png)

### <a name="start-the-app-on-a-desktop-device"></a>Inicie o aplicativo em um dispositivo da área de trabalho

Por padrão, o aplicativo é executado no computador local. O menu do dispositivo de destino fornece várias opções para depurar seu aplicativo em dispositivos da família de dispositivos da área de trabalho.

-   **Simulador**
-   **Computador local**
-   **Computador remoto**

**Para iniciar a depuração no computador local**

1.  No menu do dispositivo de destino (![Start debugging menu](images/startdebug-full.png)) na barra de ferramentas **Padrão**, verifique se **Computador Local** está selecionado. (Esta é a seleção padrão.)
2.  Clique no botão **Iniciar depuração** (![botão Iniciar depuração](images/startdebug-sm.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Iniciar Depuração**.

   –ou–

   Pressione F5.

O aplicativo é aberto em uma janela, e uma tela inicial padrão aparece primeiro. A tela inicial é definida por uma imagem (SplashScreen.png) e uma cor da tela de fundo (especificada no arquivo de manifesto do aplicativo).

A tela inicial desaparecerá, e o aplicativo será exibido em seguida. Ela terá a aparência a seguir.

![Tela inicial do aplicativo](images/win10-cs-08.png)

Pressione a tecla Windows para abrir o menu **Iniciar** e exibir todos os aplicativos. Observe que implantar o aplicativo localmente adiciona seu bloco ao menu **Iniciar**. Para executar o aplicativo novamente mais tarde (não no modo de depuração), toque ou clique no bloco no menu **Iniciar**.

Ele ainda não faz muita coisa, mas parabéns! Você criou seu primeiro aplicativo UWP!

**Para interromper a depuração**

   Clique no botão **Parar Depuração** (![botão Parar depuração](images/stopdebug.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Parar depuração**.

   –ou–

   Feche a janela do aplicativo.

## <a name="step-4-event-handlers"></a>Etapa 4: Manipuladores de eventos

Um "manipulador de eventos" parece complicado, mas é apenas outro nome para o código que é chamado quando ocorre um evento (por exemplo, o usuário clica no botão).

1.  Pare a execução do aplicativo, caso ainda não tenha feito isso.

2.  Clique duas vezes no controle de botão na tela de design para fazer com que o Visual Studio crie um manipulador de eventos para o seu botão.

  Você pode, obviamente, criar todo o código manualmente também. Ou você pode clicar no botão para selecioná-lo e examinar o painel **Propriedades** na parte inferior direita. Se você alternar para **Eventos** (o pequeno botão de raio), poderá adicionar o nome do seu manipulador de eventos.

3.  Edite o código do manipulador de eventos em *MainPage.xaml.cs*, a página code-behind. É aqui que as coisas ficam interessantes. O manipulador de eventos padrão tem esta aparência:

```cs
private void Button_Click(object sender, RoutedEventArgs e)
{

}
```

  Vamos alterá-la para que ela tenha esta aparência:

```cs
private async void Button_Click(object sender, RoutedEventArgs e)
{
    MediaElement mediaElement = new MediaElement();
    var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
    Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream = await synth.SynthesizeTextToStreamAsync("Hello, World!");
    mediaElement.SetSource(stream, stream.ContentType);
    mediaElement.Play();
}
```

Verifique se a assinatura do método inclui a palavra-chave **async**, ou você receberá um erro quando tentar executar o aplicativo.

### <a name="what-did-we-just-do"></a>O que acabamos de fazer?

Esse código usa algumas APIs do Windows para criar um objeto de síntese de fala e concede a ele algum texto a ser dito. (Para obter mais informações sobre como usar SpeechSynthesis, consulte os [documentos de namespace SpeechSynthesis](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis).)

Quando você executar o aplicativo e clicar no botão, seu computador (ou telefone) dirá literalmente "Hello, World!".


## <a name="summary"></a>Resumo

Parabéns, você criou seu primeiro aplicativo para Windows 10 e para a UWP!

Para saber como usar XAML para definir os controles usados por seu aplicativo, experimente o [tutorial de grade](../design/layout/grid-tutorial.md) ou vá diretamente para as [próximas Etapas](learn-more.md).

## <a name="see-also"></a>Consulte Também

* [Seu primeiro aplicativo](your-first-app.md)
* [Publicando seu aplicativo UWP](https://docs.microsoft.com/windows/uwp/publish/).
* [Artigos de instruções sobre como desenvolver aplicativos UWP](https://docs.microsoft.com/windows/uwp/develop/)
* [Amostras de código para desenvolvedores da UWP](https://developer.microsoft.com/windows/samples)
* [O que é um aplicativo Universal do Windows?](universal-application-platform-guide.md)
* [Inscreva-se em uma conta do Windows](sign-up.md)
