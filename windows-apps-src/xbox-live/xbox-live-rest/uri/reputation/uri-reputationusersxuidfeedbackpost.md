---
title: POST (/users/xuid({xuid})/feedback)
assetID: 48a7a510-a658-f2a3-c6bc-28a3610006e7
permalink: en-us/docs/xboxlive/rest/uri-reputationusersxuidfeedbackpost.html
description: " POST (/users/xuid({xuid})/feedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8a6e118811203fd34c310840e8688c2255c6beb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660041"
---
# <a name="post-usersxuidxuidfeedback"></a>POST (/users/xuid({xuid})/feedback)
Usado em seu título caso você deseje adicionar uma opção comentários no jogo, em vez de usar o shell. O domínio para esses URIs é `reputation.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EZ)
  * [Cabeçalhos de solicitação necessários](#ID4EEB)
  * [Corpo da solicitação](#ID4ENC)
  * [Cabeçalhos necessários](#ID4EDE)
  * [Autorização](#ID4EXF)
  * [Códigos de status HTTP](#ID4EEG)
  * [Corpo da resposta](#ID4EZH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| ULong| Xbox usuário ID (XUID) do usuário que está sendo relatado.| 
  
<a id="ID4EEB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 101.| 
  
<a id="ID4ENC"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
<a id="ID4EVC"></a>

 
### <a name="required-members"></a>Membros necessários 
 
A solicitação deve conter um [comentários](../../json/json-feedback.md) objeto. 
  
<a id="ID4EED"></a>

 
### <a name="prohibited-members"></a>Membros proibidos 
 
Todos os outros membros são proibidos em uma solicitação.
  
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação 
 

```cpp
{
    "sessionRef":
    {
        "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
        "templateName": "CaptureFlag5",
        "name": "Halo556932",
    },
    "feedbackType": "CommsAbusiveVoice",
    "textReason": "He called me a doodoo-head!",
    "voiceReasonId": null,
    "evidenceId": null
}

      
```

   
<a id="ID4EDE"></a>

 
## <a name="required-headers"></a>Cabeçalhos necessários
 
Os cabeçalhos a seguir são necessários ao fazer uma solicitação de serviços do Xbox Live.
 
| <b>Cabeçalho</b>| <b>Valor</b>| <b>Deacription</b>| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| Versão do contrato de API.| 
| Autorização| XBL3.0 x = [hash]; [token]| Token de autenticação do STS. STSTokenString é substituído pelo token retornado pela solicitação de autenticação.| 
Content-Type| 
aplicativo/json| 
Tipo de dados que estão sendo enviados.| 
  
<a id="ID4EXF"></a>

 
## <a name="authorization"></a>Autorização
 
A solicitação deve incluir um cabeçalho de autorização válido Xbox Live. Se o chamador não tem permissão para acessar este recurso, o serviço retornará um código 403 Forbidden. Se o cabeçalho é inválido ou ausente, o serviço retorna um código não autorizado 401.
  
<a id="ID4EEG"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| Sem conteúdo| A solicitação foi concluída, mas não tem conteúdo retornar.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável| Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação| A solicitação demorou muito para ser concluída.| 
| 409| Conflito| Token de continuação não é mais válido.| 
  
<a id="ID4EZH"></a>

 
## <a name="response-body"></a>Corpo da resposta 
 
Se a chamada for bem-sucedida, nenhum objeto é retornado dessa resposta. Caso contrário, o serviço retorna um [ServiceError](../../json/json-serviceerror.md) objeto.
  
<a id="ID4EOAAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EQAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

  
<a id="ID4E3AAC"></a>

 
##### <a name="reference"></a>Referência 

[Comentários (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   