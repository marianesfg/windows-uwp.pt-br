---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
description: " GET (/users/{ownerId}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7c52daf4a07914c34f1aadc84a7238771669d65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623201"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
Recupere a lista de clipes do usuário.
Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.

  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EEB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EPB)
  * [Corpo da solicitação](#ID4EPE)
  * [Cabeçalhos de resposta necessária](#ID4E1E)
  * [Cabeçalhos de resposta opcional](#ID4ENH)
  * [Corpo da resposta](#ID4EOAAC)
  * [URIs relacionados](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Comentários

Essa API permite várias maneiras de um usuário próprio recorta clipes, bem como outros usuários que estão armazenados no serviço de lista. Vários pontos de entrada retornam dados de diferentes níveis e permitem a filtragem por meio de parâmetros de consulta. Se o XUID na declaração de corresponder o proprietário especificado no URI, os clipes do usuário são retornados após verificações de isolamento de conteúdo. Se o proprietário no URI não coincide com a declaração XUID, clipes de usuário especificada serão retornadas com base em verificações de privacidade e as verificações de isolamento de conteúdo em relação a XUID solicitante.

As consultas são otimizadas por usuário por id de configuração de serviço (scid). Especificar ainda mais filtros ou ordens de classificação diferentes dos padrões conforme especificado abaixo em algumas circunstâncias demore mais tempo para retornar. Isso é mais evidente para grandes conjuntos de vídeos por usuário.

Não há nenhuma API de lote para obter a lista de vários usuários dentro da mesma chamada de API. O padrão recomendado (atualmente) de arquitetos SLS é consultar por sua vez para cada usuário.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| ownerId| cadeia de caracteres| Identidade de usuário do usuário cujo recurso está sendo acessado. Formatos com suporte: "me" ou "xuid(123456789)". Comprimento máximo: 16.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- |
| skipItems| inteiro com sinal de 32 bits| Opcional. Retorne os itens começando em N + 1 na coleção (ou seja, skip N itens).|
| continuationToken| cadeia de caracteres| Opcional. Retorne os itens começando com o token de continuação fornecido. O parâmetro continuationToken tem precedência sobre skipItems se ambas são fornecidas. Em outras palavras, o parâmetro skipItems será ignorado se o parâmetro continuationToken está presente. Tamanho máximo: 36.|
| maxItems| inteiro com sinal de 32 bits| Opcional. Número máximo de itens a serem retornados da coleção (pode ser combinado com skipItems e continuationToken para retornar um intervalo de itens). O serviço pode fornecer um valor padrão se maxItems não estiver presente e poderá retornar menos de maxItems (mesmo se a última página de resultados ainda não foi retornada).|
| Ordem| Caractere Unicode| Opcional. Especifica se a lista é retornada na decrescente (D) (mais alto valor pela primeira vez) ou (A) rescente (menor valor pela primeira vez) ordem. Default: D.|
| type| GameClipTypes| Opcional. Conjunto delimitado por vírgulas do tipo de clipes para retornar. Default: Todos os.|
| eventId| cadeia de caracteres| Opcional. Conjunto de delimitada por vírgulas de eventIDs para filtrar resultados por. Default: Null.|
| qualificador| cadeia de caracteres| Opcional. Especifica o qualificador de ordem a ser usado para obter os clipes. <ul><li>criado - Especifica que os clipes são retornados em ordem de data no sistema</li><li>classificação - [superior classificados] - Especifica que os clipes são retornados por seus valores de classificação</li><li>exibições - [mais visualizados] - Especifica que os clipes são retornados pelo número de modos de exibição</li></ul><br/> Tamanho máximo: 12. Padrão: "criado".| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há nenhum membros necessários para esta solicitação.

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.|
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.|
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.|
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.|
| Retry-After| cadeia de caracteres| Instrui o cliente tente novamente mais tarde no caso de um servidor não disponível.|
| Variar| cadeia de caracteres| Instrui como armazenar em cache as respostas de proxies de downstream.|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| cadeia de caracteres| Usada para otimização de cache. Exemplo: "686897696a7c876b7e".|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
       "gameClips":
       [
           {
               "xuid": "2716903703773872",
               "clipName": "[en-US] Localized Greatest Moment Name",
               "titleName": "[en-US] Localized Title Name",
               "gameClipLocale": "en-US",
               "gameClipId": "6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
               "state": "Published",
               "dateRecorded": "2013-06-14T01:02:55.4918465Z",
               "lastModified": "2013-06-14T01:05:41.3652693Z",
               "userCaption": "Set by user!",
               "type": "DeveloperInitiated",
               "source": "Console",
               "visibility": "Public",
               "durationInSeconds": 30,
               "scid": "00000000-0000-0012-0023-000000000070",
               "titleId": 354975,
               "rating": 3.75,
               "ratingCount": 245,
               "views": 7453,
               "titleData": "",
               "systemProperties": "",
               "savedByUser": false,
               "achievementId": "AchievementId",
               "greatestMomentId": "GreatestMomentId",
               "thumbnails": [
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/large",
                       "fileSize": 637293,
                       "thumbnailType": "Large"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/small",
                       "fileSize": 163998,
                       "thumbnailType": "Small"
                   }
               ],
               "gameClipUris": [
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest",
                       "uriType": "SmoothStreaming",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest(format=m3u8-aapl)",
                       "uriType": "Ahls",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
                       "fileSize": 88820,
                       "uriType": "Download",
                       "expiration": "2999-12-31T11:59:40Z"
                   }
               ]
           }
       ],
   "pagingInfo":
       {
           "continuationToken": null,
           "totalItems": 1
       }
   }

```


<a id="ID4EABAC"></a>


## <a name="related-uris"></a>URIs relacionados

O seguinte URI é idêntico àquele primário neste documento, mas com um parâmetro de caminho extra para especificar um SCID. Somente os clipes desse usuário para esse SCID serão retornados. O usuário solicitante deve ter acesso ao SCID solicitada, caso contrário, o erro HTTP 403 será retornado.

   * **/users/{ownerId}/scids/{scid}/clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/clips](uri-usersowneridclips.md)
