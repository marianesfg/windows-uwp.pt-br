---
author: mijacobs
Description: "O objetivo principal de qualquer aplicativo é fornecer acesso ao conteúdo. Em um aplicativo de edição de fotos, a foto é o conteúdo; em um aplicativo de viagens, os mapas e as informações sobre os destinos de viagem são os conteúdos; e assim por diante."
title: "Noções básicas de design de conteúdo para aplicativos da Plataforma Universal do Windows (UWP)"
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: c8328150601bf132c28e569c4db21095cb77f88c

---

#  Noções básicas de design de conteúdo para aplicativos UWP

O objetivo principal de qualquer aplicativo é fornecer acesso ao conteúdo: em um aplicativo de edição de fotos, a foto é o conteúdo; em um aplicativo de viagens, mapas e informações sobre destinos de viagem são o conteúdo; e assim por diante. Os elementos de navegação fornecem acesso ao conteúdo; os elementos de comando permitem que o usuário interaja com conteúdo; os elementos de conteúdo exibem o conteúdo em si.

Este artigo fornece recomendações de design do conteúdo para os três cenários de conteúdo.

## <span id="Design_for_the_right_content_scenario"></span><span id="design_for_the_right_content_scenario"></span><span id="DESIGN_FOR_THE_RIGHT_CONTENT_SCENARIO"></span>Design para o cenário de conteúdo certo


Há três cenários de conteúdo principais:

-   
            **Consumo**: uma experiência principalmente unidirecional na qual o conteúdo é consumido. Inclui tarefas como leitura, ouvir música, assistir a vídeos e visualização de fotos e imagens.
-   
            **Criação**: uma experiência principalmente unidirecional em que o foco é criar novo conteúdo. Pode ser dividida em fazer coisas do zero, como tirar uma foto ou fazer um vídeo, criar uma nova imagem em um aplicativo de pintura, ou abrir um documento atualizado.
-   
            **Interativo**: uma experiência de conteúdo bidirecional que inclui consumir, criar e revisar o conteúdo.

## <span id="Consumption-focused_apps"></span><span id="consumption-focused_apps"></span><span id="CONSUMPTION-FOCUSED_APPS"></span>Aplicativos voltados para consumo


Os elementos de conteúdo recebem a prioridade mais alta em um aplicativo focado em consumo, seguido pelos [elementos de navegação](navigation-basics.md) necessários para ajudar os usuários a localizar o conteúdo desejado. Exemplos de aplicativos voltados para consumo incluem players de filme, aplicativos de leitura, aplicativos de música e visualizadores de foto.

![um aplicativo leitor de notícias](images/news-reader/v2/newsreader-v2-tablet-phone.png)

Recomendações gerais para aplicativos voltados para consumo:

-   Considere criar páginas de [navegação](navigation-basics.md) dedicadas e páginas de visualização de conteúdo para que, quando os usuários encontrarem o conteúdo que estão procurando, eles possam visualizá-lo em uma página dedicada livre de distrações.
-   Considere a criação de uma opção de exibição de tela inteira que expande o conteúdo para preencher a tela inteira e oculta todos os outros elementos de interface do usuário.

## <span id="Creation-focused_apps"></span><span id="creation-focused_apps"></span><span id="CREATION-FOCUSED_APPS"></span>Aplicativos voltados para criação


O conteúdo e os elementos de [comando](commanding-basics.md) são os elementos da interface do usuário mais importantes em um aplicativo focado em criação: elementos de comando permitem que o usuário crie um novo conteúdo. Exemplos incluem aplicativos de pintura, aplicativos de edição de fotos, aplicativos de edição de vídeo e aplicativos de processamento de texto.

Por exemplo, aqui está um design para um aplicativo de fotos que usa barras de comandos para fornecer acesso a ferramentas e opções de manipulação de fotos. Como todos os comandos estão na barra de comandos, o aplicativo pode dedicar a maior parte do espaço de tela ao conteúdo, a foto que está sendo editada.

![exemplo de um design de aplicativo de edição de fotos que usa tela ativa](images/photo-editor/uap-photo-tabletphone-sbs.png)

