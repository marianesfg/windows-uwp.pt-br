---
title: BatchRequest (JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
description: " BatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51073c71700613edcd7c22e18cc0c00a9222d7e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654151"
---
# <a name="batchrequest-json"></a>BatchRequest (JSON)
Uma matriz de propriedades com o qual filtrar as informações de presença, como usuários, dispositivos e títulos.
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

O objeto BatchRequest tem a seguinte especificação.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| usuários| matriz de cadeia de caracteres| XUIDs da lista de usuários cuja presença que você deseja saber mais, com um máximo de 1100 XUIDs por vez.|
| deviceTypes| matriz de cadeia de caracteres| Lista de tipos de dispositivos usados pelos usuários que você deseja conhecer. Se a matriz estiver vazia, ele assume como padrão a todos os tipos de dispositivo possíveis (ou seja, nenhum são filtrados).|
| títulos| matriz de inteiro sem sinal de 32 bits| Os tipos de lista de dispositivos cujos usuários você deseja conhecer. Se a matriz é deixada em branco, o padrão é todos os títulos de possíveis (ou seja, nenhum são filtrados).|
| level| cadeia de caracteres| Valores possíveis: <ul><li>usuário – nós de usuário de get</li><li>dispositivo - usuário get e nós de dispositivo</li><li>título – obter informações de nível básico de título</li><li>todos – Obtenha informações de presença avançada, informações de mídia ou ambos</li></ul>O padrão é "title".| 
| onlineOnly| Valor booliano| Se essa propriedade for true, a operação em lote filtrará os registros para usuários offline (incluindo aqueles encobertos). Se não for fornecido, os usuários online e offline serão retornados.|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}


```


<a id="ID4EJD"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ELD"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Referência   
