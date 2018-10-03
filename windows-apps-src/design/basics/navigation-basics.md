---
author: QuinnRadich
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Noções básicas de navegação para aplicativos UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: quradic
ms.date: 7/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b731910f53a6152554b74e946374234b827f4a86
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4263863"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Noções básicas de design de navegação para aplicativos UWP

![Cabeçalho de noções básicas de navegação](images/nav/navigation-basics-header.jpg)

Se você considera um aplicativo uma coleção de páginas, *navegação* descreve o ato de movimentação entre páginas e em uma página. É o ponto de partida da experiência do usuário, e é como os usuários encontram o conteúdo e recursos em que estão interessados. É muito importante, e pode ser difícil acertar.

Há várias opções de navegação. É possível:

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

        Require users to go through a series of pages in order.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

        Provide a menu that allows users to jump directly to any page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

        Place everything on a single page and provide filtering mechanisms for viewing content.
    :::column-end:::
:::row-end:::

Embora não haja nenhum design de navegação simples que funcione para todos os aplicativos, há princípios e diretrizes para ajudá-lo a decidir o design ideal para seu aplicativo.

## <a name="principles-of-good-navigation"></a>Princípios de boa navegação

Vamos começar com os princípios básicos de bom design de navegação:

- **Consistência:** atenda às expectativas do usuário.
- **Simplicidade**: não faça mais que o necessário.
- **Clareza:** forneça caminhos claros e opções.

### <a name="consistency"></a>Consistência

A navegação deve ser consistente com as expectativas dos usuários. Usando [controles padrão](#use-the-right-controls) que os usuários estão familiarizados com e seguir convenções padrão para ícones, localização e estilo tornarão navegação previsível e intuitiva para os usuários.

![imagem dos componentes da página](images/nav/page-components.svg)

> Os usuários esperam encontrar determinados elementos da interface do usuário em locais padrão.

### <a name="simplicity"></a>Simplicidade

Menos itens de navegação simplificam a tomada de decisão para os usuários. Fornecer acesso fácil a destinos importantes e ocultar itens menos importantes ajudarão os usuários a chegarem onde desejam mais rapidamente.

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Overwhelm users with many navigation options.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Clareza

Caminhos claros permitem navegação lógica para os usuários. Tornar as opções de navegação óbvias e esclarecer as relações entre as páginas deve impedir que os usuários se percam.

![exemplo do que fazer](images/nav/clarity-image.svg)

> Os destinos têm rótulos claros para que os usuários saibam onde eles estão.

## <a name="general-recommendations"></a>Recomendações gerais

Agora, vamos usar nossos princípios de design, ou seja, consistência, simplicidade e clareza, para criar algumas recomendações gerais.

1. Pense em seus usuários. Trace os caminhos típicos que eles podem tomar por meio de seu aplicativo e, para cada página, pense no motivo de o usuário estar lá e onde eles podem querer ir.

2. Evite hierarquias de navegação profunda. Se você for além de três níveis de navegação, você corre o risco de deixar o usuário empacado em uma hierarquia profunda da qual ele terá dificuldade de sair.

3. Evite o "pula-pula". O "pula-pula" ocorre quando há conteúdo relacionado, mas a navegação até ele requer que o usuário suba um nível e desça novamente.

## <a name="use-the-right-structure"></a>Use a estrutura correta

Agora que você está familiarizado com os princípios gerais de navegação, como você deve estruturar seu aplicativo? Existem duas estruturas gerais: simples e hierárquica.

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from one page to another in any order.

        We recommend using a flat structure when:

        - As páginas podem ser exibidas em qualquer ordem.
        - As páginas são claramente distintas umas das outros e não têm uma relação pai/filho óbvia.
        - Há menos de 8 páginas no grupo. <br>
        (Quando há mais páginas, talvez seja difícil para os usuários compreender como as páginas são únicas ou entender sua localização atual dentro do grupo. Se você não achar que isso é um problema para seu aplicativo, vá em frente e torne as páginas pares. Caso contrário, considere a possibilidade de usar uma estrutura hierárquica para quebrar as páginas em dois ou mais grupos menores.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page.

        We recommend a hierarchical structure when:
        
        - As páginas devem ser percorridas em uma ordem específica.
        - Há uma relação de pai-filho clara entre as páginas.
        - Existem mais de sete páginas no grupo.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![an app with a hybrid structure](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - No nível 1, o elemento de navegação ponto a ponto deve fornecer acesso às páginas A, B, C e D.
        - No nível 2, os elementos de navegação ponto a ponto para as páginas A2 só devem vincular a outras páginas A2. Eles não devem vincular a páginas do nível 2 páginas na subárvore C.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Use os controles corretos

Após decidir-se por uma estrutura de página, você precisará decidir como os usuários navegarão por essas páginas. A UWP oferece uma variedade de controles de navegação para ajudar a garantir uma experiência de navegação consistente e confiável em seu aplicativo.

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

        Displays a horizontal list of links to pages at the same level. The [NavigationView](../controls-and-patterns/navigationview.md) control implements the top navigation and tabs patterns.
        
        Use top navigation when:

        - Você deseja mostrar todas as opções de navegação na tela.
        - Desejam mais espaço para conteúdo do seu aplicativo.
        - Ícones não descrevem claramente as categorias de navegação.
        
        Use guias quando:

        - Você deseja preservar o estado de página e histórico de navegação.
        - Você espera que os usuários alternem entre as guias com frequência.

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

        Displays a vertical list of links to top-level pages. Use when:
        
        - As páginas estiverem no nível superior.
        - Há muitos itens de navegação (mais de 5)
        - Você não espera que os usuários alternem entre as páginas com frequência.
        
:::row-end:::

:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        
        - Você espera que os usuários alternem entre os itens filho com frequência.
        - Você quer permitir que o usuário execute operações de alto nível, como excluir ou classificar, em itens individuais ou grupos de itens, e também deseja permitir que o usuário exiba ou atualize os detalhes de cada item.

        Mestre/detalhes é adequado para caixas de entrada de email, listas de contatos e entrada de dados.
:::row-end:::

:::row:::
    :::column:::
        ![Hyperlinks and buttons image](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Próximo: Adicionar código de navegação ao aplicativo

O próximo artigo, [Implementar a navegação básica](navigate-between-two-pages.md), mostra o código necessário para usar um controle de Quadro que habilitará a navegação básica entre duas páginas no aplicativo.