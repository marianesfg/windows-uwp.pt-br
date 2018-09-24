---
author: daneuber
title: Iluminação de composição
description: As APIs de iluminação de composição pode ser usadas para adicionar dinâmicos de iluminação 3D ao seu aplicativo.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e634b18fffc4f601f6512d6ceeed51efbe9c1886
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4155353"
---
# <a name="using-lights-in-windows-ui"></a>Uso de luzes na interface do usuário do Windows

As APIs Windows.UI.Composition permitem que você crie efeitos e animações em tempo real. Iluminação de composição permite que a iluminação 3D em aplicativos 2D. Nesta visão geral, vamos executar por meio de funcionalidade de como configurar luzes de composição, identificar elementos visuais para receber cada luz e usar efeitos para definir materiais para o seu conteúdo.

> [!NOTE]
> Para saber como objetos [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) aplicam [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) para iluminar UIElements XAML, consulte [a iluminação XAML](xaml-lighting.md).

Iluminação de composição permite que você crie interessantes da interface do usuário, permitindo que:

- Transformação de uma luz independente de outros objetos na cena para habilitar cenários imersivos como cenas de reprodução de música.
- A capacidade de emparelhar um objeto com uma luz para que eles se moverem juntos independente do restante da cena para habilitar cenários como fluente [Revelar](/design/style/reveal.md) destaque.
- Transformação de luz e cena inteira como um grupo para criar materiais e profundidade.

Iluminação de composição dá suporte a três principais conceitos: **luz**, **destinos**e **SceneLightingEffect**.

## <a name="light"></a>Luz

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) permite que você crie várias luzes e colocá-los no espaço de coordenadas. Essas luzes destino elementos visuais que você deseja identificar como iluminadas.

### <a name="light-types"></a>Tipos de luz

| Tipo | Descrição |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Uma fonte de luz que emite luz não direcional que aparece refletido por tudo na cena. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Uma grande infinitamente distante fonte de luz que emite luz em uma única direção. Como o sol. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Uma fonte de ponto de luz que emite luz em todas as direções. Como uma lâmpada. |
| [Destaque](/uwp/api/windows.ui.composition.spotlight) | Uma fonte de luz que emite cones internos e externos de luz. Como uma lanterna. |

## <a name="targets"></a>Destinos

Quando as luzes direcionar um Visual (Adicionar à lista de [destinos](/uwp/api/windows.ui.composition.compositionlight.targets) ), o Visual e todos os seus descendentes estão cientes e responder a essa fonte de luz. Isso pode ser algo tão simple quanto uma configuração de uma fonte PointLight na raiz de uma árvore e todos os elementos visuais abaixo reagir à animação da direção de luzes a ponto.

**ExclusionsFromTargets** oferece a capacidade de remover a iluminação de um elemento visual ou de uma subárvore de elementos visuais de maneira semelhante como adicionar destinos. Filhos na árvore de raiz por elemento visual que é excluído não são iluminados como resultado.

### <a name="sample-targets"></a>Amostra (destinos)

O exemplo a seguir, usamos um CompositionPointLight para direcionar um TextBlock XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

Adicionando animação para o deslocamento da luz ponto, um efeito cintilante é fácil.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Veja a amostra de [Texto tremida](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) completa no galé de amostra WindowUIDevLabs para saber mais.

## <a name="restrictions"></a>Restrições

Há vários fatores a serem considerados ao determinar qual conteúdo aceso por CompositionLight.

Conceito | Detalhes
--- | ---
**A luz ambiente** | Adicionando um não-a luz a sua cena desativará todas as luzes existentes.  Itens não direcionados por uma luz não ambiente aparecerá em pretos.  Para iluminar elementos visuais ao redor não direcionadas pela luz de uma maneira natural, use uma luz ambiente em conjunto com outras luzes.
**Número de luzes** | Você pode usar qualquer duas luzes de composição não ambiente em qualquer combinação de sua interface do usuário de destino. Luzes ambiente não são restritas; ponto, ponto e luzes distantes são.
**Tempo de vida** | CompositionLight pode enfrentar condições de tempo de vida (exemplo: o coletor de lixo pode reciclar o objeto de luz antes que ele seja usado).  É recomendável manter uma referência ao seus luzes adicionando luzes como um membro para ajudar o aplicativo a gerenciar o ciclo de vida.
**Transformações** | Luzes devem ser colocadas em um nó acima da interface do usuário que usa efeitos, como [transformações de perspectiva](/design/layout/3-d-perspective-effects.md) na sua estrutura visual para ser desenhado corretamente.
**Destinos e espaço de coordenadas** | CoordinateSpace é o espaço visual no qual todas as propriedades de luzes devem ser definidas. CompositionLight.Targets deve ser dentro da árvore de CoordinateSpace.

## <a name="lighting-properties"></a>Propriedades de iluminação

