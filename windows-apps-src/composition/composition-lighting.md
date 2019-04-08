---
title: Iluminação de composição
description: As APIs de iluminação de composição pode ser usadas para adicionar iluminação 3D dinâmica para seu aplicativo.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 733ce75942a05482ade88c1510e788f1cbd515d4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602201"
---
# <a name="using-lights-in-windows-ui"></a>Usando as luzes na interface do usuário do Windows

As APIs de Windows.UI.Composition permitem que você crie efeitos e animações em tempo real. Iluminação de composição permite iluminação 3D em aplicativos 2D. Esta visão geral, podemos executar a funcionalidade de como configurar luzes de composição, identificar elementos visuais para receber cada luz e usar efeitos para definir os materiais para o seu conteúdo.

> [!NOTE]
> Para ler como [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) aplicam objetos [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) para iluminar UIElements XAML, consulte [iluminação XAML](xaml-lighting.md).

Iluminação de composição permite que você crie interessantes da interface do usuário, permitindo que:

- Transformação de uma luz independentemente dos outros objetos na cena para habilitar cenários de imersão como cenas de reprodução de músicas.
- A capacidade de emparelhar um objeto com uma luz que se movimentam juntos independentemente do restante da cena para habilitar cenários como Fluent [revelar](/windows/uwp/design/style/reveal) realçar.
- Transformação da luz e toda a cena como um grupo para criar materiais e profundidade.

Iluminação de composição dá suporte a três conceitos principais: **Luz**, **destinos**, e **SceneLightingEffect**.

## <a name="light"></a>Claro

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) permite que você crie várias luzes e colocá-los no espaço de coordenadas. Essas luzes destinam visuais que você deseja identificar como iluminados pela luz.

### <a name="light-types"></a>Tipos de luz

| Tipo | Descrição |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Uma fonte de luz que emite luz não-direcional que aparece refletido por tudo na cena. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Uma infinitamente grande fonte de luz distante que emite luz em uma única direção. Como o sol. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Uma fonte de ponto de luz que emite luz em todas as direções. Como uma lâmpada. |
| [Em destaque](/uwp/api/windows.ui.composition.spotlight) | Uma fonte de luz que emite cones internas e externas da luz. Como uma lanterna. |

## <a name="targets"></a>Destinos

Quando um Visual de destino luzes (Adicionar ao [destinos](/uwp/api/windows.ui.composition.compositionlight.targets) lista), o Visual e todos os seus descendentes estão cientes e responder a essa fonte de luz. Isso pode ser algo tão simple quanto uma configuração de uma fonte PointLight na raiz de uma árvore e todos os visuais abaixo reagir a animação da direção de luzes de ponto.

**ExclusionsFromTargets** lhe dá a capacidade de remover a iluminação de um visual ou de uma subárvore de elementos visuais de maneira semelhante, como adição de destinos. Filhos na árvore com raiz pelo visual que foi excluído não são aceso como resultado.

### <a name="sample-targets"></a>Exemplo (destinos)

No exemplo a seguir, usamos um CompositionPointLight para ter como destino um TextBlock de XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

Adicionando animação ao deslocamento do ponto de luz, um efeito cintilante é fácil.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Ver todos os [tremido texto](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) exemplo no material de cozinha WindowUIDevLabs exemplo para saber mais.

## <a name="restrictions"></a>Restrições

Há vários fatores a considerar ao determinar qual conteúdo será iluminado pela CompositionLight.

Conceito | Detalhes
--- | ---
**Luz ambiente** | Adicionar uma luz não ambiente à sua cena desativará todos os luz existente.  Itens não direcionados por uma luz não ambiente aparecerá em pretos.  Para esclarecer visuais ao redor não direcionados pela luz de uma forma natural, use uma luz ambiente junto com outras luzes.
**Número de luzes** | Você pode usar qualquer duas luzes de composição não ambiente em qualquer combinação para sua interface do usuário de destino. Luzes de ambientes não são restritas; especiais, ponto e as luzes distantes são.
**tempo de vida** | CompositionLight pode enfrentar condições de tempo de vida (exemplo: o coletor de lixo pode reciclar o objeto de luz antes de ser usada).  É recomendável manter uma referência para as luzes, adicionando as luzes como um membro para ajudar a gerenciar tempo de vida do aplicativo.
**Transformações** | As luzes devem ser colocadas em um nó acima da interface do usuário que usa efeitos como [perspectiva transformações](/windows/uwp/design/layout/3-d-perspective-effects) em sua estrutura visual a ser desenhado corretamente.
**Destinos e espaço de coordenadas** | CoordinateSpace é o espaço de visual no qual todas as propriedades de luzes devem ser definidas. CompositionLight.Targets deve ser dentro da árvore de CoordinateSpace.

## <a name="lighting-properties"></a>Propriedades de iluminação

