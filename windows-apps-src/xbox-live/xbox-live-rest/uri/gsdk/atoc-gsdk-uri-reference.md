---
title: Referência do servidor do jogo Universal Resource Identifier (URI)
assetID: bbd7e3f3-77ac-6ffd-8951-fe4b8b48eb4c
permalink: en-us/docs/xboxlive/rest/atoc-gsdk-uri-reference.html
description: " Referência do servidor do jogo Universal Resource Identifier (URI)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a9a0a38cff9214485b2d7e8b1f8a28acb3207444
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662591"
---
# <a name="game-server-universal-resource-identifier-uri-reference"></a>Referência do servidor do jogo Universal Resource Identifier (URI)
URIs usados pelos clientes para criar instâncias de servidor do Kit de desenvolvimento de servidor de jogo para um título. Os domínios para esses URIs são `gameserverds.xboxlive.com` e `gameserverms.xboxlive.com`.
 
<a id="ID4EY"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/qosservers](uri-qosservers.md)

&nbsp;&nbsp;Chamado por um cliente para obter a lista de servidores de QoS disponíveis para uso com o Xbox Live Compute de URI.

[/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

&nbsp;&nbsp;URI que permite que um cliente criar uma instância de servidor do Xbox Live Compute para um título.

[/titles/{titleId}/variants](uri-titlestitleidvariants.md)

&nbsp;&nbsp;Chamado por um cliente para obter as variantes disponíveis para um título URI.

[/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

&nbsp;&nbsp;Solicita um sessionhost Xbox Live Compute a ser alocado para uma id de título especificado.

[/titles/{titleId}/sessions/{sessionId}/allocationStatus](uri-titlestitleidsessionssessionidallocationstatus.md)

&nbsp;&nbsp;Para a id de determinado título e a id de sessão, obter o status da solicitação de tíquete.
 