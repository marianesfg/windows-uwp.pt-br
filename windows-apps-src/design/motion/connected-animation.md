---
description: As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente pela animação da transição de um elemento entre duas exibições diferentes.
title: Animação conectada
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ad94d7b887e28ac01156592ac47cfc9ac4783193
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775905"
---
# <a name="connected-animation-for-windows-apps"></a>Animação conectada para aplicativos do Windows

As animações conectadas permitem que você crie uma experiência de navegação dinâmica e convincente pela animação da transição de um elemento entre duas exibições diferentes. Isso ajuda o usuário a manter o contexto e oferece continuidade entre os modos de exibição.

Em uma animação conectada, um elemento parece "continuar" entre duas exibições durante uma alteração no conteúdo da interface do usuário, trocando pela tela de seu local na exibição da fonte até seu destino na nova exibição. Isso enfatiza o conteúdo comum entre as exibições e cria um efeito bonito e dinâmico como parte de uma transição.

> **APIs importantes**: [classe ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [classe ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o aplicativo da <strong style="font-weight: semi-bold">Galeria de controles XAML</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ConnectedAnimation">abrir o aplicativo e ver a animação conectada em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Neste vídeo rápido, um aplicativo usa uma animação conectada para animar uma imagem de item enquanto ele "continua" para se tornar parte do cabeçalho da próxima página. O efeito ajuda a manter o contexto de usuário entre a transição.

![Animação Conectada](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animação conectada e o Sistema de Design Fluent

 O Sistema de Design Fluente ajuda a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. Animação conectada é um componente do Sistema de Design Fluent que acrescenta movimento ao seu aplicativo. Para saber mais, confira a [Visão geral do Fluent Design](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>Por que animação conectada?

Ao navegar entre páginas, é importante para o usuário entender qual novo conteúdo está sendo apresentado após a navegação e como isso se relaciona com sua intenção ao navegar. As animações conectadas fornecem uma metáfora visual poderosa que enfatiza a relação entre dois modos de exibição, chamando a atenção do usuário para o conteúdo compartilhado entre eles. Além disso, animações conectadas adicionam brilho e interesse visual à navegação da página, o que pode ajudar a diferenciar o design de movimento do seu aplicativo.

## <a name="when-to-use-connected-animation"></a>Quando usar a animação conectada

As animações conectadas geralmente são usadas na troca de páginas, embora possam ser aplicadas em qualquer experiência onde ocorra troca de conteúdo na interface de usuário e o usuário deva manter o contexto. Você deve considerar usar uma animação conectada em vez de um [furo na transição da navegação](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) sempre que há uma imagem ou outra parte da interface de usuário compartilhada entre as exibições de origem e destino.

## <a name="configure-connected-animation"></a>Configurar animação conectada

> [!IMPORTANT]
> Esse recurso requer que a versão de destino do aplicativo seja Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior. A propriedade de configuração não está disponível em SDKs anteriores. Você pode direcionar uma versão mínima inferior ao SDK 17763 usando código adaptável ou XAML condicional. Para obter mais informações, consulte [versões adaptáveis de aplicativos](/windows/uwp/debug-test-perf/version-adaptive-apps).

A partir do Windows 10, versão 1809, animações conectadas ainda mais incorporam design fluente fornecendo configurações de animação personalizadas especificamente para navegação de página para frente e para trás.

Você especifica uma configuração de animação definindo a propriedade de configuração no ConnectedAnimation. (Mostraremos exemplos disso na próxima seção.)

Esta tabela descreve as configurações disponíveis. Para obter mais informações sobre os princípios de movimento aplicados nessas animações, consulte [direcionalidade e gravidade](index.md).

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| Essa é a configuração padrão e é recomendada para navegação progressiva. |
À medida que o usuário navega para frente no aplicativo (A para B), o elemento conectado parece "retirar a página fisicamente". Ao fazer isso, o elemento parece avançar em z-Space e deixa um pouco como um efeito de gravidade assumindo a suspensão. Para superar os efeitos da gravidade, o elemento ganha velocidade e acelera sua posição final. O resultado é uma animação de "escala e DIP". |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| À medida que o usuário navega para trás no aplicativo (B para A), a animação é mais direta. O elemento conectado linearmente se traduz de B para um usando uma função de atenuação de Bezier cubica de desaceleração. A unificação visual com versões anteriores retorna o usuário para seu estado anterior o mais rápido possível e, ao mesmo tempo, mantém o contexto do fluxo de navegação. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| Essa é a animação padrão (e somente) usada em versões anteriores ao Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>Configuração do ConnectedAnimationService

A classe [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) tem duas propriedades que se aplicam às animações individuais em vez do serviço geral.

- [Defaultduration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Para obter os vários efeitos, algumas configurações ignoram essas propriedades em ConnectedAnimationService e usam seus próprios valores, conforme descrito nesta tabela.

| Configuração | Respeita defaultduration? | Respeita DefaultEasingFunction? |
| - | - | - |
| Gravidade | Yes | Sim* <br/> **A conversão básica de a para B usa essa função de atenuação, mas o "dip de gravidade" tem sua própria função de atenuação.*  |
| Direto | No <br/> *Anima sobre 150ms.*| No <br/> *Usa a função de atenuação de desaceleração.* |
| Básico | Sim | Sim |

## <a name="how-to-implement-connected-animation"></a>Como implementar uma animação conectada

Configurar uma animação conectada envolve duas etapas:

1. *Prepare* um objeto de animação na página de origem, que indica ao sistema que o elemento de origem participará na animação conectada.
1. *Inicie* a animação na página destino, passando uma referência para o elemento de destino.

Ao navegar da página de origem, chame [ConnectedAnimationService. GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) para obter uma instância de ConnectedAnimationService. Para preparar uma animação, chame [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) nessa instância e passe uma chave exclusiva e o elemento de interface do usuário que você deseja usar na transição. A chave exclusiva permite recuperar a animação posteriormente na página destino.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Quando ocorrer a navegação, inicie a animação na página destino. Para iniciar a animação, chame [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Você pode recuperar a instância de animação correta chamando [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) com a chave exclusiva que você forneceu ao criar a animação.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navegação progressiva

Este exemplo mostra como usar ConnectedAnimationService para criar uma transição para navegação progressiva entre duas páginas (Page_A para Page_B).

A configuração de animação recomendada para navegação progressiva é [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration). Esse é o padrão, portanto, você não precisa definir a propriedade de [configuração](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) , a menos que queira especificar uma configuração diferente.

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

Inicie a animação na página destino.

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

Para navegação regressiva (Page_B para Page_A), siga as mesmas etapas, mas as páginas de origem e de destino são invertidas.

Quando o usuário navega de volta, ele espera que o aplicativo seja devolvido ao estado anterior assim que possível. Portanto, a configuração recomendada é [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration). Essa animação é mais rápida e direta e usa a atenuação de desaceleração.

Configure a animação na página de origem.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Inicie a animação na página destino.

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

Entre o momento em que a animação é configurada e quando ela é iniciada, o elemento de origem aparece congelado acima de outra interface do usuário no aplicativo. Isso permite que você execute qualquer outra animação de transição simultaneamente. Por esse motivo, você não deve esperar mais de ~ 250 milissegundos entre as duas etapas porque a presença do elemento de origem pode se tornar uma distração. Se você preparar uma animação e não iniciá-la em três segundos, o sistema irá descartar a animação e qualquer tentativa subsequente de [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) falhará.

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

Para iniciar uma animação com esse elemento como o destino, como ao navegar de volta de um modo de exibição de detalhes, use [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Se você acabou de carregar a fonte de dados para o ListView, o TryStartConnectedAnimationAsync irá esperar para iniciar a animação até que o recipiente do item correspondente seja criado.

```csharp
private async void ContactsListView_Loaded(object sender, RoutedEventArgs e)
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

Uma *animação coordenada* é um tipo especial de animação de entrada em que um elemento aparece junto com o destino de animação conectado, animação em tandem com o elemento de animação conectado à medida que ele se move pela tela. As animações coordenadas podem adicionar maior interesse visual a uma transição e chamar mais a atenção do usuário para o contexto compartilhado entre as exibições de origem e destino. Nessas imagens, a interface de usuário da legenda para o item está animando através de uma animação conectada.

Quando uma animação coordenada usa a configuração de gravidade, a gravidade é aplicada ao elemento de animação conectado e aos elementos coordenados. Os elementos coordenados serão "vezs" junto com o elemento conectado para que os elementos permaneçam realmente coordenados.

Use a sobrecarga de dois parâmetros do **TryStart** para adicionar elementos coordenados a uma animação coordenada. Este exemplo demonstra uma animação coordenada de um layout de grade chamado "DescriptionRoot" que entra em conjunto com um elemento de animação conectado chamado "CoverImage".

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
- Use [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) para navegação progressiva.
- Use [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) para navegação regressiva.
- Não aguarde as solicitações de rede ou outras operações assíncronas de execução longa em entre preparar e iniciar uma animação conectada. Talvez seja necessário carregar previamente as informações necessárias para executar a transição antecipadamente, ou utilizar uma imagem de baixa resolução no lugar enquanto uma imagem de alta resolução é carregada na exibição de destino.
- Use [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) para evitar uma animação de transição em um **quadro** se você estiver usando **ConnectedAnimationService**, já que animações conectadas não devem ser usadas simultaneamente com as transições de navegação padrão. Consulte [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) para obter mais informações sobre como usar as transições de navegação.

## <a name="related-articles"></a>Artigos relacionados

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
