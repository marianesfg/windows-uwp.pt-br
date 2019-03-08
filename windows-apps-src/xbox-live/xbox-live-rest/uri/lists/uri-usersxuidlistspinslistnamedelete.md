---
title: DELETE (/users/xuid(xuid)/lists/PINS/{listname})
assetID: b43e3faa-7791-8bcb-3aec-7bdad8ffbebf
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamedelete.html
description: " DELETE (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eed1d73917be450038fdf098b802d0d7c1c44a7b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603341"
---
# <a name="delete-usersxuidxuidlistspinslistname"></a>DELETE (/users/xuid(xuid)/lists/PINS/{listname})
Remove itens de uma lista. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EIB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ETB)
  * [Autorização](#ID4ETC)
  * [Cabeçalhos de solicitação necessários](#ID4EAD)
  * [Corpo da solicitação](#ID4EWE)
  * [Códigos de status HTTP](#ID4EBF)
  * [Corpo da resposta](#ID4E6BAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
São indicadas pelo seu índice na lista de itens a serem removidos e são identificados no parâmetro de cadeia de caracteres de consulta **índices**. A lista de índices deve ser delimitada por vírgulas e apenas 100 índices podem ser passados em uma única chamada. No entanto, se nenhum índice forem passado (parâmetro de cadeia de caracteres de consulta vazia), em seguida, o conteúdo de toda a lista será excluído (uma lista vazia permanecerá, e o número de versão continuará incrementar). Depois que os itens são removidos, a lista é "compactada", de modo que nenhum buracos são deixados na ordenação dos índices. Portanto, essa chamada não é idempotente.
 
Essa chamada requer um **se-Match: versionNumber** cabeçalho a ser incluído na solicitação, onde **versionNumber** é o número de versão atual do arquivo. Se ele não estiver incluído ou não coincide com o número de versão atual da lista e, em seguida, um HTTP 412 (Falha na pré-condição) será retornado o código de status e o corpo da resposta conterá os metadados mais recente da lista que inclui o número da versão atual. Isso é feito para se proteger contra as atualizações de clientes diferentes trampling entre si.
  
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
| Índices| cadeia de caracteres| Opcional. Especifica onde remover itens. Valores com suporte: 0, números inteiros positivos e "end". Valor padrão: 0.| 
 
Exemplo: **índices = 1, 8** remove itens nos índices 1 e 8. Os índices devem ser exclusivos. Se nenhum índice forem fornecido, a lista inteira será limpo.
  
<a id="ID4ETC"></a>

 
## <a name="authorization"></a>Autorização
 
Essa chamada espera que um token SAML XSTS na **autorização** cabeçalho. Uma declaração Xuid deve existir dentro desse token SAML para identificar o chamador. Esse valor é usado para determinar se o chamador tem direitos de acesso à lista de dados em questão. A lista em si será identificada pelo Xuid também e será incluída no URI para a lista. Usando isso, estamos no futuro poderá acesso compartilhado de suporte a listas, mas que não é um recurso no momento. Atualmente, todas as listas que um usuário acessa será suas próprias e não há nenhum acesso compartilhado. Assim o Xuid no URI deve corresponder a Xuid no token de declarações SAML. 

> [!NOTE] 
> XBL Auth 2.0 e 3.0 tokens têm suporte no momento. 


  
<a id="ID4EAD"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Contém o token de STS usado para autenticar e autorizar a solicitação. Deve ser um token do serviço XSTS e incluir o XUID como uma das declarações. | 
| X-XBL-Contract-Version| Especifica qual versão de API está sendo solicitados (números inteiros positivos). PINs oferece suporte à versão 2. Se esse cabeçalho estiver ausente ou o valor não é suportado, o serviço retornará um 400 – Solicitação incorreta com "cabeçalho de versão de contrato ausente ou sem suporte" na descrição do status.| 
| Content-Type| Especifica se o conteúdo de corpos de solicitação/resposta estará em json ou xml. Valores aceitos são "application/json" e "application/xml"| 
| If-Match| Esse cabeçalho deve conter o número de versão atual de uma lista ao fazer a modificar solicitações. Modificar o uso de solicitações PUT, POST, ou excluir verbos. Se a versão no cabeçalho "If-Match" não coincide com a versão atual da lista, a solicitação será rejeitada com um HTTP 412 – Falha de pré-condição código de retorno. (opcional) Também pode ser usado para GETs, se estiverem presentes e a versão transmitida coincide com a versão atual da lista, em seguida, nenhum dado de lista e um HTTP 304 – código não modificado será retornado. | 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EBF"></a>

 
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
  
<a id="ID4E6BAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EFCAC"></a>

 
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

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   