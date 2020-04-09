---
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Otimizar sua marcação XAML
description: Analisar a marcação XAML para construir objetos na memória é demorado para uma interface do usuário complexa. Aqui está o que você pode fazer para melhorar a análise de marcação XAML, o tempo de carregamento e a eficiência de memória para seu aplicativo.
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: beb6dde4036019e004d94e5f60e8f3583c78d775
ms.sourcegitcommit: de34aabd90a92a083dfa17d4f8a98578597763f4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980021"
---
# <a name="optimize-your-xaml-markup"></a>Otimizar sua marcação XAML


Analisar a marcação XAML para construir objetos na memória é demorado para uma interface do usuário complexa. Eis o que você pode fazer para melhorar a análise e o tempo de carregamento de sua marcação XAML, bem como a eficiência de memória do seu aplicativo.

Na inicialização do aplicativo, limite a marcação XAML que é carregada para apenas o que você precisa para sua interface do usuário inicial. Examine a marcação na sua página inicial (incluindo recursos da página) e confirme se você não está carregando elementos extras que não são imediatamente necessários. Esses elementos podem vir de uma variedade de fontes, como dicionários de recursos, elementos que estão inicialmente recolhidos e elementos desenhados sobre outros elementos.

A otimização de XAML para aumentar a eficiência necessita de compensações; nem sempre há uma única solução para cada situação. Aqui vemos alguns problemas comuns e fornecemos orientações que podem ser usadas para fazer as compensações corretas para seu aplicativo.

## <a name="minimize-element-count"></a>Minimizar a contagem de elementos

Embora a plataforma XAML seja capaz de exibir grandes quantidades de elementos, você pode fazer com que a disposição e a renderização de seu aplicativo sejam mais rápidas, usando a menor quantidade de elementos para alcançar os elementos visuais desejados.

As escolhas que você faz no modo como dispõe seus controles de interface de usuário afetam o número de elementos de interface do usuário criados quando o aplicativo é iniciado. Confira mais informações detalhadas sobre otimização de layout em [Otimizar o layout XAML](optimize-your-xaml-layout.md).

A quantidade de elementos é muito importante em modelos de dados porque cada elemento é criado novamente para cada item de dados. Para obter informações sobre redução de quantidade de elementos em uma lista ou grade, confira *Redução de elementos por item* no artigo [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md).

Aqui estão algumas outras formas de reduzir o número de elementos que seu aplicativo precisa carregar na inicialização.

### <a name="defer-item-creation"></a>Adiar a criação do item

Caso sua marcação XAML contenha elementos que não são imediatamente mostrados, é possível adiar o carregamento desses elementos até que sejam mostrados. Por exemplo, é possível adiar a criação de conteúdo não visível, como uma guia secundária em uma interface de usuário em forma de guias. Ou você pode mostrar itens em uma exibição de grade por padrão, ainda fornecendo, porém, uma opção para o usuário exibir os dados em uma lista. É possível adiar o carregamento da lista até que ela seja necessária.

Use [atributo x:Load](../xaml-platform/x-load-attribute.md) em vez da propriedade [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.Visibility) para controlar quando um elemento é mostrado. Quando a visibilidade de um elemento é definida como **Collapsed**, ele é ignorado durante a passagem da renderização, mas você ainda paga os custos da instância do objeto na memória. Se, em vez disso, você usar x:Load, a estrutura não criará a instância do objeto até que ela seja necessária e, portanto, os custos de memória serão ainda menores. A desvantagem é que você paga por uma pequena sobrecarga de memória (cerca de 600 bytes) quando a interface do usuário não é carregada.

> [!NOTE]
> Você pode atrasar o carregamento de elementos usando o atributo [x:Load](../xaml-platform/x-load-attribute.md) ou [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md). O atributo x:Load está disponível a partir da Atualização do Windows 10 para Criadores (versão 1703, build do SDK 15063). A versão mínima de destino do seu projeto Visual Studio deve ser a *Atualização do Windows 10 para Criadores (10.0, build 15063)* para usar x:Load. Para direcionar versões anteriores, use x:DeferLoadStrategy.

Os exemplos a seguir mostram a diferença na quantidade de elementos e no uso da memória quando técnicas diferentes são usadas para ocultar os elementos da interface de usuário. Um ListView e um GridView contendo itens idênticos são colocados em uma raiz Grid da página. O ListView não é visível, mas o GridView é mostrado. A XAML produz a mesma interface de usuário na tela em cada um desses exemplos. Usamos as [ferramentas de criação de perfil e desempenho](tools-for-profiling-and-performance.md) do Visual Studio para verificar a quantidade de elementos e o uso de memória.

#### <a name="option-1---inefficient"></a>Opção 1 – Ineficiente

