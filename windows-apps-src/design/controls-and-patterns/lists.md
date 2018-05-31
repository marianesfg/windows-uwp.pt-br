---
author: muhsinking
Description: Lists display and enable interaction with collection-based content.
title: Listas
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Lists
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 85151700f4050c0cefa7d7cca893cffa3c8252f4
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1655135"
---
# <a name="lists"></a>Listas

Listas são exibidas e permitem interações com conteúdo baseado em coleção. Os quatro padrões de lista abordados neste artigo incluem:

-   Modos de exibição de lista, que são usados principalmente para exibir coleções de conteúdo com muito texto
-   Modos de exibição de grade, que são usados principalmente para exibir coleções de conteúdo com muitas imagens
-   Listas suspensas, que permitem que os usuários escolham um item em uma lista de expansão
-   Caixas de lista, que permitem que os usuários escolham um item ou vários itens em uma caixa que pode ser rolada

Diretrizes de design, recursos e exemplos são fornecidos para cada padrão de lista.

> **APIs importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/br242878), [classe GridView](https://msdn.microsoft.com/library/windows/apps/br242705), [classe ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348)


> <div id="main">
> <strong>Windows 10 Fall Creators Update - Mudança de comportamento</strong>
> </div>
> Por padrão, em vez de realizar uma seleção, a caneta ativa agora fará rolagem/movimento panorâmico em listas em aplicativos UWP (como toque, touchpad e caneta passiva).
> Se o seu aplicativo depende do comportamento anterior, você pode substituir a rolagem com caneta e reverter para o comportamento anterior. Consulte tópico de referência de API [ScrollViewer Class] (https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) para obter detalhes.

## <a name="list-views"></a>Modos de exibição de lista

Modos de exibição de lista permitem que você classifique itens e atribua cabeçalhos de grupo, arraste e solte itens, corrija conteúdo e reordene os itens.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um modo de exibição de lista para:

-   Exiba uma coleção de conteúdos que consistem principalmente em texto.
-   Navegue em uma coleção de conteúdos única ou categorizada.
-   Crie o painel mestre no [padrão mestre/detalhes](master-details.md). Um padrão mestre/detalhes é usado com frequência em aplicativos de email, em que um painel (o mestre) tem uma lista de itens selecionáveis e o outro painel (detalhes) tem uma exibição detalhada do item selecionado.

### <a name="examples"></a>Exemplos

Veja uma exibição de lista simples que mostra dados agrupados em um telefone.

![Uma exibição de lista com dados agrupados](images/simple-list-view-phone.png)

### <a name="recommendations"></a>Recomendações

-   Os itens em uma lista devem ter o mesmo comportamento.
-   Se a sua lista estiver dividida em grupos, você pode usar o [zoom semântico](semantic-zoom.md), para tornar mais fácil para os usuários navegarem pelo conteúdo agrupado.

### <a name="list-view-articles"></a>Artigos sobre exibição de lista
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Exibição de lista e exibição de grade</a></p></td>
<td align="left"><p>Conheça as noções básicas do uso de uma exibição de lista ou grade em seu aplicativo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Contêineres e modelos de itens</a></p></td>
<td align="left"><p>Os itens exibidos em uma lista ou grade podem desempenhar uma função importante na aparência geral do seu aplicativo. Modifique modelos de controle e modelos de dados para definir a aparência dos itens e deixar seu aplicativo com uma ótima aparência.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">Modelos de item para visualização de lista</a></p></td>
<td align="left"><p>Use esses exemplos de modelos de item para uma ListView para obter a aparência de tipos de app comuns.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">Listas invertidas</a></p></td>
<td align="left"><p>As listas invertidas têm novos itens adicionados na parte inferior, como em um aplicativo de chat. Siga estas orientações para usar uma lista invertida em seu aplicativo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">Puxar para atualizar</a></p></td>
<td align="left"><p>O padrão puxar para atualizar permite a um usuário extrair uma lista de dados com toque para recuperar mais dados. Use estas orientações para implementar esse padrão em sua exibição de lista.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface do usuário aninhada</a></p></td>
<td align="left"><p>Interface do usuário aninhada é uma interface do usuário (IU) que expõe controles acionáveis colocados dentro de um contêiner que o usuário também pode usar. Por exemplo, você pode ter um item de exibição de lista que contém um botão e o usuário pode selecionar o item de lista ou pressionar o botão aninhado dentro dele. Siga estas práticas recomendadas para oferecer a melhor experiência da interface do usuário aninhada para seus usuários.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>Exibições em grade

Modos de exibição de grade são adequados para organizar e navegar em coleções de conteúdos baseadas em imagens. Um layout de modo de exibição de grade rola verticalmente e faz movimento panorâmico na horizontal. Os itens são dispostos na ordem de leitura da esquerda para a direita, depois de cima para baixo.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um modo de exibição de lista para:

-   Exiba uma coleção de conteúdos que consista principalmente em imagens.
-   Exiba bibliotecas de conteúdo.
-   Formate os dois modos de exibição de conteúdo associados ao [zoom semântico](semantic-zoom.md).

### <a name="examples"></a>Exemplos

Este exemplo mostra um layout de modo de exibição de grade típico, neste caso, para navegar aplicativos. Metadados para itens de modo de exibição de grade são geralmente restritos a algumas linhas de texto e uma classificação de item.

![Exemplo de um layout de exibição de grade](images/controls_gridview_example02.png)

Um modo de exibição de grade é a solução ideal para uma biblioteca de conteúdo, que geralmente é usada para apresentar mídia, como fotos e vídeos. Em uma biblioteca de conteúdo, os usuários esperam tocar em um item para invocar uma ação.

![Exemplo de uma biblioteca de conteúdo](images/controls_list_contentlibrary.png)

### <a name="recommendations"></a>Recomendações

-   Os itens em uma lista devem ter o mesmo comportamento.
-   Se a sua lista estiver dividida em grupos, você pode usar o [zoom semântico](semantic-zoom.md), para tornar mais fácil para os usuários navegarem pelo conteúdo agrupado.

### <a name="grid-view-articles"></a>Artigos sobre exibição de grade
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Exibição de lista e exibição de grade</a></p></td>
<td align="left"><p>Conheça as noções básicas do uso de uma exibição de lista ou grade em seu aplicativo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Contêineres e modelos de itens</a></p></td>
<td align="left"><p>Os itens exibidos em uma lista ou grade podem desempenhar uma função importante na aparência geral do seu aplicativo. Modifique modelos de controle e modelos de dados para definir a aparência dos itens e deixar seu aplicativo com uma ótima aparência.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">Modelos de item para visualização de grade</a></p></td>
<td align="left"><p>Use esses exemplos de modelos de item para uma GridView para obter a aparência de tipos de app comuns.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface do usuário localizada</a></p></td>
<td align="left"><p>Interface do usuário aninhada é uma interface do usuário (IU) que expõe controles acionáveis colocados dentro de um contêiner que o usuário também pode usar. Por exemplo, você pode ter um item de exibição de lista que contém um botão e o usuário pode selecionar o item de lista ou pressionar o botão aninhado dentro dele. Siga estas práticas recomendadas para oferecer a melhor experiência da interface do usuário aninhada para seus usuários.</p></td>
</tr>
</tbody>
</table>

## <a name="drop-down-lists"></a>Listas suspensas

Listas suspensas, também conhecidas como caixas de combinação, começam em um estado compacto e se expandem para mostrar uma lista de itens selecionáveis. O item selecionado fica sempre visível, e os itens não visíveis podem ser exibidos quando o usuário toca na caixa de combinação para expandi-la.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

-   Use um controle de lista suspensa para permitir aos usuários selecionar um ou mais valores de um conjunto de itens que podem ser representados adequadamente com linhas de texto únicas.
-   Use um modo de exibição de lista ou de grade em vez de uma caixa de combinação para exibir itens que contenham várias linhas de texto ou imagens.
-   Quando houver menos de cinco itens, considere a possibilidade de usar [botões de opção](radio-button.md) (se somente um item puder ser selecionado) [ou caixas de seleção](checkbox.md) (se vários itens puderem ser selecionados).
-   Use a caixa de combinação quando os itens de seleção forem de importância secundária no fluxo do seu aplicativo. Se a opção padrão for recomendada para a maioria dos usuários em grande parte das situações, mostrar todos os itens usando uma exibição de lista pode chamar mais atenção para as opções do que o necessário. Você pode economizar espaço e minimizar a distração usando uma caixa de combinação.

### <a name="examples"></a>Exemplos

Uma caixa de combinação no estado compacto pode mostrar um cabeçalho.

![Exemplo de uma lista suspensa no estado compacto](images/combo_box_collapsed.png)

Embora caixas de combinação se expandam para dar suporte a tamanhos maiores de cadeia de caracteres, evite cadeias de caracteres excessivamente longas que são difíceis de ler.

![Exemplo de uma lista suspensa com a cadeia de caracteres de texto longo](images/combo_box_listitemstate.png)

Se a coleção em uma caixa de combinação for grande o suficiente, será exibida uma barra de rolagem para acomodá-la. Agrupe itens logicamente na lista.

![Exemplo de uma barra de rolagem em uma lista suspensa](images/combo_box_scroll.png)

### <a name="recommendations"></a>Recomendações

-   Limite o conteúdo de texto dos itens da caixa de combinação a uma única linha.
-   Classifique os itens em uma caixa de combinação na ordem mais lógica. Agrupe opções relacionadas e coloque as opções mais comuns na parte superior. Classifique os nomes em ordem alfabética, os números em ordem numérica e as datas em ordem cronológica.
-   Para fazer uma caixa de combinação com atualizações ao vivo enquanto o usuário estiver usando as teclas de seta (como um menu suspenso de seleção de Fonte), defina SelectionChangedTrigger como "Sempre".  

### <a name="text-search"></a>Pesquisa de texto

As caixas de combinação suportam automaticamente pesquisas dentro de suas coleções. Como os usuários digitam caracteres em um teclado físico enquanto enfocam uma caixa de combinação aberta ou fechada, os candidatos que correspondem à cadeia do usuário são inseridos na exibição. Essa funcionalidade é especialmente útil quando estiver navegando uma longa lista. Por exemplo, quando interage com uma lista suspensa contendo uma lista de estados, os usuários podem pressionar a tecla "w" para trazer "Washington" até a exibição para que haja uma seleção rápida.


## <a name="list-boxes"></a>Caixas de listagem

Uma caixa de listagem permite que o usuário escolha um único item ou vários itens de uma coleção. Caixas de listagem são semelhantes a listas suspensas, exceto que as caixas de listagem ficam sempre abertas: não há estado compacto (não expandido) para uma caixa de listagem. Pode-se fazer a rolagem pelos itens em uma lista se não houver espaço para mostrar tudo.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

-   Uma caixa de listagem pode ser útil quando itens da lista são importantes o suficiente para serem exibidos em destaque e quando há espaço suficiente na tela para mostrar a lista completa.
-   Uma caixa de listagem deve chamar a atenção do usuário para todo o conjunto de alternativas em uma escolha importante. Por outro lado, uma lista suspensa inicialmente atrai a atenção do usuário para o item selecionado.
-   Evite usar uma caixa de listagem se:
    -   Houver um número muito pequeno de itens para a lista. Uma caixa de listagem que tem sempre as mesmas duas opções pode ser melhor apresentada como [botões de opção](radio-button.md). Considere também a possibilidade de usar botões de opção quando há três ou quatro itens estáticos na lista.
    -   A caixa de listagem é de seleção única e apresenta sempre as mesmas 2 opções, onde uma pode ser imposta como o contrário da outra, como “ligado” e “desligado”. Use uma única caixa de seleção ou um botão de alternância.
    -   Há um número muito grande de itens. Exibição de grade e exibição de lista são escolhas melhores para listas longas. Para listas muito longas de dados agrupados, prefere-se a aplicação de zoom semântico.
    -   Os itens são valores numéricos contíguos. Se esse for o caso, considere usar um [controle deslizante](slider.md).
    -   Os itens de seleção são de importância secundária no fluxo de seu aplicativo, ou a opção padrão é recomendada para a maioria dos usuários na maioria das situações. Utilize uma lista suspensa em vez disso.

### <a name="recommendations"></a>Recomendações

-   O intervalo ideal de itens em uma caixa de listagem é de 3 a 9.
-   Uma caixa de listagem funciona bem quando seus itens podem variar dinamicamente.
-   Se possível, defina o tamanho de uma caixa de listagem de modo que a lista de itens presente nela não precise de rolagem nem de aplicação de movimento panorâmico.
-   Verifique se está claro o propósito da caixa de listagem e quais itens estão selecionados atualmente.
-   Reserve efeitos visuais e animações para feedback referente a toque e para o estado selecionado dos itens.
-   Limite o conteúdo de texto do item da caixa de listagem a uma única linha. Se os itens forem elementos visuais, você poderá personalizar o tamanho. Se um item contém diversas linhas de texto ou imagens, utilize um modo de exibição de grade ou de lista em vez disso.
-   Use fonte padrão, a menos que as diretrizes da marca indiquem o contrário.
-   Não utilize uma caixa de listagem para executar comandos ou exibir ou ocultar dinamicamente outros controles.

## <a name="selection-mode"></a>Modo de seleção

O modo de seleção permite que os usuários selecionem e executem ações em um item ou em vários itens. Ele pode ser invocado por meio de um menu de contexto usando CTRL+clique ou SHIFT+clique em um item ou rolando um destino em um item em um modo de exibição de galeria. Quando o modo de seleção está ativo, as caixas de seleção aparecem ao lado de cada item da lista, e as ações podem aparecer na parte superior ou inferior da tela.

Há três modos de seleção diferentes:

-   Única: o usuário pode selecionar apenas um item de cada vez.
-   Múltipla: o usuário pode selecionar vários itens sem usar um modificador.
-   Estendida: o usuário pode selecionar vários itens com um modificador, por exemplo, segurando a tecla SHIFT.

Tocar em qualquer lugar em um item seleciona-o. Tocar na ação da barra de comandos afeta todos os itens selecionados. Se nenhum item estiver selecionado, as ações da barra de comandos devem estar inativas, com exceção de "Selecionar Tudo".

O modo de seleção não tem um modelo light dismiss; tocar fora do quadro no qual o modo de seleção está ativo não cancelará o modo. Isso é para evitar a desativação acidental do modo. Clicar no botão Voltar ignora o modo de seleção múltipla.

Mostre uma confirmação visual quando uma ação for selecionada. Considere exibir uma caixa de diálogo de confirmação para determinadas ações, especialmente ações destrutivas, como excluir.

O modo de seleção está limitado à página na qual ele está ativo e não pode afetar os itens fora da página.

O ponto de entrada para o modo de seleção deve estar justaposto em relação ao conteúdo que ele afeta.

Veja recomendações sobre a barra de comandos em [Diretrizes de barras de comandos](app-bars.md).

## <a name="globalization-and-localization-checklist"></a>Lista de verificação de globalização e localização

<table>
<tr>
<th>Disposição</th><td>Deixe duas linhas para o rótulo de lista.</td>
</tr>
<tr>
<th>Expansão horizontal</th><td>Verifique se os campos podem acomodar a expansão de texto e são roláveis.</td>
</tr>
<tr>
<th>Espaçamento vertical</th><td>Use caracteres não latinos para espaçamento vertical para garantir que scripts não latinos sejam exibidos corretamente.</td>
</tr>
</table>


## <a name="related-articles"></a>Artigos relacionados

- [Hub](hub.md)
- [Mestre/detalhes](master-details.md)
- [Painel de navegação](navigationview.md)
- [Zoom semântico](semantic-zoom.md)
- [Arrastar e soltar](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [Imagens em miniatura](../../files/thumbnails.md)

**Para desenvolvedores**
- [Classe ListView](https://msdn.microsoft.com/library/windows/apps/br242878)
- [Classe GridView](https://msdn.microsoft.com/library/windows/apps/br242705)
- [Classe ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348)
- [Classe ListBox](https://msdn.microsoft.com/library/windows/apps/br242868)