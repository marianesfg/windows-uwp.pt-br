---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a31e6a6b541276d3191ba5d40767a1929c70168
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641621"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
Retorna o conteúdo de uma lista. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EIB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ETB)
  * [Autorização](#ID4ESD)
  * [Cabeçalhos de solicitação necessários](#ID4E6D)
  * [Corpo da solicitação](#ID4EVF)
  * [Códigos de status HTTP](#ID4EAG)
  * [Corpo da resposta](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
O **listCount** campo nos dados retornados indica quantos itens estão na lista de total mantida pelo serviço – como tal, ele pode ser usado para determinar onde está o final da lista e é possível que haja um número diferente de quantos itens específicos foram retornados pela solicitação.
 
Se a lista ainda não existir, então os resultados não conterão nenhum item de lista, o **listCount** será zero e o **listVersion** será zero.
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| ID de usuário do Xbox (XUID).| 
| listtype| cadeia de caracteres| Tipo de lista (como ele é usado e como ele funciona). Sempre "FIXA" para esses métodos relacionados.| 
| ListName| cadeia de caracteres| Nome da lista (qual lista de um determinado listtype para agir em). Sempre "XBLPins" para itens de pinos.| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| inteiro com sinal de 32 bits| Opcional. Número de itens a serem ignorados na enumeração antes de retornar resultados. Valor padrão: 0.| 
| maxItems| inteiro com sinal de 32 bits| Opcional. Número máximo de itens a serem retornados. O padrão é 25 itens se nenhum máximo for especificado na solicitação. O serviço não estabelece um máximo desse valor; Se o valor for maior que o número de itens na lista, em seguida, todos os itens serão retornados sem erro.| 
| filterItemId| cadeia de caracteres| Opcional. Especifica o item a ser localizado na lista. Retorna todas as instâncias do item na lista. Permite que o cliente determinar rapidamente se e onde um item está em uma lista. Útil para grandes listas determinar todas as instâncias de um item sem iterando por toda a lista. Valor padrão: null.| 
| filterContentType| cadeia de caracteres| Opcional. Especifica uma lista separada por vírgulas de tipos de conteúdo para retornar (não retornará tipos não está na lista). Usado para obter somente determinados tipos de conteúdo de uma lista. Uma lista separada por vírgulas dos tipos de conteúdo é usada para este filtro. (Vários tipos de conteúdo podem ser consultados em uma chamada). Tipos de conteúdo com suporte incluem todos os tipos de mídia definidos por serviços de descoberta de entretenimento (EDS). Valor padrão: null (todos os tipos de conteúdo).| 
| filterDeviceType| cadeia de caracteres| Opcional. Especifica uma lista separada por vírgulas dos tipos de dispositivo para retornar (não retornará tipos não está na lista). Filtra o conjunto de retorno para retornar apenas os itens que foram inseridas de um conjunto específico de tipos de dispositivo. Uma lista separada por vírgulas dos tipos de dispositivo é usada para este filtro (vários tipos de dispositivos podem ser consultados em uma chamada). Valores possíveis: XboxOne, MCapensis, WindowsPhone, WindowsPhone7, Web, PC, MoLive. Valor padrão: null (todos os tipos de conteúdo).| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>Autorização
 
Essa chamada espera que um token SAML XSTS na **autorização** cabeçalho. Uma declaração Xuid deve existir dentro desse token SAML para identificar o chamador. Esse valor é usado para determinar se o chamador tem direitos de acesso à lista de dados em questão. A lista em si será identificada pelo Xuid também e será incluída no URI para a lista. Usando isso, estamos no futuro poderá acesso compartilhado de suporte a listas, mas que não é um recurso no momento. Atualmente, todas as listas que um usuário acessa será suas próprias e não há nenhum acesso compartilhado. Assim o Xuid no URI deve corresponder a Xuid no token de declarações SAML. 

> [!NOTE] 
> XBL Auth 2.0 e 3.0 tokens têm suporte no momento. 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Contém o token de STS usado para autenticar e autorizar a solicitação. Deve ser um token do serviço XSTS e incluir o XUID como uma das declarações. | 
| X-XBL-Contract-Version| Especifica qual versão de API está sendo solicitados (números inteiros positivos). PINs oferece suporte à versão 2. Se esse cabeçalho estiver ausente ou o valor não é suportado, o serviço retornará um 400 – Solicitação incorreta com "cabeçalho de versão de contrato ausente ou sem suporte" na descrição do status.| 
| Content-Type| Especifica se o conteúdo de corpos de solicitação/resposta estará em json ou xml. Valores aceitos são "application/json" e "application/xml"| 
| If-Match| Esse cabeçalho deve conter o número de versão atual de uma lista ao fazer a modificar solicitações. Modificar o uso de solicitações PUT, POST, ou excluir verbos. Se a versão no cabeçalho "If-Match" não coincide com a versão atual da lista, a solicitação será rejeitada com um HTTP 412 – Falha de pré-condição código de retorno. (opcional) Também pode ser usado para GETs, se estiverem presentes e a versão transmitida coincide com a versão atual da lista, em seguida, nenhum dado de lista e um HTTP 304 – código não modificado será retornado. | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EAG"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A solicitação foi concluída com êxito. O corpo da resposta deve conter o recurso solicitado (para um GET). POST e PUT solicitações receberão lista atualizada de metadados (versão de lista, contagem, etc.).| 
| 201| Criado| Uma nova lista foi criada. É retornado no menu Inserir inicial para uma lista. A resposta inclui os metadados atualizados na lista e o cabeçalho de local contém o URI para a lista.| 
| 304| Não modificado| Retornado no obtém. Indica que o cliente tem a versão mais recente da lista. O serviço compara o valor de <b>If-Match</b> cabeçalho para a versão da lista. Se eles forem iguais, um 304 obtém retornou com nenhum dado.| 
| 400| Solicitação Inválida| A solicitação foi formada incorretamente. Pode ser um valor inválido ou o tipo para um parâmetro de cadeia de caracteres URI ou de consulta. Também pode ser o resultado de um parâmetro obrigatório ausente ou valor de dados ou a solicitação indicou uma versão ausente ou inválida da API. Consulte a <b>X-XBL-contrato-Version</b> cabeçalho.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O chamador não tem acesso ao recurso. Isso indica que nenhuma dessas listas foi criada.| 
| 412| Falha na pré-condição| Indica que a versão da lista foi alterada e uma solicitação de modificar falhou. Uma solicitação de modificar é uma inserção, atualização ou remover. O serviço verifica as <b>If-Match</b> cabeçalho para a versão da lista. Se ele não coincide com a versão atual da lista, a operação falhará e isso é retornado juntamente com os metadados de lista atual (que inclui a versão atual).| 
| 500| Erro Interno do Servidor| O serviço está se recusando a solicitação devido a um erro do lado do servidor.| 
| 501| Não implementado| O chamador solicitou um URI que não foi implementado no servidor. (Atualmente somente usado quando uma solicitação é feita para um nome de lista que não está na lista de permissões.)| 
| 503| Serviço Indisponível| O servidor está se recusando a solicitação, geralmente devido à carga excessiva. Após um atraso (consulte a <b>Retry-after</b> cabeçalho), a solicitação pode ser repetida.| 
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[URIs de Marketplace](../marketplace/atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   