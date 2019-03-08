---
title: GET (/users/xuid({xuid})/groups/{moniker} )
assetID: 63aa7e5d-0599-5850-756d-3079c0772238
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerget.html
description: " GET (/users/xuid({xuid})/groups/{moniker} )"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: df967677ce779fc128a8956f137a027a108d313d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623841"
---
# <a name="get-usersxuidxuidgroupsmoniker-"></a>GET (/users/xuid({xuid})/groups/{moniker} )
Obtém o PresenceRecord para um grupo. O domínio para esses URIs é `userpresence.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4E5)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EJB)
  * [Autorização](#ID4EKC)
  * [Efeito das configurações de privacidade no recurso](#ID4EQD)
  * [Cabeçalhos de solicitação necessários](#ID4EEH)
  * [Cabeçalhos de solicitação opcionais](#ID4EMBAC)
  * [Corpo da solicitação](#ID4EMCAC)
  * [Códigos de status HTTP](#ID4EXCAC)
  * [Cabeçalhos de resposta necessária](#ID4E3GAC)
  * [Cabeçalhos de resposta opcional](#ID4EMJAC)
  * [Corpo da resposta](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Recupera os usuários no grupo especificado pelo moniker relacionado ao usuário no URI e retorna o PresenceRecord para esses usuários. Dados que são restringidos pelo isolamento de conteúdo ou de privacidade simplesmente não serão retornados.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| Xbox usuário ID (XUID) do usuário relacionado aos XUIDs no grupo.| 
| moniker| cadeia de caracteres| Cadeia de caracteres define o grupo de usuários. O moniker aceitação somente no momento é "Pessoas", com uma letra maiuscula 'P'.| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| level| cadeia de caracteres| Retorna o nível de detalhes, conforme especificado por essa cadeia de caracteres de consulta. As opções válidas incluem "user", "device", "title" e "all". O nível de "usuário" é o objeto de PresenceRecord sem DeviceRecord objeto aninhado. O nível de "dispositivo" é os objetos PresenceRecord e DeviceRecord sem TitleRecord objeto aninhado. O nível "title" é os objetos PresenceRecord, DeviceRecord e TitleRecord sem ActivityRecord objeto aninhado. O nível "todos" é todo o registro, incluindo todos os objetos aninhados. Se esse parâmetro não for fornecido, o padrão do serviço para o nível de título (ou seja, retorna a presença desse usuário para baixo até os detalhes de título).| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>Autorização
 
Declarações de autorização usadas | Declaração| Tipo| Necessário?| Valor de exemplo| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>Xuid</b>| inteiro com sinal de 64 bits| sim| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso
 
O serviço sempre retornará 200 Okey se a solicitação em si é bem formada. No entanto, ele filtrará as informações da resposta quando verificações de privacidade não aprovado.
 
Efeito das configurações de privacidade no recurso | Usuário solicitante| Configuração de privacidade do usuário de destino| Comportamento| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Conforme descrito.| 
| Friend| Todas as pessoas| Conforme descrito.| 
| Friend| somente os amigos| Conforme descrito.| 
| Friend| bloqueado| Conforme descrito - o serviço será filtrar os dados.| 
| usuário não friend| Todas as pessoas| Conforme descrito.| 
| usuário não friend| somente os amigos| Conforme descrito - o serviço será filtrar os dados.| 
| usuário não friend| bloqueado| Conforme descrito - o serviço será filtrar os dados.| 
| site de terceiros| Todas as pessoas| Conforme descrito - o serviço será filtrar os dados.| 
| site de terceiros| somente os amigos| Conforme descrito - o serviço será filtrar os dados.| 
| site de terceiros| bloqueado| Conforme descrito - o serviço será filtrar os dados.| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valores de exemplo: 3, vnext.| 
| Aceitar| cadeia de caracteres| Tipos de conteúdo que são aceitáveis. O oferece suporte a presença apenas uma <b>application/json</b>, mas ele ainda deve ser especificado no cabeçalho.| 
| Accept-Language| cadeia de caracteres| Localidade aceitável para cadeias de caracteres na resposta. Valor de exemplo: en-US.| 
| Host| cadeia de caracteres| Nome de domínio do servidor. Valor de exemplo: userpresence.xboxlive.com.| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A sessão foi recuperada com êxito.| 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 405| Método não permitido| Método HTTP usado em um tipo de conteúdo sem suporte.| 
| 406| Não aceitável| Não há suporte para a versão do recurso.| 
| 500| Tempo Limite da Solicitação| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 503| Tempo Limite da Solicitação| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valores de exemplo: 1, vnext.| 
| Content-Type| cadeia de caracteres| O tipo mime do corpo da solicitação. Valor de exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache. Valores de exemplo: "no-cache".| 
| X-XblCorrelationId| cadeia de caracteres| Valor gerado pelo serviço para correlacionar o que retorna o servidor e o que é recebido pelo cliente. Valor de exemplo: "4106d0bc-1cb3-47bd-83fd-57d041c6febe".| 
| X-Content-Type-Option| cadeia de caracteres| Retorna o valor em conformidade com o SDL. Valor de exemplo: "nosniff".| 
| Data| cadeia de caracteres| A data/hora em que a mensagem foi enviada. Valor de exemplo: "Terça-feira, 17 de novembro de 2012 10:33:31 GMT".| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Retry-After| cadeia de caracteres| Retornado no caso de erro HTTP 503. Permite que o cliente quanto tempo para esperar antes de repetir a chamada. Valores de exemplo: "120".| 
| Content-Length| cadeia de caracteres| Comprimento do corpo da resposta. Valor de exemplo: "527".| 
| Codificação de conteúdo| cadeia de caracteres| Tipo de codificação da resposta. Valor de exemplo: "gzip".| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Essa API retorna uma matriz de objetos PresenceRecord, um para cada os XUIDs da solicitação.
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
[
     {
         xuid:"0123456789",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341234",
                         name:"Contoso 5",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement:"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Playing on Nirvana"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456780",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341235",
                         name:"Contoso Waypoint",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement;"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Viewing stats"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456781",
         state:"online",
         devices:
         [
             {
                 type:"Win8",
                 titles:
                 [
                     {
                         id:"23452345",
                         name:"Contoso Gamehelp",
                         state:"active",
                         timestamp:"2012-09-17T07:15:23.4930000",
                         activity:
                         {
                             richPresence:"Viewing help"
                         }
                     }
                 ]
             }
         ]
     }
 ]
         
```

   
<a id="ID4EQLAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ESLAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

   