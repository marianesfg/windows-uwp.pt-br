---
title: POST (/users/xuid(xuid)/lists/PINS/{listname})
assetID: 813c0bd2-aca9-a1f6-9e81-a84a4c897b1e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamepost.html
description: " POST (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d30e5407be128032947f3f701f59ef25a16daaea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589971"
---
# <a name="post-usersxuidxuidlistspinslistname"></a>POST (/users/xuid(xuid)/lists/PINS/{listname})
Insere itens na lista no índice com base no parâmetro de cadeia de caracteres de consulta **insertIndex**. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Comentários](#ID4EY)
  * [Parâmetros de URI](#ID4ETB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4E5B)
  * [Autorização](#ID4EZC)
  * [Códigos de status HTTP](#ID4EGD)
  * [Cabeçalhos de solicitação necessários](#ID4EEAAC)
  * [Exemplo de solicitação](#ID4E1BAC)
  * [Corpo da resposta](#ID4EPCAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Comentários
 
Essa chamada irá inserir itens na lista no índice com base no parâmetro de cadeia de caracteres de consulta **insertIndex** (o padrão é 0 ou o início da lista). Todos os itens no corpo da solicitação serão inseridos nesse ponto na lista. Se o **insertIndex** é maior que o número de itens na lista existente, os novos itens serão inseridos no final.
 
Itens a serem inseridos devem ter os campos obrigatórios indicados na especificação de funcional; Caso contrário, um HTTP 400 será retornado. Da mesma forma, se o resultado da inserção exceder o tamanho máximo da lista (definido por tipo de lista), em seguida, um HTTP 400 será retornado e nada será inserido.
 
Se o item for *não* a ser inserido no início ou no final da lista, em seguida, a **se-Match: versionNumber** cabeçalho é necessário para ser incluído na solicitação. O cabeçalho é opcional se a inserção é para o início ou final. Se estiver presente, o cabeçalho será validado independentemente do local de inserção. No cabeçalho **VersionNumber** é o número de versão atual da lista. Se não for incluído e necessária, ou não coincide com o número de versão atual da lista e, em seguida, um HTTP 412 (Falha na pré-condição) será retornado o código de status e o corpo da resposta conterá os metadados mais recente da lista que inclui o número da versão atual. Isso é para se proteger contra as atualizações de clientes diferentes trampling entre si.
 
Observe que essa chamada não é idempotente; chamadas repetidas com os mesmos dados pode resultar em várias inserções. No entanto, uma vez que nenhuma lista atualmente dá suporte a duplicatas, chamadas repetidas serão provavelmente resultará em HTTP 400 códigos que está sendo retornado.
  
<a id="ID4ETB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| ID de usuário do Xbox (XUID).| 
| listtype| cadeia de caracteres| Tipo de lista (como ele é usado e como ele funciona). Sempre "FIXA" para esses métodos relacionados.| 
| ListName| cadeia de caracteres| Nome da lista (qual lista de um determinado listtype para agir em). Sempre "XBLPins" para itens de pinos.| 
  
<a id="ID4E5B"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| cadeia de caracteres| Opcional. Define onde inserir itens. Valores com suporte: 0, números inteiros positivos e "end". Qualquer valor de índice maior que o número de itens de lista adicionará o novo item na parte inferior da lista e não irá inserir espaço "blank" na lista. Valor padrão: 0.| 
  
<a id="ID4EZC"></a>

 
## <a name="authorization"></a>Autorização
 
Essa chamada espera que um token SAML XSTS na **autorização** cabeçalho. Uma declaração Xuid deve existir dentro desse token SAML para identificar o chamador. Esse valor é usado para determinar se o chamador tem direitos de acesso à lista de dados em questão. A lista em si será identificada pelo Xuid também e será incluída no URI para a lista. Usando isso, estamos no futuro poderá acesso compartilhado de suporte a listas, mas que não é um recurso no momento. Atualmente, todas as listas que um usuário acessa será suas próprias e não há nenhum acesso compartilhado. Assim o Xuid no URI deve corresponder a Xuid no token de declarações SAML. 

> [!NOTE] 
> XBL Auth 2.0 e 3.0 tokens têm suporte no momento. 


  
<a id="ID4EGD"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EEAAC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Contém o token de STS usado para autenticar e autorizar a solicitação. Deve ser um token do serviço XSTS e incluir o XUID como uma das declarações. | 
| X-XBL-Contract-Version| Especifica qual versão de API está sendo solicitados (números inteiros positivos). PINs oferece suporte à versão 2. Se esse cabeçalho estiver ausente ou o valor não é suportado, o serviço retornará um 400 – Solicitação incorreta com "cabeçalho de versão de contrato ausente ou sem suporte" na descrição do status.| 
| Content-Type| Especifica se o conteúdo de corpos de solicitação/resposta estará em json ou xml. Valores aceitos são "application/json" e "application/xml"| 
| If-Match| Esse cabeçalho deve conter o número de versão atual de uma lista ao fazer a modificar solicitações. Modificar o uso de solicitações PUT, POST, ou excluir verbos. Se a versão no cabeçalho "If-Match" não coincide com a versão atual da lista, a solicitação será rejeitada com um HTTP 412 – Falha de pré-condição código de retorno. (opcional) Também pode ser usado para GETs, se estiverem presentes e a versão transmitida coincide com a versão atual da lista, em seguida, nenhum dado de lista e um HTTP 304 – código não modificado será retornado. | 
  
<a id="ID4E1BAC"></a>

 
## <a name="sample-request"></a>Exemplo de solicitação
 
**ContentType**, **ItemId** ou **ProviderId**, **provedor** e **localidade** são obrigatórios.
 

```cpp
{"Items":
  [{
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": "",
    "Provider": "",
    "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC", 
    "AltImageUrl": null, 
    "Title": "The Dark Knight", 
    "SubTitle": null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
  }]}
      
```

  
<a id="ID4EPCAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}        
         
```

   
<a id="ID4E6CAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EBDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   