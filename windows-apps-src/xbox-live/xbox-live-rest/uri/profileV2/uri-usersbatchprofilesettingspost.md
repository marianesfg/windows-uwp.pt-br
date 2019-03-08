---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
description: " POST (/users/batch/profile/settings)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f859a58e32624223d59d918d46f6230a3abd6db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662221"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
Obter o perfil para um usuário ou usuários. O domínio para esses URIs é `profile.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Autorização](#ID4EFB)
  * [Cabeçalhos de solicitação necessários](#ID4EOB)
  * [Corpo da solicitação](#ID4EZC)
  * [Corpo da resposta](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Esta é a URL de perfil só totalmente qualificado que está permitido em. Todas as outras APIs de perfil de clientes são bloqueados.
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>Autorização
 
Para acessar um perfil, são necessárias apenas um token de autenticação normal e declarações.
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | 
| x-xbl-contract-version| inteiro sem sinal de 32 bits| A versão do contrato deve ser definida como 2, para distinguir esta chamada da API Xbox 360.| 
| content-type| cadeia de caracteres| Value = <code>application/json</code>| 
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
<a id="ID4E6C"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
POST /users/batch/profile/settings
   {
      "userIds":[
         "2533274791381930"
       ],
      "settings":[
         "GameDisplayName",
         "GameDisplayPicRaw",
         "Gamerscore",
         "Gamertag"
      ]
   }
      
```

   
<a id="ID4EJD"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EPD"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

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
                  "value":"https://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
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

   
<a id="ID4EZD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E2D"></a>

 
##### <a name="parent"></a>Parent 

[/users/batch/profile/settings](uri-usersbatchprofilesettings.md)

  
<a id="ID4EFE"></a>

 
##### <a name="reference"></a>Referência 

[Profile (JSON)](../../json/json-profile.md)

   