---
title: Visão geral do tipo de dados
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
description: " Visão geral do tipo de dados"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 62932a921d51a988a5533d7ee08f4968bb67a29d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659651"
---
# <a name="data-type-overview"></a>Visão geral do tipo de dados
 
Serviços do Xbox Live usa uma variedade de tipos de dados relacionadas à identidade e autenticação. Este tópico fornece uma visão geral desses tipos.
 
| Tipo| Descrição| 
| --- | --- | 
| gamertag| Um nome de exibição exclusivo, legível por humanos para o usuário.| 
| Player| Um objeto JSON que contém o usuário XUID e o gamertag, além de índice do jogador na sessão (ou "o assento"), se o jogador ainda está participando da sessão e um blob pequeno de dados personalizados.| 
| perfil| Informações sobre o usuário acessado por meio de endereços de URI de perfil e métodos HTTP, geralmente o UserSettings do usuário, mas também, possivelmente, incluindo o jogador, gamertag, XUID e assim por diante.| 
| Configuração| Uma das configurações específicas do título em um objeto UserSettings.| 
| UserClaims| Um objeto JSON simple contendo XUID e o gamertag do usuário.| 
| UserSettings| Um objeto JSON que contém uma coleção de configurações específicas de título ou preferências do usuário autenticado atual. UserSettings pode conter dados arbitrários, possivelmente relacionados à atividade do jogo.| 
| XUID| ID de usuário do usuário Xbox, um inteiro longo sem sinal de exclusivo. Não deve ser legível por humanos.| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>Referência [Player (JSON)](../json/json-player.md)

 [UserClaims (JSON)](../json/json-userclaims.md)

 [UserSettings (JSON)](../json/json-usersettings.md)

   