---
author: mijacobs
description: As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente através da animação da transição de um elemento entre duas exibições diferentes.
title: Animação conectada
template: detail.hbs
ms.author: jimwalk
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a789a8f082192b79b3e96990827f9a4f6a0eacbc
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3847660"
---
# <a name="connected-animation-for-uwp-apps"></a>Animação conectada para aplicativos UWP

## <a name="what-is-connected-animation"></a>O que é animação conectada?

As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente através da animação da transição de um elemento entre duas exibições diferentes. Isso ajuda o usuário a manter o contexto e oferece continuidade entre os modos de exibição.
Em uma animação conectada, um elemento parece "continuar" entre duas exibições durante uma alteração no conteúdo da interface de usuário, voando pela tela desde a sua localização na exibição de origem até seu destino na nova exibição. Isso enfatiza o conteúdo comum entre os modos de exibição e cria um efeito belo e dinâmico como parte de uma transição.

## <a name="see-it-in-action"></a>Veja em ação

Neste vídeo curto, um aplicativo usa uma animação conectada para animar uma imagem conforme ela "continua" a fazer parte do cabeçalho da próxima página. O efeito ajuda a manter o contexto de usuário entre a transição.

![Exemplo de interface do usuário de movimento contínuo](images/continuous3.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animação conectada e o Sistema de Design Fluent

 O Sistema de Design Fluent ajuda você a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. Animação conectada é um componente do Sistema de Design Fluent que acrescenta movimento ao seu aplicativo. Para saber mais, consulte a [visão geral do Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="why-connected-animation"></a>Por que animação conectada?

Ao navegar entre páginas, é importante para o usuário entender qual novo conteúdo está sendo apresentado após a navegação e como isso se relaciona com sua intenção ao navegar. As animações conectadas fornecem uma metáfora visual poderosa que enfatiza a relação entre dois modos de exibição, chamando a atenção do usuário para o conteúdo compartilhado entre eles. Além disso, animações conectadas adicionam brilho e interesse visual à navegação da página, o que pode ajudar a diferenciar o design de movimento do seu aplicativo.

## <a name="when-to-use-connected-animation"></a>Quando usar a animação conectada

As animações conectadas geralmente são usadas na troca de páginas, embora possam ser aplicadas em qualquer experiência onde ocorra troca de conteúdo na interface de usuário e o usuário deva manter o contexto. Você deve considerar usar uma animação conectada em vez de um [furo na transição da navegação](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) sempre que há uma imagem ou outra parte da interface de usuário compartilhada entre as exibições de origem e destino.

## <a name="how-to-implement"></a>Como implementar

Configurar uma animação conectada envolve duas etapas:

1.  *Preparar* um objeto de animação na página de origem, o que indica ao sistema que o elemento de origem irá participar da animação conectada 
2.  *Iniciar* a animação na página de destino, transmitindo uma referência ao elemento de destino

Entre as duas etapas, o elemento de origem aparecerá congelado acima de outra interface de usuário no aplicativo, permitindo que você realize qualquer outra animação de transição simultaneamente. Por esse motivo, você não deverá esperar mais que 250 milissegundos entre as duas etapas uma vez que a presença do elemento de origem pode tornar-se distrativa. Se você preparar uma animação e não iniciá-la em três segundos, o sistema irá descartar a animação e qualquer tentativa subsequente de **TryStart** falhará.

Você pode acessar a instância atual de ConnectedAnimationService chamando **ConnectedAnimationService.GetForCurrentView**. Para preparar uma animação, chame **PrepareToAnimate** nessa instância, transmitindo uma referência a uma chave exclusiva e ao elemento da interface de usuário que deseja usar na transição. A chave exclusiva permite que você recupere a animação mais tarde, por exemplo, em uma página diferente.

```csharp
ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
```

Para iniciar a animação, chame **ConnectedAnimation.TryStart**. Você pode recuperar a instância de animação correta chamando **ConnectedAnimationService.GetAnimation** com a chave exclusiva que você forneceu ao criar a animação.

```csharp
ConnectedAnimation imageAnimation = 
    ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
if (imageAnimation != null)
{
    imageAnimation.TryStart(DestinationImage);
}
```

Aqui está um exemplo completo do uso do ConnectedAnimationService para criar uma transição entre duas páginas.

*SourcePage.xaml*

```xaml
<Image x:Name="SourceImage"
       Width="200"
       Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*SourcePage.xaml.cs*

```csharp
private void NavigateToDestinationPage()
{
    ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
    Frame.Navigate(typeof(DestinationPage));
}
```

*DestinationPage.xaml*

```xaml
<Image x:Name="DestinationImage"
       Width="400"
       Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*DestinationPage.xaml.cs*

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation imageAnimation = 
        ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
    if (imageAnimation != null)
    {
        imageAnimation.TryStart(DestinationImage);
    }
}
```

## <a name="connected-animation-in-list-and-grid-experiences"></a>Animação conectada em experiências de lista e de grade

Muitas vezes, você vai querer criar uma animação conectada de ou para um controle de lista ou grade. Você pode usar dois novos métodos de **ListView** e **GridView**, **PrepareConnectedAnimation** e **TryStartConnectedAnimationAsync**, para simplificar esse processo.
Por exemplo, digamos que você tem um **ListView** que contém um elemento com o nome "PortraitEllipse" em seu modelo de dados.

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate> 
    </ListView.ItemTemplate>
</ListView>
```

Para preparar uma animação conectada com a elipse correspondente a um determinado item de lista, chame o método **PrepareConnectedAnimation** com uma chave exclusiva, o item, e o nome "PortraitEllipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Alternativamente, para iniciar uma animação com esse elemento como destino, por exemplo ao voltar de uma exibição detalhada, use o **TryStartConnectedAnimationAsync**. Se você acabou de carregar a fonte de dados para o **ListView**, o **TryStartConnectedAnimationAsync** irá esperar para iniciar a animação até que o recipiente do item correspondente seja criado.

```csharp
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>Animação coordenada

![Animação Coordenada](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

Uma *animação coordenada* é um tipo especial de animação de entrada onde um elemento irá aparecer ao lado do alvo da animação conectada, animando em conjunto com o elemento da animação conectada à medida que ele se move através da tela. As animações coordenadas podem adicionar maior interesse visual a uma transição e chamar mais a atenção do usuário para o contexto compartilhado entre as exibições de origem e destino. Nessas imagens, a interface de usuário da legenda para o item está animando através de uma animação conectada.

Use a sobrecarga de dois parâmetros do **TryStart** para adicionar elementos coordenados a uma animação coordenada. Este exemplo demonstra uma animação coordenada de um layout de grade chamado "DescriptionRoot" que entrará em conjunto com um elemento de animação conectada chamado "CoverImage".

*DestinationPage.xaml*

```xaml
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

*DestinationPage.xaml.cs*

```csharp
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");
    
    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Use uma animação conectada em transições de página onde um elemento é compartilhado entre as páginas de origem e de destino.
- Não espere por solicitações de rede ou outras operações assíncronas de longa execução entre a preparação e o início da uma animação conectada. Talvez seja necessário carregar previamente as informações necessárias para executar a transição antecipadamente, ou utilizar uma imagem de baixa resolução no lugar enquanto uma imagem de alta resolução é carregada na exibição de destino.
- Use o [SuppressNavigationTransitionInfo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) para evitar uma animação de transição em um **Quadro** caso esteja usando o **ConnectedAnimationService**, uma vez que animações conectadas não devem ser usadas simultaneamente com as transições de navegação padrão. Consulte [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) para obter mais informações sobre como usar as transições de navegação.


## <a name="download-the-code-samples"></a>Baixar as amostras de código

Consulte [Amostra de Animação Conectada](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample) na galera de amostras [Windowsinterface de usuárioDevLabs](https://github.com/Microsoft/WindowsUIDevLabs).

## <a name="related-articles"></a>Artigos relacionados

- [ConnectedAnimation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation)
- [ConnectedAnimationService](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation.aspx)
- [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)