Aqui, o ListView é carregado, mas não está visível porque sua largura é 0. O ListView e cada um de seu elementos filho são criados na árvore visual e carregados na memória.

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <ListView x:Name="List1" Width="0">
        <ListViewItem>Item 1</ListViewItem>
        <ListViewItem>Item 2</ListViewItem>
        <ListViewItem>Item 3</ListViewItem>
        <ListViewItem>Item 4</ListViewItem>
        <ListViewItem>Item 5</ListViewItem>
        <ListViewItem>Item 6</ListViewItem>
        <ListViewItem>Item 7</ListViewItem>
        <ListViewItem>Item 8</ListViewItem>
        <ListViewItem>Item 9</ListViewItem>
        <ListViewItem>Item 10</ListViewItem>
    </ListView>

    <GridView x:Name="Grid1">
        <GridViewItem>Item 1</GridViewItem>
        <GridViewItem>Item 2</GridViewItem>
        <GridViewItem>Item 3</GridViewItem>
        <GridViewItem>Item 4</GridViewItem>
        <GridViewItem>Item 5</GridViewItem>
        <GridViewItem>Item 6</GridViewItem>
        <GridViewItem>Item 7</GridViewItem>
        <GridViewItem>Item 8</GridViewItem>
        <GridViewItem>Item 9</GridViewItem>
        <GridViewItem>Item 10</GridViewItem>
    </GridView>
</Grid>
```

Árvore visual dinâmica com o ListView carregado. A quantidade total de elementos da página é 89.

![Árvore visual com exibição de lista](images/visual-tree-1.png)

O ListView e seus filhos são carregados na memória.

![Árvore visual com exibição de lista](images/memory-use-1.png)

#### <a name="option-2---better"></a>Opção 2 – Melhor

Aqui, a visibilidade de ListView é definida como recolhida (a outra XAML é idêntica à original). O ListView é criado na árvore visual, mas seus elementos filho não são. Porém, como eles são carregados na memória, o uso da memória é idêntico ao exemplo anterior.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

Árvore visual dinâmica com o ListView recolhido. A quantidade total de elementos da página é 46.

![Árvore visual com exibição de lista recolhida](images/visual-tree-2.png)

O ListView e seus filhos são carregados na memória.

![Árvore visual com exibição de lista](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>Opção 3 – Mais eficiente

Aqui, o atributo x:Load do ListView está definido como **Falso** (a outra XAML é idêntica à original). O ListView não é criado na árvore visual nem carregado na memória na inicialização.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

Árvore visual dinâmica com o ListView não carregado. A quantidade total de elementos da página é 45.

![Árvore visual com exibição de lista não carregada](images/visual-tree-3.png)

O ListView e seus filhos não são carregados na memória.

![Árvore visual com exibição de lista](images/memory-use-3.png)

> [!NOTE]
> As quantidades de elementos e o uso de memória nesses exemplos são muito pequenos e exibidos apenas para demonstrar o conceito. Nesses exemplos, como a sobrecarga ao usar x:Load é maior que a economia de memória, o aplicativo não se beneficiaria. Você deve usar as ferramentas de criação de perfil no seu aplicativo para determinar se há ou não benefícios com o carregamento adiado.

### <a name="use-layout-panel-properties"></a>Usar propriedades de painel de layout

Os painéis de layout têm uma propriedade [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.background), de modo que não é preciso colocar um [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) na frente de um painel apenas para colori-lo.

**Ineficiente**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid>
    <Rectangle Fill="Black"/>
</Grid>
```

**Eficiente**

```xaml
<Grid Background="Black"/>
```

Os painéis de layout também têm propriedades de borda internas. Por isso, não é necessário colocar um elemento [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) ao redor do painel de layout. Confira [Otimizar o layout XAML](optimize-your-xaml-layout.md) para obter mais informações e exemplos.

### <a name="use-images-in-place-of-vector-based-elements"></a>Usar imagens no lugar de elementos baseados em vetor

Se você reutilizar muito o mesmo elemento baseado em vetor, será mais eficiente usar um elemento [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) em vez disso. Os elementos baseados em vetor podem ser mais caros porque a CPU precisa criar cada elemento individual separadamente. O arquivo de imagem precisa ser decodificado apenas uma vez.

## <a name="optimize-resources-and-resource-dictionaries"></a>Otimizar recursos e dicionários de recursos

Normalmente, você usa [dicionários de recursos](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md) para armazenar, em um nível praticamente global, recursos aos quais você deseja fazer referência em vários pontos de seu aplicativo. Por exemplo, estilos, pincéis, modelos e assim por diante.

No geral, otimizamos [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) para não instanciar recursos, a menos que seja solicitado. No entanto, há situações que devem ser evitadas para que os recursos não sejam desnecessariamente instanciados.

