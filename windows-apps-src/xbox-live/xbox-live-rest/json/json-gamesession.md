---
title: GameSession (JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
description: " GameSession (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca7276ccdc13d896d19873811b4fa9df9a831cd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646511"
---
# <a name="gamesession-json"></a>GameSession (JSON)
Um objeto JSON que representa dados de jogos para uma sessão com vários participantes. 
<a id="ID4ER"></a>

  
 
O objeto JSON GameSession tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| creationTime| DateTime| A data e hora em que a sessão foi criada, em UTC. | 
| customData| matriz de inteiros sem sinal de 8 bits| 1024 bytes de dados de sessão específicas do jogo. Esse valor é opaco para o servidor. | 
| displayName| cadeia de caracteres| A sessão de nome do jogo de exibição, com um comprimento máximo de 128 caracteres. Esse valor é opaco para o servidor. | 
| hasEnded| Valor booliano| True se a sessão foi encerrada e false caso contrário. Definir este campo para as marcas de true a sessão de jogo como somente leitura, impedindo que os dados adicionais que estão sendo enviadas à sessão. | 
| isClosed| Valor booliano| True se a sessão é fechada e não mais jogadores podem ser adicionado e false caso contrário. Se esse valor for true, as solicitações para ingressar na sessão serão rejeitadas. | 
| maxPlayers| inteiro com sinal de 32 bits| Número máximo de players que pode ser na sessão simultaneamente. O intervalo de valores é 2-16. Depois que a sessão contém o número máximo de jogadores, ainda mais as solicitações para ingressar na sessão são rejeitadas. | 
| playersCanBeRemovedBy| PlayerAcl| Um valor que indica o jogador que tem permissão para remover outros participantes da sessão. Os valores possíveis são NoOne, Self e AnyPlayer. | 
| roster| matriz de objetos de player| Uma matriz de jogadores na sessão. A lista contém dos jogadores atuais e jogadores que estavam anteriormente na sessão, mas ainda tem. A ordem dos jogadores na lista o curso nunca muda. Novos jogadores são adicionados ao final da matriz. | 
| seatsAvailable| inteiro com sinal de 32 bits| O número de jogadores que ainda podem associar a sessão antes de atingir o número máximo de jogadores. Esse valor é somente leitura e é sempre menor que o valor do campo maxPlayers. | 
| sessionId| cadeia de caracteres| A ID de sessão atribuída pelo MPSD quando a sessão é criada. Esse valor geralmente é incluído no URI ao acessar as informações armazenadas em uma sessão.| 
| titleId| inteiro sem sinal de 32 bits| A ID do título do qual criar a sessão de jogo.| 
| Variant| inteiro com sinal de 32 bits| A variante do jogo. Esse valor é opaco para o servidor.| 
| visibilidade| VisibilityLevel| Um valor que indica a visibilidade de sessão. Os valores possíveis são: PlayersCurrentlyInSession, PlayersEverInSession e todas as pessoas.| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "maxPlayers": 4,
    "seatsAvailable": 4,
    "isClosed": false,
    "hasEnded": false,
    "roster": [],
    "visibility": "PlayersCurrentlyInSession",
    "playersCanBeRemovedBy": "Self",
 }
    
```

  
<a id="ID4ENF"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EPF"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>Referência 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [Player (JSON)](json-player.md)

   