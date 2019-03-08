---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 19083937d48d8c312215f1734513d83c59f52f0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627501"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
Obtém os detalhes de realização. O domínio para esses URIs é `achievements.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
  * [Autorização](#ID4EAB)
  * [Efeito das configurações de privacidade no recurso](#ID4E4C)
  * [Cabeçalhos de solicitação necessários](#ID4EPG)
  * [Cabeçalhos de solicitação opcionais](#ID4EPH)
  * [Corpo da solicitação](#ID4ECBAC)
  * [Códigos de status HTTP](#ID4ENBAC)
  * [Corpo da resposta](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário cujo recurso está sendo acessado. Deve corresponder a XUID do usuário autenticado.| 
| scid| GUID| Identificador exclusivo da configuração do serviço cuja medalha está sendo acessada.| 
| achievementid| inteiro sem sinal de 32 bits| Identificador exclusivo (dentro de SCID especificado) da realização que está sendo acessada.| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>Autorização
 
Declarações de autorização usadas | Declaração| Necessário?| Descrição| Comportamento se ausente| 
| --- | --- | --- | --- | --- | --- | --- | 
| Usuário| Sim| Um usuário válido no Xbox LIVE para quem a solicitação está sendo feita.| 403 Proibido| 
| Título| Não| O título de chamada.| Depende do AuthZ. A partir de 1º de maio de 2013, AuthZ não fornece uma declaração quando ausente e, portanto, negar o acesso a qualquer SCIDs não marcados como públicos.| 
| Área restrita| Não| A área restrita do qual os resultados devem ser recuperados.| Depende do AuthZ. A partir de 1º de maio de 2013, AuthZ não fornece uma declaração padrão quando ausente.| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso
 
Efeito das configurações de privacidade no recurso | Usuário solicitante| Configuração de privacidade do usuário de destino| Comportamento| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Conforme descrito.| 
| Friend| Todas as pessoas| OK| 
| Friend| somente os amigos| OK| 
| Friend| bloqueado| Proibido.| 
| usuário não friend| Todas as pessoas| OK| 
| usuário não friend| somente os amigos| Proibido.| 
| usuário não friend| bloqueado| Proibido.| 
| site de terceiros| Todas as pessoas| OK| 
| site de terceiros| somente os amigos| Proibido.| 
| site de terceiros| bloqueado| Proibido.| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.| 
| x-xbl-contract-version| cadeia de caracteres| O padrão é V1.| 
| Accept-Language| cadeia de caracteres| Lista de localidades desejadas e fallbacks (por exemplo, "fr-FR", "fr, en-GB, en-WW, en-US"). O serviço realizações funcionará por meio da lista até encontrar a correspondência de cadeias de caracteres localizadas. Se nenhum for encontrado, ele tenta corresponder ao local definido no token de usuário, que é proveniente do endereço IP do usuário. Se ainda não há correspondência cadeias de caracteres localizadas são encontradas, ele usa as cadeias de caracteres padrão fornecidas pelo editor/desenvolvedor de título. | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A sessão foi recuperada com êxito.| 
| 301| Movido permanentemente| O serviço foi movido para um URI diferente.| 
| 307| Redirecionamento temporário| O URI para esse recurso foi alterado temporariamente.| 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável| Não há suporte para a versão do recurso; deve ser rejeitada pela camada de MVC.| 
| 408| Tempo Limite da Solicitação| A solicitação demorou muito para ser concluída.| 
| 410| Passou| O recurso solicitado não está mais disponível.| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
    "achievement":
    {
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   