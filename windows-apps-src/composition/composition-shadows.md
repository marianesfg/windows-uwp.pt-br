---
title: Sombras de composição
description: A sombra APIs permitem que você adicionar sombras personalizáveis dinâmicas ao conteúdo de interface do usuário.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7837265"
---
# <a name="shadows-in-windows-ui"></a>Sombras na interface do usuário do Windows

A classe [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) fornece meios de criação de uma sombra configurável que pode ser aplicada a um [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) ou [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (subárvore de elementos visuais). Como é comum para objetos na camada Visual, todas as propriedades do DropShadow podem ser animadas usando CompositionAnimations.

## <a name="basic-drop-shadow"></a>Sombra básico

Para criar uma sombra básica, basta criar um novo DropShadow e associá-lo à sua visual. A sombra é retangular por padrão. Um conjunto padrão de propriedades estão disponíveis para ajustar a aparência de sua sombra.

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

![Retangular Visual com DropShadow básico](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Modelar a sombra

Existem algumas maneiras de definir a forma para seu DropShadow:

- **Use o padrão** , por padrão a forma de DropShadow é definido pelo modo 'Padrão' em CompositionDropShadowSourcePolicy. Para SpriteVisual, o padrão é Rectangular, a menos que uma máscara é fornecida. Para LayerVisual, o padrão é herdar uma máscara usando o alfa do pincel do elemento visual.
- **Definir uma máscara** – você pode definir a propriedade [máscara](/uwp/api/windows.ui.composition.dropshadow.mask) para definir uma máscara de opacidade da sombra.
- **Especifique usar herdadas máscara** – defina a propriedade [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) usar [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent Use a máscara gerada a partir da alfa de pincel do visual.

## <a name="masking-to-match-your-content"></a>Mascaramento para corresponder ao seu conteúdo

Se você quiser que a sombra para corresponder ao conteúdo do elemento Visual, você pode usar o pincel do elemento Visual para sua propriedade de máscara de sombra ou definir a sombra automaticamente herdar máscara de conteúdo. Se usar um LayerVisual, a sombra herdará a máscara por padrão.

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

## <a name="using-an-alternative-mask"></a>Usando uma máscara alternativa

Em alguns casos, convém sombra de forma que ele não coincidir com o conteúdo do elemento Visual. Para obter esse efeito, você precisará definir explicitamente a propriedade de máscara usando um pincel com alfa.

No exemplo abaixo, carregamos duas superfícies - uma para o conteúdo Visual e outra para a máscara de sombra:

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

![Imagem da web conectados com círculo mascarados sombra](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animando

Conforme o padrão na camada Visual, DropShadow propriedades podem ser animadas usando animações de composição. Abaixo, podemos modificar o código de exemplo pitadas acima para animar o raio de desfoque da sombra.

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

Se você deseja adicionar uma sombra a elementos de estrutura mais complexos, há algumas maneiras de interoperabilidade com sombras entre XAML e composição:

1. Use o [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponíveis no Kit de ferramentas de comunidade Windows. Consulte a [documentação de DropShadowPanel](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) para obter detalhes sobre como usá-lo.
1. Crie um elemento Visual para usar como o host de sombra e associe-o a "Handout" XAML Visual.
1. Use controle de CompositionShadow da Galeria de exemplos de composição [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) personalizado. Consulte o exemplo a seguir para uso.

## <a name="performance"></a>Desempenho

Embora a camada Visual tem muitas otimizações no lugar para criar efeitos eficaz e utilizáveis, gerar sombras pode ser uma operação relativamente cara dependendo de quais opções que você definir. Abaixo estão altos nível 'custos' para diferentes tipos de sombras. Observe que embora determinadas sombras podem ser caras eles ainda poderão estar devem ser usados com moderação em determinados cenários.

Características de sombra| Custo
------------- | -------------
Retangular    | Baixa
Shadow.Mask      | Alto
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Alto
Raio de desfoque estático | Baixa
Animação de desfoque Radius | Alto

## <a name="additional-resources"></a>Recursos adicionais

- [Composição DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [Repositório do GitHub WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)