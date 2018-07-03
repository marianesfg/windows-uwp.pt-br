---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Noções básicas de navegação para aplicativos UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0980f24394a075596b60e4a8005b303857b91304
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843066"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Noções básicas de design de navegação para aplicativos UWP

![Cabeçalho de noções básicas de navegação](images/nav/navigation-basics-header.jpg)

Se você considera um aplicativo uma coleção de páginas, *navegação* descreve o ato de movimentação entre páginas e em uma página. É o ponto de partida da experiência do usuário, e é como os usuários encontram o conteúdo e recursos em que estão interessados. É muito importante, e pode ser difícil acertar.

Há várias opções de navegação. É possível:

:::linha::: :::coluna::: ![exemplo de navegação 1](images/nav/nav-1.svg)

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
:::fim da linha:::

Embora não haja nenhum design de navegação simples que funcione para todos os aplicativos, há princípios e diretrizes para ajudá-lo a decidir o design ideal para seu aplicativo.

## <a name="principles-of-good-navigation"></a>Princípios de boa navegação

Vamos começar com os princípios básicos de bom design de navegação:

- **Consistência:** atenda às expectativas do usuário.
- **Simplicidade**: não faça mais que o necessário.
- **Clareza:** forneça caminhos claros e opções.

### <a name="consistency"></a>Consistência

