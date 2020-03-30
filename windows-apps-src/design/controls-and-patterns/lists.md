---
Description: Listas são exibidas e permitem a interação com conteúdo baseado em coleção.
title: Coleções e listas
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e198e65052e9ef79ee38863260bce1c1f798ba38
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081744"
---
# <a name="collections-and-lists"></a>Coleções e listas

As coleções e as listas referem-se à representação de vários itens de dados relacionados que aparecem juntos. As coleções podem ser representadas de várias maneiras, por diferentes controles de coleção (e também podem ser chamadas de exibições de coleção). Os controles de coleção exibem e habilitam interações com conteúdo baseado em coleção, como uma lista de contatos, uma lista de datas, uma coleção de imagens e assim por diante.

> **APIs importantes**: [Classe ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [classe GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView), [classe FlipView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flipview), [classe TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview) e [classe ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

Os controles abordados neste artigo incluem:

- Modos de exibição de lista, que são usados principalmente para exibir coleções de conteúdo com muito texto
- Modos de exibição de grade, que são usados principalmente para exibir coleções de conteúdo com muitas imagens
- Modos de exibição invertida, que são usados principalmente para exibir coleções de conteúdo com muitas imagens e que exigem o foco em exatamente um item de cada vez
- Modos de exibição de árvore, que são usados principalmente para exibir coleções de conteúdo com muito texto em uma hierarquia específica
- ItemsRepeater, que é um bloco de construção personalizável para criar controles de coleção personalizados

Diretrizes de design, recursos e exemplos são fornecidos abaixo para cada controle.

Cada um desses controles (com exceção do ItemsRepeater) proporciona estilos e interação internos. No entanto, para personalizar ainda mais a aparência visual da exibição de coleção e os itens dentro dela, é usado um [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). Informações detalhadas sobre os modelos de dados e a personalização da aparência de uma exibição de coleção podem ser encontradas na página [Contêineres e modelos do item](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/item-containers-templates).

Cada um desses controles (com exceção do ItemsRepeater) também tem o comportamento interno de permitir a seleção de um ou vários itens. Consulte a [Visão geral dos modos de seleção](selection-modes.md) para saber mais.

> **Windows 10 Fall Creators Update – alteração de comportamento** Por padrão, em vez de realizar uma seleção, uma caneta ativa agora faz a rolagem/aplica panorâmica em uma lista nos aplicativos UWP (como toque, touchpad e caneta passiva).
> Se o seu aplicativo depende do comportamento anterior, você pode substituir a rolagem com caneta e reverter para o comportamento anterior. Para obter detalhes, confira o tópico de referência de API para a [Classe ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, veja as classes <a href="xamlcontrolsgallery:/item/ListView">ListView</a>, <a href="xamlcontrolsgallery:/item/GridView">GridView</a>, <a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>, <a href="xamlcontrolsgallery:/item/TreeView">TreeView</a> e <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>Modos de exibição de lista

Os modos de exibição em lista representam itens com muito texto, normalmente em layout de coluna única e empilhado verticalmente. Eles permitem que você classifique itens, atribua cabeçalhos de grupo, arraste e solte itens, corrija conteúdos e reordene itens.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um modo de exibição de lista para:

- Exibir uma coleção que consiste principalmente em itens baseados em texto, na qual todos os itens devem ter o mesmo comportamento visual e de interação.
- Representar uma coleção de conteúdo única ou categorizada.
- Acomodar uma série de casos de uso, incluindo os seguintes tipos comuns:
    - Crie uma lista de mensagens ou um log de mensagens.
    - Crie uma lista de contatos.
    - Crie o painel mestre no [padrão mestre/detalhes](master-details.md). Um padrão mestre/detalhes é usado com frequência em aplicativos de email, em que um painel (o mestre) tem uma lista de itens selecionáveis e o outro painel (detalhes) tem uma exibição detalhada do item selecionado.
    

### <a name="examples"></a>Exemplos

Aqui está um modo de exibição de lista simples que mostra uma lista de contatos e agrupa os itens de dados em ordem alfabética. Os cabeçalhos de grupo (cada letra do alfabeto, neste exemplo) também podem ser personalizados para permanecerem "fixados", ficando sempre visíveis na parte superior do ListView durante a rolagem.

![Uma exibição de lista com dados agrupados](images/listview-grouped-example-resized-final.png)

Este é um ListView que foi invertido para exibir um log de mensagens, com as mensagens mais recentes aparecendo na parte inferior. Com um ListView invertido, os itens aparecem na parte inferior da tela com uma animação interna.

![Modo de exibição em lista invertido](images/listview-inverted-2.png)

### <a name="related-articles"></a>Artigos relacionados
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
<td align="left"><p>Os itens exibidos em uma exibição em lista ou de grade podem desempenhar uma função importante na aparência geral do seu aplicativo. Faça com que seu aplicativo fique ótimo personalizando a aparência dos itens de coleção por meio da modificação dos modelos de controle e de dados.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">Modelos de item para exibição de lista</a></p></td>
<td align="left"><p>Use estes exemplos de modelos de item para uma ListView para obter a aparência de tipos comuns de aplicativos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">Listas invertidas</a></p></td>
<td align="left"><p>As listas invertidas têm novos itens adicionados na parte inferior, como em um aplicativo de chat. Siga as orientações deste artigo para usar uma lista invertida em seu aplicativo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">Deslizar para atualizar</a></p></td>
<td align="left"><p>O mecanismo deslizar para atualizar permite que o usuário explore uma lista de dados usando o toque para recuperar mais dados. Use este artigo para implementar o recurso de deslizar para atualizar em sua exibição em lista.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface do usuário aninhada</a></p></td>
<td align="left"><p>Interface do usuário aninhada é uma interface do usuário (IU) que expõe controles acionáveis colocados dentro de um contêiner que o usuário também pode usar. Por exemplo, você pode ter um item de exibição de lista que contém um botão e o usuário pode selecionar o item de lista ou pressionar o botão aninhado dentro dele. Siga estas práticas recomendadas para oferecer a melhor experiência da interface do usuário aninhada para seus usuários.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>Exibições em grade

Modos de exibição de grade são adequados para organizar e navegar em coleções de conteúdos baseadas em imagens. Um layout de modo de exibição de grade rola verticalmente e faz movimento panorâmico na horizontal. Os itens estão em um layout encapsulado, à medida que aparecem no sentido de leitura da esquerda para a direita e de cima para baixo.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um modo de exibição de grade para:

- exibir uma coleção de conteúdo na qual o ponto focal de cada item seja uma imagem e cada item deva ter o mesmo comportamento visual e de interação.
- Exiba bibliotecas de conteúdo.
- Formate os dois modos de exibição de conteúdo associados ao [zoom semântico](semantic-zoom.md).
- Acomodar uma série de casos de uso, incluindo os seguintes tipos comuns:
    - Interface do usuário do tipo vitrine (ou seja, pesquisando aplicativos, músicas, produtos etc.)
    - Bibliotecas de fotos interativas

### <a name="examples"></a>Exemplos

Este exemplo mostra um layout de modo de exibição de grade típico, neste caso, para navegar aplicativos. Metadados para itens de modo de exibição de grade são geralmente restritos a algumas linhas de texto e uma classificação de item.

![Exemplo de um layout de exibição de grade](images/controls_gridview_example02.png)

Um modo de exibição de grade é a solução ideal para uma biblioteca de conteúdo, que geralmente é usada para apresentar mídia, como fotos e vídeos. Em uma biblioteca de conteúdo, os usuários esperam tocar em um item para invocar uma ação.

![Exemplo de uma biblioteca de conteúdo](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>Artigos relacionados
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
<td align="left"><p>Os itens exibidos em uma exibição em lista ou de grade podem desempenhar uma função importante na aparência geral do seu aplicativo. Faça com que seu aplicativo fique ótimo personalizando a aparência dos itens de coleção por meio da modificação dos modelos de controle e de dados.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">Modelos de item para exibição de grade</a></p></td>
<td align="left"><p>Use estes exemplos de modelos de item para uma GridView para obter a aparência de tipos comuns de aplicativos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface do usuário aninhada</a></p></td>
<td align="left"><p>Interface do usuário aninhada é uma interface do usuário (IU) que expõe controles acionáveis colocados dentro de um contêiner que o usuário também pode usar. Por exemplo, você pode ter um item de exibição de grade que contém um botão e o usuário pode selecionar o item de grade ou pressionar o botão aninhado dentro dele. Siga estas práticas recomendadas para oferecer a melhor experiência da interface do usuário aninhada para seus usuários.</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>Exibições de inversão

As exibições de inversão são adequadas para navegar por coleções de conteúdo baseadas em imagem, especificamente aquelas em a experiência desejada é que apenas uma imagem fique visível de cada vez. Uma exibição de inversão permite que o usuário "percorra" os itens da coleção (vertical ou horizontalmente), fazendo com que cada item seja exibido, um de cada vez, após a interação do usuário.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use a exibição de inversão para:

- Exibir uma coleção pequena ou média (com menos de 25 itens), que seja composta por imagens com pouco ou nenhum metadado.
- Exibir os itens, um de cada vez, e permitir que o usuário final percorra os itens em seu próprio ritmo.
- Acomodar uma série de casos de uso, incluindo os seguintes tipos comuns:
    - Galerias de fotos
    - Galerias de produtos ou demonstrações

### <a name="examples"></a>Exemplos

Os dois exemplos a seguir mostram um FlipView que é invertido horizontal e verticalmente, respectivamente.

![Exibição de inversão horizontal](images/controls_flipview_horizonal.jpg)

![Exibição de inversão vertical](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>Artigos relacionados
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
<td align="left"><p><a href="flipview.md">Exibição de inversão</a></p></td>
<td align="left"><p>Aprenda os conceitos básicos de como usar uma exibição de inversão em seu aplicativo, além de como personalizar a aparência dos itens em uma exibição de inversão.</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>Modos de exibição de árvore

Os modos de exibição de árvore são adequados para a exibição de coleções baseadas em texto que têm uma hierarquia importante que precisa ser demonstrada. Os itens do modo de exibição de árvore são recolhíveis/expansíveis, são mostrados em uma hierarquia visual, podem ser complementados com ícones e transferidos entre modos de exibição de árvore por meio do recurso arrastar e soltar. Os modos de exibição de árvore permitem o aninhamento de N níveis.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um modo de exibição de árvore para:

- Exibir uma coleção de itens aninhados cujo contexto e significado dependam de uma hierarquia ou cadeia organizacional específica.
- Acomodar uma série de casos de uso, incluindo os seguintes tipos comuns:
    - Navegador de arquivos
    - Organograma da empresa

### <a name="examples"></a>Exemplos

Aqui está um exemplo de um modo de exibição de árvore que representa um explorador de arquivos e exibe vários itens aninhados diferentes, complementados por ícones.

![Modo de exibição de árvore com ícones](images/treeview-icons.png)

### <a name="related-articles"></a>Artigos relacionados
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
<td align="left"><p><a href="tree-view.md">Modo de exibição de árvore</a></p></td>
<td align="left"><p>Aprenda os conceitos básicos de como usar um modo de exibição de árvore em seu aplicativo, além de como personalizar a aparência e o comportamento de interação dos itens de um modo de exibição de árvore.</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

O ItemsRepeater é diferente do restante dos controles de coleção mostrados nesta página porque ele não oferece nenhum tipo de estilo ou interação prontos para o uso, ou seja, quando é simplesmente colocado em uma página sem definir nenhuma propriedade. O ItemsRepeater é, em vez disso, um bloco de construção que você pode usar, caso seja um desenvolvedor, para criar seu próprio controle de coleções personalizado – mais especificamente, um controle que não pode ser obtido usando os demais controles deste artigo. O ItemsRepeater é um painel controlado por dados e de alto desempenho que pode ser adaptado de acordo com suas necessidades específicas.

### <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um ItemsRepeater se:

- Você tiver em mente ideias específicas de interface do usuário e experiência do usuário, que não poderão ser criadas usando os controles de coleção existentes.
- Você já tiver uma fonte de dados para seus itens (como dados extraídos da Internet, de um banco de dados ou de uma coleção preexistente em seu code-behind).

### <a name="examples"></a>Exemplos

Os três exemplos a seguir são todos controles ItemsRepeater associados à mesma fonte de dados (uma coleção de números). A coleção de números é representada de três maneiras distintas, com cada um dos ItemsRepeaters abaixo, usando um [Layout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layout) personalizado diferente e um [ItemTemplate](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2) personalizado diferente para cada representação.

![ItemsRepeater com barras horizontais](images/itemsrepeater-1.png)
![ItemsRepeater com barras verticais](images/itemsrepeater-2.png)
![ItemsRepeater com representação circular](images/itemsrepeater-3.png)

### <a name="related-articles"></a>Artigos relacionados
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
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>Aprenda conceitos básicos de como usar um ItemsRepeater em seu aplicativo, além de como implementar toda a interação e os componentes visuais necessários para a exibição de coleção.</p></td>
</tr>
</tbody>
</table>


## <a name="globalization-and-localization-checklist"></a>Lista de verificação de globalização e localização

<table>
<tr>
<th>Disposição</th><td>Deixe duas linhas para o rótulo de lista.</td>
</tr>
<tr>
<th>Expansão horizontal</th><td>Verifique se os campos podem acomodar a expansão do texto e se eles são roláveis.</td>
</tr>
<tr>
<th>Espaçamento vertical</th><td>Use caracteres não latinos para espaçamento vertical a fim de garantir que os scripts não latinos sejam exibidos corretamente.</td>
</tr>
</table>

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

**Diretrizes de design e UX**
- [Mestre/detalhes](master-details.md)
- [Painel de navegação](navigationview.md)
- [Zoom semântico](semantic-zoom.md)
- [Arrastar e soltar](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [Imagens em miniatura](../../files/thumbnails.md)

**Referência de API**
- [Classe ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [Classe GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [Classe ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)
- [Classe ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
