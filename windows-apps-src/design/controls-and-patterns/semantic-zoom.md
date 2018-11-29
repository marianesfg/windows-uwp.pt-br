---
Description: A semantic zoom control allows the user to zoom between two different semantic views of the same data set.
title: Zoom semântico
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 65cf8afbb423c72b9960b58a1b80dc3477f3a5fc
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194896"
---
# <a name="semantic-zoom"></a>Zoom semântico

 

Zoom Semântico permite que o usuário alterne duas exibições diferentes do mesmo conteúdo, de maneira que possa navegar rapidamente em um grande conjunto de dados agrupados.
 
- A exibição ampliada é o modo de exibição principal do conteúdo. Essa é a exibição principal na qual você mostra itens de dados individuais. 
- A exibição reduzida é um modo de exibição de alto nível do mesmo conteúdo. Normalmente, você mostra os cabeçalhos de grupo para um conjunto de dados agrupado nesse modo de exibição. 

Por exemplo, ao exibir um catálogo de endereços, o usuário pode reduzir a fim de ir rapidamente para a letra "W" e ampliar essa letra para ver os nomes associados a ela. 

> **APIs importantes**: [classe SemanticZoom](https://msdn.microsoft.com/library/windows/apps/hh702601), [classe ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx)

**Recursos**:

-   O tamanho da exibição reduzida é restrito pelos limites do controle de zoom semântico.
-   Ao tocar em um cabeçalho de grupo, os modos de exibição são alternados. É possível habilitar a pinçagem para alternar entre os modos de exibição.
-   Os cabeçalhos ativos alternam entre os modos de exibição.

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um controle **SemanticZoom** quando precisar mostrar um conjunto de dados agrupado que seja suficientemente grande para que ele não possa ser todo mostrado em uma ou duas páginas.

Não confunda zoom semântico com zoom óptico. Apesar deles compartilharem a mesma interação e o mesmo comportamento básico (exibindo mais ou menos detalhes baseados em um fator de zoom), o zoom óptico se refere ao ajuste da ampliação para uma área de conteúdo ou objeto como uma fotografia. Para obter informações sobre um controle que executa ampliação óptica, veja o controle [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/SemanticZoom">abrir o aplicativo e ver o SemanticZoom em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Aplicativo Fotos**

Eis um zoom semântico usado no aplicativo Fotos. As fotos são agrupadas por mês. A seleção do cabeçalho de um mês na exibição em grade padrão é reduzida para a exibição de lista do mês para uma navegação mais rápida.

![Um zoom semântico usado no aplicativo Fotos](images/control-examples/semantic-zoom-photos.png)

**Catálogo de endereços**

Um catálogo de endereços é outro exemplo de conjunto de dados que pode ser muito mais fácil de navegar com um zoom semântico. Você pode usar a exibição reduzida para ir rapidamente até a letra de que precisa (imagem à esquerda), e a exibição ampliada mostra os itens de dados individuais (imagem à direita).

![exemplo de zoom semântico usado em uma lista de contatos](images/semanticzoom-win10.png)

## <a name="create-a-semantic-zoom"></a>Criar um Zoom Semântico

O controle **SemanticZoom** não tem nenhuma representação visual dele próprio. Ele é um controle de host que gerencia a transição entre 2 outros controles que fornecem as exibição do conteúdo, normalmente os controles **ListView** ou **GridView**.  Você define os controles de exibição como as propriedades [ZoomedInView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedinview.aspx) e [ZoomedOutView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedoutview.aspx) do SemanticZoom.

Os 3 elementos de que você precisa para um Zoom Semântico são:
- Uma fonte de dados agrupados
- Uma exibição ampliada que mostre os dados no nível do item.
- Uma exibição reduzida que mostre os dados no nível do grupo.

Antes de usar um Zoom Semântico, você deve entender como usar uma exibição de lista com dados agrupados. Para obter mais informações, consulte [Exibição de lista e exibição em grade](listview-and-gridview.md) e [Agrupamento de itens em uma lista](). 

> **Observação**&nbsp;&nbsp;Para definir as exibições ampliada e reduzida do controle SemanticZoom, você pode usar qualquer um dos controles que implementam a interface [ISemanticZoomInformation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.isemanticzoominformation.aspx). A estrutura XAML fornece 3 controles que implementam essa interface: ListView, GridView e Hub.
 
 Esta XAML mostra a estrutura do controle SemanticZoom. Você atribui outros os controles às propriedades ZoomedInView e ZoomedOutView.
 
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed in view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed out view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
Os exemplos aqui foram tirados da página SemanticZoom da [Amostra de noções básicas de interface do usuário XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992). Você pode baixar a amostra para ver o código completo, inclusive a origem de dados. O Zoom Semântico usa um GridView para fornecer a exibição ampliada e um ListView para a exibição reduzida.
  
**Definir a exibição ampliada**

Aqui está o controle GridView para a exibição ampliada. A exibição ampliada deve mostrar os itens de dados individuais em grupos. Este exemplo mostra como exibir os itens em uma grade com uma imagem e um texto. 

```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
A aparência dos cabeçalhos de grupo é definida no recurso `ZoomedInGroupHeaderTemplate`. A aparência dos itens é definida no recurso `ZoomedInTemplate`. 

```xaml
<DataTemplate x:Key="ZoomedInGroupHeaderTemplate" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**Definir a exibição reduzida**

Esta XAML define um controle ListView para a exibição reduzida. Este exemplo mostra como exibir os cabeçalhos de grupo como texto em uma lista.

```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 A aparência é definida no recurso `ZoomedOutTemplate`.
 
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**Sincronizar as exibições**

As exibições ampliadas e reduzidas devem ser sincronizadas, de forma que se um usuário selecionar um grupo na exibição reduzida, os detalhes desse mesmo grupo sejam mostrados na exibição ampliada. Você pode usar um [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) ou adicionar código para sincronizar as exibições.

Quaisquer controles associados ao mesmo CollectionViewSource sempre terão o mesmo item atual. Se as duas exibições usarem o mesmo CollectionViewSource como fonte de dados, o CollectionViewSource sincronizará as exibições automaticamente. Para saber mais, consulte a [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

Se você não usar um CollectionViewSource para sincronizar as exibições, será preciso manipular o evento [ViewChangeStarted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.viewchangestarted.aspx) e sincronizar os itens no manipulador de eventos como esse, conforme mostrado aqui.

```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## <a name="recommendations"></a>Recomendações

-   Ao usar o zoom semântico em seu aplicativo, o layout do item e a direção do movimento panorâmico não devem mudar de acordo com o nível de zoom. Os layouts e as interações de movimento panorâmico devem ser consistentes e previsíveis em todos os níveis de zoom.
-   Como o zoom semântico, o usuário pode saltar rapidamente para o conteúdo. Então, limite o número de páginas/telas a três no modo com zoom reduzido. Muito movimento panorâmico diminui a viabilidade do zoom semântico.
-   Evite usar o zoom semântico para alterar o escopo do conteúdo. Por exemplo, um álbum de fotos não deve alternar para um modo de exibição de pasta no Gerenciador de Arquivos.
-   Use uma estrutura e a semântica essenciais ao modo de exibição.
-   Use os nomes dos grupos para os itens em uma coleção agrupada.
-   Use ordens de classificação para uma coleção desagrupada, porém classificada; por exemplo, a ordem cronológica para datas e ordem alfabética para uma lista de nomes.


## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.


## <a name="related-articles"></a>Artigos relacionados

- [Noções básicas de design de navegação](../basics/navigation-basics.md)
- [Exibição de lista e exibição de grade](listview-and-gridview.md)
- [Contêineres e modelos de itens](item-containers-templates.md)





