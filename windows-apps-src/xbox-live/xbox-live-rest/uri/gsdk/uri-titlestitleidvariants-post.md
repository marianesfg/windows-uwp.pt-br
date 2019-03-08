---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
description: " POST (/titles/{titleId}/variants)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 17974ddf7dec26abac18ccee9fda5249bc9d656f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618491"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
URI chamado por um cliente que recupera uma lista de variantes de jogos para o título especificado ID. Os domínios para esses URIs são `gameserverds.xboxlive.com` e `gameserverms.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EZ)
  * [Cabeçalhos de solicitação necessários](#ID4EIB)
  * [Cabeçalhos de solicitação opcionais](#ID4EED)
  * [Autorização](#ID4E3D)
  * [Corpo da solicitação](#ID4EEE)
  * [Cabeçalhos de resposta necessária](#ID4ELF)
  * [Cabeçalhos de resposta opcional](#ID4EMG)
  * [Corpo da resposta](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Parâmetros de URI
 
| Parâmetro| Descrição| 
| --- | --- | 
| TitleID| ID do título que a solicitação deve operar.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nome do host

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
Ao fazer uma solicitação, os cabeçalhos mostrados na tabela a seguir são necessários.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | 
| Content-Type| aplicativo/json| Tipo de dados que estão sendo enviados.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Comprimento do objeto de solicitação.| 
| x-xbl-contract-version| 1| Versão do contrato de API.| 
| Autorização| XBL3.0 x = [hash]; [token]| Token de autenticação.| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
Ao fazer uma solicitação, os cabeçalhos mostrados na tabela a seguir são opcionais.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | O tipo mime do corpo da solicitação.| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>Autorização

A solicitação deve incluir um cabeçalho de autorização válido Xbox Live. Se o chamador não tem permissão para acessar este recurso, o serviço retorna 403 Proibido em resposta. Se o cabeçalho é inválido ou ausente, o serviço retornará o 401 não autorizado em resposta.
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
A solicitação deve conter um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| locale| O local de variantes para retornar.| 
| maxVariants| O número máximo de variantes para retornar.| 
| publisherOnly|  | 
| Restrição|  | 
 
<a id="ID4EDF"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
  "locale": "en-us",
  "maxVariants": "100",
  "publisherOnly": "false",
  "restriction": null
}

```

   
<a id="ID4ELF"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
Uma resposta sempre incluirá os cabeçalhos mostrados na tabela a seguir.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| aplicativo/json| Tipo de dados no corpo da resposta.| 
| Content-Length|  | Comprimento do corpo da resposta.| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional
 
Uma resposta pode incluem os cabeçalhos mostrados a seguir.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | O tipo mime do corpo da resposta.| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>Corpo da Resposta
 
Se a chamada for bem-sucedida, o serviço retornará um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Variantes| Uma matriz de variantes.| 
| variantId| A Id de uma variante.| 
| name| O nome de uma variante.| 
| isPublisher|  | 
| classificação|  | 
| gameVariantSchemaId|  | 
| variantSchemas| Uma matriz de esquemas variantes.| 
| variantSchemaId| A Id do esquema.| 
| schemaContent| O conteúdo de esquema| 
| name| Nome do esquema| 
| gsiSets| Define uma matriz de GSI.| 
| minRequiredPlayers| O número mínimo de players para a variante.| 
| maxAllowedPlayers| O número máximo de players para a variante.| 
| gsiSetId| A Id do conjunto de GSI.| 
| gsiSetName| O nome do conjunto de GSI.| 
| selectionOrder|  | 
| variantSchemaId| A ID do esquema varaint usado no GSI definida.| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
 "variants": [
     { 
       "variantId": "8B6EF8A0-7807-42C4-9CB0-1D9B8B8CE742", 
       "name": "tankWarsV2.0",
       "isPublisher": "true",
       "rank": "1",
       "gameVariantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }],
  "variantSchemas": [
     {
        "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB",
        "schemaContent": "&lt;?xml version=\"1.0\" encoding=\"UTF-8\" ?>&lt;xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">&lt;xs:element name=\"root\">&lt;/xs:element>&lt;/xs:schema>"
        "name": "tanksSchema"
     }],
     "gsiSets":
     [{ 
          "minRequiredPlayers": "5", 
          "maxAllowedPlayers": "10", 
          "gsiSetId": "B28047F5-B52F-477E-97C2-4C1C39E31D42",
          "gsiSetName": "TanksGSISet",
          "selectionOrder": "1",
          "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }]
 }

  

```

   
<a id="ID4ERCAC"></a>

 
## <a name="see-also"></a>Consulte também
 [/titles/{titleId}/variants](uri-titlestitleidvariants.md)

  