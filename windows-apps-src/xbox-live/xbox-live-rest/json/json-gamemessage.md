---
title: GameMessage (JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
description: " GameMessage (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a2bddd9e26b4716fd1e33c4b5bbde56672b5d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651291"
---
# <a name="gamemessage-json"></a>GameMessage (JSON)
Um objeto JSON definindo os dados para uma mensagem na fila de mensagens de uma sessão jogo. 
<a id="ID4EN"></a>

  
 
O objeto JSON GameMessage tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| dados| matriz de inteiros sem sinal de 8 bits| Os dados codificada em Base64 que o cliente do jogo que deseja enviar para os outros clientes de jogos. Esse valor é opaco para o servidor. | 
| senderXuid| inteiro sem sinal de 64 bits| A ID de usuário do Xbox do player enviando a mensagem. | 
| sequenceNumber| inteiro com sinal de 32 bits| O número de sequência da mensagem de jogo. Esse valor é atribuído pelo servidor. Números de sequência são garantidos para ser de forma monotônica, mas podem não ser consecutivas. Números de sequência são exclusivos dentro de uma fila de mensagens, mas não entre filas de mensagens. | 
| queueIndex| inteiro com sinal de 32 bits| O índice da fila de mensagens de sessão para a mensagem. Os valores possíveis são 0 a 3.| 
| timeStamp| DateTime| Hora em que a mensagem de jogo foi criada na fila pelo servidor, em UTC. | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "queueIndex": 0,
    "sequenceNumber": 5,
    "senderXuid": 65536,
    "timestamp": "2011-06-23T18:49:50Z",
    "data": null
}
    
```

  
<a id="ID4E1C"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E3C"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>Referência 

[GameSession (JSON)](json-gamesession.md)

   