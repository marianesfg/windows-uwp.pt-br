---
author: mijacobs
description: As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente através da animação da transição de um elemento entre duas exibições diferentes.
title: Animação conectada
template: detail.hbs
ms.author: jimwalk
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 51cf9dd0d28590d86bf05cc16634e465e260626c
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5977078"
---
# <a name="connected-animation-for-uwp-apps"></a>Animação conectada para aplicativos UWP

As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente através da animação da transição de um elemento entre duas exibições diferentes. Isso ajuda o usuário a manter o contexto e oferece continuidade entre os modos de exibição.

Em uma animação conectada, um elemento parece "Continuar" entre duas exibições durante uma alteração no conteúdo de interface do usuário, voando pela tela desde a sua localização na exibição de origem até seu destino na nova exibição. Isso enfatiza o conteúdo comum entre os modos de exibição e cria um efeito belo e dinâmico como parte de uma transição.

> **APIs importantes**: [classe ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), o [ConnectedAnimationService classe](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

## <a name="see-it-in-action"></a>Veja em ação

Neste vídeo curto, um aplicativo usa uma animação conectada para animar uma imagem conforme ela "continua" a fazer parte do cabeçalho da próxima página. O efeito ajuda a manter o contexto de usuário entre a transição.

![Animação Conectada](images/connected-animations/example.gif)

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

## <a name="configure-connected-animation"></a>Configurar animação conectada

> [!IMPORTANT]
> Esse recurso requer que a versão de destino do seu aplicativo ser Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior. A propriedade de configuração não está disponível no SDKs anteriores. Você pode direcionar uma versão mínima inferior ao SDK 17763 usando código adaptável ou XAML condicional. Para obter mais informações, consulte [aplicativos adaptáveis de versão](/debug-test-perf/version-adaptive-apps).

A partir do Windows 10, versão 1809, ainda mais as animações conectadas incorporam o design Fluent, fornecendo animação configurações personalizadas especificamente para frente e para trás navegação de página.

Você pode especificar uma configuração de animação, definindo a propriedade de configuração sobre o ConnectedAnimation. (Mostraremos exemplos na próxima seção).

Esta tabela descreve as configurações disponíveis. Para obter mais informações sobre os princípios de movimento aplicadas nessas animações, consulte [direção e gravidade](index.md).

| [GravityConnectedAnimationConfiguration]() |
| - |
| Isso é a configuração padrão e é recomendado para navegação para frente. |
Conforme o usuário avança no aplicativo (A para B), o elemento conectado aparece fisicamente "puxe fora da página". Ao fazer isso, o elemento aparecerá em frente no espaço de z e cair um pouco como um efeito de gravidade levando espera. Para superar os efeitos de gravidade, o elemento ganha velocidade e acelera para sua posição final. O resultado é uma animação de "escala e dip". |

| [DirectConnectedAnimationConfiguration]() |
| - |
| Conforme o usuário navega para trás no aplicativo (B para A), a animação é mais direta. O elemento conectado linearmente traduz dos B para um usando uma função de suavização por desaceleração cúbica Bézier. A funcionalidade visual para trás retorna o usuário para seu estado anterior mais rápido possível e ainda manter o contexto do fluxo de navegação. |

| [BasicConnectedAnimationConfiguration]() |
| - |
| Este é o padrão (e somente) animação usada em versões anteriores ao Windows 10, versão 1809 ([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>Configuração de ConnectedAnimationService

A classe [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) tem duas propriedades que se aplicam às animações individuais em vez do serviço geral.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Para obter os diversos efeitos, algumas configurações ignoram essas propriedades em ConnectedAnimationService e usam seus próprios valores em vez disso, conforme descrito nesta tabela.

| Configuração | Aspectos DefaultDuration? | Aspectos DefaultEasingFunction? |
| - | - | - |
| Gravidade | Sim | Sim* <br/> **A conversão básica de A para B usa essa função de easing, mas o "dip gravidade" tem sua própria função de easing.*  |
| Direto | Não <br/> *Anima mais de 150 ms.*| Não <br/> *Usa a por desaceleração função de easing.* |
| Básico | Sim | Sim |

## <a name="how-to-implement-connected-animation"></a>Como implementar animação conectada

Configurar uma animação conectada envolve duas etapas:

1. *Preparar* um objeto de animação na página de origem, o que indica ao sistema que o elemento de origem irá participar da animação conectada.
1. *Iniciar* a animação na página de destino, transmitindo uma referência ao elemento de destino.

Ao navegar da página de origem, chame o [Connectedanimationservice](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) para obter uma instância de ConnectedAnimationService. Para preparar uma animação, chame [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) nessa instância e passe uma chave exclusiva e o elemento de interface do usuário que você deseja usar na transição. A chave exclusiva permite que você recupere a animação posteriormente a página de destino.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Quando a navegação ocorre, inicie a animação na página de destino. Para iniciar a animação, chame [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Você pode recuperar a instância de animação correta chamando [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) com a chave exclusiva que você forneceu ao criar a animação.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navegação para frente

Este exemplo mostra como usar o ConnectedAnimationService para criar uma transição para frente navegação entre duas páginas (Page_A para Page_B).

A configuração de animação recomendada para navegação progressiva é [GravityConnectedAnimationConfiguration](). Este é o padrão, portanto, você não precisa definir a propriedade de [configuração](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) , a menos que você deseja especificar uma configuração diferente.

Configure a animação na página de origem.

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

Inicie a animação na página de destino.

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>Navegação regressiva

Para navegação regressiva (Page_B para Page_A), que você siga as mesmas etapas, mas as páginas de origem e destino são revertidas.

Quando o usuário navega de volta, eles esperam que o aplicativo seja retornado para o estado anterior assim que possível. Portanto, a configuração recomendada é [DirectConnectedAnimationConfiguration](). Esta animação é mais rápido, mais direto e usa a suavização por desaceleração.

Configure a animação na página de origem.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimationService.GetForCurrentView()
            .PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Inicie a animação na página de destino.

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

Entre o momento em que a animação é configurada e quando ele é iniciado, o elemento de origem aparecerá congelado acima de outra interface do usuário no aplicativo. Isso permite que você realize qualquer outra animação de transição simultaneamente. Por esse motivo, você não deverá esperar mais de 250 milissegundos entre as duas etapas porque a presença do elemento de origem pode tornar-se distrativa. Se você preparar uma animação e não iniciá-la em três segundos, o sistema irá descartar a animação e qualquer tentativa subsequente de [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) falhará.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Animação conectada em experiências de lista e de grade

Muitas vezes, você vai querer criar uma animação conectada de ou para um controle de lista ou grade. Você pode usar os dois métodos em [ListView](/uwp/api/windows.ui.xaml.controls.listview) e [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) e [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), para simplificar esse processo.

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

Para preparar uma animação conectada com a elipse correspondente a um determinado item de lista, chame o método [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) com uma chave exclusiva, o item e o nome "PortraitEllipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Para iniciar uma animação com esse elemento como destino, por exemplo, quando navegar de volta de uma exibição detalhada, use [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Se você acabou de carregar a fonte de dados para o ListView, o TryStartConnectedAnimationAsync irá esperar para iniciar a animação até que o recipiente do item correspondente seja criado.

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

Uma *animação coordenada* é um tipo especial de animação de entrada onde um elemento parece juntamente com o alvo da animação conectada, animando em conjunto com o elemento de animação conectada conforme ele se move pela tela. As animações coordenadas podem adicionar maior interesse visual a uma transição e chamar mais a atenção do usuário para o contexto compartilhado entre as exibições de origem e destino. Nessas imagens, a interface de usuário da legenda para o item está animando através de uma animação conectada.

Quando uma animação conectada usa a configuração de gravidade, gravidade é aplicada para o elemento de animação conectada e os elementos coordenados. Os elementos coordenados serão "vez" junto com o elemento conectado para que os elementos se realmente coordenados.

Use a sobrecarga de dois parâmetros do **TryStart** para adicionar elementos coordenados a uma animação coordenada. Este exemplo demonstra uma animação coordenada de um layout de grade chamado "DescriptionRoot" que entrará em conjunto com um elemento de animação conectada chamado "CoverImage".

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
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
- Use [GravityConnectedAnimationConfiguration]() para navegação para frente.
- Use [DirectConnectedAnimationConfiguration]() para navegação regressiva.
- Não espere em solicitações de rede ou outras operações assíncronas de longa execução entre a preparação e a partir de uma animação conectada. Talvez seja necessário carregar previamente as informações necessárias para executar a transição antecipadamente, ou utilizar uma imagem de baixa resolução no lugar enquanto uma imagem de alta resolução é carregada na exibição de destino.
- Use [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) para evitar uma animação de transição em um **quadro** , se você estiver usando o **ConnectedAnimationService**, já que animações conectadas não devem ser usadas simultaneamente com a navegação padrão faz a transição. Consulte [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) para obter mais informações sobre como usar as transições de navegação.

## <a name="download-the-code-samples"></a>Baixar as amostras de código

Consulte [Amostra de Animação Conectada](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample) na galera de amostras [Windowsinterface de usuárioDevLabs](https://github.com/Microsoft/WindowsUIDevLabs).

## <a name="related-articles"></a>Artigos relacionados

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
