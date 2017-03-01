---
author: mijacobs
Description: "O objetivo principal de qualquer app é fornecer acesso ao conteúdo. Em um app de edição de fotos, a foto é o conteúdo; em um app de viagens, os mapas e as informações sobre os destinos de viagem são os conteúdos; e assim por diante."
title: "Noções básicas de design de conteúdo para apps da Plataforma Universal do Windows (UWP)"
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 9bb39b117fe4c4e95616c06921b05199e153cddd
ms.lasthandoff: 02/07/2017

---

#  <a name="content-design-basics-for-uwp-apps"></a>Noções básicas de design de conteúdo para apps UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

O objetivo principal de qualquer app é fornecer acesso ao conteúdo: em um app de edição de fotos, a foto é o conteúdo; em um app de viagens, mapas e informações sobre destinos de viagem são o conteúdo; e assim por diante. Os elementos de navegação fornecem acesso ao conteúdo; os elementos de comando permitem que o usuário interaja com conteúdo; os elementos de conteúdo exibem o conteúdo em si.

Este artigo fornece recomendações de design do conteúdo para os três cenários de conteúdo.

## <a name="design-for-the-right-content-scenario"></a>Design para o cenário de conteúdo certo


Há três cenários de conteúdo principais:

-   **Consumo**: uma experiência principalmente unidirecional na qual o conteúdo é consumido. Inclui tarefas como leitura, ouvir música, assistir a vídeos e visualização de fotos e imagens.
-   **Criação**: uma experiência principalmente unidirecional em que o foco é criar novo conteúdo. Pode ser dividida em fazer coisas do zero, como tirar uma foto ou fazer um vídeo, criar uma nova imagem em um app de pintura, ou abrir um documento atualizado.
-   **Interativo**: uma experiência de conteúdo bidirecional que inclui consumir, criar e revisar o conteúdo.

## <a name="consumption-focused-apps"></a>Aplicativos voltados para consumo


Os elementos de conteúdo recebem a prioridade mais alta em um app focado em consumo, seguido pelos [elementos de navegação](navigation-basics.md) necessários para ajudar os usuários a localizar o conteúdo desejado. Exemplos de apps voltados para consumo incluem players de filme, apps de leitura, apps de música e visualizadores de foto.

![um app leitor de notícias](images/news-reader/v2/newsreader-v2-tablet-phone.png)

Recomendações gerais para apps voltados para consumo:

-   Considere criar páginas de [navegação](navigation-basics.md) dedicadas e páginas de visualização de conteúdo para que, quando os usuários encontrarem o conteúdo que estão procurando, eles possam visualizá-lo em uma página dedicada livre de distrações.
-   Considere a criação de uma opção de exibição de tela inteira que expande o conteúdo para preencher a tela inteira e oculta todos os outros elementos de interface do usuário.

## <a name="creation-focused-apps"></a>Aplicativos voltados para criação


O conteúdo e os elementos de [comando](commanding-basics.md) são os elementos da interface do usuário mais importantes em um app focado em criação: elementos de comando permitem que o usuário crie um novo conteúdo. Exemplos incluem apps de pintura, apps de edição de fotos, apps de edição de vídeo e apps de processamento de texto.

Por exemplo, aqui está um design para um app de fotos que usa barras de comandos para fornecer acesso a ferramentas e opções de manipulação de fotos. Como todos os comandos estão na barra de comandos, o app pode dedicar a maior parte do espaço de tela ao conteúdo, a foto que está sendo editada.

![exemplo de um design de app de edição de fotos que usa tela ativa](images/photo-editor/uap-photo-tabletphone-sbs.png)

Recomendações gerais para apps voltados para criação:

-   Minimize o uso de elementos de [navegação](navigation-basics.md).
-   Os elementos de [comando](commanding-basics.md) são especialmente importantes em apps voltados para criação. Como os usuários executarão muitos comandos, é recomendável fornecer funcionalidade de histórico/desfazer um comando.

## <a name="apps-with-interactive-content"></a>Aplicativos com conteúdo interativo


Em um app com conteúdo interativo, os usuários criam, visualizam e editam conteúdo; muitos apps se enquadram nessa categoria. Exemplos desses tipos de apps incluem apps de linha de negócios, apps de gerenciamento de estoque, apps de culinária que permitem ao usuário criar ou modificar receitas.

![um design para uma ferramenta de colaboração, um app que tem conteúdo interativo](images/collaboration-tool/uap-collaboration-tabphone-700.png)

Esse tipo de app precisa equilibrar todos os três elementos de interface do usuário:

-   Os elementos de [navegação](navigation-basics.md) ajudam os usuários a encontrar e visualizar o conteúdo. Se a exibição e localização de conteúdo for o cenário mais importante, priorize elementos de navegação, filtrar e classificar, e pesquisa.
-   Os elementos de [comando](commanding-basics.md) permitem ao usuário criar, editar e tratar o conteúdo.

Recomendações gerais para apps com conteúdo interativo:

-   Pode ser difícil equilibrar elementos de navegação, conteúdo e comando quando todos os três são importantes. Se possível, considere a criação de telas separadas para navegação, criação e edição de conteúdo ou o fornecimento de opções de modo.

## <a name="commonly-used-content-elements"></a>Elementos de conteúdo mais usados


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
<td align="left">[Inverter exibição](../controls-and-patterns/flipview.md), [imagem](../controls-and-patterns/images-imagebrushes.md)</td>
<td align="left">Exibe imagens. A exibição virando a página mostra as imagens em uma coleção, como fotos em um álbum ou itens em uma página de detalhes do produto, uma imagem de uma vez.</td>
</tr>
<tr class="odd">
<td align="left">Listas</td>
<td align="left">[lista suspensa, caixa de listagem, modo de exibição de lista e modo de exibição de grade](../controls-and-patterns/lists.md)</td>
<td align="left">Apresenta itens em uma lista interativa ou em uma grade. Use esses elementos para permitir que os usuários selecionem um filme em uma lista de lançamentos ou gerenciem um estoque.</td>
</tr>
<tr class="even">
<td align="left">Texto e entrada de texto</td>
<td align="left"><p>[Bloco de texto](../controls-and-patterns/text-block.md), [caixa de texto](../controls-and-patterns/text-box.md), [caixa de edição com formato](../controls-and-patterns/rich-edit-box.md)</p>
</td>
<td align="left">Exibe texto. Alguns elementos permitem que o usuário edite texto. Para obter mais informações, consulte [Controles de texto](../controls-and-patterns/text-controls.md)</td>
</tr>
</tbody>
</table>



 

 





