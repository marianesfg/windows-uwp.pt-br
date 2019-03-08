---
title: GameResult (JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
description: " GameResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b408b1aaae5e6f54958a016575c4a2c37765f1e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602331"
---
# <a name="gameresult-json"></a>GameResult (JSON)
Um objeto JSON que representa dados que descreve os resultados de uma sessão de jogo. 
<a id="ID4EN"></a>

  
 
O objeto JSON GameResult tem os seguintes membros.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| blob| matriz de inteiros sem sinal de 8 bits| Dados de resultados personalizados do título específico.| 
| Resultado| cadeia de caracteres| O resultado de participação do jogador na sessão do jogo. Os valores válidos são "Win", "Perda" ou "Vincular". | 
| pontuação| inteiro com sinal de 64 bits| A pontuação de que o jogador recebido na sessão do jogo.| 
| tempo| inteiro com sinal de 64 bits| Hora do player para a sessão de jogo.| 
| xuid| inteiro sem sinal de 64 bits| A ID de usuário do Xbox do player aos quais os resultados se aplicam.| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "xuid": 2533274790412952,
   "outcome": "Win",
   "score": 100
}
    
```

  
<a id="ID4EYC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E1C"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   