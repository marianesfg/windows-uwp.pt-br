---
title: APIs EDS auxiliares
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
description: " APIs EDS auxiliares"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2f729359f52b09879e7227ede71e238fe63801
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603231"
---
# <a name="auxiliary-eds-apis"></a>APIs EDS auxiliares

Há várias entretenimento descoberta de serviços (EDS) APIs que diretamente não fornecem informações sobre o conteúdo, mas fornecem informações gerais sobre como usar o serviço ou ajudar a modelos comuns de interface do usuário da unidade.

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>APIs auxiliares

| API| URI| Descrição|
| --- | --- | --- |
| Valores de parâmetro de API| /{locale}/metadata| Enumeração de possíveis valores de parâmetros que podem ser usados em chamadas para o serviço|
| Combinados gerador de classificação de conteúdo| /{locale}/contentRating| Cria um valor que pode ser usado em outras APIs para filtragem de conteúdo potencialmente indesejáveis ou explícito. Consulte abaixo para obter mais informações.|
| Gerador de nomes de campo combinado| /{locale}/fields| Cria um valor que pode ser usado no detalhe APIs para controlar quais campos são retornados. Consulte abaixo para obter mais informações.|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>Valores de parâmetro de API

Essa API descreve os parâmetros que podem ser usados com o serviço. As informações retornadas serão utilizáveis pelo cliente da interface do usuário porque o texto localizado acompanha cada parâmetro.

Nenhuma das APIs a seguir aceitam quaisquer parâmetros de consulta.

| API| URI| Descrição|
| --- | --- | --- | --- | --- | --- |
| Tipos| / {localidade} / metadados/mediaGroups| A lista completa de grupos de mídia|
| Tipos por grupo de mídia de item de mídia| /{locale}/metadata/mediaGroups/{mediaItemTypeGroup}/mediaItemTypes| Tipos contidos no grupo de mídia fornecido de item de lista de mídia.|
| Todos os tipos de item de mídia| /{locale}/metadata/mediaItemTypes| A lista completa de tipos de item de mídia|
| Campos por tipo de item de mídia| /{locale}/metadata/mediaItemTypes/{mediaItemType}/fields| A lista de campos em um tipo de item de mídia único|
| Consulta Refiners| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners| A lista de refiners de consulta com suporte para o tipo de item de mídia fornecido|
| Todos os valores de refinamento de consulta| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}| Os valores para o refinamento de consulta especificada para a mídia de determinado tipo de item|
| Todos os valores de subpropriedades de refinamento de consulta| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}/subQueryRefinerValues| A lista de valores inferiores para um valor de refinamento de determinada consulta (por exemplo, "subgenres em um determinado gênero"). O valor de refinamento da consulta é passado como um parâmetro de cadeia de caracteres de consulta chamado "queryRefinerValue", que é feito para permitir que consulta os valores de refinamento com caracteres proibidos em troncos URI a ser passado.|
| Classificações| /{locale}/metadata/mediaItemTypes/{mediaItemType}/sortOrders| A lista de ordens de classificação para o tipo de item de mídia fornecido|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>Combinados gerador de classificação de conteúdo

Impor sobre o conteúdo de filhos que têm permissão para ver os controles dos pais é uma tarefa complicada. Não apenas a cada tipo de item de mídia tem seu próprio sistema de classificação, mas esses sistemas de classificação podem variar de país para país. Isso significa que há várias partes distintas de dados que precisam ser especificado para filtrar corretamente todos os itens.

Em vez de especificar todos os parâmetros em todas as chamadas de API, essa API gera um valor para passar para os parâmetros de combinedContentRating em outras APIs e ainda se comunicar as mesmas informações. Isso foi projetado para tornar as APIs mais fácil de usar e manter, como vários parâmetros passados para essa API são recolhidos para um valor único e reutilizável para outras APIs.

Embora os valores exatos retornados por essa API, eventualmente, podem mudar, eles devem alterar com muito pouca frequência (como entre versões de EDS) e, portanto, pode ser armazenado em cache por longos períodos de tempo. Qualquer API que aceita que um parâmetro combinedContentRating fornecerá uma mensagem de erro significativa se o valor passado é inválido, que é uma indicação do chamador meramente precisa chamar essa API novamente para receber um valor atualizado. Se uma API aceita um parâmetro de combinedContentRating, mas um não for fornecido, nenhuma filtragem de conteúdo será realizada com base em controles dos pais

> [!NOTE]
> Isso não significa que conteúdo somente "seguro" é retornado – isso significa que todo o conteúdo é retornado, incluindo conteúdo potencialmente explícito).



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>Nome do campo combinado

Por padrão, as APIs de EDS, retornar um conjunto mínimo muito pequeno de campos para cada item:

   * Tipo de item de mídia
   * Grupo de mídia
   * Id
   * Nome

Para obter mais informações, as APIs de aceitam um parâmetro "campos" que especifica quais partes extras de dados devem ser retornados. Como há muitos campos possíveis, especificar seu nome completo para cada chamada à API seria inchar consideravelmente a solicitação. Em vez disso, os nomes podem ser passados para dentro dessa API que irá gerar um muito valor menor que pode ser passado para outras APIs.

Para qualquer API que aceita esse parâmetro, o valor fornecido deve ser o superconjunto de todos os campos em todos os tipos de item de mídia especificada. Não é possível especificar diferentes conjuntos de campos de tipos de item de mídia diferente. No entanto, se um campo se aplica ao tipo de item de uma mídia, mas não em outra, ele só será exibido na mídia de tipos de item de onde os dados existem (por exemplo, se "AvatarBodyType" estiver incluído na chamada à API do nome de campo combinado, apenas AvatarItems conterá o campo).

Os valores retornados dessa API são altamente armazenáveis em cache – na verdade, eles não devem alterar exceto entre implantações da EDS. É recomendável que, se o cache for desejado, o cache durar não mais do que a sessão do usuário.

Além de aceitar os nomes de campo reais, essa API aceita "all" como um valor válido. Isso irá gerar um valor que contém cada campo que é possível especificar. O valor "all" é provavelmente será usado apenas para o desenvolvimento, depuração e fins de teste.

<a id="ID4ERG"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ETG"></a>


##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>Informações adicionais

[URIs de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)
