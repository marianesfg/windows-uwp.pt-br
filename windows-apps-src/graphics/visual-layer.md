---
author: scottmill
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Camada visual
description: "A API Windows.UI.Composition concede acesso a uma camada de composição entre a camada de estrutura (XAML) e a camada de elementos gráficos (DirectX)."
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d1c676808b8b63f42b89a22862eaab63ddc94141
ms.lasthandoff: 02/07/2017

---
# <a name="visual-layer"></a>Camada visual

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

No Windows 10, foi realizado um trabalho significativo para criar um novo mecanismo unificado de composição e renderização para todos os aplicativos Windows, sejam eles para área de trabalho ou dispositivos móveis. Um resultado desse trabalho foi a API de composição unificada do WinRT, chamada Windows.UI.Composition, que oferece acesso a novos objetos de composição leves juntamente com novas animações e efeitos controlados pelo Compositor.

Windows.UI.Composition é uma API declarativa, [de modo retido](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) que pode ser chamada por qualquer aplicativo da Plataforma Universal do Windows (UWP) para criar objetos de composição, animações e efeitos diretamente em um aplicativo. A API é um suplemento eficiente para estruturas existentes como XAML para fornecer aos desenvolvedores de aplicativos UWP uma superfície C# familiar para adicionar ao seu aplicativo. Essas APIs também podem ser usadas para criar aplicativos sem estrutura estilo DX.

Um desenvolvedor XAML pode "recorrer" à camada de composição em C# para realizar um trabalho personalizado na camada de composição usando o WinRT, em vez de usar a camada de elementos gráficos e o DirectX e C++ para qualquer trabalho de interface do usuário personalizada. Essa técnica pode ser usada para animar um elemento existente usando as APIs de composição ou para ampliar uma interface do usuário, criando-se um "Visual Island" do conteúdo Windows.UI.Composition dentro da árvore de elementos XAML.

![Disposição em camadas de estrutura de interface do usuário: a camada de estrutura (Windows.UI. XAML) baseia-se na camada visual (Windows.UI.Composition) que é baseada na camada de elementos gráficos (DirectX)](images/layers-win-ui-composition.png)
## <a name="span-idcompositionobjectsandthecompositorspanspan-idcompositionobjectsandthecompositorspanspan-idcompositionobjectsandthecompositorspancomposition-objects-and-the-compositor"></a><span id="Composition_Objects_and_The_Compositor"></span><span id="composition_objects_and_the_compositor"></span><span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"></span>Objetos de composição e o Compositor

Os objetos de composição são criados pelo [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789), que atua como um alocador de objetos de composição. O compositor pode criar objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), que permitem a criação de uma estrutura de árvore visual que todos os outros recursos e objetos de composição na API usam e têm como referência.

A API permite que os desenvolvedores definam e criem um ou vários objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), cada um representando um único nó em uma árvore visual.

Elementos visuais podem ser contêineres de outros elementos visuais ou podem hospedar elementos visuais de conteúdo. A API viabiliza facilidade de uso, fornecendo um conjunto claro de objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) para tarefas específicas que existem em uma hierarquia:

-   [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – O objeto base. A maioria das propriedades estão aqui e são herdadas por outros objetos visuais.
-   [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – É derivado de [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) e adiciona a capacidade de inserir elementos visuais filho.
-   [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – É derivado de [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) e tem conteúdo na forma de imagens, efeitos e cadeias de troca.
-   [**LayerVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.layervisual.aspx) - Um ContainerVisual cujos filhos são achatados em uma única camada.  
-   [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) – O alocador de objetos que gerencia a relação entre um aplicativo e o processo do compositor do sistema.

O compositor também é um alocador de uma série de outros objetos de composição usados para recortar ou transformar elementos visuais na árvore, bem como um conjunto avançado de animações e efeitos.

## <a name="span-ideffectssystemspanspan-ideffectssystemspanspan-ideffectssystemspaneffects-system"></a><span id="Effects_System"></span><span id="effects_system"></span><span id="EFFECTS_SYSTEM"></span>Sistema de efeitos

O Windows.UI.Composition permite efeitos em tempo real que podem ser animados, personalizados e encadeados. Os efeitos incluem transformações afins 2D, composições aritméticas, mesclagens, fontes de cor, composição, contraste, exposição, escala de cinza, transferência de gama, giro de matiz, inversão, saturação, sépia, temperatura e tonalidade.

Para obter mais informações, consulte a visão geral de [Efeitos de composição](composition-effects.md).

## <a name="span-idanimationsystemspanspan-idanimationsystemspanspan-idanimationsystemspananimation-system"></a><span id="Animation_System"></span><span id="animation_system"></span><span id="ANIMATION_SYSTEM"></span>Sistema de animação

O Windows.UI.Composition contém um sistema de animação independente de estrutura expressivo, que permite configurar dois tipos de animações: animações de quadro chave e animações de expressão. Elas são usadas para mover objetos visuais, gerar uma transformação ou um clipe, ou animar um efeito. Executar diretamente no processo do compositor garante suavidade e escala, permitindo que você execute um grande número de animações simultâneas únicas.

Para obter mais informações, consulte a visão geral de [Animações de composição](composition-animation.md).

## <a name="span-idxamlinteroperationspanspan-idxamlinteroperationspanspan-idxamlinteroperationspanxaml-interoperation"></a><span id="XAML_Interoperation"></span><span id="xaml_interoperation"></span><span id="XAML_INTEROPERATION"></span>Interoperação com XAML

Além de criar uma árvore visual do zero, a API de composição pode interoperar com uma interface do usuário XAML existente usando a classe [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) em [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908).

- [**ElementCompositionPreview.GetElementVisual()**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual): obtém o Visual de um elemento para animá-lo usando as APIs de composição
- [**ElementCompositionPreview.SetChildVisual()**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual): adiciona um "Visual island" do conteúdo da composição a uma árvore XAML.
- [**ElementCompositionPreview.GetScrollViewerManipulationPropertySet()**](https://msdn.microsoft.com/library/windows/apps/mt608980.aspx): utiliza a manipulação do [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) como entrada de uma animação de composição


**Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <a name="span-idadditionalresourcesspanspan-idadditionalresourcesspanspan-idadditionalresourcesspanadditional-resources"></a><span id="Additional_Resources_"></span><span id="additional_resources_"></span><span id="ADDITIONAL_RESOURCES_"></span>Recursos adicionais:

-   Leia o artigo de Kenny Kerr no MSDN sobre essa API: [Elementos Gráficos e Animação - Opções de Composição do Windows 10](https://msdn.microsoft.com/magazine/mt590968)
-   Exemplos de Interface do usuário avançada e composição no [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs).
-   [**Documentação de referência completa da API**](https://msdn.microsoft.com/library/windows/apps/Dn706878).


 

 





