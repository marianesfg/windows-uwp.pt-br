---
Description: Uma visão geral de padrões comuns de página e elementos de interface do usuário para exibir conteúdo no aplicativo do Windows.
title: Noções básicas de design de conteúdo para aplicativos do Windows
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.date: 12/01/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ea7d6da9461aa3c8dc2bd4d3f50c5d14c3928bb
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83233933"
---
# <a name="content-design-basics-for-windows-apps"></a>Noções básicas de design de conteúdo para aplicativos do Windows

O objetivo principal de qualquer aplicativo é fornecer acesso ao conteúdo. Já que os aplicativos existem por diversos motivos, o conteúdo vem em diversos formatos: em um aplicativo de edição de fotos, a foto é o conteúdo; em um aplicativo de viagens, mapas e informações sobre destinos de viagem são o conteúdo; e assim por diante. 

Este artigo fornece uma visão geral de como você pode apresentar conteúdo em um aplicativo. Descrevemos padrões comuns de página e elementos de interface do usuário que você pode usar para exibir seu conteúdo, qualquer que seja sua forma.

## <a name="common-page-patterns"></a>Padrões comuns de página

Muitos aplicativos usam alguns ou todos esses padrões comuns de página para exibir diferentes tipos de conteúdo. Da mesma forma, fique à vontade para misturar e combinar esses padrões para otimizar o conteúdo do aplicativo.

### <a name="landing"></a>Inicial

![Página de aterrissagem](images/content-basics/hero-screen.png)

Páginas de aterrissagem, também conhecidas como telas de celebridades, geralmente aparecem no nível superior de uma experiência de aplicativo. A área de superfície grande serve como um palco de aplicativos para realçar o conteúdo que os usuários podem querer procurar e consumir.

### <a name="collections"></a>Coleções

![galeria](images/content-basics/gridview.png)

Coleções permitem que os usuários naveguem em grupos de conteúdo ou dados. O [modo de exibição de grade](../controls-and-patterns/item-templates-gridview.md) é uma boa opção para fotos ou conteúdo centrado em mídia e a [exibição de lista](../controls-and-patterns/item-templates-listview.md) é uma boa opção para conteúdo de uso intenso de texto ou dados.


### <a name="masterdetail"></a>Mestre/detalhes

![mestre/detalhes](images/content-basics/master-detail.png)

O modelo [mestre/detalhes](../controls-and-patterns/master-details.md) consiste em uma exibição de lista (mestre) e um modo de exibição de conteúdo (detalhes). Ambos os painéis são fixos e têm a rolagem vertical. Há uma relação clara entre o item de lista e a exibição de conteúdo: o item no modo de exibição mestre é selecionado e o modo de exibição de detalhes é respectivamente atualizado. Além de fornecer navegação no modo de exibição de detalhes, os itens no modo de exibição mestre podem ser adicionados e removidos.

### <a name="details"></a>Detalhes

![várias exibições](images/multi-view.png)

Quando os usuários encontram o conteúdo que estão procurando, considere criar uma página de exibição de conteúdo dedicado para que eles possam exibir a página sem distrações. Se possível, [crie uma opção de exibição de tela inteira](../layout/show-multiple-views.md) que expande o conteúdo para preencher a tela inteira e oculta todos os outros elementos de interface do usuário. 

Para ajustar as alterações no tamanho da tela, considere também criar um [design responsivo](design-and-ui-intro.md) que oculta/mostra os elementos de interface do usuário conforme apropriado.

### <a name="forms"></a>Formulários
![formulário](images/content-basics/forms.png)

Um [formulário](../controls-and-patterns/forms.md) é um grupo de controles que coleta e envia dados de usuários. A maioria, se não todos os aplicativos, usam um formulário para páginas de configurações, portais de logon, hubs de comentários, criação de conta ou outros fins. 

## <a name="common-content-elements"></a>Elementos de conteúdo comum

Para criar esses padrões de página, você precisará usar uma combinação de elementos de conteúdo individuais. Veja alguns elementos de interface do usuário comumente usados para exibir o conteúdo. Para ver uma lista completa dos elementos de interface do usuário, confira [controles e padrões](../controls-and-patterns/index.md).

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Categoria</th>
<th align="left">Elementos</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Áudio e vídeo<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">Controles de transporte de mídia do sistema</a></td>
<td align="left">Reproduz áudio e vídeo.</td>
</tr>
<tr class="even">
<td align="left">Visualizadores de imagem<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">Inverter exibição</a>, <a href="../controls-and-patterns/images-imagebrushes.md">imagem</a></td>
<td align="left">Exibe imagens. A exibição virando a página mostra as imagens em uma coleção, como fotos em um álbum ou itens em uma página de detalhes do produto, uma imagem de uma vez.</td>
</tr>
<tr class="odd">
<td align="left">Coleções <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">Exibição de lista e exibição de grade</a></td>
<td align="left">Apresenta itens em uma lista interativa ou em uma grade. Use esses elementos para permitir que os usuários selecionem um filme em uma lista de lançamentos ou gerenciem um estoque.</td>
</tr>
<tr class="even">
<td align="left">Texto e entrada de texto <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">Bloco de texto</a>, <a href="../controls-and-patterns/text-box.md">caixa de texto</a>, <a href="../controls-and-patterns/rich-edit-box.md">caixa de edição com formato</a></p>
</td>
<td align="left">Exibe texto. Alguns elementos permitem que o usuário edite texto. Para obter mais informações, confira <a href="../controls-and-patterns/text-controls.md">Controles de texto</a>.
<p>Para obter diretrizes sobre como exibir texto, confira <a href="../style/typography.md">Tipografia</a>.</p>
</td>
</tr>
<tr class="odd">
<td align="left">Mapas<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">Exibe um mapa simbólico ou fotorrealista da Terra.</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">Renderiza o conteúdo de HTML.</td>
</tr>
</tbody>
</table>
</div>
