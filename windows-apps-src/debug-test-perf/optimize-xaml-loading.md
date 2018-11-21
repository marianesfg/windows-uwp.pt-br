---
author: jwmsft
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Otimizar sua marcação XAML
description: Analisar a marcação XAML para construir objetos na memória é demorado para uma interface do usuário complexa. Aqui está o que você pode fazer para melhorar a análise de marcação XAML, o tempo de carregamento e a eficiência de memória para seu app.
ms.author: jimwalk
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 884825f2e9639f620d8db4e6110791fddf2d7e77
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7428127"
---
# <a name="optimize-your-xaml-markup"></a>Otimizar sua marcação XAML


Analisar a marcação XAML para construir objetos na memória é demorado para uma interface do usuário complexa. Eis o que você pode fazer para melhorar a análise e o tempo de carregamento de sua marcação XAML, bem como a eficiência de memória do seu aplicativo.

Na inicialização do aplicativo, limite a marcação XAML que é carregada para apenas o que você precisa para sua interface do usuário inicial. Examine a marcação na sua página inicial (incluindo recursos da página) e confirme se você não está carregando elementos extras que não são necessários imediatamente. Esses elementos podem vir de uma variedade de fontes, como dicionários de recursos, elementos que são inicialmente recolhidos e elementos desenhados sobre outros elementos.

A otimização de sua XAML para obter eficiência requer a realização de compensações; nem sempre há uma única solução para cada situação. Aqui vemos alguns problemas comuns e fornecemos orientações que podem ser usadas para fazer as compensações corretas para o seu aplicativo.

## <a name="minimize-element-count"></a>Minimizar a contagem de elementos

Embora a plataforma XAML seja capaz de exibir grandes quantidades de elementos, você pode fazer com que a disposição e a renderização de seu aplicativo sejam mais rápidas, usando a menor quantidade de elementos para alcançar os elementos visuais desejados.

As opções que você faz no modo como define seus controles de interface de usuário afetarão o número de elementos de interface do usuário que são criados quando seu aplicativo é iniciado. Para obter informações detalhadas sobre otimização de layout, consulte [Otimizar seu layout XAML](optimize-your-xaml-layout.md).

A quantidade de elementos é extremamente importante em modelos de dados porque cada elemento é criado novamente para cada item de dados. Para obter informações sobre redução de quantidade de elementos em uma lista ou grade, consulte *Redução de elemento por item* no artigo [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md).

Aqui vemos algumas outras formas de reduzir o número de elementos que seu aplicativo precisa carregar na inicialização.

### <a name="defer-item-creation"></a>Adiar a criação do item

Se a sua marcação XAML contiver elementos que não são imediatamente mostrados, é possível adiar o carregamento desses elementos até que sejam mostrados. Por exemplo, é possível adiar a criação de conteúdo não visível, como uma guia secundária em uma interface de usuário em forma de guias. Ou você pode mostrar itens em uma exibição de grade por padrão, fornecendo, porém, uma opção para o usuário exibir os dados em uma lista em vez disso. É possível adiar o carregamento da lista até que ela seja necessária.

Use o [atributo x:Load](../xaml-platform/x-load-attribute.md) em vez da propriedade [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.Visibility) para controlar quando um elemento é mostrado. Quando a visibilidade de um elemento é definida como **Collapsed**, ele é ignorado durante a passagem de renderização, mas você ainda paga os custos da instância do objeto na memória. Ao usar x:Load em vez disso, a estrutura não cria a instância do objeto até que ela seja necessária e, portanto, os custos de memória são ainda menores. A desvantagem é que você paga por uma pequena sobrecarga de memória (cerca de 600 bytes) quando a interface do usuário não é carregada.

