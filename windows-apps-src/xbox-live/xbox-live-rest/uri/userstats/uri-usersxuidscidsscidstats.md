---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53a6c7bb0e7390b024b01e221d8061316a80509e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650411"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
Acessa uma configuração de serviço com escopo de uma lista delimitada por vírgulas de nomes de estatística de usuário em nome do usuário especificado. O domínio para esses URIs é `userstats.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| GUID| Xbox usuário ID (XUID) do usuário em cujo nome para acessar a configuração de serviço.| 
| scid| GUID| Identificador da configuração do serviço que contém o recurso que está sendo acessado.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;Obtém uma configuração de serviço com escopo de uma lista delimitada por vírgulas de nomes de estatística de usuário em nome do usuário especificado.

[OBTER com valor de metadados](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;Obtém uma lista de estatística especificado, incluindo os metadados associados com os valores de estatística, para um usuário em uma configuração de serviço especificado.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de estatísticas do usuário](atoc-reference-userstats.md)

   