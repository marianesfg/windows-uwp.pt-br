---
Description: Modelos de item para visualização de grade
title: Modelos de item para visualização de grade
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 1e2c8b7d9fb7bdc61595296a137c4448cadf52d3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629261"
---
# <a name="item-templates-for-grid-view"></a>Modelos de item para visualização de grade

Esta seção contém modelos de item que você pode usar com um controle [**GridView**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.GridView). Use esses modelos para obter a aparência de tipos de app comuns.

Para demonstrar a ligação de dados, esses modelos se associar **GridViewItems** para a classe de gravação de exemplo do [visão geral da vinculação de dados](../../data-binding/data-binding-quickstart.md).

> [!NOTE] 
No momento, quando um **DataTemplate** contém vários controles (por exemplo, mais de um único **TextBlock**), o nome acessível padrão para leitores de tela é proveniente de .ToString () no item. Para fins de praticidade, você pode definir o [**AutomationProperties.Name**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.automationproperties) no elemento raiz de **DataTemplate**. Para obter mais informações sobre acessibilidade, consulte [Visão geral de acessibilidade](../accessibility/accessibility-overview.md).

## <a name="icon-and-text"></a>Ícone e texto
Use esses modelos para exibir uma coleção de apps em uma grade com ícone e texto.

![exemplo de gridview pequena de ícone e texto](images/listitems/icontext.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="IconTextTemplate" x:DataType="local:Recording">
            <StackPanel Width="264" Height="48" Padding="12" Orientation="Horizontal" AutomationProperties.Name="{x:Bind CompositionName}">
                <SymbolIcon Symbol="Audio" VerticalAlignment="Top"/>
                <TextBlock Margin="12,0,0,0" Width="204" Text="{x:Bind CompositionName}"/>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal" HorizontalAlignment="Center"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

![exemplo de gridview de ícone e texto de linha dupla](images/listitems/icontext2.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="IconTextTemplate2" x:DataType="local:Recording">
            <StackPanel Width="264" Height="120" Padding="12" Orientation="Horizontal" AutomationProperties.Name="{x:Bind CompositionName}">
                <FontIcon Margin="0,6,0,0" FontSize="48" FontFamily="Segoe MDL2 Assets" FontWeight="Bold" Glyph="&#xE8D6;" VerticalAlignment="Top"/>
                <StackPanel Margin="16,1,0,0">
                    <TextBlock Width="176" Margin="0,0,0,2" TextWrapping="WrapWholeWords" TextTrimming="Clip" Text="{x:Bind CompositionName}"/>
                    <TextBlock Width="176" Height="48" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}" TextWrapping="WrapWholeWords" TextTrimming="Clip" Text="{x:Bind ArtistName}" />
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal" HorizontalAlignment="Center" Margin="40,0"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="image-gallery"></a>Galeria de imagens
Use esse modelo para exibir uma coleção de imagens em uma grade com modo de seleção múltipla.

![layout de itens de gridview](images/listitems/gridviewitems.png)
```xaml
<GridView SelectionMode="Multiple">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageGalleryDataTemplate">
            <Image Source="Placeholder.png" Height="180" Width="180" Stretch="UniformToFill"/>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="3" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```
## <a name="image-and-text"></a>Imagem e texto
Use esses modelos para exibir uma coleção de mídias com texto abaixo.

![exemplo de gridview de imagem quadrada e texto](images/listitems/imageandtext.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageTextDataTemplate" x:DataType="local:Recording">
            <StackPanel Height="280" Width="180" Margin="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="180" Width="180" Stretch="UniformToFill"/>
                <StackPanel Margin="0,12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="10" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

![exemplo de gridview de imagem retangular e texto](images/listitems/imageandtext2.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageTextDataTemplate2" x:DataType="local:Recording">
            <StackPanel Height="280" Width="320" Margin="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="180" Width="320" Stretch="UniformToFill"/>
                <StackPanel Margin="0,12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="image-with-text-overlay"></a>Imagem com sobreposição de texto
Use esse modelo para exibir uma coleção de mídias com sobreposição de texto.

![Exemplo de gridview de imagem e sobreposição de texto](images/listitems/imageoverlay.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageOverlayDataTemplate" x:DataType="local:Recording">
            <Grid Height="180" Width="320" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Stretch="UniformToFill"/>
                <StackPanel Orientation="Vertical" Height="60" VerticalAlignment="Bottom" Background="{ThemeResource SystemBaseLowColor}" Padding="12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="related-articles"></a>Artigos relacionados
- [Classe de GridView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [Visão geral da vinculação de dados](../../data-binding/data-binding-quickstart.md)
- [Visão geral de Accessibililty](../accessibility/accessibility-overview.md)
- [Exemplo de ListView e GridView (Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Imagens em miniatura](../../files/thumbnails.md)