Dependendo do tipo de luz usado, uma luz pode ter propriedades de atenuação e espaço. Nem todos os tipos de luzes usam todas as propriedades.

Propriedade | Descrição
--- | ---
**Cor** | A [cor](/uwp/api/windows.ui.color) da luz. Iluminação cor valores são definidos pela [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) difusa, ambientes e especulares que define a cor emitida. Iluminação Use valores RGBA de luzes; o componente de cor alfa não é usado.
**Direção** | A direção da luz. A direção em que a luz está apontando é especificada em relação ao seu Visual [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) .
**Espaço de coordenadas** | Cada elemento Visual que tem um espaço de coordenadas 3D implícito. Direção X é da esquerda para a direita. Direção Y é de cima para baixo. Direção Z é um ponto fora do plano. O ponto original essa coordenada é o canto superior esquerdo do elemento visual e a unidade é pixels independentes do dispositivo (DIP). Deslocamento da luz definido nessa coordenada.
**Cones internos e externos** | Destaques emitem um cone de luz com duas partes: um cone interno brilhante e um cone externo. Composição permite que você controle cor e ângulos de cone interno e externo.
**Offset** | Deslocamento da fonte de luz em relação ao seu espaço de coordenadas Visual.

> [!NOTE]
> Quando várias luzes atinge o mesmo Visual ou sempre que o valor de cor da luz obtém grande o suficiente para exceder 1.0, pode alterar a cor da luz devido a fixação de um canal de cor de luzes.

### <a name="advanced-lighting-properties"></a>Avançada propriedades de iluminação

Propriedade | Descrição
--- | ---
**Intensidade** | Controla o brilho da luz.
**Atenuação** | A atenuação controla como a intensidade da luz diminui em direção à distância máxima especificada pela propriedade do intervalo.  Constante, propriedades de atenuação Quadradic e Linear podem ser usadas.

## <a name="getting-started-with-lighting"></a>Introdução a iluminação

Siga estas etapas gerais para adicionar luzes:

- Crie e coloque as luzes: criar luzes e colocá-los em um espaço de coordenadas especificado.
- Identificar os objetos à luz: luz em elementos visuais relevantes de destino.
- [Opcional] Definir objetos individuais como reagir às luzes: Use SceneLightingEffect com um EffectBrush personalizar reflexão de luz para exibir o SpriteVisual. Padrões de reflexão dão suporte a iluminação de filhos CoordinateSpace da fonte de luz.  Um elemento visual pintado com um SceneLightingEffect substitui a iluminação padrão para esse elemento visual.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) é usado para modificar a iluminação padrão aplicada ao conteúdo de um [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) direcionadas por um [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) é usado com frequência para a criação de material. Um SceneLightingEffect é um efeito usado quando você deseja obter algo mais complexas, como habilitar propriedades reflexivas em uma imagem e/ou fornecer a ilusão de profundidade com um mapa normal. Um SceneLightingEffect fornece a capacidade de personalizar sua interface do usuário usando as propriedades de iluminação como valores especulares e difusas. Você pode personalizar os efeitos de iluminação com o resto do pipeline de efeitos, permitindo que você individualmente misturar e compor reações iluminação diferentes com seu conteúdo.

> [!NOTE]
> Iluminação de cena não produz sombras; é um efeito focado na renderização 2D.  Ele não leva em cenários de iluminação 3D consideração que incluem os modelos de iluminação real, incluindo sombras.


Propriedade | Descrição
--- | ---
**Mapa normal** | NormalMaps criar um efeito de uma textura onde um apontando normal para a luz estarão mais brilhante e um apontando normal distância será esmaecido. Para adicionar um NormalMap ao uso visual direcionado [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) usando LoadedImageSurface para carregar um ativo NormalMap.
**Ambiente** | Propriedades de ambiente são usadas principalmente para controlar o reflexo de cor geral.
**Especular** | A reflexão especular cria Destaques em objetos, tornando-a aparecer brilhante. Você pode controlar o nível de reflexão especular, bem como o nível de brilho.  Essas propriedades são manipuladas para criar efeitos de material como shinny metais ou papel brilhante.
**Difusa** | Reflexão difusa scatters a luz em todas as direções.
**Modelo de reflexão** | [Modelo de reflexão](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) permite que você escolha entre [Blinn reflexo](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) e com base em reflexo de Blinn fisicamente.  Escolha com base em fisicamente reflexo de Blinn quando você deseja ter condensado realces especulares.

### <a name="sample-scenelightingeffect"></a>Amostra (SceneLightingEffect)

O exemplo a seguir mostra como adicionar um mapa normal para uma SceneLightingEffect.

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>Artigos relacionados

- [Criar luzes e materiais na camada Visual](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Visão geral de iluminação](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [API CompositionCapabilities](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [Matemática da iluminação](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [Repositório do GitHub WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)
