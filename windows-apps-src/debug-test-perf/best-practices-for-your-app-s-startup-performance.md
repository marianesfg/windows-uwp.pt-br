---
ms.assetid: 00ECF6C7-0970-4D5F-8055-47EA49F92C12
title: Práticas recomendadas para o desempenho inicial de seu aplicativo
description: Crie apps da Plataforma Universal do Windows (UWP) com tempos de inicialização ótimos melhorando a maneira como você manipula a inicialização e a ativação.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e9d0fdee4ba9f44f15c461e5a53dad28700023a4
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8947036"
---
# <a name="best-practices-for-your-apps-startup-performance"></a>Práticas recomendadas para o desempenho inicial de seu aplicativo


Crie aplicativos da Plataforma Universal do Windows (UWP) com tempos de inicialização ótimos melhorando a maneira como você manipula a inicialização e a ativação.

## <a name="best-practices-for-your-apps-startup-performance"></a>Práticas recomendadas para o desempenho inicial de seu aplicativo

Em parte, os usuários percebem se seu aplicativo é rápido ou lento com base no tempo que leva para ele iniciar. Para os fins deste tópico, o tempo de inicialização de um aplicativo começa quando o usuário inicia o aplicativo e termina quando o usuário pode interagir com o aplicativo de alguma forma significativa. Esta seção oferece sugestões de como obter um desempenho melhor de seu aplicativo quando ele é iniciado.

### <a name="measuring-your-apps-startup-time"></a>Medição do tempo de inicialização do aplicativo

Inicie seu aplicativo algumas vezes antes de realmente medir o tempo de inicialização dele. Isso lhe dá uma linha de base para a medição e garante que você esteja medindo um tempo de inicialização mais curto possível, razoavelmente.

Quando seu aplicativo UWP chegar aos computadores de seus clientes, seu aplicativo estará compilado com a cadeia de ferramentas .NET Native. .NET Native é uma tecnologia de pré-compilação que converte MSIL em código de máquina executável nativamente. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL. Os aplicativos criados com o .NET Native vinculam estaticamente um tempo de execução personalizado e o novo .NET Core convergido que pode ser executado em todos os dispositivos, para que não dependam da implementação nativa do .NET. Por padrão, seu aplicativo usa .NET Native, se você estiver criando em modo "Versão", e usa CoreCLR, se você estiver criando em modo "depuração", na sua máquina de desenvolvimento. Você pode configurar isso no Visual Studio usando a página Compilar em "Propriedades" C#) ou Compilar -> Avançado em "Meu Projeto" (VB). Procure uma caixa de seleção que diz "Compilar com a cadeia de ferramentas nativa do .NET".

Obviamente, você deve adotar medidas que representem o que o usuário final experimentará. Portanto, se você não tiver certeza de que está compilando seu aplicativo para código nativo em sua máquina de desenvolvimento, pode executar a ferramenta Native Image Generator (Ngen.exe) para pré-compilar seu aplicativo antes de medir o tempo de inicialização.

O procedimento a seguir descreve como executar Ngen.exe para pré-compilar seu aplicativo.

**Para executar Ngen.exe**

1.  Execute seu aplicativo uma vez, no mínimo, para garantir que Ngen.exe o detecte.
2.  Abra o **Agendador de Tarefas** seguindo um destes procedimentos:
    -   Pesquise "Agendador de Tarefas" na tela inicial.
    -   Execute "taskschd.msc".
3.  No painel esquerdo do **Agendador de Tarefas**, expanda **Biblioteca do Agendador de Tarefas**.
4.  Expanda **Microsoft.**
5.  Expanda **Windows.**
6.  Selecione **.NET Framework**.
7.  Selecione **.NET Framework NGEN 4.x** na lista de tarefas.

    Se estiver usando um computador de 64 bits, também existe um **.NET Framework NGEN v4.x 64**. Se estiver compilando um aplicativo de 64 bits, selecione .**NET Framework NGEN v4.x 64**.

8.  No menu **Ação**, clique em **Executar**.

A Ngen.exe pré-compila todos os aplicativos na máquina que foram usados e não possuem imagens nativas. Se houver muitos aplicativos que precisem se pré-compilados, isso poderá levar bastante tempo, mas as execuções posteriores são muito mais rápidas.

