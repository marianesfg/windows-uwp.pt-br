---
author: KarlErickson
title: Criar vinculações de dados
description: Este artigo aborda os conceitos básicos da vinculação de dados no XAML
keywords: XAML, UWP, Introdução
ms.author: karler
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8000d9105481bc177eb2fc64646aec009fd80d36
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6981517"
---
# <a name="create-data-bindings"></a>Criar vinculações de dados

Suponhamos que você tenha projetado e implementado uma interface do usuário de boa aparência, repleta imagens de espaço reservado, texto clichê "lorem ipsum" e controles que ainda não fazem nada. Em seguida, você a conectará a dados reais, e ela deixará de ser um protótipo de design para se transformar em um app dinâmico. 

Neste tutorial, você aprenderá a substituir seu clichê por vinculações de dados e criar outros links diretos entre a interface do usuário e os dados. Você também aprenderá a formatar ou converter os dados para exibição e manter a interface do usuário e os dados sincronizados. Quando você concluir este tutorial, será capaz de melhorar a simplicidade e a organização do XAML e do código C#, facilitando a manutenção e a extensão.

Você iniciará com uma versão simplificada do exemplo do PhotoLab. Esta versão de iniciante inclui a camada de dados completa, além dos layouts de página XAML básicos, e deixa de fora muitos recursos para facilitar a navegação do código. Este tutorial não é compilado para o app completo; portanto, confira a versão final para ver recursos como animações personalizadas e suporte por telefone. Você pode encontrar a versão final na pasta raiz do repositório [Windows-appsample-photo-laboratório](https://github.com/Microsoft/Windows-appsample-photo-lab). 

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017, e a última versão do SDK do Windows 10](https://developer.microsoft.com/windows/downloads).

## <a name="part-0-get-the-code"></a>Parte 0: Obter o código
O ponto de partida deste laboratório é o repositório de exemplos do PhotoLab, na pasta [xaml-basics-starting-points/data-binding](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/data-binding). Após ter clonado ou baixado o repositório, você poderá editar o projeto abrindo o PhotoLab.sln com o Visual Studio 2017.

O app PhotoLab tem duas páginas principais:

**MainPage.xaml:** exibe o modo de exibição de galeria de fotos, juntamente com algumas informações sobre cada arquivo de imagem.
![MainPage](../design/basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** exibe uma única foto após ela ter sido selecionada. Um menu de edição de submenu permite que a foto seja alterada, renomeada e salva.
![DetailPage](../design/basics/images/xaml-basics/detailpage.png)

## <a name="part-1-replace-the-placeholders"></a>Parte 1: Substituir os espaços reservados

Aqui você criará ligações únicas no modelo de dados XAML para exibir imagens reais e metadados de imagem, em vez de conteúdo de espaço reservado. 

As vinculações únicas destinam-se a dados somente leitura e inalteráveis, o que significa que eles são de alto desempenho e fáceis de criar, permitindo que você exiba grandes conjuntos de dados grandes nos controles **GridView** e **ListView**. 

**Substitua os espaços reservados por vinculações únicas**

1. Abra a pasta xaml-basics-starting-points\data-binding e inicie o arquivo PhotoLab.sln. 

2. Verifique se a Plataforma de Solução foi definida para x86 ou x64, e não para ARM, e execute o app. Isso mostra o estado do app com espaços reservados de interface do usuário, antes que as vinculações tenham sido adicionadas. 

    ![Executando o app com imagens e texto de espaço reservado](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. Abra o MainPage.xaml e procure um **DataTemplate** chamado **ImageGridView_DefaultItemTemplate**. Você atualizará esse modelo para usar vinculações de dados. 

    **Antes:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    O valor **X:Key.** é usado pelo **ImageGridView** para selecionar esse modelo para exibição de objetos de dados. 

4. Adicione um valor **x: DataType** ao modelo. 

    **Depois:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
    ```

    **x:DataType** indica a qual tipo este um modelo se destina. Nesse caso, trata-se de um modelo para a classe **ImageFileInfo** (em que "local:" indica o namespace local, conforme definido em uma declaração xmlns próximo à parte superior do arquivo).
    
    **x: DataType** é necessário ao usar expressões **x:Bind** em um modelo de dados, conforme descrito a seguir. 

5. No **DataTemplate**, localize o elemento **Image** chamado **ItemImage** e substitua seu valor **Source** conforme mostrado. 

    **Antes:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="/Assets/StoreLogo.png" 
           Stretch="Uniform" />
    ```
    
    **Depois:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="{x:Bind ImageSource}" 
           Stretch="Uniform" />
    ```
    
    **x:Name** identifica um elemento XAML para que você possa consultá-lo em outro lugar no XAML e no code-behind. 

    A expressões **x:Bind** fornecem um valor a uma propriedade de interface do usuário, obtendo o valor de uma propriedade **data-object**. Nos modelos, a propriedade indicada destina-se a qualquer valor para o qual **x: DataType** tenha sido definido. Portanto, nesse caso, a fonte de dados é a propriedade **ImageFileInfo.ImageSource**. 
    
    > [!NOTE] 
    > O valor **x:Bind** também permite ao editor saber sobre o tipo de dados, para que você possa usar o IntelliSense em vez de digitar o nome da propriedade em uma expressão **x:Bind**. Experimente-o no código que você acabou de colar: coloque o cursor logo após **x:Bind** e pressione a barra de espaço para ver uma lista de propriedades às quais é possível se vincular.

6. Substitua os valores dos outros controles de interface do usuário da mesma maneira. (Tente fazer isso com o IntelliSense em vez de copiar/colar!)

    **Antes:**
    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <telerikInput:RadRating Value="3" ... />
    ```
    
    **Depois:**
    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <telerikInput:RadRating Value="{x:Bind ImageRating}" ... />
    ```

Execute o app para ver sua aparência até agora. Não há mais espaços reservados! Começamos bem! 

![Executando o app com imagens e texto reais, e não com espaços reservados](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> Se você quiser fazer mais testes, tente adicionar um novo TextBlock ao modelo de dados e use o truque x:Bind IntelliSense para localizar uma propriedade a ser exibida. 

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>Parte 2: Usar a vinculação para conectar a interface do usuário da galeria às imagens

Aqui, você criará vinculações únicas na página XAML para conectar a exibição da galeria à coleção de imagens, substituindo o código de procedimento existente que faz isso no code-behind. Você também criará um botão **Excluir** para ver como a exibição da galeria muda quando as imagens são removidas da coleção. Ao mesmo tempo, você aprenderá a vincular eventos a manipuladores de eventos para obter mais flexibilidade do que a fornecido pelos manipuladores de eventos tradicionais. 

Todas as vinculações abordadas até agora estão dentro de modelos de dados e referem-se a propriedades da classe indicada pelo valor **x:DataType**. E o restante do XAML na página? 

As expressões **x:Bind** fora dos modelos de dados sempre estão vinculadas à página. Isso significa que você pode fazer referência a qualquer item colocado no code-behind ou declarado no XAML, incluindo propriedades personalizadas e propriedades de outros controles de interface do usuário na página (contanto que elas tenham um valor **x:Name** ). 

No exemplo do PhotoLab, uma as funções de uma vinculação como estsa é conectar o controle **GridView** principal diretamente à coleção de imagens, em vez de fazê-lo no code-behind. Mais tarde, você verá outros exemplos. 

**Vincular o controle GridView principal à coleção de imagens**

1. No MainPage.xaml.cs, localize o método **OnNavigatedTo** e remova o código que define **ItemsSource**.

    **Antes:**
    ```c#
    ImageGridView.ItemsSource = Images;
    ```

    **Depois:**
    ```c#
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. No MainPage.xaml, localize o **GridView** chamado **ImageGridView** e adicione um atributo **ItemsSource**. Como valor, use uma expressão **x:Bind** que faça referência à propriedade **Images** implementada no code-behind. 

    **Antes:**
    ```xaml
    <GridView x:Name="ImageGridView" 
    ```

    **Depois:**
    ```xaml
    <GridView x:Name="ImageGridView" 
              ItemsSource="{x:Bind Images}" 
    ```

    A propriedade **Images** é do tipo **ObservableCollection\<ImageFileInfo\>**; portanto, os itens individuais exibidos no **GridView** são do tipo **ImageFileInfo**. Isso corresponde ao valor **x: DataType** descrito na Parte 1. 

Todas as vinculações que examinamos até agora são únicas e somente leitura, que é o comportamento padrão das expressões **x:Bind** sem formatação. Os dados são carregados apenas na inicialização, o que torna as vinculações de alto desempenho perfeito para suporte a várias exibições complexas de grandes conjuntos de dados. 

Até mesmo a vinculação **ItemsSource** que você acabou de adicionar é única e somente leitura para um valor de propriedade inalterável, mas há uma distinção importante a fazer aqui. O valor inalterável da propriedade **Images** é uma instância única e específica de uma coleção, inicializada uma vez, conforme mostrado aqui.

```Csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

O valor da propriedade **Images** nunca muda, mas como a propriedade é do tipo **ObservableCollection\<T\>**, o *conteúdo* da coleção pode ser alterado, e a vinculação perceberá automaticamente as alterações e atualizará a interface do usuário. 

Para testar isso, vamos adicionar temporariamente um botão que exclua a imagem atualmente selecionada. Esse botão não está na versão final porque a seleção de uma imagem o levará a uma página de detalhes. No entanto, o comportamento de **ObservableCollection\<T\>** 1 ainda é importante no exemplo final do PhotoLab porque o XAML é inicializado no construtor da página (por meio da chamada do método **InitializeComponent** ), mas a coleção **Images** é preenchida posteriormente no método **OnNavigatedTo**. 

**Adicionar um botão Excluir**

1. No MainPage.xaml, localize o **CommandBar** chamado **MainCommandBar** e adicione um novo botão antes do botão de zoom. (Os controles de zoom não funcionam ainda. Você os conectará na próxima parte do tutorial.)

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    Se você já está familiarizado com o XAML, esse valor **Click** pode parecer incomum. Nas versões anteriores do XAML, você tinha que configurar isso para um método com uma assinatura específica de manipulador de eventos, geralmente incluindo parâmetros para o remetente do evento e um objeto de argumento específico de evento. Você ainda pode usar essa técnica quando precisar de argumentos de evento, mas, com x:Bind, você também pode se conectar a outros métodos. Por exemplo, se você não precisar dos dados de evento, poderá se conectar a métodos que não tenham parâmetros, como fazemos aqui.

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. No MainPage.xaml.cs, adicione o método **DeleteSelectedImage**.

    ```c#
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    Esse método simplesmente exclui a imagem selecionada da coleção **Images**. 

Agora execute o app e use o botão para apagar algumas imagens. Como você pode ver, a interface do usuário é atualizada automaticamente, graças à vinculação de dados e ao tipo **ObservableCollection\ < T\ >**. 

> [!Note]
> Se você quiser um desafio ainda maior, tente adicionar dois botões que movam a imagem selecionada para cima ou para baixo na lista e, em seguida, aplique a expressão x:Bind a seus eventos Click para dois novos métodos similares a DeleteSelectedImage.
 
## <a name="part-3-set-up-the-zoom-slider"></a>Parte 3: Configurar o controle deslizante de zoom 

Nesta parte, você criará vinculações unidirecionais de um controle no modelo de dados para o controle deslizante de zoom, que está fora do modelo. Você também aprenderá que pode usar a vinculação de dados com várias propriedades de controle, e não apenas com as mais óbvias, como **TextBlock.Text** e **Image.Source**. 

**Vincular o modelo de dados da imagem ao controle deslizante de zoom**

* Localize o **DataTemplate** chamado **ImageGridView_DefaultItemTemplate** e substitua os valores **Height** e **Width** do controle **Grid** na parte superior do modelo.

    **Antes**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Depois**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    <!-- TODO talk about dependency properties --> 
    
    Você notou que essas são expressões **Binding**, e não expressões **x:Bind**? Essa é a maneira antiga de fazer vinculações de dados e, na maioria das vezes, é considerada obsoleta. **x:Bind** faz quase tudo que **Binding** faz, e muito mais. No entanto, quando você usa **x:Bind** em um modelo de dados, ele se vincula ao tipo declarado no valor **x:DataType**. Então, como vincular algo no modelo a algo na página XAML ou no code-behind? Você deve usar a antiga expressão **Binding**. 
    
    As expressões**Binding** não reconhecem o valor **x:DataType**, mas elas têm valores **ElementName** que funcionam quase da mesma maneira. Eles informam ao mecanismo de vinculação que **Binding Value** é uma vinculação à propriedade **Value** do elemento especificado na página (ou seja, o elemento com esse valor **x:Name**). Se você deseja vincular-se a uma propriedade no code-behind, a aparência seria algo semelhante a ```{Binding MyCodeBehindProperty, ElementName=page}```, em que **page** representa o valor **X:Names** definido no elemento **Page** no XAML. 
    
    > [!NOTE]
    > Por padrão, as expressões **Binding** são one-*way*, o que significa que elas atualizarão automaticamente a interface do usuário quando o valor de propriedade vinculado for alterado. 
    > 
    > Em contrapartida, o padrão de **x:Bind** é one-*time*, o que significa que todas as alterações à propriedade vinculada serão ignoradas. Esse é o padrão porque é a opção de mais alto desempenho, e a maioria das vinculações são dados estáticos e somente leitura. 
    >
    > A lição aqui é que, se você usar **x:Bind** com propriedades que podem alterar seus valores, não deixe de adicionar ```Mode=OneWay``` ou ```Mode=TwoWay```. Você verá exemplos disso na próxima seção.

Execute o app e use o controle deslizante para alterar as dimensões do modelo de imagem. Como você pode ver, o efeito é muito eficaz, sem a necessidade de muito código. 

![Executando o app com a exibição do controle deslizante de zoom](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> Se quiser um desafior ainda maior, tente vincular outras propriedades de interface do usuário à propriedade **Value** ou a outros controles deslizantes adicionados após o controle deslizante de zoom. Por exemplo, você pode vincular a propriedade **FontSize** do **TitleTextBlock** a um novo controle deslizante com o valor padrão **24**. Não deixe de definir valores mínimos e máximos razoáveis.

## <a name="part-4-improve-the-zoom-experience"></a>Parte 4: Melhorar a experiência de zoom 

Nesta parte, você adicionará uma propriedade personalizada **ItemSize** ao code-behind e criará vinculações undirecionais do modelo de imagem à nova propriedade. O valor **ItemSize** será atualizado pelo controle deslizante de zoom e outros fatores, como alternância **Fit to screen** e tamanho de janela, proporcionando uma experiência mais refinada. 

Diferente das propriedades de controle internas, as propriedades personalizadas não atualizam automaticamente a interface do usuário, mesmo com vinculações unidirecionais e bidirecionais. Elas funcionam bem com vinculações **únicas**, mas se você quiser que as alterações de propriedade apareçam na interface do usuário, você precisará executar outras etapas. 

**Criar a propriedade ItemSize de modo que ela atualize a interface do usuário**

1. No MainPage.xaml.cs, altere a assinatura da classe **MainPage** para que ela implemente a interface **INotifyPropertyChanged**.

    **Antes:**
    ```c#
    public sealed partial class MainPage : Page
    ```

    **Depois:**
    ```c#
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    Isso informa ao sistema de vinculação que o MainPage possui um evento PropertyChanged (que será adicionado em seguida) no qual as vinculações podem realizar a escuta para atualizar a interface do usuário. 

2. Adicione um evento **PropertyChanged** à classe **MainPage**.

    ```c#
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    Esse evento fornece a implementação completa exigida pela interface **INotifyPropertyChanged**. No entanto, para que ela surta algum efeito, você deve gerar explicitamente o evento nas propriedades personalizadas. 

3. Adicione uma propriedade **ItemSize** e gere o evento **PropertyChanged** no setter.

    ```c#
    public double ItemSize
    {
        get => _itemSize;
        set
        {
            if (_itemSize != value)
            {
                _itemSize = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ItemSize)));
            }
        }
    }
    private double _itemSize;
    ```

    A propriedade **ItemSize** expõe o valor de um campo privado **_itemSize**. O uso de um campo de apoio como esse permite que a propriedade verifique se um novo valor é igual ao valor antigo antes de gerar um evento **PropertyChanged** potencialmente desnecessário.

    O próprio evento é gerado pelo método **Invoke**. O ponto de interrogação verifica se o evento **PropertyChanged** é nulo, ou seja, se algum manipulador de eventos já foi adicionado. Cada vinculação unidirecional ou bidirecional adiciona um manipulador de eventos nos bastidores, mas se nenhuma vinculação estiver realizando a escuta, nada mais acontecerá aqui. No entanto, se **PropertyChanged** não for nulo, **Invoke** será chamado com uma referência à origem do evento (a própria página, representada pela palavra-chave **this** ) e a um objeto **event-args** que indica o nome da propriedade. Com essas informações, todas as vinculações unidirecionais ou bidirecionais à propriedade **ItemSize** serão informadas sobre quaisquer alterações para que possam atualizar a interface do usuário vinculada. 

4. No MainPage.xaml, localize o **DataTemplate** chamado **ImageGridView_DefaultItemTemplate** e substitua os valores **Height** e **Width** do controle **Grid** na parte superior do modelo. (Se você fez a vinculação entre controles na parte anterior deste tutorial, as únicas alterações serão substituir **Value** por **ItemSize** e **ZoomSlider** por **page**. Não deixe de fazer isso para Height e Width!)

    **Antes**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Depois**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Agora que a interface do usuário pode responder às alterações de **ItemSize**, você precisa fazer algumas alterações. Como mencionado anteriormente, o valor **ItemSize** é calculado a partir do estado atual de vários controles de interface do usuário, mas o cálculo deve ser executado sempre que esses controles mudarem de estado. Para fazer isso, você usará a vinculação de eventos para que determinadas alterações de interface do usuário chamem um método auxiliar que atualize **ItemSize**. 

**Atualizar o valor da propriedade ItemSize**

1. Adicione o método **DeleteSelectedImage** a MainPage.xaml.cs.

    ```c#
    private void DetermineItemSize()
    {
        if (FitScreenToggle != null
            && FitScreenToggle.IsOn == true
            && ImageGridView != null
            && ZoomSlider != null)
        {
            // The 'margins' value represents the total of the margins around the
            // image in the grid item. 8 from the ItemTemplate root grid + 8 from
            // the ItemContainerStyle * (Right + Left). If those values change, 
            // this value needs to be updated to match.
            int margins = (int)this.Resources["LargeItemMarginValue"] * 4;
            double gridWidth = ImageGridView.ActualWidth -
                (int)this.Resources["DesktopWindowSidePaddingValue"];
    
            if (AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile" &&
                UIViewSettings.GetForCurrentView().UserInteractionMode ==
                    UserInteractionMode.Touch)
            {
                margins = (int)this.Resources["SmallItemMarginValue"] * 4;
                gridWidth = ImageGridView.ActualWidth -
                    (int)this.Resources["MobileWindowSidePaddingValue"];
            }
    
            double ItemWidth = ZoomSlider.Value + margins;
            // We need at least 1 column.
            int columns = (int)Math.Max(gridWidth / ItemWidth, 1);
    
            // Adjust the available grid width to account for margins around each item.
            double adjustedGridWidth = gridWidth - (columns * margins);
    
            ItemSize = (adjustedGridWidth / columns);
        }
        else
        {
            ItemSize = ZoomSlider.Value;
        }
    }
    ```

2. No MainPage.xaml, navegue até a parte superior do arquivo e adicione um evento **SizeChanged** vinculado ao elemento **Page**.

    **Antes:**
    ```xaml
    <Page x:Name="page"  
    ```

    **Depois:**
    ```xaml
    <Page x:Name="page" 
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. Localize o **Slider** chamado **ZoomSlider** e adicione uma vinculação de eventos **ValueChanged**.

    **Antes:**
    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **Depois:**
    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. Localize o **ToggleSwitch** chamado **FitScreenToggle** e adicione uma vinculação de eventos **Toggled**.

    **Antes:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **Depois:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

Execute o app e use o controle deslizante de zoom e a alternância **Fit to screen** para alterar as dimensões de modelo de imagem. Como você pode ver, as últimas alterações permitem uma experiência de zoom/redimensionamento mais refinada, mantendo o código bem organizado. 

![Executando o app com o ajuste à tela habilitado](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> Se quiser um desafio ainda maior, tente adicionar um **TextBlock** após o **ZoomSlider** e vincular a propriedade **Text** à propriedade **ItemSize**. Como não se trata de um modelo de dados, você pode usar **x:Bind**, em vez de **Binding**, como nas vinculações **ItemSize** anteriores.  
}

## <a name="part-5-enable-user-edits"></a>Parte 5: Habilitar edições do usuário

Aqui, você criará vinculações bidirecionais para permitir aos usuários atualizar valores, incluindo o título da imagem, a classificação e vários efeitos visuais. 

Para conseguir isso, você atualizará o **DetailPage** existente, que fornece um visualizador de imagem única, controle de zoom e interface do usuário para edição.  

No entanto, primeiro você precisa anexar o **DetailPage** para que o app navegue até ele quando o usuário clicar em uma imagem na exibição da galeria.

**Anexar o DetailPage**

1. No MainPage.xaml, localize o **GridView** chamado **ImageGridView** e adicione um valor **ItemClick**. 

    > [!TIP] 
    > Se você digitar a alteração abaixo, em vez de copiá-la ou colá-la, verá um pop-up IntelliSense informando "\<New Event Handler\>". Se você pressionar a tecla Tab, ela preencherá o valor com um nome de manipulador de método padrão e desligará automaticamente o método mostrado na próxima etapa. Pressione F12 para navegar até o método no code-behind. 

    **Antes:**
    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **Depois:**
    ```xaml
    <GridView x:Name="ImageGridView"
              ItemClick="ImageGridView_ItemClick"
    ```

    > [!NOTE] 
    > Estamos usando um manipulador de eventos convencional aqui, em vez de uma expressão x:Bind. Isso porque precisamos ver os dados de evento, como mostrado a seguir. 

2. No MainPage.xaml.cs, adicione o manipulador de eventos (ou preencha-o, se você tiver seguido a dica na última etapa).

    ```c#
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    Esse método simplesmente navega até a página de detalhes, passando o item clicado, que é um objeto **ImageFileInfo** usado por **DetailPage.OnNavigatedTo** para inicializar a página. Você não precisará implementar esse método neste tutorial, mas pode dar uma olhada para ver o que ele faz. 
    
3. (Opcional) Exclua ou comente todos os controles adicionados nos pontos de reprodução anteriores que funcionam com a imagem selecionada. Mantê-los não fará mal nenhum, mas agora é muito mais difícil selecionar uma imagem sem navegar até a página de detalhes. 

Agora que você conectou as duas páginas, execute o app e dê uma olhada. Tudo funciona, exceto os controles no painel de edição, que não respondem quando você tenta alterar os valores. 

Como você pode ver, a caixa de texto do título exibe o título e permite que você digite alterações. Você precisará alterar o foco para outro controle para confirmar as alterações, mas o título no canto superior esquerdo da tela ainda não foi atualizado. 

Todos os controles já estão vinculados por meio da expressão **x:Bind** sem formatação que abordamos na Parte 1. Se você relembrar, isso significa que todas as vinculações são únicas, o que explica por que as alterações nos valores não são registradas. Para corrigir isso, tudo o que temos a fazer é transformá-las em vinculações bidirecionais. 

**Tornar os controles de edição interativos**

1. No DetailPage.xaml, localize o **TextBlock** chamado **TitleTextBlock** e, em seguida, o controle **RadRating**; atualize suas expressões **x:Bind** de modo que incluam **Mode=TwoWay**.

    **Antes:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **Depois:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock" 
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}" 
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating, Mode=TwoWay}" 
                            ... >
    ```

2. Faça o mesmo para todos os controles deslizantes de efeito que acompanham o controle de classificação.

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ... 
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ... 
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ... 
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ... 
    ```

O modo bidirecional, como era de se esperar, significa que os dados são movidos em ambas as direções sempre que há alterações em ambos os lados. 

Assim como as vinculações unidirecionais abordadas anteriormente, agora essas vinculações bidirecionais atualizarão a interface do usuário sempre que as propriedades vinculadas forem alteradas, graças à implementação **INotifyPropertyChanged** na classe **ImageFileInfo**. No entanto, com a vinculação bidirecional, os valores também serão movidos da interface do usuário para as propriedades vinculadas sempre que o usuário interagir com o controle. Nada mais é necessário no lado do XAML. 

Execute o app e teste os controles de edição. Como você pode ver, quando você fizer uma alteração, ela agora afetará os valores de imagem, e essas alterações persistirão quando você retornar à página principal. 

## <a name="part-6-format-values-through-function-binding"></a>Parte 6: Formatar valores por meio da vinculação de função

Um último problema permanece. Quando você move os controles deslizantes de efeito, as etiquetas ao lado deles ainda não são alteradas. 

![Controles deslizantes de efeito com valores de etiqueta padrão](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

A parte final deste tutorial é adicionar vinculações que formatam os valores do controle deslizante para exibição.

**Vincular as etiquetas do controle deslizante de efeito e formatar os valores para exibição**

1. Localize o **TextBlock** após o controle deslizante **Exposure** e substitua o valor **Text** pela expressão de vinculação mostrada aqui.

    **Antes:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="0.00" />
    ```

    **Depois:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Isso é chamado de vinculação de função porque você está se vinculando ao valor de retorno de um método. O método estará acessível através do code-behind da página ou do tipo **x:DataType** se você estiver em um modelo de dados. Nesse caso, ele é o método familiar **ToString** do .NET, que é acessado através da propriedade de item da página e, depois, através da propriedade **Exposure** do item. (Isso ilustra como você pode se vincular a métodos e propriedades que estão profundamente aninhados em uma cadeia de conexões.)

    A vinculação de função é a maneira ideal de formatar valores para exibição porque você pode passar outras fontes de vinculação como argumentos de métod, e a expressão de vinculação fará a escuta das alterações desses valores conforme esperado com o modo unidirecional. Neste exemplo, o argumento **culture** é uma referência a um campo inalterável implementado no code-behind, mas ele poderia tranquilamente ter sido uma propriedade que gera eventos **PropertyChanged**. Nesse caso, qualquer alteração no valor da propriedade faria com que a expressão **x:Bind** chamasse **ToString** com o novo valor e depois atualizasse a interface do usuário com o resultado. 

2. Faça o mesmo para o **TextBlocks** que identifica os outros controles deslizantes de efeito.

    ```xaml
    <Slider Header="Temperature" ... />
    <TextBlock ... Text="{x:Bind item.Temperature.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Tint" ... />
    <TextBlock ... Text="{x:Bind item.Tint.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Contrast" ... />
    <TextBlock ... Text="{x:Bind item.Contrast.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Saturation" ... />
    <TextBlock ... Text="{x:Bind item.Saturation.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Blur" ... />
    <TextBlock ... Text="{x:Bind item.Blur.ToString('N', culture), Mode=OneWay}" />
    ```

Agora, quando você executa o app, tudo funciona, inclusive as etiquetas do controle deslizante. 

![Controles deslizantes de efeito com as etiquetas funcionando](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

> [!NOTE]
> Tente usar a vinculação de função com o **TextBlock** do último ponto de reprodução e vinculá-lo a um novo método que retorne uma cadeia de caracteres no formato "000 x 000" quando você passa o valor **ItemSize**.


## <a name="conclusion"></a>Conclusão

Este tutorial fez uma demonstração da vinculação de dados e mostrou algumas das funcionalidades disponíveis. Um aviso antes de concluir: nem tudo pode ser vinculado e, às vezes, os valores que você tenta conectar são incompatíveis com as propriedades que você está tentando vincular. O processo de vinculação é muito flexível, mas não funcionará em todas as situações.

Um exemplo de um problema não abordado pela vinculação é quando um controle não tem propriedades adequadas às quais vincular, como o recurso de zoom da página de detalhes. Esse controle deslizante de zoom precisará interagir com o **ScrollViewer** que exibe a imagem, mas **ScrollViewer** só pode ser atualizado por meio do método **ChangeView**. Nesse caso, usamos os manipuladores de eventos convencionais para manter o **ScrollViewer** e o controle deslizante de zoom em sincronia; consulte os métodos **DetailPage**, **ZoomSlider_ValueChanged** e **MainImageScroll_ViewChanged** para obter detalhes.

No entanto, a vinculação é uma maneira eficaz e flexível de simplificar o código e manter a lógica de interface do usuário separada da lógica de dados. Isso facilitará muito o ajuste para ambos os lados, reduzindo o risco de inserção de bugs no outro lado. 

Um exemplo de interface do usuário e separação de dados ocorre na propriedade **ImageFileInfo.ImageTitle**. Essa propriedade (e a propriedade **ImageRating**) é um pouco diferente da propriedade **ItemSize** que você criou na Parte 4 porque o valor é armazenado nos metadados do arquivo (exposto através do tipo **ImageProperties**), e não em um campo. Além disso, **ImageTitle** retorna o valor **ImageName** (definido para o nome do arquivo) se não houver título nos metadados do arquivo. 

```c#
public string ImageTitle
{
    get => String.IsNullOrEmpty(ImageProperties.Title) ? ImageName : ImageProperties.Title;
    set
    {
        if (ImageProperties.Title != value)
        {
            ImageProperties.Title = value;
            var ignoreResult = ImageProperties.SavePropertiesAsync();
            OnPropertyChanged();
        }
    }
}
```

Como você pode ver, o setter atualiza a propriedade **ImageProperties.Title** e, em seguida, chama **SavePropertiesAsync** para gravar o novo valor no arquivo. (Esse é um método assíncrono, mas não podemos usar a palavra-chave **await** em uma propriedade; você não gostaria de fazer isso porque os getters e os setters da propriedade precisariam ser concluídos imediatamente. Então, em vez disso, você chamaria o método e ignoraria o objeto **Task** retornado por ele.) 

<!-- TODO more screenshots? -->

## <a name="going-further"></a>Aprofundamento

Agora que você concluiu este laboratório, tem conhecimentos de vinculação suficientes para enfrentar um problema por conta própria.

Como você deve ter notado, se alterar o nível de zoom na página de detalhes, ele será redefinido automaticamente quando você navegar para trás e selecionar a mesma imagem novamente. Você consegue descobrir como preservar e restaurar o nível de zoom em cada imagem? Boa sorte!
    
Você tem todas as informações necessárias neste tutorial, mas, se precisar de mais orientação, os documentos de vinculação de dados estão a apenas um clique de distância. Iniciar aqui:

+ [Extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)
+ [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)