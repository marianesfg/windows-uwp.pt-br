---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Camada visual
description: A API Windows.UI.Composition concede acesso a uma camada de composição entre a camada de estrutura (XAML) e a camada de elementos gráficos (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c61f6580039b9fe3da915491acd84c939088370
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361393"
---
# <a name="visual-layer"></a>Camada visual

A camada Visual fornece uma API de modo retido para elementos gráficos, efeitos e animações de alto desempenho e é a base para todas as interfaces do usuário em dispositivos Windows. Você define sua interface do usuário de maneira declarativa e a camada Visual depende da aceleração de hardware de elementos gráficos para garantir que seu conteúdo, efeitos e animações sejam renderizados de forma suave e sem falhas, de forma independente do thread de interface do usuário do aplicativo.

Destaques notáveis:

* APIs conhecidas do WinRT
* Arquitetada para interações e interface do usuário mais dinâmicos
* Conceitos alinhados com ferramentas de design
* Dimensionamento linear sem quedas repentinas de desempenho

Seus aplicativos UWP do Windows já estão usando a camada Visual por meio de uma das estruturas de interface do usuário. Você também pode tirar proveito da camada Visual diretamente para a renderização personalizada, efeitos e animações com muito pouco esforço.

![Disposição em camadas de estrutura de interface do usuário: a camada de estrutura (Windows.UI. XAML) baseia-se na camada visual (Windows.UI.Composition) que é baseada na camada de elementos gráficos (DirectX)](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>O que há na camada Visual?

As funções principais de camada Visual são:

1. **Conteúdo**: Composição leve de conteúdo de desenho personalizado
1. **Efeitos**: Sistema cujos efeitos podem ser animados, encadeados e personalizados de efeitos de interface do usuário em tempo real
1. **Animações**: Animações expressivas, independente da estrutura de execução independentemente do thread de interface do usuário

### <a name="content"></a>Conteúdo

O conteúdo é hospedado, transformado e disponibilizado para uso pelo sistema de animação e de efeitos usando elementos visuais. Na base da hierarquia de classe, há a classe [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual), um proxy de threads leve e ágil no processo do app para o estado visual no compositor. Incluem subpastas de classes do Visual  [**ContainerVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) para permitir a filhos criar árvores de elementos visuais e [ **SpriteVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) que contém o conteúdo e pode ser pintado com qualquer um dos cores sólidas, efeitos personalizados de conteúdo ou visuais desenhados. Juntos, esses tipos de Visual compõem a estrutura de árvore visual para a interface do usuário 2D e contêm os FrameworkElements XAML mais visíveis.

Para saber mais, veja a visão geral de [Composição de Visual](composition-visual-tree.md).

### <a name="effects"></a>Efeitos

O sistema de efeitos na camada Visual permite que você aplique uma cadeia de efeitos de filtro e de transparência para um elemento Visual ou uma árvore de Visuals. Este é um sistema de efeitos de interface do usuário, que não deve ser confundido com efeitos de imagem e mídia. Os efeitos funcionam em conjunto com o sistema de animação, permitindo que os usuários obtenham animações suaves e dinâmicas de propriedades de efeitos, renderizadas de forma independente do thread da interface do usuário. Os efeitos na camada Visual fornecem os blocos de construção criativos que podem ser combinados e animados para construir experiências personalizadas e interativas.

Além de cadeias de efeitos que podem ser animados, a camada Visual também dá suporte a um modelo de iluminação que permite que os elementos visuais simulem as propriedades do material ao responderem às luzes que podem ser animadas. Os elementos visuais também podem projetar sombras. Iluminação e sombras podem ser combinadas para criar uma percepção de profundidade e realismo.

Para obter mais informações, consulte a visão geral de [Efeitos de composição](composition-effects.md).

### <a name="animations"></a>Animações

O sistema de animação na camada Visual permite que você mova elementos visuais, anime efeitos e promova transformações, clipes e outras propriedades.  Ele é um sistema independente de estrutura projetado desde o início com o desempenho em mente.  Ele é executado independentemente do thread da interface do usuário para garantir suavidade e escalabilidade.  Enquanto permite que você use animações de KeyFrame para promover alterações de propriedade ao longo do tempo, ele também permite que você configure relacionamentos matemáticos entre as propriedades diferentes, incluindo entrada do usuário, permitindo que você crie experiências coreografadas contínuas de forma direta.

Para obter mais informações, consulte a visão geral de [Animações de composição](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Como trabalhar com seu aplicativo UWP XAML

Você pode acessar um Visual criado pela estrutura XAML e apoiar um FrameworkElement visível usando a classe [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) em [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting). Observe que os elementos visuais criados pela estrutura para você vêm com alguns limites em termos de personalização. Isso ocorre porque a estrutura está gerenciando deslocamentos, transformações e tempos de vida. No entanto, você pode criar seus próprios Visuals e anexá-los a um elemento XAML existente por meio de ElementCompositionPreview ou adicionando-os a um ContainerVisual existente em algum lugar na estrutura de árvore visual.

Para saber mais, confira a visão geral [Uso da camada Visual com XAML](using-the-visual-layer-with-xaml.md).

### <a name="working-with-your-desktop-app"></a>Trabalhando com seu aplicativo de desktop

Você pode usar a camada Visual para aprimorar a aparência, a aparência e a funcionalidade do seu WPF, Windows Forms, e C++ aplicativos de desktop do Win32. Você pode migrar ilhas de conteúdo para usar a camada Visual e manter o restante da sua interface do usuário em sua estrutura existente. Isso significa que você pode fazer atualizações significativas e aprimoramentos para o seu aplicativo da interface do usuário sem precisar fazer grandes alterações em seu código existente base.

Para obter mais informações, consulte [Modernize seu aplicativo da área de trabalho usando a camada Visual](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps).

## <a name="additional-resources"></a>Recursos adicionais

* [**Documentação de referência completa para a API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* Exemplos de Interface do usuário avançada e composição no [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs)
* [Galeria de exemplos de Windows.UI.Composition](https://aka.ms/winuiapp)
* [@windowsui Feed do Twitter ](https://twitter.com/windowsui)
* Leia o artigo do MSDN de Kerr sobre essa API: [Gráficos e animação - opções de composição do Windows 10](https://msdn.microsoft.com/magazine/mt590968)