> [!NOTE]
> Você pode atrasar o carregamento de elementos usando o atributo [x:Load](../xaml-platform/x-load-attribute.md) ou [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md). O atributo x:Load está disponível a partir da Atualização do Windows 10 para Criadores (versão 1703, compilação do SDK 15063). A versão mínima segmentada pelo seu projeto Visual Studio deve ser *Atualização do Windows 10 para Criadores (10.0, compilação 15063)* para usar x:Load. Para direcionar versões anteriores, use x:DeferLoadStrategy.

Os seguintes exemplos mostram a diferença na quantidade de elementos e no uso da memória quando técnicas diferentes são usadas para ocultar os elementos de interface de usuário. Um ListView e um GridView contendo itens idênticos são colocados em uma raiz da página Grade. O ListView não é visível, mas o GridView é mostrado. A XAML em cada um desses exemplos produz a mesma interface de usuário na tela. Usamos [ferramentas de criação de perfil e desempenho](tools-for-profiling-and-performance.md) do Visual Studio para verificar a quantidade de elementos e o uso de memória.

#### <a name="option-1---inefficient"></a>Opção 1 - Ineficiente

Aqui o ListView é carregado, mas não é visível porque sua largura é 0. O ListView e cada um de seu elementos filho é criado na árvore visual e carregado na memória.

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

Live Visual Tree com o ListView carregado. A quantidade total de elementos para a página é 89.

![Árvore visual com exibição de lista](images/visual-tree-1.png)

O ListView e seus filhos são carregados na memória.

![Árvore visual com exibição de lista](images/memory-use-1.png)

#### <a name="option-2---better"></a>Opção 2 - Melhor

Aqui a visibilidade da ListView é definida como recolhida (a outra XAML é idêntica à original). O ListView é criado na árvore visual, mas seus elementos filho não o são. No entanto, eles são carregados na memória. Portanto, o uso de memória é idêntico ao exemplo anterior.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

Live Visual Tree com o ListView recolhido. A quantidade total de elementos para a página é 46.

![Árvore visual com exibição de lista recolhida](images/visual-tree-2.png)

O ListView e seus filhos são carregados na memória.

