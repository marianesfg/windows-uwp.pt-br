---
title: Property (JSON)
assetID: 93de547e-d936-6fcc-92cb-e4dd284dd609
permalink: en-us/docs/xboxlive/rest/json-property.html
description: " Property (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e2a721886509c49c60d663d491f8d49bc3c95e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659701"
---
# <a name="property-json"></a>Property (JSON)
Contém dados de propriedade fornecidos pelo cliente para critérios de relacionamento de pessoas da solicitação.
<a id="ID4EN"></a>


## <a name="property"></a>Propriedade

O objeto de propriedade tem a seguinte especificação.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| id| cadeia de caracteres| Uma id para essa propriedade.|
| type| inteiro com sinal de 32 bits | Tipo da propriedade. Valores com suporte são: <ul><li>0 = inteiro</li><li>1 = cadeia de caracteres</li></ul>| 
| value| cadeia de caracteres| Valor dessa propriedade.|

<a id="ID4EGC"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
{
    "id":"1",
    "value":"20",
    "type":0
}

```


<a id="ID4EPC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ERC"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