### <a name="resources-with-xname"></a>Recursos com x:Name

Use o [atributo x:Key](../xaml-platform/x-key-attribute.md) para referenciar seus recursos. Os recursos com o [atributo x:Name](../xaml-platform/x-name-attribute.md) não se beneficiam com a otimização da plataforma; em vez disso, eles serão instanciados assim que ResourceDictionary for criado. Isso ocorre porque x:Name instrui a plataforma de que seu aplicativo precisa de acesso de campo a esse recurso de forma que a plataforma precise criar algo para o qual irá criar um referência.

### <a name="resourcedictionary-in-a-usercontrol"></a>ResourceDictionary em um UserControl

Um ResourceDictionary definido dentro de um [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) provoca uma penalidade. A plataforma cria uma cópia desse ResourceDictionary para cada instância do UserControl. Se você tiver um UserControl que seja muito usado, mova ResourceDictionary para fora do UserControl e coloque-o no nível de página.

### <a name="resource-and-resourcedictionary-scope"></a>Escopo de recurso e ResourceDictionary

Se a página fizer referência a um controle de usuário ou um recurso definido em um arquivo diferente, a estrutura analisará esse arquivo também.

Aqui, como _InitialPage.xaml_ usa um recurso de _ExampleResourceDictionary.xaml_, o _ExampleResourceDictionary.xaml_ inteiro deve ser analisado na inicialização.

**InitialPage.xaml.**

```xaml
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
        are used in the app, but are not needed during startup.-->
</ResourceDictionary>
```

Caso você use um recurso em várias páginas por todo o aplicativo, armazená-lo em _App.xaml_ é uma boa prática e evita duplicação. Mas _App.xaml_ é analisado na inicialização do aplicativo para que qualquer recurso usado em apenas uma página (a menos que essa seja a página inicial) seja colocado nos recursos locais da página. Este exemplo mostra um _App.xaml_ que contém recursos usados apenas por uma página (que não é a página inicial). Isso aumenta desnecessariamente o tempo de inicialização do aplicativo.

**App.xaml**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Application ...>
     <Application.Resources>
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/>
    </Application.Resources>
</Application>
```

**InitialPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <StackPanel>
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/>
    </StackPanel>
</Page>
```

**SecondPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.SecondPage" ...>
    <StackPanel>
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" />
    </StackPanel>
</Page>
```

Para deixar esse exemplo mais eficiente, mova `SecondPageTextBrush` para _SecondPage.xaml_ e mova `ThirdPageTextBrush` para _ThirdPage.xaml_. `InitialPageTextBrush` pode permanecer em _App.xaml_ porque é necessário analisar os recursos de aplicativo na inicialização do aplicativo em qualquer caso.

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>Consolide vários pinceis com a mesma aparência em um recurso

A plataforma XAML tenta armazenar objetos comumente usados em cache de forma que eles possam ser reutilizados com mais frequência possível. Porém, o XAML não consegue dizer facilmente se um pincel declarado em um pedaço de marcação é o mesmo que um pincel declarado em outro. O exemplo aqui usa [SolidColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) para demonstrar, mas o caso é mais provável e mais importante com [GradientBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientBrush). Além disso, procure por pincéis que usam cores pré-definidas; por exemplo, `"Orange"` e `"#FFFFA500"` são a mesma cor.

**Ineficiente.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBlock>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

Para corrigir a duplicação, defina o pincel como um recurso. Se os controles em outras páginas usarem o mesmo pincel, mova-os para _App.xaml_.

**Eficiente.**

```xaml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>

    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## <a name="minimize-overdrawing"></a>Minimizar o excesso de desenho

O excesso de desenho ocorre quando mais de um objeto é desenhado nos mesmos pixels da tela. Às vezes, há um compromisso entre essa orientação e o desejo de minimizar a contagem de elementos.

Use [**DebugSettings.IsOverdrawHeatMapEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.debugsettings.isoverdrawheatmapenabled) como um diagnóstico visual. Pode ser que você encontre objetos sendo desenhados que não sabia que estavam na cena.

### <a name="transparent-or-hidden-elements"></a>Elementos transparentes ou ocultos

Se um elemento não está visível porque é transparente ou está escondido atrás de outros elementos e não contribui para o layout, exclua-o. Se o elemento não estiver visível no estado visual inicial, mas estiver visível em outros estados visuais, use x:Load para controlar seu estado ou defina [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) como **Collapsed** no próprio elemento e altere o valor para **Visible** nos estados adequados. Haverá exceções a essa heurística: em geral, o valor que uma propriedade tem na maioria dos estados visuais é melhor definido localmente no elemento.

### <a name="composite-elements"></a>Elementos compostos

