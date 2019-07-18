---
Description: A navegação em aplicativos da Plataforma Universal do Windows (UWP) é baseada em um modelo flexível de estruturas de navegação, elementos de navegação e recursos no nível do sistema.
title: Noções básicas de navegação para aplicativos UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6290b142eee4aff7287b9542b645df89164d173b
ms.sourcegitcommit: 34671182c26f5d0825c216a6cededc02b0059a9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67286939"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Noções básicas de design de navegação para aplicativos UWP

![Cabeçalho de noções básicas de navegação](images/nav/navigation-basics-header.jpg)

Se você considera um aplicativo uma coleção de páginas, a *navegação* descreve a movimentação entre páginas e dentro de uma página. É o ponto de partida da experiência do usuário, e é como os usuários encontram o conteúdo e os recursos em que estão interessados. É muito importante, e pode ser difícil acertar.

Há várias opções de navegação. possível:

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

Exigir que os usuários passem por uma série de páginas em ordem.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

Fornecer um menu que permite aos usuários ir diretamente para qualquer página.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

Colocar tudo em uma única página e fornecer mecanismos de filtragem para exibir o conteúdo.
    :::column-end:::
:::row-end:::

Embora não haja nenhum design de navegação simples que funcione para todos os aplicativos, há princípios e diretrizes para ajudá-lo a decidir o design ideal para seu aplicativo.

## <a name="principles-of-good-navigation"></a>Princípios de boa navegação

Vamos começar com os princípios básicos do bom design de navegação:

- **Consistência:** atenda às expectativas dos usuários.
- **Simplicidade:** não faça mais que o necessário.
- **Clareza:** forneça caminhos e opções evidentes.

### <a name="consistency"></a>Consistência

