---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
description: " GET (/users/xuid({xuid})/history/titles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cadbf38385bbc321ef5bf23eb93c3fbc5c1a2417
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655581"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
Obtém uma lista de títulos para o qual o usuário foi desbloqueado ou progrediu em suas conquistas. Essa API não retorna o histórico completo de um usuário de títulos reproduzida ou iniciado. O domínio para esses URIs é `achievements.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EY)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EDB)
  * [Autorização](#ID4EFD)
  * [Cabeçalhos de solicitação opcionais](#ID4EGE)
  * [Corpo da solicitação](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário cujo histórico de título está sendo acessado.| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Obrigatório| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| Não| inteiro com sinal de 32 bits| Retorne itens começando depois determinado número de itens. Por exemplo, <b>skipItems = "3"</b> irá recuperar itens que começa com o quarto item recuperado. | 
| continuationToken| Não| cadeia de caracteres| Retorne os itens começando com o token de continuação fornecido. | 
| maxItems| Não| inteiro com sinal de 32 bits| Número máximo de itens a serem retornados da coleção, que pode ser combinada com <b>skipItems</b> e <b>continuationToken</b> para retornar um intervalo de itens. O serviço pode fornecer um valor padrão se <b>maxItems</b> não estiver presente e pode retornar menos de <b>maxItems</b>, mesmo se a última página de resultados ainda não foi retornada. | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>Autorização
 
| Declaração| Necessário?| Descrição| Comportamento se ausente| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Usuário| O chamador é um usuário autorizado do Xbox LIVE.| O chamador precisa ser um usuário válido no Xbox LIVE.| 403 Proibido| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc.| 
| <b>x-xbl-contract-version</b>| inteiro sem sinal de 32 bits| Se estiver presente e definido como 2, a versão V2 dessa API será usada. Otherwise, V1.| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/history/titles](uri-titlehistoryusersxuidhistorytitlesv2.md)

  
<a id="ID4EPG"></a>

 
##### <a name="reference"></a>Referência 

[UserTitle (JSON)](../../json/json-usertitlev2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Parâmetros de paginação](../../additional/pagingparameters.md)

   