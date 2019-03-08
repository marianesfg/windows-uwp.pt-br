---
title: Conquista (JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Conquista (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0e20f46a0d97cba496df5c6fb9cda14fbeccccd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628471"
---
# <a name="achievement-json"></a>Conquista (JSON)
Um objeto de medalha (versão 2).
<a id="ID4EN"></a>


## <a name="achievement"></a>Medalha

O objeto de medalha tem a seguinte especificação. Todos os membros são necessários.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| id| cadeia de caracteres| Identificador de recurso.|
| serviceConfigId| cadeia de caracteres| SCID para este recurso. Identifica os títulos que esta conquista está relacionada ao. |
| name| cadeia de caracteres| O nome localizado de realização.|
| titleAssociations| matriz de [TitleAssociation](json-titleassociation.md)| Uma matriz de TitleAssociation.|
| progressState| **ProgressState** enumeração| O estado de progressão: <ul><li>inválido (0): A progressão de medalha está em um estado desconhecido.</li><li>obtido (1): A realização foi desbloqueada.</li><li>inProgress (2): Veja o que está bloqueado, mas o usuário fez o progresso para desbloqueá-lo.</li><li>notStarted (3): Veja o que está bloqueado e o usuário não tiver feito qualquer progresso para desbloqueá-lo.</li></ul> | 
| Progressão| [Progressão](json-progression.md)| Progressão do usuário dentro de realização.|
| mediaAssets| matriz de [MediaAsset](json-mediaasset.md)| Os ativos de mídia associados com a realização, como IDs de imagem. |
| Plataforma| cadeia de caracteres| A plataforma a realização foi acumulada em.|
| isSecret| Valor booliano| Se é ou não veja o segredo.|
| description| cadeia de caracteres| A descrição da realização quando desbloqueado.|
| lockedDescription| cadeia de caracteres| A descrição da realização antes que ele seja desbloqueado.|
| productId| cadeia de caracteres| O ProductId a realização foi lançado com o.|
| achievementType| **AchievementType** enumeração| O tipo de realização (não o mesmo como o tipo anterior na herdados conquistas): <ul><li>inválido (0): Um tipo de medalha desconhecidos e sem suporte.</li><li>persistente (1): Uma realização que não tem nenhuma data de término e pode ser desbloqueadas em qualquer momento.</li><li>desafio (2): Uma realização que tem uma janela de tempo específicos durante o qual pode ser desbloqueada.</li></ul> |
| participationType| **ParticipationType** enumeração| O tipo de participação para a realização. Os valores válidos são o indivíduo ou grupo.|
| timeWindow| TimeWindow| A janela de tempo durante o qual a realização pode ser desbloqueada. Suporte apenas para os desafios.|
| rewards| matriz de [recompensa](json-reward.md)| A coleção de remunerações acumulado quando desbloqueado.|
| estimatedTime| TimeSpan| O tempo estimado para a realização levará para ganhar.|
| deeplink| cadeia de caracteres| Um deeplink para o título.|
| isRevoked| Valor booliano| Ou não a realização é revogada por imposição.|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
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
          "requirements":
          [{
            "id":"12345678-1234-1234-1234-123456789012",
            "current":null,
            "target":"100"
          }],
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

```


<a id="ID4ERAAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ETAAC"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
