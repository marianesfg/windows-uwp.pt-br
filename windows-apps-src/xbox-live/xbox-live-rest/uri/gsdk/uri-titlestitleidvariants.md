---
title: /titles/{titleId}/variants
assetID: bca30c8f-1f09-729f-4955-38b7809404eb
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants.html
description: " /titles/{titleId}/variants"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a0a0df14b442babec363dfebc96a0c33935563e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658541"
---
# <a name="titlestitleidvariants"></a>/titles/{titleId}/variants
Chamado por um cliente para obter as variantes disponíveis para um título URI. Os domínios para esses URIs são `gameserverds.xboxlive.com` e `gameserverms.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EU)
  * [Nome do host](#ID4EIB)
  * [Métodos válidos](#ID4EPB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>Parâmetros de URI
 
| Parâmetro| Descrição| 
| --- | --- | 
| TitleID| ID do título que a solicitação deve operar.| 
  
<a id="ID4EIB"></a>

 
## <a name="host-name"></a>Nome do host
 
gameserverds.xboxlive.com
  
<a id="ID4EPB"></a>

 
## <a name="valid-methods"></a>Métodos válidos
  
[POST](uri-titlestitleidvariants-post.md)
 
&nbsp;&nbsp;URI chamado por um cliente que recupera uma lista de variantes de jogos para o título especificado ID.
   