A navegação deve ser consistente com as expectativas dos usuários. Usar [controles padrão](#use-the-right-controls) com os quais os usuários estão familiarizados e seguir convenções padrão para ícones, localização, e estilo tornarão a navegação previsível e intuitiva para os usuários.

![imagem dos componentes da página](images/nav/page-components.svg)

> Os usuários esperam encontrar determinados elementos da interface do usuário em locais padrão.

### <a name="simplicity"></a>Simplicidade

Menos itens de navegação simplificam a tomada de decisão para os usuários. Fornecer acesso fácil a destinos importantes e ocultar itens menos importantes ajudarão os usuários a chegarem onde desejam mais rapidamente.

:::linha::: :::coluna::: ![exemplo do que fazer](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Constantly provide many navigation options to overwhelm the user.
    :::column-end:::
:::fim da linha:::

### <a name="clarity"></a>Clareza

Caminhos claros permitem navegação lógica para os usuários. Tornar as opções de navegação óbvias e esclarecer as relações entre as páginas deve impedir que os usuários se percam.

![exemplo do que fazer](images/nav/clarity-image.svg)

> Os destinos têm rótulos claros para que os usuários saibam onde eles estão.

## <a name="general-recommendations"></a>Recomendações gerais

Agora, vamos usar nossos princípios de design, ou seja, consistência, simplicidade e clareza, para criar algumas recomendações gerais.

1. Pense em seus usuários. Trace os caminhos típicos que eles podem tomar por meio de seu aplicativo e, para cada página, pense no motivo de o usuário estar lá e onde eles podem querer ir. 

2. Evite aprofundar as hierarquias de navegação. Se você for além de três níveis de navegação, você corre o risco de deixar o usuário empacado em uma hierarquia profunda da qual ele terá dificuldade de sair.

3. Evite o "pula-pula". O "pula-pula" ocorre quando há conteúdo relacionado, mas a navegação até ele requer que o usuário suba um nível e desça novamente.

## <a name="use-the-right-structure"></a>Use a estrutura correta

Agora que você está familiarizado com os princípios gerais de navegação, como você deve estruturar seu aplicativo? Existem duas estruturas gerais: simples e hierárquica.

:::linha::: :::coluna::: ![Páginas organizadas em uma estrutura simples](images/nav/flat-lateral-structure.svg) :::fim da coluna::: :::extensão da coluna="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from on page to another in any order. 

        We recommend using a flat structure when:
        <ul>
        <li>The pages can be viewed in any order.</li>
        <li>The pages are clearly distinct from each other and don't have an obvious parent/child relationship.</li>
        <li>There are fewer than 8 pages in the group.<br/>
        (When there are more pages, it might be difficult for users to understand how the pages are unique or to understand their current location within the group. If you don't think that's an issue for your app, go ahead and make the pages peers. Otherwise, consider using a hierarchical structure to break the pages into two or more smaller groups.)</li>
        </ul>

    :::column-end:::
:::fim da linha:::

:::linha::: :::coluna::: ![Páginas organizadas em uma hierarquia](images/nav/hierarchical-structure.svg) :::fim da coluna::: :::extensão da coluna="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page. 

        We recommend a hiearchical structure when:
        <ul>
        <li>Pages should be traversed in a specific order.</li>
        <li>There is a clear parent-child relationship between pages.</li>
        <li>There are more than 7 pages in the group.</li>
        </ul>
    :::column-end:::
:::fim da linha:::

:::linha::: :::coluna::: ![um aplicativo com uma estrutura híbrida](images/nav/combining-structures.svg) :::fim da coluna::: :::extensão da coluna="2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - At level 1, the peer-to-peer navigation element should provide access to pages A, B, C, and D.
        - At level 2, the peer-to-peer navigation elements for the A2 pages should only link to the other A2 pages. They should not link to level 2 pages in the C subtree. 
    :::column-end:::
:::fim da linha:::

## <a name="use-the-right-controls"></a>Use os controles corretos

Após decidir-se por uma estrutura de página, você precisará decidir como os usuários navegarão por essas páginas. A UWP oferece uma variedade de controles de navegação para ajudar a garantir uma experiência de navegação consistente e confiável em seu aplicativo. 

É recomendável selecionar um controle de navegação com base no número de elementos de navegação em seu aplicativo. Se você tiver cinco ou menos itens de navegação, utilize navegação de nível superior, como [guias e pivô](../controls-and-patterns/tabs-pivot.md). Se você tem seis ou mais itens de navegação, use a navegação à esquerda, como [modo de exibição navegação](../controls-and-patterns/navigationview.md) ou [mestre/detalhes](../controls-and-patterns/master-details.md).

:::linha::: :::coluna::: ![Imagem do quadro](images/nav/thumbnail-frame.svg) :::fim da coluna::: :::extensão da coluna="2"::: <a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame"><b>Quadro</b></a>

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::fim da linha:::

:::linha::: :::coluna::: ![guias e imagem pivô](images/nav/thumbnail-tabs-pivot.svg) :::fim da coluna::: :::extensão da coluna="2"::: <a href="../controls-and-patterns/tabs-pivot.md"><b>Guias e pivô</b></a><br>

        Displays a horizontal list of links to pages at the same level. Use when:
        <ul>
        <li>There are 2-5 pages. (You can use tabs/pivots when there are more than 5 pages, but it might be difficult to fit all the tabs/pivots on the screen.)</li>
        <li>You expect users to switch between pages frequently.</li>
        </ul>
:::fim da linha:::

:::linha::: :::coluna::: ![guias e imagem pivô](images/nav/thumbnail-navview.svg) :::fim da coluna::: :::extensão da coluna="2"::: <a href="../controls-and-patterns/navigationview.md"><b>Exibição de navegação</b></a><br>

        Displays a vertical list of links to top-level pages. Use when:
        <ul>
        <li>The pages exist at the top level.</li>
        <li>There are many navigational items (more than 5).</li>
        <li>You don't expect users to switch between pages frequently.</li>
        </ul>
:::fim da linha:::

:::linha::: :::coluna::: ![Imagem de detalhes principal](images/nav/thumbnail-master-detail.svg) :::fim da coluna::: :::extensão da coluna="2"::: <a href="../controls-and-patterns/master-details.md"><b>Principal/detalhes</b></a><br>

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        <ul>
        <li>You expect users to switch between child items frequently.</li>
        <li>You want to enable the user to perform high-level operations, such as deleting or sorting, on individual items or groups of items, and also want to enable the user to view or update the details for each item.</li>
        </ul>

        Master/details is well suited for email inboxes, contact lists, and data entry.
:::fim da linha:::

:::linha::: :::coluna::: ![Hiperlinks e imagem de botões](images/nav/thumbnail-hyperlinks-buttons.svg) :::fim da coluna::: :::extensão da coluna="2"::: <a href="../controls-and-patterns/hyperlinks.md"><b>Hiperlinks</b></a> e <a href="../controls-and-patterns/buttons.md"><b>botões</b></a><br>

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::fim da linha:::

## <a name="next-add-navigation-code-to-your-app"></a>Próximo: Adicionar código de navegação ao aplicativo

O próximo artigo, [Implementar a navegação básica](navigate-between-two-pages.md), mostra o código necessário para usar um controle de Quadro que habilitará a navegação básica entre duas páginas no aplicativo. 