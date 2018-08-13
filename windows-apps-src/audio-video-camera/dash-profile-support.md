---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: Este artigo lista os perfis Dynamic Adaptive Streaming over HTTP (DASH) compatíveis com os apps UWP.
title: Suporte do perfil Dynamic Adaptive Streaming over HTTP (DASH)
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c411d479430f793d85863f66c64758155b8a5758
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842333"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Suporte do perfil Dynamic Adaptive Streaming over HTTP (DASH)


## <a name="supported-dash-profiles"></a>Perfis DASH com suporte
A tabela a seguir lista os perfis DASH que oferecem suporte aos apps UWP.

|Marca | Tipo de manifesto | Observações|Versão de julho do Windows 10|Windows 10, versão 1511|Windows 10, versão 1607 |Windows 10, versão 1607 |Windows 10, versão 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Estático |     |Com suporte            |  Com suporte              | Com suporte        |Com suporte| Com suporte|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | Melhor esforço | Com suporte            |  Com suporte              | Com suporte        |Com suporte| Com suporte|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Dinâmico | $Time$ é compatível, mas $Number$ não é compatível aos modelos de segmento | Sem suporte            | Sem suporte              | Sem suporte        |Sem suporte| Com suporte|


## <a name="unsupported-dash-profiles"></a>Perfis DASH sem suporte
Os perfis não listados na tabela acima não têm suporte, incluindo, mas não limitado ao seguinte:

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:isoff-on-demand:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>Tópicos relacionados

* [Reprodução de mídia](media-playback.md)
* [Streaming adaptável](adaptive-streaming.md)
 

 



