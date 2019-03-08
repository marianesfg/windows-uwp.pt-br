---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 85a6470a64ceef3b154384d1ca859fb28733aad3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632571"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
Acessa um placar de líderes social (classificação).
O domínio para esses URIs é `leaderboards.xboxlive.com`.

  * [Parâmetros de URI](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid| cadeia de caracteres| Identificador do usuário.|
| scid| cadeia de caracteres| Identificador da configuração do serviço que contém o recurso que está sendo acessado.|
| statname| cadeia de caracteres| Identificador exclusivo do recurso stat usuário que está sendo acessado.|
| todos os\|favorito| enumeração| Se deve classificar o stat valores (pontuações) para todos os contatos de conhecidos do usuário atual ou somente os contatos designados como favoritas pessoas que o usuário.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;Retorna um placar de líderes de redes sociais, classificação que de stat valores (pontuações) para qualquer um dos conhecidos e todos os contatos do usuário atual ou somente os contatos designados como favoritas pessoas que o usuário.

<a id="ID4EYC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent

[URIs de placares de líderes](atoc-reference-leaderboard.md)
