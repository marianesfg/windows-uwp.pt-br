---
title: URIs de presença
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
description: " URIs de presença"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a46ecd48c2b0bf523ab234a5f20cf9ed6669e75
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632601"
---
# <a name="presence-uris"></a>URIs de presença
 
Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) e os métodos de protocolo de transporte de hipertexto (HTTP) associados do Xbox Live Services pela *presença*.
 
Somente os jogos em execução em um Xbox 360, um dispositivo Windows Phone ou Windows podem usar esse serviço.
 
O domínio para esses URIs é userpresence.xboxlive.com.
 
Você pode assinar as alterações de presença de um usuário usando o serviço de atividade em tempo Real (RTA).
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;Presença de acesso para um lote de usuários.

[/users/me](uri-usersme.md)

&nbsp;&nbsp;Acesse a presença do usuário atual.

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;Acessa o PresenceRecord para o meu grupo.

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;Acesso a presença de outro usuário ou cliente.

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;Acesse a presença de um título ou usuário do título.

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;Acessa o PresenceRecord para um grupo.

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;Acessa o registro de presença dos usuários difusão especificados pelo moniker grupos relacionados ao XUID que aparece no URI.

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;Acessos a contagem de usuários difusão especificados pelo moniker grupos relacionados ao XUID que aparece no URI.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   