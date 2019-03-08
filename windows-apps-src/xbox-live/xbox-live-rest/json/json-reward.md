---
title: Reward (JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
description: " Reward (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d58d34263e7e0e90091c41c1df4fd5e078f5055
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607491"
---
# <a name="reward-json"></a>Reward (JSON)
A recompensa associada com a realização.
<a id="ID4EN"></a>


## <a name="reward"></a>Recompensa

O objeto de recompensa tem a seguinte especificação.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| name| cadeia de caracteres| O nome de voltadas ao usuário sobre o prêmio.|
| description| cadeia de caracteres| A descrição voltadas ao usuário a recompensa.|
| value| cadeia de caracteres| Valor de recompensa.|
| type| Enumeração RewardType| O tipo de recompensa: <ul><li>inválido (0): Um tipo de recompensa desconhecidos e sem suporte foi configurado.</li><li>Jogos (1): A recompensa adiciona pontos para jogos do jogador.</li><li>inApp (2): A recompensa é definida e entregue pelo título.</li><li>Arte (3): A recompensa é um ativo digital.</li></ul> | 
| valueType| Enumeração ProgressValueDataType| O tipo de valor. Ver [requisito (JSON)](json-requirement.md) para obter mais informações.|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
{
  "name":null,
  "description":null,
  "value":"10",
  "type":"Gamerscore",
  "valueType":"Int"
}

```


<a id="ID4EKD"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EMD"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
