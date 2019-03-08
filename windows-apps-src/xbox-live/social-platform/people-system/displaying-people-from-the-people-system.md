---
title: Pessoas a exibição do sistema de pessoas
description: Saiba sobr o fluxo de código para exibir as pessoas usando o sistema de pessoas do Xbox Live.
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 512de51f2a0e30a9b41a5e49f3dc3ababe30fc4d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658301"
---
# <a name="display-people-from-the-people-system"></a>Pessoas a exibição do sistema de pessoas

Serviços entre Xbox Live retornam apenas os dados pertencentes a esse serviço e retornam somente XUID referências para os usuários; Por exemplo, o serviço pessoas só possui e retorna os XUIDs que estão na lista de pessoas do usuário e algumas informações básicas sobre cada um desses XUIDs (como o status de Favoritos). O serviço de presença possui dados sobre as informações de status online de XUIDs. O serviço de placares de líderes possui informações de classificação em listas de XUIDs. Exibir informações de nome e o nome de jogador nunca são retornadas de qualquer serviço que não seja o serviço de perfil e, portanto, é necessário para renderizar listas de pessoas em experiências de chamar vários serviços.

O padrão geral de chamada para o serviço é de APIs fazer uma viagem de arredondamento para primeiro obter uma lista de XUIDs do serviço que melhor pode filtrar ou classificar a lista e, em seguida, fazer chamadas simultâneas de ida e volta a outros serviços necessários para obter todos os metadados adicionais necessárias para cada XIUD. No caso de imagens, uma terceiro ida e volta de chamadas deverão obter imagens de URLs de imagem.

Para reduzir o número de viagens de ida e necessárias para obter dados sobre a lista de pessoas de um usuário, uma pessoas *moniker* está sendo introduzido para serviços relevantes. Esse novo recurso permite que os chamadores agrupa abstratamente express para o serviço primário, o serviço deve obter a lista de pessoas do usuário do serviço de pessoas e, em seguida, usar esse conjunto de XUIDs para definir o escopo de retorno.

A seguir estão alguns cenários de fluxo de chamada de exemplo que ilustram como títulos de obtêm dados de serviços relacionados às pessoas:

-   Lista de usuários atualmente no jogo
-   Lista de pessoas do usuário atual que estão online
-   Placar de líderes global que contém usuários aleatórios
-   Placar de líderes de pessoas do usuário


## <a name="list-of-users-currently-in-game"></a>Lista de usuários atualmente no jogo

| Tem título  | Meta  | Campo para renderizar  | Fluxo de chamadas
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| Lista de XUIDs aleatórios de outros usuários no jogo | Para processar as informações mínimas para cada um dos outros usuários | GameDisplayName \[perfil\] | Chame o perfil com a lista de XUIDs. |


## <a name="list-of-the-current-users-people-who-are-online"></a>Lista de pessoas do usuário atual que estão online

## <a name="title-has"></a>Título tem:
XUID do usuário atual

## <a name="goal"></a>Meta
Para renderizar a lista avançada de usuários online que estão na lista de pessoas do usuário atual

## <a name="field-to-render-owning-service"></a>Campo para renderizar \[proprietário do serviço\]
* Indicador de favoritos [pessoas]
* Imagem de exibição [Profile]
* GameDisplayName [Profile]
* Status online básico (bola verde) [presença]

## <a name="call-flow"></a>Fluxo de chamadas
1. Chame a presença, passando o moniker de pessoas para obter o status online e XUIDs para cada uma das pessoas do usuário.
1. Em paralelo:
 1. Chame o perfil, passando a lista inteira de XUIDs para obter o nome de exibição e a URL de imagem para cada um.
 1. Chame as pessoas, passando na lista de XUIDs para descobrir se houver algum Favoritos do usuário.
1. Depois de chamar o perfil:
 1. Obter imagens para cada URL de imagem

## <a name="global-leaderboard-containing-random-users"></a>Placar de líderes global que contém usuários aleatórios

## <a name="title-has"></a>Título tem:
O nome/ID de placar de líderes

## <a name="goal"></a>Meta
Para renderizar informações básicas para cada usuário no placar de líderes

## <a name="field-to-render-owning-service"></a>Campo para renderizar [serviço proprietário]
* Indicador de favoritos [pessoas]
* GameDisplayName [Profile]
* Classificação [placares de líderes]
* Pontuação [placares de líderes]

## <a name="call-flow"></a>Fluxo de chamadas
1. Placar de líderes de chamada para obter os XUIDs, classificação e as pontuações para o placar de líderes específico.
1. Em paralelo:
 1. Chame o perfil, passando na lista de XUIDs para obter o nome de exibição e a URL de imagem para cada um.
 1. Chame as pessoas, passando na lista de XUIDs para descobrir se houver algum Favoritos do usuário.

## <a name="leaderboard-of-users-people"></a>Placar de líderes de pessoas do usuário

## <a name="title-has"></a>Título tem:
* O nome/ID de placar de líderes
* XUID do usuário atual

## <a name="goal"></a>Meta
Para renderizar informações básicas para cada usuário no placar de líderes

## <a name="field-to-render-owning-service"></a>Campo para renderizar [serviço proprietário]
* Indicador de favoritos [pessoas]
* GameDisplayName [Profile]
* Classificação [placares de líderes]
* Pontuação [placares de líderes]

## <a name="call-flow"></a>Fluxo de chamadas
1. Chame placares de líderes, passando o moniker de pessoas para obter os XUIDs, classificação e as pontuações para o placar de líderes específica, limitada à lista de pessoas do usuário.
1. Em paralelo:
 1. Chame o perfil, passando na lista de XUIDs para obter o nome de exibição e a URL de imagem para cada um.
 1. Chame as pessoas, passando na lista de XUIDs para descobrir se houver algum Favoritos do usuário.
