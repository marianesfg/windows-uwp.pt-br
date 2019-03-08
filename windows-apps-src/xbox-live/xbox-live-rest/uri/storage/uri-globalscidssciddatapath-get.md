---
title: GET (/global/scids/{scid}/data/{path})
assetID: 838abdf2-6fe3-cd7a-4356-6fb0b25a505b
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapath-get.html
description: " GET (/global/scids/{scid}/data/{path})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a242041566f2d493f178b139734327c477cad9d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640601"
---
# <a name="get-globalscidssciddatapath"></a>GET (/global/scids/{scid}/data/{path})
Lista informações de arquivo em um caminho especificado. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
  * [Parâmetros de cadeia de caracteres de consulta opcionais](#ID4ECB)
  * [Autorização](#ID4EWC)
  * [Cabeçalhos de solicitação necessários](#ID4EDD)
  * [Corpo da solicitação](#ID4EME)
  * [Códigos de status HTTP](#ID4EZE)
  * [Corpo da resposta](#ID4EMCAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| path| cadeia de caracteres| O caminho para os itens de dados para retornar. Todos os diretórios e subdiretórios correspondentes são retornados. Caracteres válidos incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e barra (/). Pode estar vazia. Comprimento máximo de 256.| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta opcionais 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| int| Retorne os itens começando em N + 1 na coleção, por exemplo, ignorar N itens.| 
| continuationToken| cadeia de caracteres| Retorne os itens começando com o token de continuação fornecido. O parâmetro continuationToken tem precedência sobre skipItems se ambas são fornecidas. Em outras palavras, o parâmetro skipItems será ignorado se o parâmetro continuationToken está presente.| 
| maxItems| int| Número máximo de itens a serem retornados da coleção, que pode ser combinada com skipItems e continuationToken para retornar um intervalo de itens. O serviço pode fornecer um valor padrão se maxItems não estiver presente e pode retornar menos de maxItems, mesmo se a última página de resultados ainda não foi retornada. | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>Autorização 
 
A solicitação deve incluir um cabeçalho de autorização válido Xbox LIVE. Se o chamador não tem permissão para acessar este recurso, o serviço retornará uma resposta 403 Proibido. Se o cabeçalho é inválido ou ausente, o serviço retornará uma resposta 401 não autorizado. 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Versão do contrato de API.| 
| Autorização| XBL3.0 x = [hash]; [token]| Token de autenticação do STS. STSTokenString é substituído pelo token retornado pela solicitação de autenticação. Para obter mais informações sobre como recuperar um token STS e criar um cabeçalho de autorização, consulte Autenticando e autorizando Xbox LIVE as solicitações de serviços.| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP 
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK | A solicitação foi bem-sucedida.| 
| 201| Criado | A entidade foi criada.| 
| 400| Solicitação Inválida | Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado | A solicitação exige autenticação do usuário.| 
| 403| Proibido | A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado | O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável | Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação | A solicitação demorou muito para ser concluída.| 
| 500| Erro Interno do Servidor | O servidor encontrou uma condição inesperada que o impediu de atender à solicitação.| 
| 503| Serviço Indisponível | A solicitação foi restringida, tente a solicitação novamente após o valor de repetição do cliente em segundos (por exemplo, 5 segundos mais tarde).| 
  
<a id="ID4EMCAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Se a chamada for bem-sucedida, o serviço retornará uma matriz de [TitleBlob](../../json/json-titleblob.md) objetos. 
 
<a id="ID4E1CAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
"blobs":
[
    {
        "fileName":"foo\bar\blob.txt,binary",
        "clientFileTime":"2012-01-01T01:02:03.1234567Z",
        "displayName":"Friendly Name",
        "size":12,
        "etag":"0x8CEB3E4F8F3A5BF"
    },
    {
        "fileName":"foo\bar\blob2.txt,binary",
        "displayName":"Blob 2",
        "size":4,
        "etag":"0x8CEB3FE57F1A142"
    },
    {
        "fileName":"foo\jsonblob.txt,json",
        "size":15,
        "etag":"0x8CEB40152B4A6F8"
    }
],
"pagingInfo":
    {
        "continuationToken":"54",
    }
}
         
```

   
<a id="ID4EGDAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIDAC"></a>

 
##### <a name="parent"></a>Parent  

[/global/scids/{scid}/data/{path}](uri-globalscidssciddatapath.md)

  
<a id="ID4EUDAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Referência [TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

   