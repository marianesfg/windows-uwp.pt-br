---
title: MatchTicket (JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
description: " MatchTicket (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4bc638dfe7735856295ed92f35e244213be7bc1e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608331"
---
# <a name="matchticket-json"></a>MatchTicket (JSON)
Um objeto JSON que representa um tíquete de correspondência, usado pelos players para localizar outros jogadores através do diretório de sessão para múltiplos jogadores (MPSD). 
<a id="ID4EN"></a>

  
 
O objeto JSON MatchTicket tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| serviceConfig| GUID| Identificador de configuração de serviço (SCID) para a sessão.| 
| hopperName| cadeia de caracteres| Nome do hopper nos quais essa permissão deve ser colocado.| 
| giveUpDuration| inteiro com sinal de 32 bits| Tempo de espera máximo (número integral de segundos).| 
| preserveSession| enumeração| Um valor que indica se a sessão deve ser reutilizada como a sessão na qual corresponder. Os valores possíveis são "sempre" ou "nunca". | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b> objeto para a sessão em que o player ou o grupo está sendo executado atualmente. Este membro sempre é necessário. | 
| ticketAttributes| matriz de objetos| Coleção de atributos fornecidos pelo usuário e valores sobre os tíquetes para os jogadores.| 
| jogadores| matriz de objetos| Coleção de objetos de player, cada um com um conjunto de propriedades de atributos fornecidos pelo usuário. | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
        "serviceConfig": "07617C5B-3423-4505-B6C6-10A16E1E5DDB",
        "hopperName": "TestHopper",
        "giveUpDuration": 10,
        "preserveSession": "never",
        "ticketSessionRef": {
        "scid": "AFFEABDF-0000-0000-0000-000000000001",
        "templateName": "TestTemplate",
        "sessionName": "5E551041-0000-0000-0000-000000000001"
    },
    "ticketAttributes": {
        "desiredMap": "Test Map",
        "desiredGameType": "Test GameType"
    },
    "players": [
        {
            "xuid": 123412345123,
            "playerAttributes": {
                "skill": 5,
                "ageRange": "teenager"
            }
        },
        {
          "xuid": 123412345124,
          "playerAttributes": {
              "skill": 15,
              "ageRange": "twenty-something"
          }
        }
      ]
    }
  
    
```

  
<a id="ID4EEB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EGB"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   