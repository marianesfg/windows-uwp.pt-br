---
title: /titles/{titleId}/sessions/{sessionId}/allocationStatus
assetID: 55611f4b-4ba4-fa9a-ce44-fcc4a6df1b35
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus.html
description: " /titles/{titleId}/sessions/{sessionId}/allocationStatus"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aa66b81d1e363488a6969ab31d7091b695f09ae4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603301"
---
# <a name="titlestitleidsessionssessionidallocationstatus"></a>/titles/{titleId}/sessions/{sessionId}/allocationStatus
Para a id de determinado título e a id de sessão, obter o status da solicitação de tíquete. Os domínios para esses URIs são `gameserverds.xboxlive.com` e `gameserverms.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EU)
  * [Nome do host](#ID4EPB)
  * [Métodos válidos](#ID4EWB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>Parâmetros de URI
 
| Parâmetro| Descrição| 
| --- | --- | 
| titleId| ID do título que a solicitação deve operar.| 
| sessionId| a ID da sessão para pesquisar.| 
  
<a id="ID4EPB"></a>

 
## <a name="host-name"></a>Nome do host
 
gameserverms.xboxlive.com
  
<a id="ID4EWB"></a>

 
## <a name="valid-methods"></a>Métodos válidos
  
[GET](uri-titlestitleidsessionssessionidallocationstatus-get.md)
 
&nbsp;&nbsp;Retorna o status de alocação do sessionhost identificado por sua ID de sessão.
   