Use um elemento composto em vez de mostrar vários elementos em camadas para criar um efeito. Neste exemplo, o resultado é uma forma de dois tons em que a metade superior é preta (da tela de fundo do [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)) e a metade inferior é cinza (do [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) branco semitransparente em combinação alfa sobre a tela de fundo preta do **Grid**). Aqui, 150% dos pixels necessários para obter o resultado estão sendo preenchidos.

**Ineficiente.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Black">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/>
</Grid>
```

**Eficiente.**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Fill="Black"/>
    <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
</Grid>
```

### <a name="layout-panels"></a>Painéis de layout

Um painel de layout pode ter duas finalidades: colorir uma área e definir o layout dos elementos filho. Se um elemento adicional na ordem z já está colorindo uma área, então um painel de layout na frente não precisa pintar área: em vez disso, ele pode apenas se concentrar em dispor seus filhos. Aqui está um exemplo.

**Ineficiente.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid Background="Blue"/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

**Eficiente.**

```xaml
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

Se for necessário aplicar teste de clique no [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), defina para ele um valor de tela de fundo transparente.

### <a name="borders"></a>Bordas

Use um elemento [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) para desenhar uma borda em torno de um objeto. Neste exemplo, um [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) é usado como borda provisória em torno de um [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Mas todos os pixels na célula central se excedem.

**Ineficiente.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Blue" Width="300" Height="45">
    <Grid.RowDefinitions>
        <RowDefinition Height="5"/>
        <RowDefinition/>
        <RowDefinition Height="5"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="5"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="5"/>
    </Grid.ColumnDefinitions>
    <TextBox Grid.Row="1" Grid.Column="1"></TextBox>
</Grid>
```

**Eficiente.**

```xaml
 <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
     <TextBox/>
</Border>
```

### <a name="margins"></a>Margens

Fique atento às margens. Dois elementos vizinhos ficarão sobrepostos (possivelmente por acidente) caso margens negativas se estendam para as bordas de outro e causem excesso de desenhos.

### <a name="cache-static-content"></a>Armazenar conteúdo estático em cache

Outra fonte de excesso de desenho é uma forma feita a partir de muitos elementos sobrepostos. Se você definir [CacheMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CacheMode) para **BitmapCache** no [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) que contém a forma composta, a plataforma renderizará o elemento em um bitmap uma vez e, depois, o utilizará a cada quadro, em vez de exceder os desenhos.

**Ineficiente.**

```xaml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![Diagrama de Venn com três círculos sólidos](images/solidvenn.png)

A imagem acima é o resultado, mas este é um mapa das regiões com excesso de desenho. O vermelho mais escuro indica maiores quantidades de desenho em sobreposição.

![Diagrama de Venn que mostra áreas sobrepostas](images/translucentvenn.png)

**Eficiente.**

```xaml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Observe o uso de [CacheMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CacheMode). Não use essa técnica se qualquer uma das subformas for animada, porque o cache de bitmap provavelmente precisará ser regenerado a cada quadro, destruindo o propósito.

## <a name="use-xbf2"></a>Usar o XBF2

XBF2 é uma representação binária da marcação XAML que evita todos os custos da análise de texto em runtime. Ele também otimiza o binário para a criação de árvore e o carregamento, além de proporcionar um "caminho rápido" para que tipos XAML melhorem os custos de criação de objeto e de heap, por exemplo VSM, ResourceDictionary, Styles e assim por diante. Ele é totalmente mapeado por memória de forma que não haja uma volume de heap no carregamento e leitura de uma página XAML. Além disso, ele reduz o volume de disco das páginas XAML armazenadas em um appx. XBF2 é uma representação mais compacta e pode reduzir o volume de disco de arquivos XAML/XBF1 comparativos em até 50%. Por exemplo, o aplicativo Fotos integrado experimentou uma redução em torno de 60% após a conversão para XBF2, baixando de cerca de ~ 1mb de ativos XBF1 para ~ 400kb de ativos XBF2. Também vimos aplicativos se beneficiarem entre 15 e 20% em termos de uso de CPU e 10 e 15% em relação ao heap do Win32.

Dicionários e controles XAML internos que a estrutura fornece já são totalmente habilitados para XBF2. Para seu próprio aplicativo, assegure-se de que o arquivo de projeto declare TargetPlatformVersion 8.2 ou posterior.

Para verificar se você tem o XBF2, abra seu aplicativo em um editor binário; o 12º e o 13º bytes são 00 02 se você tiver o XBF2.

## <a name="related-articles"></a>Artigos relacionados

- [Práticas recomendadas para o desempenho inicial de seu aplicativo](best-practices-for-your-app-s-startup-performance.md)
- [Otimizar o layout XAML](optimize-your-xaml-layout.md)
- [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md)
- [Ferramentas de criação de perfil e de desempenho](tools-for-profiling-and-performance.md)
