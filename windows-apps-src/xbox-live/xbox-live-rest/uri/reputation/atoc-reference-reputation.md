---
title: URIs de reputação
assetID: d1cb76c0-86a4-8c51-19f6-5f223b517d46
permalink: en-us/docs/xboxlive/rest/atoc-reference-reputation.html
description: " URIs de reputação"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93d6d6e6acfd8fa39bd9d26c87ed99362d2c88d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614001"
---
# <a name="reputation-uris"></a>URIs de reputação
 
Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) e os métodos associados do protocolo de transporte de hipertexto (HTTP) de serviços do Xbox Live para o **Microsoft.Xbox.Services.Social.ReputationService** . O domínio para a reputação URIs é reputation.xboxlive.com. Uma representação de URI típica pode ser https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback. 
 
O serviço de reputação usa os comentários, conforme descrito em [comentários (JSON)](../../json/json-feedback.md), para calcular uma pontuação de reputação. Essa pontuação é salvo na área de estatísticas para o usuário sob a chave ReputationOverall. Para obter mais informações sobre como recuperar as estatísticas do usuário, consulte [obter (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md). 
 
Jogos em todas as plataformas podem usar o serviço de reputação.
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;Usado em seu título caso você deseje adicionar uma opção comentários no jogo, em vez de usar o shell.

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;Usado pelo serviço do seu título para enviar comentários na forma de lote fora da interface do seu título.

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;Permite que a equipe de imposição acessar as pontuações de reputação do usuário atual.

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;Redefine completamente os dados de reputação de um usuário de teste. Somente para teste.

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;Permite que a equipe de imposição acessar as pontuações de reputação do usuário especificado.
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   