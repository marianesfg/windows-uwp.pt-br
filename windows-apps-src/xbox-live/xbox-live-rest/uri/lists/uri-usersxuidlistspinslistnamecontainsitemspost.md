---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
assetID: 86ee6d1a-fb1f-b918-f605-a9b494c0e787
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamecontainsitemspost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c18bf70773de60d4c831d4be891255f98750efa8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636871"
---
# <a name="post-usersxuidxuidlistspinslistnamecontainsitems"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
Determina se uma lista contém um conjunto de itens (especificada pelo itemId) sem recuperar toda a lista. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EAB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EJC)
  * [Corpo da solicitação](#ID4EUC)
  * [Códigos de status HTTP](#ID4E6C)
  * [Cabeçalhos de solicitação necessários](#ID4EVAAC)
  * [Corpo da resposta](#ID4ELCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários 
 
Esse ponto de extremidade permite que o chamador determinar se um ou mais itens existem na lista especificada sem realmente Obtendo a lista e procurando em si. O conjunto de itens são identificados por itemId (ou combinação de provedor/providerId) e os dados de retornados são os dados passados com um valor booleano de true ou false indicadora se a lista contém cada item. 
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| XUID| cadeia de caracteres| XUID do usuário.| 
| ListName| cadeia de caracteres| Nome da lista para manipular.| 
  
<a id="ID4EJC"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta 
 
Não há suporte para parâmetros de consulta.
  
<a id="ID4EUC"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 

```cpp
{
  "Items":
  [{"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }]
}


    
```

  
<a id="ID4E6C"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP 
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK | A solicitação foi concluída com êxito. O corpo da resposta deve conter o recurso solicitado (para um GET). POST e PUT solicitações receberão lista atualizada de metadados (versão de lista, contagem, etc.).| 
| 201| Criado | Uma nova lista foi criada. É retornado no menu Inserir inicial para uma lista. A resposta inclui os metadados atualizados na lista e o cabeçalho de local contém o URI para a lista.| 
| 304| Não modificado| Retornado no obtém. Indica que o cliente tem a versão mais recente da lista. O serviço compara o valor de <b>If-Match</b> cabeçalho para a versão da lista. Se eles forem iguais, um 304 obtém retornou com nenhum dado. | 
| 400| Solicitação Inválida | A solicitação foi formada incorretamente. Pode ser um valor inválido ou o tipo para um parâmetro de cadeia de caracteres URI ou de consulta. Também pode ser o resultado de um parâmetro obrigatório ausente ou valor de dados ou a solicitação indicou uma versão ausente ou inválida da API. Consulte a <b>X-XBL-contrato-Version</b> cabeçalho. | 
| 401| Não autorizado | A solicitação exige autenticação do usuário.| 
| 403| Proibido | A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado | O chamador não tem acesso ao recurso. Isso indica que nenhuma dessas listas foi criada.| 
| 412| Falha na pré-condição | Indica que a versão da lista foi alterada e uma solicitação de modificar falhou. Uma solicitação de modificar é uma inserção, atualização ou remover. O serviço verifica as <b>If-Match</b> cabeçalho para a versão da lista. Se ele não coincide com a versão atual da lista, a operação falhará e isso é retornado juntamente com os metadados de lista atual (que inclui a versão atual). | 
| 500| Erro Interno do Servidor | O serviço está se recusando a solicitação devido a um erro do lado do servidor.| 
| 501| Não implementado | O chamador solicitou um URI que não foi implementado no servidor. (Atualmente somente usado quando uma solicitação é feita para um nome de lista que não está na lista de permissões.)| 
| 503| Serviço Indisponível | O servidor está se recusando a solicitação, geralmente devido à carga excessiva. Após um atraso (consulte a <b>Retry-after</b> cabeçalho), a solicitação pode ser repetida. | 
  
<a id="ID4EVAAC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Contém o token de STS usado para autenticar e autorizar a solicitação. Deve ser um token do serviço XSTS e incluir o XUID como uma das declarações. | 
| X-XBL-Contract-Version| Especifica qual versão de API está sendo solicitados (números inteiros positivos). PINs oferece suporte à versão 2. Se esse cabeçalho estiver ausente ou o valor não é suportado, o serviço retornará um 400 – Solicitação incorreta com "cabeçalho de versão de contrato ausente ou sem suporte" na descrição do status. | 
| Content-Type| Especifica se o conteúdo de corpos de solicitação/resposta estará em json ou xml. Valores aceitos são "application/json" e "application/xml"| 
| If-Match| Esse cabeçalho deve conter o número de versão atual de uma lista ao fazer a modificar solicitações. Modificar o uso de solicitações PUT, POST, ou excluir verbos. Se a versão no cabeçalho "If-Match" não coincide com a versão atual da lista, a solicitação será rejeitada com um HTTP 412 – Falha de pré-condição código de retorno. (opcional) Também pode ser usado para GETs, se estiverem presentes e a versão transmitida coincide com a versão atual da lista, em seguida, nenhum dado de lista e um HTTP 304 – código não modificado será retornado. | 
  
<a id="ID4ELCAC"></a>

 
## <a name="response-body"></a>Corpo da resposta 
 
Se a chamada for bem-sucedida, a lista de itens for retornada, junto com um valor booliano para cada item que especifica se o item está na lista ou não. 
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo 
 

```cpp
{
  "ContainedItems":
  [{"Contained": "true",
    "Item":
   {"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId": null,
    "Provider": null
   }
  }]
}


      
```

   
<a id="ID4EBDAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EDDAC"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   