---
Description: O modo de seleção permite que os usuários selecionem e executem ações sobre um ou vários itens.
title: Modo de seleção
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d7b781e6074468fbe73446e4057e36ff31266d05
ms.sourcegitcommit: 9625f8fb86ff6473ac2851e600bc02e996993660
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165093"
---
# <a name="selection-mode-overview"></a>Visão geral do modo de seleção

O modo de seleção permite que os usuários selecionem e executem ações em um item ou em vários itens. Ele pode ser invocado por meio de um menu de contexto usando CTRL+clique ou SHIFT+clique em um item ou rolando um destino em um item em um modo de exibição de galeria. Quando o modo de seleção está ativo, as caixas de seleção aparecem ao lado de cada item da lista, e as ações podem aparecer na parte superior ou inferior da tela.

## <a name="different-selection-modes"></a>Modos de seleção diferentes
Há três modos de seleção diferentes:

- Único: o usuário pode selecionar apenas um item de cada vez.
- Múltiplo: o usuário pode selecionar vários itens sem usar um modificador.
- Estendido: o usuário pode selecionar diversos itens com um modificador, como ao segurar a tecla SHIFT.

## <a name="selection-mode-examples"></a>Exemplos de modos de seleção
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>Aqui está um ListView com o modo de seleção única habilitado.
![Modo de exibição de lista com modo de seleção única](images/listview-selection-single.png)

O item Art Venere está selecionado e o item Mitsue Tollner está sendo focalizado no momento.

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>Aqui está um ListView com o modo de seleção múltipla habilitado:
![Modo de exibição de lista com modo de seleção múltipla](images/listview-selection-multiple.png)

Há três itens selecionados e o item Donette Foller está sendo focalizado.

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>Aqui está um GridView com o modo de seleção única habilitado:
![Modo de exibição de grade com modo de seleção única](images/gridview-selection-single.png)

O item 7 está selecionado e o item 3 está sendo focalizado no momento.

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>Aqui está um GridView com o modo de seleção múltipla habilitado:
![Modo de exibição de grade com modo de seleção múltipla](images/gridview-selection-multiple.png)

Os itens 2, 4 e 5 estão selecionados e o item 7 está sendo focalizado no momento.

## <a name="behavior-and-interaction"></a>Comportamento e interação
Tocar em qualquer lugar em um item seleciona-o. Tocar na ação da barra de comandos afeta todos os itens selecionados. Se nenhum item estiver selecionado, as ações da barra de comandos devem estar inativas, com exceção de "Selecionar Tudo".

O modo de seleção não tem um modelo light dismiss; tocar fora do quadro no qual o modo de seleção está ativo não cancelará o modo. Isso é para evitar a desativação acidental do modo. Clicar no botão Voltar ignora o modo de seleção múltipla.

Mostre uma confirmação visual quando uma ação for selecionada. Considere exibir uma caixa de diálogo de confirmação para determinadas ações, especialmente ações destrutivas, como excluir.

O modo de seleção está limitado à página na qual ele está ativo e não pode afetar os itens fora da página.

O ponto de entrada para o modo de seleção deve estar justaposto em relação ao conteúdo que ele afeta.

Veja recomendações sobre a barra de comandos em [Diretrizes de barras de comandos](app-bars.md).

Para saber mais sobre os modos de seleção e como manipular eventos de seleção para determinados controles, visite a página de diretrizes daquele controle específico.
