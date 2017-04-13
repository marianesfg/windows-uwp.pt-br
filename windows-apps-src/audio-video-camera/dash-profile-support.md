---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: "Este artigo lista os perfis Dynamic Adaptive Streaming over HTTP (DASH) compatíveis com os apps UWP."
title: Suporte do perfil Dynamic Adaptive Streaming over HTTP (DASH)
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9ca8f2d1783a73ae38fed1d3ee1e58b809ddd2aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Suporte do perfil Dynamic Adaptive Streaming over HTTP (DASH)
A tabela a seguir lista os perfis DASH que oferecem suporte aos apps UWP.



|Marca | Tipo de manifesto | Observações|Versão de julho do Windows 10|Windows 10, versão 1511|Windows 10, versão 1607 |Windows 10, versão 1607 |Windows 10, versão 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn: mpeg:dash:profile:isoff-live: 2011 | Estático |     |Com suporte            |  Com suporte              | Com suporte        |Com suporte| Com suporte|
|urn:mpeg:dash:profile:isoff-main:2011 |        | Melhor esforço | Com suporte            |  Com suporte              | Com suporte        |Com suporte| Com suporte|
|urn: mpeg:dash:profile:isoff-live: 2011 | Dinâmico | $Time$ é compatível, mas $Number$ não é compatível aos modelos de segmento | Sem suporte            | Sem suporte              | Sem suporte        |Sem suporte| Com suporte|


## <a name="related-topics"></a>Tópicos relacionados

* [Reprodução de mídia](media-playback.md)
* [Streaming adaptável](adaptive-streaming.md)
 

 