A navegação deve ser consistente com as expectativas dos usuários. Usar [controles padrão](#use-the-right-controls) com os quais os usuários estão familiarizados e seguir convenções padrão para ícones, localização e estilo tornarão a navegação previsível e intuitiva para os usuários.

![imagem dos componentes da página](images/nav/page-components.svg)

> Os usuários esperam encontrar determinados elementos da interface do usuário em locais padrão.

### <a name="simplicity"></a>Simplicidade

Menos itens de navegação simplificam a tomada de decisão para os usuários. Fornecer acesso fácil a destinos importantes e ocultar itens menos importantes ajuda os usuários a chegarem onde desejam mais rapidamente.

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

Apresente os itens de navegação em um menu de navegação familiar.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

Não sobrecarregue os usuários com várias opções de navegação.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Clareza

Caminhos claros permitem navegação lógica para os usuários. Tornar as opções de navegação óbvias e esclarecer as relações entre as páginas deve impedir que os usuários se percam.

![exemplo do que fazer](images/nav/clarity-image.svg)

> Os destinos têm rótulos claros para que os usuários saibam onde estão.

## <a name="general-recommendations"></a>Recomendações gerais

Agora, vamos usar nossos princípios de design, ou seja, consistência, simplicidade e clareza, para criar algumas recomendações gerais.

1. Pense em seus usuários. Trace os caminhos típicos que eles podem tomar em seu aplicativo e, para cada página, pense no motivo do usuário estar lá e onde ele pode querer ir.

2. Evite aprofundar as hierarquias de navegação. Se você for além de três níveis de navegação, corre o risco de deixar o usuário empacado em uma hierarquia profunda da qual ele terá dificuldade de sair.

3. Evite o "pula-pula". O "pula-pula" ocorre quando há conteúdo relacionado, mas a navegação até ele requer que o usuário suba um nível e desça novamente.

## <a name="use-the-right-structure"></a>Use a estrutura correta

Agora que você está familiarizado com os princípios gerais de navegação, como deve estruturar seu aplicativo? Há duas estruturas gerais: simples e hierárquica.

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

Em uma estrutura simples/lateral, as páginas são dispostas lado a lado. Você pode ir de uma página para outra em qualquer ordem.

Recomendamos usar uma estrutura simples quando:

- As páginas podem ser vistas em qualquer ordem.
- As páginas são claramente distintas umas das outros e não têm uma relação pai/filho óbvia.
- Existem menos de oito páginas no grupo. <br>
(Quando há mais páginas, talvez seja difícil para os usuários compreenderem a exclusividade das páginas ou entender sua localização atual dentro do grupo. Se você não achar que isso é um problema para seu aplicativo, vá em frente e torne as páginas pares. Caso contrário, considere a possibilidade de usar uma estrutura hierárquica para dividir as páginas em dois ou mais grupos menores.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

Em uma estrutura hierárquica, as páginas são organizadas em uma estrutura de árvore. Cada página filho tem um pai, mas um pai pode ter uma ou mais páginas filho. Para acessar uma página filho, você passa pela página pai.

Estruturas hierárquicas são adequadas para organizar conteúdo complexo que abrange várias páginas. A desvantagem é um pouco de sobrecarga na navegação: quanto mais profunda for a estrutura, mais cliques são necessários para os usuários passarem de uma página para outra.

Recomendamos uma estrutura hierárquica quando:
        
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

Você não precisa escolher uma ou outra estrutura; muitos aplicativos com design satisfatório usam ambas. Um aplicativo pode usar estruturas simples em páginas de nível superior que podem ser exibidas em qualquer ordem, e estruturas hierárquicas em páginas que têm relações mais complexas.

Se a sua estrutura de navegação tiver vários níveis, recomendamos que elementos de navegação ponto a ponto sejam vinculados apenas aos pares em sua subárvore atual. Considere a ilustração ao lado, que mostra uma estrutura de navegação que tem dois níveis:

- No nível 1, o elemento de navegação ponto a ponto deve fornecer acesso às páginas A, B, C e D.
- No nível 2, os elementos de navegação ponto a ponto para as páginas A2 só devem vincular a outras páginas A2. Eles não devem vincular a páginas do nível 2 páginas na subárvore C.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Use os controles corretos

Após decidir-se por uma estrutura de página, precisará decidir como os usuários navegarão por essas páginas. A UWP oferece uma variedade de controles de navegação para ajudar a garantir uma experiência de navegação consistente e confiável em seu aplicativo.

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

Com algumas exceções, qualquer aplicativo com várias páginas usa um quadro. Geralmente, um aplicativo tem uma página principal que contém o quadro e um elemento de navegação principal, como um controle de modo de exibição de navegação. Quando o usuário seleciona uma página, o quadro carrega e exibe essa página.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

Exibe uma lista horizontal de links para páginas no mesmo nível. O controle [NavigationView](../controls-and-patterns/navigationview.md) implementa o painel de navegação superior e padrões de guias.
        
Use o painel de navegação superior quando:

- Quiser mostrar todas as opções de navegação na tela.
- Quiser mais espaço para o conteúdo do aplicativo.
- Os ícones não descrevem claramente as categorias de navegação do seu aplicativo.
        
Use guias quando:

- Quiser preservar o estado da página e o histórico de navegação.
- Você espera que os usuários alternem entre as guias com frequência.

:::row-end:::

:::row:::
    :::column:::
         ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivô**](../controls-and-patterns/pivot.md)
    
Similar ao [Modo de exibição de navegação](../controls-and-patterns/navigationview.md), mas com suporte adicional para toque e um comportamento de navegação um pouco diferente.
    
Use um pivô quando:
- Quiser que seu aplicativo permita a entrada por toque entre categorias
- Quiser que as opções de navegação girem infinitamente
- Não precisar de um controle abrangente em relação ao comportamento de navegação entre as categorias

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

Exibe uma lista vertical de links para páginas de nível superior. Use quando:
        
- As páginas existirem no nível superior.
- Houver muitos itens de navegação (mais de 5)
- Você não espera que os usuários alternem entre as páginas com frequência.

:::row-end:::
        
:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

Exibe uma lista (exibição mestre) de itens. Selecionar um item exibe a página correspondente na seção de detalhes. Use quando:
        
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

Elementos de navegação inseridos podem aparecer no conteúdo de uma página. Diferente de outros elementos de navegação, que devem ser consistentes em todas as páginas, os elementos de navegação de conteúdo inserido são exclusivos de uma página para outra.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Próximo: adicionar código de navegação ao aplicativo

O próximo artigo, [Implementar a navegação básica](navigate-between-two-pages.md), mostra o código necessário para usar um controle Frame que habilitará a navegação básica entre duas páginas no aplicativo.
