---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Camada Visual
description: A API Windows.UI.Composition concede acesso a uma camada de composição entre a camada de estrutura (XAML) e a camada de elementos gráficos (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ac41d461982a39a939e460b7a81b144e5a08fdb3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255520"
---
# <a name="visual-layer"></a>Camada visual

A camada Visual fornece uma API de modo retido para elementos gráficos, efeitos e animações de alto desempenho e é a base para todas as interfaces do usuário em dispositivos Windows. Você define sua interface de usuário de maneira declarativa e a camada Visual depende da aceleração de hardware de gráficos para garantir que o conteúdo, os efeitos e as animações sejam renderizados de forma suave e livre de falhas, independentemente do thread de interface do usuário do aplicativo.

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

O conteúdo é hospedado, transformado e disponibilizado para uso pelo sistema de animação e de efeitos usando elementos visuais. Na base da hierarquia de classe, há a classe [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual), um proxy de threads leve e ágil no processo do app para o estado visual no compositor. As subclasses do Visual incluem  [**ContainerVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) para permitir que os filhos criem árvores de visuais e [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) que contenham conteúdo e possam ser pintados com cores sólidas, conteúdo desenhado personalizado ou efeitos visuais. Juntos, esses tipos de Visual compõem a estrutura de árvore visual para a interface do usuário 2D e contêm os FrameworkElements XAML mais visíveis.

Para saber mais, veja a visão geral de [Composição de Visual](composition-visual-tree.md).

### <a name="effects"></a>Efeitos

O sistema de efeitos na camada Visual permite que você aplique uma cadeia de efeitos de filtro e de transparência para um elemento Visual ou uma árvore de Visuals. Este é um sistema de efeitos de interface do usuário, que não deve ser confundido com efeitos de imagem e mídia. Os efeitos funcionam em conjunto com o sistema de animação, permitindo que os usuários obtenham animações suaves e dinâmicas de propriedades de efeitos, renderizadas de forma independente do thread da interface do usuário. Os efeitos na camada Visual fornecem os blocos de construção criativos que podem ser combinados e animados para construir experiências personalizadas e interativas.

Além de cadeias de efeitos que podem ser animados, a camada Visual também dá suporte a um modelo de iluminação que permite que os elementos visuais simulem as propriedades do material ao responderem às luzes que podem ser animadas. Os elementos visuais também podem projetar sombras. Iluminação e sombras podem ser combinadas para criar uma percepção de profundidade e realismo.

Para obter mais informações, consulte a visão geral de [Efeitos de composição](composition-effects.md).

### <a name="animations"></a>Animações

O sistema de animação na camada Visual permite que você mova elementos visuais, anime efeitos e promova transformações, clipes e outras propriedades.  É um sistema independente de estrutura que foi projetado desde o início com o desempenho em mente.  Ele é executado independentemente do thread da interface do usuário para garantir a suavidade e a escalabilidade.  Embora ele permita que você use animações com quadro-chave familiares ao longo do tempo, ele também permite que você configure relações matemáticas entre diferentes propriedades, incluindo a entrada do usuário, permitindo que você crie diretamente experiências de coreografado diretas.

Para obter mais informações, consulte a visão geral de [Animações de composição](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Como trabalhar com seu aplicativo UWP XAML

Você pode acessar um Visual criado pela estrutura XAML e apoiar um FrameworkElement visível usando a classe [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) em [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting). Observe que os elementos visuais criados pela estrutura para você vêm com alguns limites em termos de personalização. Isso ocorre porque a estrutura está gerenciando deslocamentos, transformações e tempos de vida. No entanto, você pode criar seus próprios Visuals e anexá-los a um elemento XAML existente por meio de ElementCompositionPreview ou adicionando-os a um ContainerVisual existente em algum lugar na estrutura de árvore visual.

Para saber mais, confira a visão geral [Uso da camada Visual com XAML](using-the-visual-layer-with-xaml.md).

### <a name="working-with-your-desktop-app"></a>Trabalhando com seu aplicativo de área de trabalho

Você pode usar a camada Visual para aprimorar a aparência e a funcionalidade de seus aplicativos de área de trabalho do C++ WPF, Windows Forms e Win32. Você pode migrar ilhas de conteúdo para usar a camada visual e manter o restante da sua interface do usuário em sua estrutura existente. Isso significa que você pode fazer atualizações e aprimoramentos significativos em sua interface do usuário do aplicativo sem a necessidade de fazer alterações extensivas em sua base de código existente.

Para obter mais informações, consulte [Modernize seu aplicativo da área de trabalho usando a camada Visual](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps).

## <a name="additional-resources"></a>Recursos adicionais

* [**Documentação de referência completa para a API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* Exemplos de Interface do usuário avançada e composição no [WindowsUIDevLabs GitHub](https://github.com/microsoft/WindowsCompositionSamples)
* [Galeria de exemplos do Windows. UI. composição](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui feed do Twitter](https://twitter.com/windowsui)
* Leia o artigo de Kenny Kerr no MSDN sobre essa API: [Gráficos e Animação - Opções do Windows Composition 10](https://msdn.microsoft.com/magazine/mt590968)
