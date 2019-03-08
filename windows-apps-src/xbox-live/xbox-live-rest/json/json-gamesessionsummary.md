---
title: GameSessionSummary (JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
description: " GameSessionSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8dace19404ae7c8b1d1ef296a21c874e4dd14c6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613381"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary (JSON)
Um objeto JSON que representa dados de resumo para uma sessão de jogo. 
<a id="ID4EN"></a>

  
 
O objeto JSON GameSessionSummary tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| creationTime| DateTime| A data e hora em que a sessão foi criada, em UTC. | 
| customData| matriz de inteiros sem sinal de 8 bits| 1024 bytes de dados de sessão específicas do jogo. Esse valor é opaco para o servidor. | 
| displayName| cadeia de caracteres| A sessão de nome do jogo de exibição, com um comprimento máximo de 128 caracteres. Esse valor é opaco para o servidor. | 
| hasEnded| Valor booliano| True se a sessão foi encerrada e false caso contrário. Definir este campo para as marcas de true a sessão de jogo como somente leitura, impedindo que os dados adicionais que estão sendo enviadas à sessão. | 
| sessionId| cadeia de caracteres de ID da sessão. | 
| titleId| inteiro sem sinal de 32 bits| A ID do título do qual criar a sessão de jogo.| 
| Variant| inteiro com sinal de 32 bits| A variante do jogo. Esse valor é opaco para o servidor.| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "hasEnded": false,
}
    
```

  
<a id="ID4ERD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ETD"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Referência 

[GameSession (JSON)](json-gamesession.md)

   