![Árvore visual com exibição de lista](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>Opção 3 - Mais eficiente

Aqui o ListView tem o atributo x:Load definido como **Falso** (a outra XAML é idêntica à original). O ListView não é criado na árvore visual ou carregado na memória na inicialização.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

Live Visual Tree com o ListView não carregado. A quantidade total de elementos para a página é 45.

![Árvore visual com exibição de lista não carregada](images/visual-tree-3.png)

O ListView e seus filhos não são carregados na memória.

![Árvore visual com exibição de lista](images/memory-use-3.png)

> [!NOTE]
> As quantidades de elementos e o uso de memória nesses exemplos são muito pequenos e mostrados apenas para demonstrar o conceito. Nesses exemplos, como a sobrecarga ao usar x:Load é maior que a economia de memória, o aplicativo não se beneficiaria. Você deve usar as ferramentas de criação de perfil no seu aplicativo para determinar se este se beneficiará ou não com o carregamento adiado.

### <a name="use-layout-panel-properties"></a>Usar propriedades de painel de layout

Como os painéis de layout têm uma propriedade [Background](https://msdn.microsoft.com/library/windows/apps/BR227512) não é preciso puxar um [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) na frente de um painel apenas para colori-lo.

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

Os painéis de layout também têm propriedades de borda internas. Por isso, não é necessário colocar um elemento [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) ao redor do painel de layout. Consulte [Otimizar seu layout XAML](optimize-your-xaml-layout.md) para obter mais informações e exemplos.

### <a name="use-images-in-place-of-vector-based-elements"></a>Usar imagens no lugar de elementos baseados em vetor

Se você reutilizar bastante o mesmo elemento baseado em vetor, torna-se mais eficiente usar um elemento [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) em vez disso. Os elementos baseados em vetor podem ser mais caros porque a CPU precisa criar cada elemento individual separadamente. O arquivo de imagem precisa ser decodificado apenas uma vez.

## <a name="optimize-resources-and-resource-dictionaries"></a>Otimizar recursos e dicionários de recursos

Normalmente, você usa [dicionários de recursos](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md) para armazenar, em um nível um pouco global, recursos aos quais você deseja fazer referência em vários lugares no seu aplicativo. Por exemplo, estilos, pincéis, modelos e assim por diante.

No geral, otimizamos [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) para não instanciar recursos, a menos que seja solicitado. No entanto, não há situações que devem ser evitadas para que os recursos não sejam desnecessariamente instanciados.

### <a name="resources-with-xname"></a>Recursos com x:Name

Use o [atributo x:Key](../xaml-platform/x-key-attribute.md) para referenciar seus recursos. Os recursos com o [atributo x:Name](../xaml-platform/x-name-attribute.md) não se beneficiarão com a otimização da plataforma; em vez disso, eles serão instanciados assim que ResourceDictionary for criado. Isso ocorre porque x:Name instrui a plataforma de que seu aplicativo precisa de acesso de campo a esse recurso de forma que a plataforma precise criar algo para o qual irá criar um referência.

### <a name="resourcedictionary-in-a-usercontrol"></a>ResourceDictionary em um UserControl

Um ResourceDictionary definido dentro de um [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) provoca uma penalidade. A plataforma cria uma cópia de tal ResourceDictionary para cada instância do UserControl. Se você tiver um UserControl que é muito usado, mova ResourceDictionary do UserControl e coloque-o no nível de página.

### <a name="resource-and-resourcedictionary-scope"></a>Escopo de recurso e ResourceDictionary

Se a página fizer referência a um controle de usuário ou um recurso definido em um arquivo diferente, a estrutura analisará esse arquivo também.

Aqui, como _InitialPage.xaml_ usa um recurso de _ExampleResourceDictionary.xaml_, todo o _ExampleResourceDictionary.xaml_ deve ser analisado na inicialização.

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

Se você usar um recurso em várias páginas em todo o aplicativo, armazenando-o em _App.xaml_ é uma boa prática e evita a duplicação. Mas _App.xaml_ é analisado na inicialização do aplicativo para que qualquer recurso usado em apenas uma página (a menos que essa página seja a página inicial) seja colocado nos recursos locais da página. Este exemplo mostra um _App.xaml_ que contém recursos que são usados por apenas uma página (que não é a página inicial). Isso aumenta desnecessariamente o tempo de inicialização do aplicativo.

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

Para tornar esse exemplo mais eficiente, mova `SecondPageTextBrush` para _SecondPage.xaml_ e mova `ThirdPageTextBrush` para _ThirdPage.xaml_. `InitialPageTextBrush` pode permanecer em _App.xaml_ porque os recursos de aplicação devem ser analisados na inicialização do aplicativo de qualquer forma.

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>Consolide vários pinceis com a mesma aparência em um recurso

A plataforma XAML tenta armazenar objetos comumente usados em cache de forma que eles possam ser reutilizados com mais frequência possível. Porém, o XAML não consegue dizer facilmente se um pincel declarado em um pedaço de marcação é o mesmo que um pincel declarado em outro. O exemplo aqui usa [SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962) para demonstrar, mas o caso é mais provável e mais importante com [GradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210068). Além disso, procure por pinceis que usam cores pré-definidas; por exemplo, `"Orange"` e `"#FFFFA500"` são a mesma cor.

**Ineficiente.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBox>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

Para corrigir a duplicação, defina o pincel como um recurso. Se os controles em outras páginas usam o mesmo pincel, mova-o para _App.xaml_.

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

O excesso de desenho ocorre quando mais de um objeto é desenhado nos mesmos pixels de tela. Às vezes, há um compromisso entre essa orientação e o desejo de minimizar a contagem de elementos.

Use [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) como um diagnóstico visual. Você pode encontrar objetos sendo desenhados que você não sabia que estavam na cena.

### <a name="transparent-or-hidden-elements"></a>Elementos transparentes ou ocultos

Se um elemento não está visível porque é transparente ou está escondido atrás de outros elementos e não contribui para o layout, exclua-o. Se o elemento não estiver visível no estado visual inicial, mas se estiver visível em outros estados visuais, então use x:Load para controlar seu estado ou defina [Visibility](https://msdn.microsoft.com/library/windows/apps/BR208992) como **Collapsed** no próprio elemento e altere o valor para **Visible** nos estados adequados. Haverá exceções a essa heurística: em geral, o valor que uma propriedade possui na maioria dos estados visuais é melhor definida localmente no elemento.

### <a name="composite-elements"></a>Elementos compostos

Use um elemento composto em vez de mostrar vários elementos em camadas para criar um efeito. Nesse exemplo, o resultado é uma forma de dois tons em que a metade superior é preta (do plano de fundo do [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704)) e a metade inferior é cinza (do [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) branco semitransparente em combinação alfa sobre o plano de fundo preto do **Grid**). Aqui, 150% dos pixels necessários para obter o resultado estão sendo preenchidos.

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

Se o [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) precisa ser testado para ocorrências, defina um valor de plano de fundo transparente nele.

### <a name="borders"></a>Bordas

Use um elemento [Border](https://msdn.microsoft.com/library/windows/apps/BR209253) para desenhar uma borda em torno de um objeto. Nesse exemplo, um [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) é usado como uma borda provisória em torno de um [TextBox](https://msdn.microsoft.com/library/windows/apps/BR209683). Mas todos os pixels na célula central se excedem.

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

Outra fonte de excesso de desenho é uma forma feita a partir de muitos elementos sobrepostos. Se você definir [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084) para **BitmapCache** no [UIElement](https://msdn.microsoft.com/library/windows/apps/BR208911) que contém a forma composta, então a plataforma renderiza o elemento em um bitmap uma vez e, depois, o utiliza a cada quadro, em vez de exceder os desenhos.

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

Observe o uso de [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084). Não use essa técnica se qualquer uma das subformas for animada, porque o cache de bitmap provavelmente precisará ser regenerado a cada quadro, destruindo o propósito.

## <a name="use-xbf2"></a>Usar o XBF2

XBF2 é uma representação binária da marcação XAML que evita todos os custos da análise de texto em tempo de execução. Ele também otimiza o binário para a criação de árvore e o carregamento, além de proporcionar um "caminho rápido" para que tipos XAML melhorem os custos de criação de objeto e de heap, por exemplo VSM, ResourceDictionary, Styles e assim por diante. Ele é totalmente mapeado por memória de forma que não haja uma volume de heap no carregamento e leitura de uma página XAML. Além disso, ele reduz o volume de disco das páginas XAML armazenadas em um appx. XBF2 é uma representação mais compacta e pode reduzir o volume de disco de arquivos XAML/XBF1 comparativos em até 50%. Por exemplo, o aplicativo Fotos integrado experimentou uma redução em torno de 60% após a conversão para XBF2, baixando de cerca de ~ 1mb de ativos XBF1 para ~ 400kb de ativos XBF2. Também vimos aplicativos se beneficiarem entre 15 e 20% em termos de uso de CPU e 10 e 15% em relação ao heap do Win32.

Dicionários e controles XAML internos que a estrutura fornece já são totalmente habilitados para XBF2. Para seu próprio aplicativo, assegure-se de que o arquivo de projeto declare TargetPlatformVersion 8.2 ou posterior.

Para verificar se você tem o XBF2, abra seu aplicativo em um editor binário; o 12º e o 13º bytes são 00 02 se você tiver o XBF2.

## <a name="related-articles"></a>Artigos relacionados

- [Práticas recomendadas para o desempenho inicial de seu app](best-practices-for-your-app-s-startup-performance.md)
- [Otimizar seu layout XAML](optimize-your-xaml-layout.md)
- [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md)
- [Ferramentas para criação de perfil e desempenho](tools-for-profiling-and-performance.md)
