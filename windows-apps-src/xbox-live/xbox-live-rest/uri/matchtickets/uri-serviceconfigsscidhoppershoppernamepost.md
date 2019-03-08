---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7682b92ec61c98679904825e360d73318e9fee90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659831"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

Cria o tíquete de correspondência especificadas.

> [!IMPORTANT]
> Esse método é destinado para uso com o contrato 103 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 103 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4E5)
  * [Autorização](#ID4EJB)
  * [Códigos de status HTTP](#ID4E3C)
  * [Corpo da solicitação](#ID4EFD)
  * [Corpo da resposta](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST cria um tíquete de correspondência para um hopper com um nome específico em nível de ID (SCID) de configuração do serviço. Esse método pode ser encapsulado pela **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync** método.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| O identificador do configuração de serviço (SCID) para a sessão.|
| hoppername | cadeia de caracteres | O nome do hopper. |

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorização

| Tipo| Obrigatório| Descrição| Resposta se ausente|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Privilégios e o tipo de dispositivo| sim| Quando deviceType do usuário é definido para o console, somente os usuários com o privilégio para múltiplos jogadores em suas declarações têm permissão para fazer chamadas para o serviço de cruzamento. | 403|
| Tipo de dispositivo| sim| Quando o deviceType do usuário está ausente ou definido como não-console, o título que está sendo correspondido em não deve ser um título somente do console. | 403|
| ID/prova do título do tipo de compra/dispositivo| sim| O título que está sendo correspondido em deve permitir emparelhamento para a declaração de título especificado, a combinação de tipo de dispositivo. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Corpo da solicitação

<a id="ID4ELD"></a>


### <a name="required-members"></a>Membros necessários

| Membro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| SCID para a sessão.|
| hopperName| cadeia de caracteres| Nome do hopper.|
| giveUpDuration| inteiro com sinal de 32 bits| Tempo de espera máximo (número integral de segundos).|
| preserveSession| enumeração| Um valor que indica se a sessão é reutilizada como a sessão na qual corresponder. Os valores possíveis são "sempre" e "nunca". |
| ticketSessionRef| MultiplayerSessionReference| O objeto MultiplayerSessionReference para a sessão em que o player ou o grupo está sendo executado atualmente. |
| ticketAttributes| coleção de objetos| Os atributos e valores fornecidos pelo usuário sobre o grupo de jogadores.|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>Membros proibidos

Todos os outros membros são proibidos em uma solicitação.

<a id="ID4ECG"></a>


### <a name="sample-request"></a>Exemplo de solicitação

A sessão à qual o **ticketSessionRef** objeto deve ser criado antes de um tíquete de correspondência pode ser criado e a sessão deve conter os jogadores a serem correspondidos, junto com seus atributos específicos do player. Cada jogador deve criar ou ingressar na sessão em relação a MPSD, adicionando atributos de correspondência associado à sessão. Os atributos de correspondência são colocados em um campo de propriedade personalizada chamado matchAttrs em cada jogador.

Uma solicitação para criar ou junção é enviada ao **https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** e pode ter esta aparência:


```cpp
{
   "members": {
     "me": {
       "constants": {
         "system": {
           "xuid": 2535285330879750
         }
      },
      "properties": {
         "custom": {
           "matchAttrs": {
             "skill": 5,
             "ageRange": "teenager"
           }
         }
      }
    }
  }
}

```


Quando a sessão tiver sido criada, o título pode chamar o serviço de cruzamento para criar o tíquete para a sessão.


> [!NOTE] 
> Um título pode permitir que um usuário de tentar novamente essa chamada, mas não deve tentar novamente ele automaticamente se os dados não.  



```cpp
POST /serviceconfigs/{scid}/hoppers/{hoppername}

{
  "giveUpDuration":10,
  "preserveSession": "never",
  "ticketSessionRef": {
     "scid": "ABBACDDC-0000-0000-0000-000000000001",  
     "templateName": "TestTemplate",
     "name": "5E55104-0000-0000-0000-000000000001"
  },
  "ticketAttributes": {
    "desiredMap": "Test Map",
    "desiredGameType": "Test GameType"
  }
}

```


<a id="ID4E3G"></a>


## <a name="response-body"></a>Corpo da resposta

| Membro| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ticketId| GUID| A ID para a permissão está sendo criada.|
| Tempo de espera| inteiro com sinal de 32 bits| A média de tempo de espera para o hopper (número integral de segundos).|


```cpp
{
  "ticketId":"0584338f-a2ff-4eb9-b167-c0e8ddecae72",
  "waitTime":60
}

```


<a id="ID4EHAAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EJAAC"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)