Quando você recompila seu aplicativo, a imagem nativa não é mais usada. Em vez disso, o aplicativo é compilado no momento, o que significa que ele é compilado conforme o aplicativo é executado. Você deve executar Ngen.exe novamente para obter uma nova imagem nativa.

### <a name="defer-work-as-long-as-possible"></a>Adie o trabalho tanto quanto possível

Para melhorar o tempo de inicialização de seu aplicativo, faça apenas o trabalho que precisa ser feito para permitir que o usuário comece a interagir com o aplicativo. Isso pode ser especialmente benéfico se você possa atrasar o carregamento de assemblies adicionais. O tempo de execução de linguagem comum carrega um assembly na primeira vez que é usado. Se você puder minimizar a quantidade de assemblies carregados, talvez consiga melhorar o tempo de inicialização e o consumo de memória do aplicativo.

### <a name="do-long-running-work-independently"></a>Faça trabalhos longos de forma independente

Seu aplicativo pode ser interativo mesmo que haja partes do aplicativo que não estão totalmente funcionais. Por exemplo, se o seu aplicativo exibe dados que levam algum tempo para serem recuperados, você poderá fazer com que o código seja executado de forma independente do código de inicialização do aplicativo, recuperando os dados de forma assíncrona. Quando os dados estiverem disponíveis, preencha a interface do usuário do aplicativo com os dados.

