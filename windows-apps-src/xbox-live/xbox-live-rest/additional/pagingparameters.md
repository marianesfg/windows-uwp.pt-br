---
title: Parâmetros de paginação
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
description: " Parâmetros de paginação"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe80e1666f9eab8caf7e0bbdb2b537bd7661c9a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627291"
---
# <a name="paging-parameters"></a>Parâmetros de paginação
 
Alguns URIs de serviços do Xbox Live retornam coleções de objetos de notação JSON (JavaScript Object). Essas coleções podem ser paginadas por meio de especificando parâmetros de paginação como parte da cadeia de caracteres de consulta anexada ao URI. Segue uma lista completa dos parâmetros de paginação. Todos os URIs que permitem que os parâmetros de paginação são vinculados na parte inferior desta página.
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta 
 
| Parâmetro| Obrigatório| Tipo| Descrição| 
| --- | --- | --- | --- | 
| continuationToken| Não| cadeia de caracteres| Retorne os itens começando com o token de continuação fornecido. | 
| maxItems| Não| inteiro com sinal de 32 bits| Número máximo de itens a serem retornados da coleção, que pode ser combinada com <b>skipItems</b> e <b>continuationToken</b> para retornar um intervalo de itens. O serviço pode fornecer um valor padrão se <b>maxItems</b> não estiver presente e pode retornar menos de <b>maxItems</b>, mesmo se a última página de resultados ainda não foi retornada. | 
| skipItems| Não| inteiro com sinal de 32 bits| Retorne itens começando depois determinado número de itens. Por exemplo, <b>skipItems = "3"</b> irá recuperar itens que começa com o quarto item recuperado. | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>Referência [GET (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   