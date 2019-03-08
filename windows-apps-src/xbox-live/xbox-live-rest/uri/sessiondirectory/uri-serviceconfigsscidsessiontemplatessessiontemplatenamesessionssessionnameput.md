---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d35b3f89f8b866a5236e8f5ac91eb37d9a82d306
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598551"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
Cria, atualiza ou ingressa em uma sessão.

> [!IMPORTANT]
> Esse método URI requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EYB)
  * [Códigos de status HTTP](#ID4EFC)
  * [Corpo da solicitação](#ID4EOC)
  * [Corpo da resposta](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST cria junções, ou atualiza uma sessão, dependendo de qual subconjunto do mesmo modelo de corpo de solicitação JSON é enviado. Em caso de sucesso, ele retorna um **MultiplayerSession** do objeto que contém a resposta retornada do servidor. Os atributos podem ser diferentes dos atributos no passado **MultiplayerSession** objeto. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**.

Operações de criação e atualização de sessão usam PUT com um corpo de aplicativo/json que representa as alterações sejam aplicadas. As operações são idempotentes, ou seja, vários aplicativos das alterações não tem nenhum efeito adicional.

O corpo da solicitação JSON espelha a estrutura de dados de sessão. Todos os campos e subcampos são opcionais.

O formato de conexão para a criação da sessão do método PUT ou ingressar em modo é mostrado abaixo.

> [!NOTE]
> Tenha cuidado ao usar esse padrão. Atualiza são aplicadas às cegas, não importa o estado atual da sessão.



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



O formato de conexão para o modo de atualização de sessão do método PUT é mostrado abaixo.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



O formato de conexão para o método PUT atualizar as propriedades de sessão é mostrado abaixo. É equivalente a uma operação PUT para a sessão de URI com um corpo de não ter nada, mas o objeto abaixo como propriedades. A diferença é que essa operação retorna o código de erro 404 não encontrado se a sessão não existe. Essa operação dá suporte ao cabeçalho If-Match.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 do identificador de sessão.|
| sessionTemplateName| cadeia de caracteres| Nome da instância atual do modelo de sessão. Parte 2 do identificador de sessão.|
| sessionName| GUID| ID exclusiva da sessão. Parte 3 do identificador de sessão.|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EOC"></a>


## <a name="request-body"></a>Corpo da solicitação

Abaixo está um exemplo de corpo de solicitação para criar ou ingressar em uma sessão. Os seguintes membros do corpo da solicitação são opcionais. Todos os outros membros possíveis são proibidos em uma solicitação.

| Membro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Constantes| objeto| Configurações de somente leitura que são mescladas com o modelo de sessão para produzir as constantes para a sessão. |
| propriedades | objeto | Alterações a ser mesclado com as propriedades de sessão.|
| members.me | objeto| Constantes e as propriedades que são muito parecidas, como suas contrapartes de nível superior. Qualquer método PUT exige que o usuário seja membro da sessão e adiciona o usuário se necessário. Se "me" é especificado como nulo, o membro que está fazendo a solicitação será removido da sessão. |
| membros | objeto| Outros objetos que representam os usuários a adicionar à sessão, inseridos por um índice baseado em zero. O número de membros em uma solicitação sempre começa com 0, mesmo se a sessão já contém membros. Os membros são adicionados à sessão na ordem em que aparecem na solicitação. Propriedades de membro só podem ser definidas pelo usuário aos quais eles pertencem. |
| servidores | objeto| Valores que indicam as atualizações e adições à sessão do conjunto de participantes do servidor associado. Se um servidor for especificado como null, essa entrada do servidor será removida da sessão. |



```cpp
{
  "properties": {
    "custom": {"KANWE": "MGMSY"},
    "system": {}
  },
  "constants": {
    "custom": {},
    "system": {"visibility": "open"}
  },
  "members": {
    "reserve_0": {
    "constants": {
      "custom": {"type": "leader"},
      "system": {"xuid": "5500461"} }}
   }
}

```


<a id="ID4E4C"></a>


## <a name="response-body"></a>Corpo da resposta

Corpo da resposta de exemplo para criar ou ingressar em uma sessão:


```cpp
{
  "contractVersion": 104,
  "correlationId": "0FE81338-EE96-46E3-A3B5-2DBBD6C41C3B",
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": {
    "system": {"visibility": "open"},
    "custom": {}
  },

  "properties": {
     "system": { "turn": [] },
     "custom": { "myProperty": "myValue" }
  },

  "members": {
      "1": {
        "properties": {
        "system": { },
        "custom": { }
      },

      "constants": {
        "system": { "xuid": "5500461" },
        "custom": { }
      }

      "gamertag": "stacy",
      "deviceToken": "9f4032ba7",
      "reserved": true,
      "activeTitleId": "8397267",
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      "turn": true,
      "initializationFailure": "latency",
      "initializationEpisode": 1,
      "next": 4
    },
  },

  "membersInfo": {
      "first": 1,
      "next": 4,
      "count": 1,
      "accepted": 0
  },

  "servers": {
      "name": {
        "constants": { },
        "properties": { }
      }
  }
}

```


<a id="ID4EID"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EKD"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