Dependendo do tipo de luz usado, uma luz pode ter propriedades de atenuação e espaço. Nem todos os tipos de luzes usam todas as propriedades.

Propriedade | Descrição
--- | ---
**Cor** | O [cor](/uwp/api/windows.ui.color) da luz. Valores de cor de iluminação são definidos pela [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) difusa, ambientes e especular que define a cor que está sendo emitida. Iluminação usa valores RGBA para luzes; o componente de cor alfa não é usado.
**Direção** | A direção da luz. A direção na qual a luz está apontando for especificada, relativo ao seu [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual.
**Espaço de coordenadas** | Cada Visual tem um espaço de coordenadas 3D implícito. Direção de X é da esquerda para a direita. Direção de Y é de cima para baixo. Direção Z é um ponto fora do plano. O ponto original essa coordenada é o canto superior esquerdo do visual e a unidade é o Pixel independente do dispositivo (DIP). Deslocamento de uma luz definido nessa coordenada.
**Cones internos e externos** | Destaques emitem um cone de luz com duas partes: um cone interno brilhante e um cone externo. Composição permite que você controle sobre a cor e os ângulos de cone interno e externo.
**deslocamento** | Deslocamento da fonte de luz em relação ao seu espaço de coordenadas Visual.

> [!NOTE]
> Quando várias luzes atingir o mesmo elemento Visual, ou sempre que o valor de cor da luz um fica grande o suficiente para exceder 1.0, a cor da luz pode mudar devido a fixação de um canal de cor luzes.

### <a name="advanced-lighting-properties"></a>Avançado de propriedades de iluminação

Propriedade | Descrição
--- | ---
**Intensidade** | Controla o brilho da luz.
**Atenuação** | A atenuação controla como a intensidade da luz diminui em direção à distância máxima especificada pela propriedade do intervalo.  Constantes, propriedades de atenuação Linear e de Quadradic podem ser usadas.

## <a name="getting-started-with-lighting"></a>Guia de Introdução com iluminação

Siga estas etapas gerais para adicionar as luzes:

- Criar e colocar as luzes: Criar luzes e colocá-los em um espaço de coordenadas especificado.
- Identificar os objetos de luz: Luz em visuais relevantes de destino.
- [Opcional] Definir objetos individuais como reagir a luzes: Use SceneLightingEffect com um EffectBrush para personalizar luz reflexão para exibir o SpriteVisual. A iluminação de filhos de CoordinateSpace de uma fonte de luz dão suporte a padrões de reflexão.  Um visual pintado com um SceneLightingEffect substitui a iluminação padrão para esse elemento visual.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) é usado para modificar a iluminação padrão aplicada ao conteúdo de um [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) direcionados por uma [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) é frequentemente usado para a criação de material. Um SceneLightingEffect é um efeito usado quando você deseja fazer algo mais complexo, como habilitar as propriedades de reflexão em uma imagem e/ou fornecendo uma ilusão de profundidade com um mapa normal. Um SceneLightingEffect fornece a capacidade de personalizar a interface do usuário usando as propriedades de iluminação como quantidades difusa e especulares. Você pode personalizar ainda mais efeitos de iluminação com o restante do pipeline efeitos, permitindo que você individualmente blend e compor reações de iluminação diferente com seu conteúdo.

> [!NOTE]
> Iluminação da cena não produz sombras; é um efeito voltado para renderização 2D.  Ele não leva em cenários de iluminação 3D de consideração que incluem modelos de iluminação real, incluindo sombras.


Propriedade | Descrição
--- | ---
**Mapa normal** | NormalMaps criar um efeito de uma textura em que um normal apontando na direção da luz será mais brilhante e um normal que aponta para a distância será esmaecida. Para adicionar um NormalMap ao destino visual uso uma [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) usando LoadedImageSurface para carregar um ativo de NormalMap.
**Ambiente** | Propriedades de ambiente são usadas principalmente para controlar a reflexão de cor geral.
**Especular** | Reflexão especular cria Destaques em objetos, tornando-os aparecer brilhante. Você pode controlar o nível de reflexão especular, bem como o nível de brilho.  Essas propriedades são manipuladas para criar efeitos de material como shinny metais ou papel acetinado.
**Difuso** | Reflexão difuso scatters a luz em todas as direções.
**Modelo de reflexão** | [Modelo de reflexão](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) permite que você escolha entre [Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) e fisicamente com base em Blinn Phong.  Quando você deseja ter condensado realces especulares, você escolheria fisicamente com base em Blinn Phong.

### <a name="sample-scenelightingeffect"></a>Exemplo (SceneLightingEffect)

O exemplo a seguir mostra como adicionar um mapa normal para um SceneLightingEffect.

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

- [Criando materiais e luzes na camada Visual](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Visão geral de iluminação](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [Cálculos de iluminação](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [Repositório do GitHub WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)
