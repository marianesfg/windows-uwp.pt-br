---
author: scottmill
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Camada visual
description: "A API Windows.UI.Composition concede acesso a uma camada de composição entre a camada de estrutura (XAML) e a camada de elementos gráficos (DirectX)."
translationtype: Human Translation
ms.sourcegitcommit: b3d198af0c46ec7a2041a7417bccd56c05af760e
ms.openlocfilehash: 164c01737d27451adcb685f9cda544cc00634af4

---
# Camada visual

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

No Windows 10, foi realizado um trabalho significativo para criar um novo mecanismo unificado de composição e renderização para todos os aplicativos Windows, sejam eles para área de trabalho ou dispositivos móveis. Um resultado desse trabalho foi a API de composição unificada do WinRT, chamada Windows.UI.Composition, que oferece acesso a novos objetos de composição leves juntamente com novas animações e efeitos controlados pelo Compositor.

Windows.UI.Composition é uma API declarativa, [de modo retido](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) que pode ser chamada por qualquer aplicativo da Plataforma Universal do Windows (UWP) para criar objetos de composição, animações e efeitos diretamente em um aplicativo. A API é um suplemento eficiente para estruturas existentes como XAML para fornecer aos desenvolvedores de aplicativos UWP uma superfície C# familiar para adicionar ao seu aplicativo. Essas APIs podem ser usadas para criar aplicativos sem estrutura estilo DX.

Um desenvolvedor XAML pode "recorrer" à camada de composição em C# para realizar um trabalho personalizado na camada de composição usando o WinRT para criar uma "ilha de composição" de objetos em seu aplicativo XAML, em vez de usar a camada de elementos gráficos e o DirectX e C++ para qualquer trabalho de interface do usuário personalizada.

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"></span><span id="composition_objects_and_the_compositor"></span><span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"></span>Objetos de composição e o Compositor

Os objetos de composição são criados pelo [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789), que atua como um alocador de objetos de composição. O compositor pode criar objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), que permitem a criação de uma estrutura de árvore visual que todos os outros recursos e objetos de composição na API usam e têm como referência.

A API permite que os desenvolvedores definam e criem um ou vários objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), cada um representando um único nó em uma árvore visual.

Elementos visuais podem ser contêineres de outros elementos visuais ou podem hospedar elementos visuais de conteúdo. A API viabiliza facilidade de uso, fornecendo um conjunto claro de objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) para tarefas específicas que existem em uma hierarquia:

-   [
              **Visual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706858) – o objeto base. A maioria das propriedades estão aqui e são herdadas por outros objetos visuais.
-   [
              **ContainerVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706810) – deriva de [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) e adiciona a capacidade de inserir elementos visuais filho.
-   [
              **SpriteVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589433) – deriva de [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) e tem conteúdo na forma de imagens, efeitos e cadeias de troca.
-   [
              **Compositor**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706789) – o alocador de objetos que gerencia a relação entre um aplicativo e o processo do compositor do sistema.

O compositor também é um alocador de uma série de outros objetos de composição usados para recortar ou transformar elementos visuais na árvore, bem como um conjunto avançado de animações e efeitos.

## <span id="Effects_System"></span><span id="effects_system"></span><span id="EFFECTS_SYSTEM"></span>Sistema de efeitos

O Windows.UI.Composition permite efeitos em tempo real que podem ser animados, personalizados e encadeados. Os efeitos incluem transformações afins 2D, composições aritméticas, mesclagens, fontes de cor, composição, contraste, exposição, escala de cinza, transferência de gama, giro de matiz, inversão, saturação, sépia, temperatura e tonalidade.

Para obter mais informações, consulte a visão geral de [Efeitos de composição](composition-effects.md).

## <span id="Animation_System"></span><span id="animation_system"></span><span id="ANIMATION_SYSTEM"></span>Sistema de animação

O Windows.UI.Composition contém um sistema de animação independente de estrutura expressivo, que permite configurar dois tipos de animações: animações de quadro chave e animações de expressão. Elas são usadas para mover objetos visuais, gerar uma transformação ou um clipe, ou animar um efeito. Executar diretamente no processo do compositor garante suavidade e escala, permitindo que você execute um grande número de animações simultâneas únicas.

Para obter mais informações, consulte a visão geral de [Animações de composição](composition-animation.md).

## <span id="XAML_Interoperation"></span><span id="xaml_interoperation"></span><span id="XAML_INTEROPERATION"></span>Interoperação com XAML

Além de criar uma árvore visual do zero, a API de composição pode interoperar com uma interface do usuário XAML existente usando a classe [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) em [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908).


**Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <span id="Additional_Resources_"></span><span id="additional_resources_"></span><span id="ADDITIONAL_RESOURCES_"></span>Recursos adicionais:

-   Leia o artigo de Kenny Kerr no MSDN sobre essa API: [Gráficos e Animação - Opções de Composição do Windows 10](https://msdn.microsoft.com/magazine/mt590968)
-   Exemplos de composição no [GitHub de composição](https://github.com/Microsoft/composition).
-   [
              **Documentação de referência completa da API**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706878).
-   Problemas conhecidos: [Problemas conhecidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues).

 

 







<!--HONumber=Jun16_HO4-->


