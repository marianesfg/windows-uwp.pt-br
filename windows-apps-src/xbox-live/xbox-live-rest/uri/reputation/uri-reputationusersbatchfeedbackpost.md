---
title: POST (/users/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
description: " POST (/users/batchfeedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0906d32a0e15b2eaaf9c33e7f658e9e9f0cd5124
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622721"
---
# <a name="post-usersbatchfeedback"></a>POST (/users/batchfeedback)
Usado pelo serviço do seu título para enviar comentários na forma de lote fora da interface do seu título. O domínio para esses URIs é `reputation.xboxlive.com`.
 
  * [Corpo da solicitação](#ID4EX)
  * [Cabeçalhos necessários](#ID4E3E)
  * [Códigos de status HTTP](#ID4EWG)
  * [Corpo da resposta](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
Os chamadores devem incluir seu Cert declarações na seção ClientCertificates do seu objeto de solicitação da web.
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>Membros necessários 
 
A solicitação deve conter uma matriz de **BatchFeedback** objetos. 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>Membros proibidos 
 
Todos os outros membros são proibidos em uma solicitação.
  
<a id="ID4E3B"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação 
 

```cpp
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Killed 19 team members in a single session",
            "evidenceId": null
        },
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayQuitter",
            "textReason": "Quit 6 times from 9 sessions",
            "evidenceId": null
        }
    ]
}

      
```

 
| <b>Campo</b>| <b>Tipo JSON</b>| <b>Descrição</b>| 
| --- | --- | --- | 
| itens| matriz| Uma coleção de documentos JSON de comentários.| 
| targetXuid| cadeia de caracteres| XUID do usuário de destino| 
| titleId| cadeia de caracteres| O título que este comentário foi enviado de ou nulo.| 
| sessionRef| objeto| Um objeto que descreve a sessão MPSD este comentário se relaciona ao, ou nulo.| 
| feedbackType| cadeia de caracteres| Uma versão de cadeia de caracteres de um valor na enumeração FeedbackType.| 
| textReason| cadeia de caracteres| Texto fornecido pelo parceiro que o remetente pode adicionar para obter mais detalhes sobre os comentários que foi enviado.| 
| evidenceId| cadeia de caracteres| A ID de um recurso que pode ser usado como evidências dos comentários que estão sendo enviadas. Por exemplo, a ID de um arquivo de vídeo.| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>Cabeçalhos necessários
 
Os cabeçalhos a seguir são necessários ao fazer uma solicitação de serviços do Xbox Live. 

> [!NOTE] 
> Um certificado de declarações do parceiro deve ser enviado com a solicitação para enviar comentários de lote. 


 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| Versão do contrato de API.| 
| Content-Type| aplicativo/json| Tipo de dados que estão sendo enviados.| 
| Autorização| "XBL3.0 x =&lt;userhash >;&lt; token > "| Credenciais de autenticação para a autenticação HTTP.| 
| X-RequestedServiceVersion| 101| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante.| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 500| Erro Interno do Servidor| O servidor encontrou uma condição inesperada que o impediu de atender à solicitação.| 
| 503| Serviço Indisponível| A solicitação foi restringida, tente a solicitação novamente após o valor de repetição do cliente em segundos (por exemplo, 5 segundos mais tarde).| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>Corpo da resposta 
 
Se a chamada for bem-sucedida, nenhum objeto é retornado dessa resposta. Caso contrário, o serviço retorna um [ServiceError](../../json/json-serviceerror.md) objeto.
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>Referência 

[Comentários (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   