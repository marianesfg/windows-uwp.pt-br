---
author: jwmsft
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Camada Visual
description: A API Windows.UI.Composition concede acesso a uma camada de composição entre a camada de estrutura (XAML) e a camada de elementos gráficos (DirectX).
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2dd8c53dad735cf1094410bf97a81f6b0247bdc7
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5684458"
---
# <a name="visual-layer"></a>Camada Visual

A camada Visual fornece uma API de modo retido para elementos gráficos, efeitos e animações de alto desempenho e é a base para todas as interfaces do usuário em dispositivos Windows.Você define sua interface do usuário de maneira declarativa e a camada Visual depende da aceleração de hardware de elementos gráficos para garantir que seu conteúdo, efeitos e animações sejam renderizados de forma suave e sem falhas, de forma independente do thread de interface do usuário do aplicativo.

Destaques notáveis:

* APIs conhecidas do WinRT
* Arquitetada para interações e interface do usuário mais dinâmicos
* Conceitos alinhados com ferramentas de design
* Dimensionamento linear sem quedas repentinas de desempenho

Seus aplicativos UWP do Windows já estão usando a camada Visual por meio de uma das estruturas de interface do usuário. Você também pode tirar proveito da camada Visual diretamente para a renderização personalizada, efeitos e animações com muito pouco esforço.

![Disposição em camadas de estrutura de interface do usuário: a camada de estrutura (Windows.UI. XAML) baseia-se na camada visual (Windows.UI.Composition) que é baseada na camada de elementos gráficos (DirectX)](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>O que há na camada Visual?

As funções principais de camada Visual são:

1. **Conteúdo**: composição leve de conteúdo desenhado de forma personalizada
1. **Efeitos**: sistema de efeitos de interface do usuário em tempo real cujos efeitos podem ser animados, encadeados e personalizados
1. **Animações**: animações independentes de estrutura expressivas em execução independente do thread da interface do usuário

### <a name="content"></a>Conteúdo

O conteúdo é hospedado, transformado e disponibilizado para uso pelo sistema de animação e de efeitos usando elementos visuais. Na base da hierarquia de classe, há a classe [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), um proxy de threads leve e ágil no processo do app para o estado visual no compositor. Subclasses de Visual incluem [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) para permitir que os filhos criem árvores de elementos visuais e [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) que contém conteúdo e podem ser pintados com qualquer uma das cores sólidas, efeitos visuais ou conteúdos desenhado personalizado. Juntos, esses tipos de Visual compõem a estrutura de árvore visual para a interface do usuário 2D e contêm os FrameworkElements XAML mais visíveis.

Para saber mais, veja a visão geral de [Composição de Visual](composition-visual-tree.md).

### <a name="effects"></a>Efeitos

O sistema de efeitos na camada Visual permite que você aplique uma cadeia de efeitos de filtro e de transparência para um elemento Visual ou uma árvore de Visuals. Este é um sistema de efeitos de interface do usuário, que não deve ser confundido com efeitos de imagem e mídia. Os efeitos funcionam em conjunto com o sistema de animação, permitindo que os usuários obtenham animações suaves e dinâmicas de propriedades de efeitos, renderizadas de forma independente do thread da interface do usuário. Os efeitos na camada Visual fornecem os blocos de construção criativos que podem ser combinados e animados para construir experiências personalizadas e interativas.

Além de cadeias de efeitos que podem ser animados, a camada Visual também dá suporte a um modelo de iluminação que permite que os elementos visuais simulem as propriedades do material ao responderem às luzes que podem ser animadas. Os elementos visuais também podem projetar sombras. Iluminação e sombras podem ser combinadas para criar uma percepção de profundidade e realismo.

Para obter mais informações, consulte a visão geral de [Efeitos de composição](composition-effects.md).

### <a name="animations"></a>Animações

O sistema de animação na camada Visual permite que você mova elementos visuais, anime efeitos e promova transformações, clipes e outras propriedades.Ele é um sistema independente de estrutura projetado desde o início com o desempenho em mente.Ele é executado independentemente do thread da interface do usuário para garantir suavidade e escalabilidade.Enquanto permite que você use animações de KeyFrame para promover alterações de propriedade ao longo do tempo, ele também permite que você configure relacionamentos matemáticos entre as propriedades diferentes, incluindo entrada do usuário, permitindo que você crie experiências coreografadas contínuas de forma direta.

Para obter mais informações, consulte a visão geral de [Animações de composição](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Como trabalhar com seu aplicativo UWP XAML

Você pode acessar um Visual criado pela estrutura XAML e apoiar um FrameworkElement visível usando a classe [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) em [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908). Observe que os elementos visuais criados pela estrutura para você vêm com alguns limites em termos de personalização. Isso ocorre porque a estrutura está gerenciando deslocamentos, transformações e tempos de vida. No entanto, você pode criar seus próprios Visuals e anexá-los a um elemento XAML existente por meio de ElementCompositionPreview ou adicionando-os a um ContainerVisual existente em algum lugar na estrutura de árvore visual.

Para saber mais, confira a visão geral [Uso da camada Visual com XAML](using-the-visual-layer-with-xaml.md).

## <a name="additional-resources"></a>Recursos adicionais

* [**Documentação de referência completa da API**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
* Exemplos de Interface do usuário avançada e composição no [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs)
* [Windows.UI.Composition Sample Gallery](https://aka.ms/winuiapp)
* [Feed de @windowsui no Twitter ](https://twitter.com/windowsui)
* Leia o artigo de Kenny Kerr no MSDN sobre essa API: [Gráficos e Animação - Opções do Windows Composition 10](https://msdn.microsoft.com/magazine/mt590968)
