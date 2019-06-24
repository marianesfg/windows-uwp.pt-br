---
title: Sombras de composição
description: A sombra APIs permitem que você adicionar dinâmicas sombras personalizáveis para conteúdo de interface do usuário.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a47a5f8ffca1d9ca2ddab05fe0baf2f85977d7f
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318189"
---
# <a name="shadows-in-windows-ui"></a>Sombras na interface do usuário do Windows

O [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) classe fornece meios de criar uma sombra configurável que pode ser aplicada a um [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) ou [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (subárvore de elementos visuais). Como é comum para objetos na camada Visual, todas as propriedades de DropShadow podem ser animadas usando CompositionAnimations.

## <a name="basic-drop-shadow"></a>Sombra básico

Para criar uma sombra básica, crie um novo DropShadow e associá-la ao seu visual. A sombra é retangular por padrão. Um conjunto padrão de propriedades estão disponíveis para ajustar a aparência de seu tipo de sombra.

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![Visual retangular com DropShadow básico](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Formatação de sombra

Há algumas maneiras de definir a forma para seu DropShadow:

- **Use o padrão** – por padrão, a forma de DropShadow é definida pelo modo em CompositionDropShadowSourcePolicy 'Default'. Para SpriteVisual, o padrão é retangular, a menos que uma máscara for fornecida. Para LayerVisual, o padrão é herdar de uma máscara de usando o alfa do pincel do visual.
- **Definir uma máscara** – você pode definir o [máscara](/uwp/api/windows.ui.composition.dropshadow.mask) propriedade para definir uma máscara de opacidade da sombra.
- **Especifique para usar a máscara Inherited** – defina o [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) propriedade usar [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent Use a máscara gerada a partir o alfa do pincel do visual.

## <a name="masking-to-match-your-content"></a>Mascaramento para corresponder ao seu conteúdo

Se você quiser a sombra para corresponder ao conteúdo do Visual, você pode usar o pincel do Visual para sua propriedade de máscara de sombra, ou definir a sombra para herdam automaticamente a máscara do conteúdo. Se usar um LayerVisual, a sombra herdará a máscara por padrão.

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Imagem da web conectados com sombra mascarada](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Usando uma máscara de alternativa

Em alguns casos, você talvez queira formatar a sombra, de modo que ele não corresponde ao conteúdo do seu Visual. Para obter esse efeito, você precisará definir explicitamente a propriedade de máscara usando um pincel com alfa.

No exemplo abaixo, podemos carregar duas superfícies – uma para o conteúdo Visual e outra para a máscara de sombra:

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Imagem da web conectados com círculo mascarado sombra](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animando

Conforme o padrão na camada Visual, propriedades de DropShadow podem ser animadas com animações de composição. Abaixo, podemos modificar o código de exemplo pitadas acima para animar o raio de desfoque da sombra.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>Sombras em XAML

Se você quiser adicionar uma sombra para os elementos de estrutura mais complexos, há duas maneiras para fornecer interoperabilidade com sombras entre XAML e composição:

1. Use o [DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponíveis no Kit de ferramentas de comunidade do Windows. Consulte a [DropShadowPanel documentação](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) para obter detalhes sobre como usá-lo.
1. Crie um Visual para usar como host do sombra e vinculá-lo para o Visual de folheto do XAML.
1. Usar a Galeria de exemplos de composição [SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) controle personalizado de CompositionShadow. Consulte o exemplo para uso.

## <a name="performance"></a>Desempenho

Embora a camada Visual tem muitas otimizações em vigor para criar efeitos utilizáveis e eficientes, gerar sombras pode ser uma operação relativamente cara, dependendo de quais opções que você definir. Abaixo estão nível 'custos altos' para tipos diferentes de sombras. Observe que embora determinadas sombras podem ser caras eles ainda possam ser apropriados para ser usada com moderação em determinados cenários.

Características de sombra| Custo
------------- | -------------
Retangular    | Baixo
Shadow.Mask      | Alto
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Alto
Raio de desfoque estático | Baixo
Animar o raio de desfoque | Alto

## <a name="additional-resources"></a>Recursos adicionais

- [API de DropShadow de composição](/uwp/api/Windows.UI.Composition.DropShadow)
- [Repositório do GitHub WindowsUIDevLabs](https://github.com/microsoft/WindowsCompositionSamples)