---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f868fdf4f3d5cd36000784d9c5a3437fa5d67ffa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593851"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
Obter o perfil para um usuário ou dar suporte a usuários, com o Moniker de pessoas. O domínio para esses URIs é `profile.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EKB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EVB)
  * [Cabeçalhos de solicitação necessários](#ID4EQC)
  * [Corpo da solicitação](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
**userList** e **userIds** são parâmetros mutuamente exclusivas. Se um ou ambos forem especificados, você obterá um **BadRequest** novamente. **userList** é uma matriz para durabilidade em cenários em que várias listas nomeadas são úteis para a solicitação. **userIds** é composto de cadeias de caracteres decimais para XUIDs - JSON é ruim em serializar inteiros sem sinal de 64 bits. Por fim, as configurações no Xbox One serão nomeadas configurações, com nomes legíveis normais, em vez de números inteiros sem sinal de 64 bits ou obscuros constantes como **XONLINE_PROFILE_ASDF**.
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| userId| cadeia de caracteres| Pode ser 'xuid(12345)', 'gt(myGamertag)' ou 'me'.| 
| userList| cadeia de caracteres| Uma lista nomeada de pessoas para obter as configurações para. Atualmente, as pessoas é a lista somente tem suporte.| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| configurações| cadeia de caracteres| Uma lista delimitada por vírgulas de nomes de configuração.| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| inteiro com sinal de 32 bits| Value = 2| 
| content-type| cadeia de caracteres| Value = <code>application/json</code>| 
  
<a id="ID4E2D"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
<a id="ID4EBE"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
GET /users/me/profile/settings/people/people?settings=GameDisplayName,GameDisplayPicRaw,Gamerscore,Gamertag
      
```

  
<a id="ID4EKE"></a>

  
 
<a id="ID4EME"></a>

 
##### <a name="response-body"></a>Corpo da resposta 
A resposta é um **ReadMultiSettingsResponseV2** objeto. Supondo que o usuário que está chamando tem apenas um amigo:
  

```cpp
{
      "profileUsers":[
         {
            "id":"2533274791381930",
            "settings":[
               {
                  "id":"GameDisplayName",
                  "value":"John Smith"
               },
               {
                  "id":"GameDisplayPicRaw",
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F&format=png&w=64&h=64"
               },
               {
                  "id":"Gamerscore",
                  "value":"0"
               },
               {
                  "id":"Gamertag",
                  "value":"CracklierJewel9"
               }
            ]
         }
      ]
   }
         
```

   
<a id="ID4E3E"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E5E"></a>

 
##### <a name="parent"></a>Parent 

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [Profile (JSON)](../../json/json-profile.md)

   