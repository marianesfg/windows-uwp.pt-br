---
title: DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
assetID: ad049340-f752-e05e-8b34-62adb8e4771f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameremoveitemsdelete.html
description: " DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d26d8eaf54dcc14de3e31d7c2b54cd4454f2029f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594091"
---
# <a name="delete-usersxuidxuidlistspinslistnameremoveitems"></a>DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
Remove itens de uma lista por índice. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4ECB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ELC)
  * [Corpo da solicitação](#ID4END)
  * [Códigos de status HTTP](#ID4EYD)
  * [Cabeçalhos de solicitação necessários](#ID4EOBAC)
  * [Corpo da resposta](#ID4EEDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários 
 
Remova itens de uma lista. Itens a serem removidos são indicadas pelo seu índice na lista e são identificados no parâmetro de cadeia de caracteres de consulta "índices". A lista de índices deve ser delimitados por vírgulas e apenas 100 índices podem ser passados em uma única chamada. No entanto, se nenhum índice é passada (parâmetro de cadeia de caracteres de consulta vazia), em seguida, a lista inteira de conteúdo será excluída (lista permanecerão mas vazio, o número de versão continuará a incrementar). Depois que os itens são removidos, a lista é "compactada", de modo que nenhum buracos são deixados na ordenação dos índices. 
 
Essa chamada requer um "se-Match: versionNumber" cabeçalho a ser incluído na solicitação, em que versionNumber é o número de versão atual do arquivo. Se ele não estiver incluído ou não coincide com a atual versão lista o código de status de falha de pré-condição de número e, em seguida, um HTTP 412 – será retornada e o corpo da resposta conterá os metadados mais recente da lista que inclui o número da versão atual. Isso é feito para se proteger contra as atualizações de clientes diferentes trampling uns nos outros. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| XUID| cadeia de caracteres| XUID do usuário.| 
| ListName| cadeia de caracteres| Nome da lista para manipular.| 
  
<a id="ID4ELC"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Índices| cadeia de caracteres| Zero ou um número inteiro positivo. Os números não tem que ser contíguos. Por exemplo, os índices de parâmetro de consulta = 1, 8 removerá os itens nos índices 1 e 8. <b>Se o índice não for especificado, a lista inteira será excluída.</b>| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
O corpo da solicitação deve estar vazio para esta chamada.
  
<a id="ID4EYD"></a>

 
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
  
<a id="ID4EOBAC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Contém o token de STS usado para autenticar e autorizar a solicitação. Deve ser um token do serviço XSTS e incluir o XUID como uma das declarações. | 
| X-XBL-Contract-Version| Especifica qual versão de API está sendo solicitados (números inteiros positivos). PINs oferece suporte à versão 2. Se esse cabeçalho estiver ausente ou o valor não é suportado, o serviço retornará um 400 – Solicitação incorreta com "cabeçalho de versão de contrato ausente ou sem suporte" na descrição do status. | 
| Content-Type| Especifica se o conteúdo de corpos de solicitação/resposta estará em json ou xml. Valores aceitos são "application/json" e "application/xml"| 
| If-Match| Esse cabeçalho deve conter o número de versão atual de uma lista ao fazer a modificar solicitações. Modificar o uso de solicitações PUT, POST, ou excluir verbos. Se a versão no cabeçalho "If-Match" não coincide com a versão atual da lista, a solicitação será rejeitada com um HTTP 412 – Falha de pré-condição código de retorno. (opcional) Também pode ser usado para GETs, se estiverem presentes e a versão transmitida coincide com a versão atual da lista, em seguida, nenhum dado de lista e um HTTP 304 – código não modificado será retornado. | 
  
<a id="ID4EEDAC"></a>

 
## <a name="response-body"></a>Corpo da resposta 
 
Se a chamada for bem-sucedida, o serviço retorna os metadados mais recentes para a lista. 
 
<a id="ID4EODAC"></a>

 
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

   
<a id="ID4E1DAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E3DAC"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   