Muitas das APIs da Plataforma Universal do Windows (UWP) que recuperam dados são assíncronas, então você provavelmente vai recuperar dados de forma assíncrona de todo o modo. Para saber mais sobre APIs assíncronas, consulte [Chamar APIs assíncronas em C# ou no Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). Se o seu trabalho não usar APIs assíncronas, poderá usar a classe Task para fazer trabalhos de execução longa, a fim de não bloquear a interação do usuário com o aplicativo. Isso manterá seu aplicativo responsivo para o usuário enquanto os dados são carregados.

Se o seu aplicativo leva um tempo especialmente longo para carregar parte de sua interface do usuário, considere a adição de uma cadeia de caracteres nesta área que diga algo como "Obtendo dados mais recentes", para que o usuário saiba que o aplicativo ainda está sendo processado.

## <a name="minimize-startup-time"></a>Minimize o tempo de inicialização

Todos os aplicativos, com exceção dos mais simples, precisam de um tempo perceptível para carregar recursos, analisar XAML, configurar estruturas de dados e executar lógica na ativação. Aqui vamos analisar o processo de ativação, dividindo-o em três fases. Nós também damos dicas para reduzir o tempo gasto em cada fase e explicamos as técnicas para tornar cada fase da inicialização de seu aplicativo mais agradável para o usuário.

O período de ativação é o tempo entre o momento em que o usuário inicia o aplicativo e o momento em que o aplicativo se torna funcional. Esse é um tempo crítico, pois é a primeira impressão do usuário sobre o seu aplicativo. Ele espera retorno instantâneo e contínuo do sistema e dos aplicativos. O sistema e o aplicativo são percebidos como quebrados ou mal projetados quando o aplicativo não inicia rapidamente. Pior ainda, se um aplicativo demorar muito para ser ativado, o Gerenciador do Tempo de Vida do Processo (PLM) poderá eliminá-lo, ou o usuário poderá desinstalá-lo.

### <a name="introduction-to-the-stages-of-startup"></a>Introdução aos estágios de inicialização

A inicialização envolve uma série de peças móveis, e todas elas precisam ser coordenadas corretamente para garantir a melhor experiência do usuário. As etapas a seguir ocorrem entre o clique no bloco de seu aplicativo executado pelo usuário e o conteúdo do aplicativo que está sendo mostrado.

-   O shell do Windows inicia o processo, e Main é chamado.
-   O objeto Application é criado.
    -   (Modelo de projeto) O construtor chama InitializeComponent, que faz com que App.xaml seja analisado e objetos sejam criados.
-   O evento Application.OnLaunched é lançado.
    -   (ProjectTemplate) O código do aplicativo cria um controle Frame e navega para MainPage.
    -   (ProjectTemplate) O construtor Mainpage chama InitializeComponent, que faz com que MainPage.xaml seja analisado e objetos sejam criados.
    -   (ProjectTemplate) Window.Current.Activate() é chamado.
-   A Plataforma XAML executa o cálculo de Layout incluindo Measure & Arrange.
    -   ApplyTemplate fará com que o conteúdo do modelo de controle seja criado para cada controle, que geralmente corresponde à maior parte do tempo de Layout para a inicialização.
-   Render é chamado para criar elementos visuais para todo o conteúdo da janela.
-   Frame é apresentado para o Gerenciador de Janelas da Área de Trabalho (DWM).

### <a name="do-less-in-your-startup-path"></a>Faça menos em seu caminho de inicialização

Mantenha o caminho do código de inicialização livre de tudo o que não é necessário para o primeiro quadro.

-   Se você tiver dlls do usuário que contêm controles que não são necessários durante o primeiro quadro, considere atrasar o carregamento deles.
-   Se houver uma parte de sua interface do usuário dependente de dados da nuvem, divida essa interface do usuário. Primeiro, abra a interface do usuário que depende de dados na nuvem e abra assincronamente a interface do usuário que depende da nuvem. Você também deve considerar armazenar os dados em cache localmente para que o aplicativo funcione offline ou não seja afetado pela conectividade de rede lenta.
-   Mostre a interface do usuário de progresso se a interface estiver aguardando dados.
-   Tenha cuidado com designs de aplicativos que envolvam muita análise de arquivos de configuração ou uma interface do usuário que seja gerada dinamicamente por código.

### <a name="reduce-element-count"></a>Reduza a quantidade de elementos

O desempenho de inicialização em um aplicativo XAML é correlacionado diretamente ao número de elementos que você cria durante a inicialização. Quanto menos elementos você criar, menos tempo seu aplicativo levará para ser inicializado. Como um parâmetro de comparação aproximado, considere que cada elemento leva 1 ms para ser criado.

-   Os modelos usados nos controles de itens podem ter o maior impacto, pois são repetidos várias vezes. Consulte [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md).
-   UserControls e modelos de controle serão expandidos, então, eles também devem ser levados em consideração.
-   Se você criar qualquer XAML que não apareça na tela, deverá justificar se essas partes do XAML devem ser criadas durante a inicialização.

A janela [Live Visual Tree no Visual Studio](http://blogs.msdn.com/b/visualstudio/archive/2015/02/24/introducing-the-ui-debugging-tools-for-xaml.aspx) mostra a quantidade de elementos filho para cada nó na árvore.

![Live Visual Tree.](images/live-visual-tree.png)

**Use o adiamento**. Recolher um elemento ou definir sua opacidade para 0 não impede que o elemento seja criado. Usando x:Load ou x:DeferLoadStrategy, você pode atrasar o carregamento de uma parte da interface do usuário e carregá-la quando necessário. Essa é uma boa maneira de atrasar o processamento da interface do usuário que não é visível durante a tela de inicialização, para que você possa carregá-la quando necessário, ou como parte de uma definição de lógica atrasada. Para disparar o carregamento, você só precisa chamar FindName para o elemento. Para obter um exemplo e mais informações, veja [Atributo x:Load](../xaml-platform/x-load-attribute.md) e [Atributo x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785).

**Virtualização**. Se houver conteúdo em lista ou de repetição em sua interface do usuário, é altamente recomendável que você use a virtualização da interface do usuário. Se a interface do usuário de lista não for virtualizada, você pagará o preço de criar todos os elementos com antecedência, e isso pode retardar a inicialização. Consulte [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md).

O desempenho do aplicativo não envolve apenas o desempenho bruto, também envolve percepção. Alterar a ordem das operações para que os aspectos visuais ocorram primeiro comumente faz com que o usuário tenha a impressão de que o aplicativo é mais rápido. Os usuários considerarão o aplicativo carregado quando o conteúdo estiver na tela. Normalmente, os aplicativos precisam fazer várias coisas como parte da inicialização, e nem todas elas são necessárias para exibir a interface do usuário, então, essas partes devem ser atrasadas ou ter menor prioridade que a interface do usuário.

Este tópico fala sobre o "primeiro quadro" que vem da animação/TV e é uma medida de quanto tempo leva para o conteúdo ser visto pelo usuário final.

### <a name="improve-startup-perception"></a>Melhore a percepção da inicialização

Vamos usar o exemplo de um jogo online simples para identificar cada fase da inicialização e diferentes técnicas para dar ao usuário retorno ao longo de todo o processo. Para esse exemplo, a primeira fase de ativação é o tempo entre o usuário tocar o bloco do jogo e o jogo começar a executar o código dele. Durante esse tempo, o sistema não tem nenhum conteúdo a exibir ao usuário para até mesmo indicar que o jogo correto foi iniciado. Mas fornecer uma tela inicial fornece esse conteúdo ao sistema. Em seguida, o jogo informa ao usuário que a primeira fase da ativação foi concluída, substituindo a tela inicial estática por sua própria interface do usuário quando ele começa a executar o código.

A segunda fase da ativação abrange a criação e inicialização de estruturas críticas para o jogo. Se o aplicativo puder criar rapidamente sua interface do usuário inicial com os dados disponíveis após a primeira fase da ativação, a segunda fase será trivial, e você poderá exibir a interface do usuário imediatamente. Caso contrário, recomendamos que o aplicativo exiba uma página de carregamento enquanto estiver sendo inicializado.

Você decide como é a aparência da página de carregamento, que pode ser tão simples quanto a exibição de uma barra ou anel de progresso. O ponto chave é que o aplicativo indica que está realizando tarefas antes de se tornar responsivo. No caso do jogo, ele deve exibir a tela inicial, mas a interface do usuário requer que algumas imagens e sons sejam carregados do disco para a memória. Essas tarefas levam alguns segundos, para que o aplicativo mantenha o usuário informado substituindo a tela inicial por uma página de carregamento, com uma animação simples relacionada ao tema do jogo.

O terceiro estágio começa depois que o jogo tem um conjunto mínimo de informações para criar uma interface do usuário interativa, que substitui a página de carregamento. Nesse ponto, as únicas informações disponíveis para o jogo online são o conteúdo que o aplicativo carregou do disco. O jogo pode vir com conteúdo suficiente para criar uma interface do usuário interativa; mas por se tratar de um jogo online, não estará funcional até que se conecte à Internet e baixe informações adicionais. Até que ele tenha todas as informações de que precisa para estar funcional, o usuário poderá interagir com a interface do usuário, mas os recursos que precisarem de dados adicionais da Web deverão dar algum tipo de retorno de que o conteúdo ainda está sendo carregado. Poderá levar algum tempo até que o aplicativo se torne plenamente funcional; então, é importante que a funcionalidade seja disponibilizada assim que possível.

Agora que identificamos os três estágios de ativação em um jogo online, vamos vinculá-los ao código real.

### <a name="phase-1"></a>Fase 1

Antes de um aplicativo iniciar, ele precisa dizer ao sistema o que ele quer exibir como tela inicial. Ele faz isso fornecendo uma imagem e a cor da tela de fundo ao elemento SplashScreen no manifesto do aplicativo, como no exemplo. O Windows exibe isso depois que o aplicativo inicia a ativação.

```xml
<Package ...>
  ...
  <Applications>
    <Application ...>
      <VisualElements ...>
        ...
        <SplashScreen Image="Images\splashscreen.png" BackgroundColor="#000000" />
        ...
      </VisualElements>
    </Application>
  </Applications>
</Package>
```

Para saber mais, veja [Adicionar uma tela inicial](https://msdn.microsoft.com/library/windows/apps/Mt187306).

Use o construtor do aplicativo apenas para inicializar estruturas de dados que sejam críticas para o aplicativo. O construtor é chamado apenas na primeira vez em que o aplicativo é executado e não necessariamente toda vez que ele é ativado. Por exemplo, o construtor não é chamado para um aplicativo que foi executado, colocado em segundo plano e depois ativado por meio do contrato de pesquisa.

### <a name="phase-2"></a>Fase 2

Há inúmeros motivos para um aplicativo ser ativado, sendo que cada um deve ser manipulado de modo diferente. Você pode substituir os métodos [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/BR242330), [**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701797), [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/BR242331), [**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701799), [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701801), [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/BR242335), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/BR242336) e [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701806) para manipular cada motivo de ativação. Uma das coisas que um aplicativo deve fazer nesses métodos é criar uma IU, atribui-la a [**Window.Content**](https://msdn.microsoft.com/library/windows/apps/BR209051) e, então, chamar [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046) Nesse ponto, a tela inicial é substituída pela IU que o aplicativo criou. Esse elemento visual poderá carregar a tela ou a interface do usuário do aplicativo se houver informação disponível suficiente na ativação para criá-la.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public partial class App : Application
> {
>     // A handler for regular activation.
>     async protected override void OnLaunched(LaunchActivatedEventArgs args)
>     {
>         base.OnLaunched(args);
> 
>         // Asynchronously restore state based on generic launch.
> 
>         // Create the ExtendedSplash screen which serves as a loading page while the
>         // reader downloads the section information.
>         ExtendedSplash eSplash = new ExtendedSplash();
> 
>         // Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash;
> 
>         // Notify the Window that the process of activation is completed
>         Window.Current.Activate();
>     }
> 
>     // a different handler for activation via the search contract
>     async protected override void OnSearchActivated(SearchActivatedEventArgs args)
>     {
>         base.OnSearchActivated(args);
> 
>         // Do an asynchronous restore based on Search activation
> 
>         // the rest of the code is the same as the OnLaunched method
>     }
> }
> 
> partial class ExtendedSplash : Page
> {
>     // This is the UIELement that's the game's home page.
>     private GameHomePage homePage;
> 
>     public ExtendedSplash()
>     {
>         InitializeComponent();
>         homePage = new GameHomePage();
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Public Class App
>     Inherits Application
> 
>     ' A handler for regular activation.
>     Protected Overrides Async Sub OnLaunched(ByVal args As LaunchActivatedEventArgs)
>         MyBase.OnLaunched(args)
> 
>         ' Asynchronously restore state based on generic launch.
> 
>         ' Create the ExtendedSplash screen which serves as a loading page while the
>         ' reader downloads the section information.
>         Dim eSplash As New ExtendedSplash()
> 
>         ' Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash
> 
>         ' Notify the Window that the process of activation is completed
>         Window.Current.Activate()
>     End Sub
> 
>     ' a different handler for activation via the search contract
>     Protected Overrides Async Sub OnSearchActivated(ByVal args As SearchActivatedEventArgs)
>         MyBase.OnSearchActivated(args)
> 
>         ' Do an asynchronous restore based on Search activation
> 
>         ' the rest of the code is the same as the OnLaunched method
>     End Sub
> End Class
> 
> Partial Friend Class ExtendedSplash
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' Downloading the data necessary for 
>         ' initial UI on a background thread.
>         Task.Run(Sub() DownloadData())
>     End Sub
> 
>     Private Sub DownloadData()
>         ' Download data to populate the initial UI.
> 
>         ' Create the first page. 
>         Dim firstPage As New MainPage()
> 
>         ' Add the data just downloaded to the first page
> 
>         ' Replace the loading page, which is currently 
>         ' set as the window's content, with the initial UI for the app
>         Window.Current.Content = firstPage
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class 
> ```

Os aplicativos que exibem uma página de carregamento no manipulador de carregamento começam a trabalhar para criar a IU em segundo plano. Após esse elemento ter sido criado, seu evento [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723) ocorre. No manipulador de evento, você substitui o conteúdo da janela, que, no momento, é a tela de carregamento, com a home page recém-criada.

É crucial que um aplicativo com um período de inicialização prolongado mostre uma página de carregamento. Além de fornecer ao usuário feedback garantido sobre o processo de ativação, o processo será finalizado se [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046) não for chamado dentro de 15 segundos após a inicialização do processo de ativação.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> partial class GameHomePage : Page
> {
>     public GameHomePage()
>     {
>         InitializeComponent();
> 
>         // add a handler to be called when the home page has been loaded
>         this.Loaded += ReaderHomePageLoaded;
> 
>         // load the minimal amount of image and sound data from disk necessary to create the home page.        
>     }
>     
>     void ReaderHomePageLoaded(object sender, RoutedEventArgs e)
>     {
>         // set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = this;
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Friend Class GameHomePage
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' add a handler to be called when the home page has been loaded
>         AddHandler Me.Loaded, AddressOf ReaderHomePageLoaded
> 
>         ' load the minimal amount of image and sound data from disk necessary to create the home page.        
>     End Sub
> 
>     Private Sub ReaderHomePageLoaded(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         ' set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = Me
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class
> ```

Para ver um exemplo do uso de telas iniciais prolongadas, consulte [Exemplo de tela inicial](http://go.microsoft.com/fwlink/p/?linkid=234889).

### <a name="phase-3"></a>Fase 3

O fato de o aplicativo ter exibido a interface do usuário não significa que ele esteja completamente pronto para uso. No caso do nosso jogo, a interface do usuário é exibida com espaços reservados para os recursos que exigem dados da Internet. Nesse ponto, o jogo baixa os dados adicionais necessários para tornar o aplicativo plenamente funcional e progressivamente habilita os recursos à medida que os dados são adquiridos.

Às vezes, uma boa parte do conteúdo necessário para ativação pode vir junto com o aplicativo. Esse é o caso em um jogo simples, o que torna o processo de ativação bastante fácil. No entanto, muitos programas (como leitores de notícias e visualizadores de fotos) precisam extrair informações da Web para se tornarem funcionais. Esses dados podem ser pesados e levar um tempo considerável para serem baixados. A maneira como o aplicativo obtém esses dados durante o processo de ativação pode ter um enorme impacto sobre a percepção de desempenho do aplicativo.

Uma página de carregamento, ou pior, uma tela inicial, poderia ser exibida por vários minutos se o aplicativo tentasse baixar um conjunto de dados inteiro necessário para a funcionalidade na fase um ou dois da ativação. Isso faria o aplicativo parecer que não está respondendo ou provocaria seu encerramento pelo sistema. Recomendamos que o aplicativo baixe a quantidade mínima necessária de dados para mostrar uma interface do usuário interativa com elementos de espaço reservado na fase 2 e depois progressivamente carregue dados que substituam os elementos de espaço reservado na fase 3. Para obter mais informações sobre como lidar com os dados, consulte [Otimizar ListView e GridView](optimize-gridview-and-listview.md).

Como exatamente o aplicativo vai reagir a cada fase da inicialização é você que decide, mas ao fornecer o máximo de retorno possível (tela inicial, tela de carregamento, interface do usuário enquanto os dados são carregados), você faz o usuário ter a impressão de que o aplicativo e o sistema como um todo são rápidos.

### <a name="minimize-managed-assemblies-in-the-startup-path"></a>Minimize assemblies gerenciados no caminho de inicialização

Os códigos reutilizáveis costumam vir na forma da inclusão de módulos (dlls) inclusos em um projeto. O carregamento desses módulos requer acesso ao disco e, como você pode imaginar, o custo de fazê-lo pode aumentar. Isso tem mais impacto na inicialização a frio, mas pode ter impacto na inicialização a quente, também. No caso do C# e do Visual Basic, o CLR tenta adiar esse custo o máximo possível carregando assemblies sob demanda. Ou seja, o CLR não carrega um módulo até que um método executado faça referência a ele. Portanto, faça referência apenas a assemblies necessários para a inicialização do aplicativo no código de inicialização, para que o CLR não carregue módulos desnecessários. Se tiver caminhos de código não utilizados em seu caminho de inicialização com referências desnecessárias, você poderá migrar esses caminhos de código para outros métodos, a fim de evitar as cargas desnecessárias.

Uma outra maneira de reduzir as cargas de módulos é combinar os módulos do seu aplicativo. Carregar um assembly grande geralmente leva menos tempo do que carregar dois pequenos. Isso nem sempre é possível, e você deverá combinar módulos somente se isso não fizer uma diferença relevante para a produtividade do desenvolvedor ou a capacidade de reutilização do código. Você pode usar ferramentas como o [PerfView](http://go.microsoft.com/fwlink/p/?linkid=251609) ou o [Windows Performance Analyzer (WPA)](https://msdn.microsoft.com/library/windows/apps/xaml/ff191077.aspx) para descobrir quais módulos são carregados na inicialização.

### <a name="make-smart-web-requests"></a>Faça solicitações da Web inteligentes

É possível melhorar drasticamente o tempo de carregamento de um aplicativo compactando seu conteúdo localmente, incluindo XAML, imagens e qualquer outro arquivo importante para o aplicativo. Operações de disco são mais rápidas do que operações de rede. Se um aplicativo precisa de um determinado arquivo na inicialização, você pode reduzir o tempo geral de inicialização carregando-o do disco em vez de recuperá-lo de um servidor remoto.

## <a name="journal-and-cache-pages-efficiently"></a>Registre e armazene as páginas em cache com eficiência

O controle Frame fornece recursos de navegação. Ele oferece navegação para uma página (método Navigate), diário de navegação (propriedades BackStack/ForwardStack, método GoForward/GoBack), armazenamento de páginas em cache (Page.NavigationCacheMode) e suporte para serialização (método GetNavigationState).

O desempenho com o qual devemos ficar atentos com o Frame envolve principalmente o registro no diário e o armazenamento de páginas em cache.

**Registro de Frame no diário**. Quando você navega até uma página com Frame.Navigate(), uma PageStackEntry para a página atual é adicionada à coleção Frame.BackStack. A PageStackEntry é relativamente pequena, mas não nenhum limite interno para o tamanho da coleção BackStack. Possivelmente, um usuário pode navegar em um loop e ampliar essa coleção indefinidamente.

A PageStackEntry também inclui o parâmetro que foi passado para o método Frame.Navigate(). É recomendável que esse parâmetro seja um tipo primitivo serializável (como int ou string), para permitir que o método Frame.GetNavigationState() funcione. Mas esse parâmetro pode referenciar um objeto que envolva quantidades de conjuntos de trabalho mais significativas ou outros recursos, tornando cada entrada no BackStack muito mais cara. Por exemplo, você poderia usar um StorageFile como parâmetro e, consequentemente, o BackStack manteria um número indefinido de arquivos abertos.

Portanto, é recomendável manter os parâmetros de navegação ao mínimo e limitar o tamanho do BackStack. O BackStack é um vetor padrão (IList em C#, Platform::Vector em C++/CX) e, portanto, pode ser cortado com a simples remoção de entradas.

**Armazenamento de páginas em cache**. Por padrão, quando você navega para uma página com o método Frame.Navigate, uma nova instância da página é instanciada. Da mesma forma, se você retornar à página anterior com Frame.GoBack, uma nova instância da página anterior será alocada.

O Frame, porém, oferece um cache de página opcional que pode evitar essas criações de instâncias. Para armazenar uma página em cache, use a propriedade Page.NavigationCacheMode. Definindo esse modo como Required forçará a página a ser armazenada em cache, definindo-a como Enabled permitirá que ela seja armazenada em cache. Por padrão, o tamanho do cache é de 10 páginas, mas isso pode ser alterado com a propriedade Frame.CacheSize. Todas as páginas necessárias serão armazenadas em cache, e se houver menos de páginas CacheSize Required, as páginas Enabled também poderão ser armazenadas em cache.

O armazenamento de páginas em cache pode ajudar no desempenho evitando a criação de instâncias e, portanto, melhorando o desempenho da navegação. O armazenamento de páginas em cache pode prejudicar o desempenho com o armazenamento em cache excessivo, afetando, assim conjunto de trabalho.

Portanto, recomendamos usar o armazenamento de páginas em cache conforme apropriado para seu aplicativo. Por exemplo, digamos que você tenha um aplicativo que mostre uma lista de itens em um quadro e, quando você toca em um item, ele move o quadro para uma página de detalhes desse item. A página da lista provavelmente deve ser definida para o cache. Se a página de detalhes for a mesma para todos os itens, provavelmente ela também deverá ser armazenada em cache. Mas se a página de detalhes for mais heterogênea, talvez seja melhor desativar o armazenamento em cache.
