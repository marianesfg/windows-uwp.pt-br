---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: df61be42-c229-7408-5e4c-dbf4ae95b52b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindexpost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7711beee6551c40afe1afcb031278484a3dc5820
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627461"
---
# <a name="post-usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Move um item em uma lista para uma posição diferente na lista. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EEB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EWC)
  * [Corpo da solicitação](#ID4EVD)
  * [Códigos de status HTTP](#ID4EEE)
  * [Cabeçalhos de solicitação necessários](#ID4E1BAC)
  * [Corpo da resposta](#ID4EQDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários 
 
Essa chamada é fornecida para permitir que o cliente facilmente mover um item para um índice diferente dentro da lista em uma única operação. Somente um item pode ser movido por vez. Se não existir o índice do item a ser movido de um HTTP 400 será retornado. O índice para o ponto de inserção segue as mesmas regras que inserir um padrão. O padrão será 0 – o início da lista e qualquer número maior que o número de itens na lista novamente irá inserir o item ao final da lista. Da mesma forma, o final da lista pode ser indicado, passando "end" para insertIndex. 
 
Essa chamada também permite que você identifique o item a ser movido por itemId (ou combinação de provedor/providerId). Nesse caso, o itemId deve ser passado no corpo da solicitação e ele deve corresponder a um item existente na lista. Se não existir, um erro HTTP 400 será retornado com IndexOutOfRange na descrição; para esta versão especial da chamada, use "-1" como o índice do item a ser movido. 
 
Essa chamada requer um "se-Match: versionNumber" cabeçalho a ser incluído na solicitação se especificar o item por índice. Se usar itemId para indicar qual item para mover, o cabeçalho "If-Match" é opcional. Se estiver presente, o cabeçalho "If-Match" sempre será validado. No cabeçalho "if-Match", o versionNumber é o número de versão atual da lista. Se ele não é incluído (e necessário), ou não coincide com a atual versão lista o código de status de falha de pré-condição de número e, em seguida, um HTTP 412 – será retornada e o corpo da resposta conterá os metadados mais recente da lista que inclui o número da versão atual . Isso é feito para se proteger contra as atualizações de clientes diferentes trampling uns nos outros. 
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| XUID| cadeia de caracteres| XUID do usuário.| 
| ListName| cadeia de caracteres| Nome da lista para manipular.| 
| Índice| cadeia de caracteres| Especifica o índice atual do item a ser movido. Se o valor de índice é zero ou um número inteiro positivo, isso se refere ao índice do item atual e o corpo da solicitação da chamada deve estar vazio. No entanto, se o valor de índice for "-1", o item a ser movido deve ser especificado por ItemId ou provedor/ProviderID no corpo da solicitação da chamada.| 
  
<a id="ID4EWC"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| cadeia de caracteres| Especifica o local da lista para inserir o item. Valores permitidos são "end", números inteiros positivos e zero. "end" coloca o item no final da lista atual. Se o valor especificado estiver além do fim da lista, o item é inserido no final da lista. | 
  
<a id="ID4EVD"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
Um corpo de solicitação é enviado somente ao especificar o item para mover, itemId ou provedor/ProviderId.
 
<a id="ID4E6D"></a>

  
Exemplo de solicitação 

```cpp
{
  "Item":
  {
    "ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }
}
    
```

  
<a id="ID4EEE"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP 
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4E1BAC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Contém o token de STS usado para autenticar e autorizar a solicitação. Deve ser um token do serviço XSTS e incluir o XUID como uma das declarações. | 
| X-XBL-Contract-Version| Especifica qual versão de API está sendo solicitados (números inteiros positivos). PINs oferece suporte à versão 2. Se esse cabeçalho estiver ausente ou o valor não é suportado, o serviço retornará um 400 – Solicitação incorreta com "cabeçalho de versão de contrato ausente ou sem suporte" na descrição do status.| 
| Content-Type| Especifica se o conteúdo de corpos de solicitação/resposta estará em json ou xml. Valores aceitos são "application/json" e "application/xml"| 
| If-Match| Esse cabeçalho deve conter o número de versão atual de uma lista ao fazer a modificar solicitações. Modificar o uso de solicitações PUT, POST, ou excluir verbos. Se a versão no cabeçalho "If-Match" não coincide com a versão atual da lista, a solicitação será rejeitada com um HTTP 412 – Falha de pré-condição código de retorno. (opcional) Também pode ser usado para GETs, se estiverem presentes e a versão transmitida coincide com a versão atual da lista, em seguida, nenhum dado de lista e um HTTP 304 – código não modificado será retornado. | 
  
<a id="ID4EQDAC"></a>

 
## <a name="response-body"></a>Corpo da resposta 
 
Se a chamada for bem-sucedida, o serviço retorna os metadados mais recentes para a lista. 
 
<a id="ID4E1DAC"></a>

 
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

   
<a id="ID4EIEAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EKEAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

   