Recomendações gerais para aplicativos voltados para criação:

-   Minimize o uso de elementos de [navegação](navigation-basics.md).
-   
            Os elementos de [comando](commanding-basics.md) são especialmente importantes em aplicativos voltados para criação. Como os usuários executarão muitos comandos, é recomendável fornecer funcionalidade de histórico/desfazer um comando.

## <span id="Apps_with_interactive_content"></span><span id="apps_with_interactive_content"></span><span id="APPS_WITH_INTERACTIVE_CONTENT"></span>Aplicativos com conteúdo interativo


Em um aplicativo com conteúdo interativo, os usuários criam, visualizam e editam conteúdo; muitos aplicativos se enquadram nessa categoria. Exemplos desses tipos de aplicativos incluem aplicativos de linha de negócios, aplicativos de gerenciamento de estoque, aplicativos de culinária que permitem ao usuário criar ou modificar receitas.

![um design para uma ferramenta de colaboração, um aplicativo que tem conteúdo interativo](images/collaboration-tool/uap-collaboration-tabphone-700.png)

Esse tipo de aplicativo precisa equilibrar todos os três elementos de interface do usuário:

-   
            Os elementos de [navegação](navigation-basics.md) ajudam os usuários a encontrar e visualizar o conteúdo. Se a exibição e localização de conteúdo for o cenário mais importante, priorize elementos de navegação, filtrar e classificar, e pesquisa.
-   
            Os elementos de [comando](commanding-basics.md) permitem ao usuário criar, editar e manipular o conteúdo.

Recomendações gerais para aplicativos com conteúdo interativo:

-   Pode ser difícil equilibrar elementos de navegação, conteúdo e comando quando todos os três são importantes. Se possível, considere a criação de telas separadas para navegação, criação e edição de conteúdo ou o fornecimento de opções de modo.

## <span id="Commonly_used_content_elements"></span><span id="commonly_used_content_elements"></span><span id="COMMONLY_USED_CONTENT_ELEMENTS"></span>Elementos de conteúdo mais usados


Veja alguns elementos de interface do usuário comumente usados para exibir o conteúdo. Para obter uma lista completa dos elementos de interface de usuário, consulte [Controles e elementos de interface do usuário](https://msdn.microsoft.com/library/windows/apps/dn611856).

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
<td align="left">Áudio e vídeo</td>
<td align="left">[Controles de transporte de mídia do sistema](../controls-and-patterns/media-playback.md)</td>
<td align="left">Reproduz áudio e vídeo.</td>
</tr>
<tr class="even">
<td align="left">Visualizadores de imagem</td>
<td align="left">
            [Inverter exibição](../controls-and-patterns/flipview.md), [imagem](../controls-and-patterns/images-imagebrushes.md)</td>
<td align="left">Exibe imagens. A exibição virando a página mostra as imagens em uma coleção, como fotos em um álbum ou itens em uma página de detalhes do produto, uma imagem de uma vez.</td>
</tr>
<tr class="odd">
<td align="left">Listas</td>
<td align="left">[lista suspensa, caixa de listagem, modo de exibição de lista e modo de exibição de grade](../controls-and-patterns/lists.md)</td>
<td align="left">Apresenta itens em uma lista interativa ou em uma grade. Use esses elementos para permitir que os usuários selecionem um filme em uma lista de lançamentos ou gerenciem um estoque.</td>
</tr>
<tr class="even">
<td align="left">Texto e entrada de texto</td>
<td align="left"><p>
            [Bloco de texto](../controls-and-patterns/text-block.md), [caixa de texto](../controls-and-patterns/text-box.md), [caixa de edição com formato](../controls-and-patterns/rich-edit-box.md)</p>
</td>
<td align="left">Exibe texto. Alguns elementos permitem que o usuário edite texto. Para obter mais informações, consulte [Controles de texto](../controls-and-patterns/text-controls.md)</td>
</tr>
</tbody>
</table>



 

 







<!--HONumber=Jun16_HO4-->


