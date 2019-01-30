---
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Controles de submenu
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 52de0933bf51adaae6b0923868e12eb92ced4a1a
ms.sourcegitcommit: a60ab85e9f2f9690e0141050ec3aa51f18ec61ec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2019
ms.locfileid: "9037128"
---
# <a name="flyouts"></a>Submenus

Um submenu é um contêiner de ignorar aberto que pode mostrar uma interface do usuário arbitrária como conteúdo. Os submenus podem conter outros submenus ou menus de contexto para criar uma experiência aninhada.

![Menu de contexto aninhado em um submenu](../images/flyout-nested.png)

> **APIs importantes**: [classe Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

* Não use um submenu no lugar de [tooltip](../tooltips.md) ou [menu de contexto](../menus.md). Use tooltip para mostrar uma breve descrição que fica oculta depois de um determinado tempo. Use um menu de contexto para ações contextuais relacionadas a um elemento da interface do usuário, como copiar e colar.

Para obter recomendações sobre quando usar um submenu versus quando usar uma caixa de diálogo (um controle semelhante), consulte [as caixas de diálogo e submenus](index.md). 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

##  <a name="how-to-create-a-flyout"></a>Como criar um submenu


Submenus são anexados a controles específicos. Você pode usar a propriedade [Placement](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) para especificar onde um submenu aparece: parte superior, esquerda, inferior, direita ou inteira. Se você selecionar o [Modo de posicionamento completo](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode), o aplicativo se estende o submenu e centraliza na janela do aplicativo. Alguns controles, como [Button](/uwp/api/Windows.UI.Xaml.Controls.Button), fornecem uma propriedade [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) que você pode usar para associar a um submenu ou um [menu de contexto](../menus.md).

Este exemplo cria um submenu simples que exibe algum texto quando o botão é pressionado.
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Se o controle não tem uma propriedade flyout, você pode usar a propriedade [Flyoutbase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) anexada em vez disso. Quando você fizer isso, você também precisará chamar o método [Showattachedflyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) para mostrar o submenu.

Este exemplo adiciona um submenu simples a uma imagem. Quando o usuário toca na imagem, o aplicativo mostra o submenu.

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

Os exemplos anteriores definiram seus submenus embutido. Você pode também definir um submenu como um recurso estático e, em seguida, usá-lo com vários elementos. Este exemplo cria um submenu mais complicado que exibe uma versão maior de uma imagem quando sua miniatura é tocada.

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

## <a name="style-a-flyout"></a>Dê estilo ao submenu
Para estilizar um submenu, modifique o [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle). Este exemplo mostra um parágrafo de quebra de texto e torna o bloco de texto acessível para um leitor de tela.

![Submenu acessível com disposição de texto](../images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## <a name="styling-flyouts-for-10-foot-experiences"></a>Estilo de submenus para experiências de 10 pés

Ignore rapidamente os controles, como teclado de interceptação de submenu e foco de gamepad na interface do usuário transitória, até serem ignorados. Para fornecer uma indicação visual desse comportamento, ao ignorar rapidamente os controles de ignorar no Xbox para desenhar uma sobreposição que esmaece o contraste e visibilidade da interface do usuário fora do escopo. Esse comportamento pode ser modificado com a propriedade [`LightDismissOverlayMode`](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode). Por padrão, os submenus desenham a sobreposição de ignorar rapidamente no Xbox, mas não em outras famílias de dispositivos, mas os aplicativos podem optar por forçar a sobreposição para estar sempre **Ativada** ou **Desativada**.

![Submenu com sobreposição de esmaecimento](../images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

## <a name="light-dismiss-behavior"></a>Ignorar rapidamente comportamento
Submenus podem ser fechados com uma ação de ignorar rápida, incluindo
-   Tocar fora do submenu
-   Pressionar a tecla Escape no teclado
-   Pressionar o botão de Voltar do sistema de hardware ou software
-   Pressionar o botão do gamepad B

Quando ignorar com um toque, esse gesto é normalmente absorvido e não passado para a interface do usuário abaixo. Por exemplo, se houver um botão visível por trás de um submenu aberto, o primeiro toque do usuário fecha o submenu, mas não ativa esse botão. Pressionar o botão requer um segundo toque.

Você pode alterar esse comportamento ao especificar o botão como um elemento de entrada de passagem para o submenu. O submenu fechará como resultado da ação de ignorar descrita acima e passará o evento de toque para o `OverlayInputPassThroughElement` designado. Considere adotar esse comportamento para acelerar as interações do usuário em itens com funcionalidade semelhante. Se o aplicativo tem uma coleção de favoritos e cada item na coleção inclui um submenu anexado, é razoável que os usuários possam querer interagir com diversos submenus em sucessão rápida.

[!NOTE] Tenha cuidado para não designar um sobreposição de elemento de passagem de entrada que resulta em uma ação destrutiva. Os usuários se acostumaram às ações de ignorar rápidas que não ativam a interface do usuário principal. Fechar, Excluir ou botões destrutivos semelhantes não devem ser ativados ao ignorar rapidamente para evitar o comportamento inesperado e interrupções.

No exemplo a seguir, todos os três botões dentro FavoritesBar serão ativados no primeiro toque.

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados
- [Dicas de ferramenta](../tooltips.md)
- [Menus e menu de contexto](../menus.md)
- [Classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)