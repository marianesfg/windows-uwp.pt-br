---
ms.assetid: 66a9cfe2-b212-4c73-8a36-963c33270099
description: Este artigo lista as marcas de protocolo HLS (HTTP Live Streaming) com suporte para aplicativos UWP.
title: Suporte para marcas HLS (HTTP Live Streaming)
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c6664d13e76a5774172094d632de9db25109fdc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613081"
---
# <a name="http-live-streaming-hls-tag-support"></a>Suporte para marcas HLS (HTTP Live Streaming)
A tabela a seguir lista as marcas HLS que têm suporte para aplicativos UWP.

> [!NOTE] 
> As marcas personalizadas que começam com "X-" podem ser acessadas como metadados programados conforme descrito no artigo [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

|Tag |Introduzida na versão do protocolo HLS|Versão de rascunho do documento do protocolo HLS|Obrigatória no cliente|Versão de julho do Windows 10|Windows 10, versão 1511|Windows 10, versão 1607 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  Marcas básicas                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte|
| 4.3.1.2.  EXT-X-VERSION |2|3|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte
|4.3.2.  Marcas do segmento de mídia                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|OPCIONAL|Com suporte|Com suporte|Com suporte|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|OPCIONAL|Com suporte|Com suporte|Com suporte|
| 4.3.2.4.  EXT-X-KEY |1|0|OPCIONAL|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp; MÉTODO|1|0|Atributo|"NONE, AES-128"|"NONE, AES-128"|"NONE, AES-128, SAMPLE-AES"|
|&nbsp;&nbsp;&nbsp; URI|1|0|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp; IV|2|3|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
| 4.3.2.5.  EXT-X-MAP |5|9|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp; URI|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp; SOBRE|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
|4.3.3.  Marcas de playlist de mídia                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|OPCIONAL|Com suporte|Com suporte|Com suporte|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|OPCIONAL|Com suporte|Com suporte|Com suporte|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|OPCIONAL|Com suporte|Com suporte|Com suporte|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
|4.3.4.  Marcas de playlist mestre                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|OPCIONAL|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  TIPO|4|7|Atributo|"AUDIO, VIDEO"|"AUDIO, VIDEO"|"AUDIO, VIDEO, SUBTITLES"|
|&nbsp;&nbsp;&nbsp;  URI|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  ID DO GRUPO|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  IDIOMA|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  NOME|4|7|Atributo|Sem suporte|Sem suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  PADRÃO|4|7|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  SELEÇÃO AUTOMÁTICA|4|7|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  FORÇADO|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  ID DO FLUXO|6|12|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  CARACTERÍSTICAS|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  LARGURA DE BANDA|1|0|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  ID DO PROGRAMA|1|0|Atributo|N/D|N/D|N/D|
|&nbsp;&nbsp;&nbsp;  LARGURA DE BANDA MÉDIA|7|14|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  RESOLUÇÃO|2|3|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  TAXA DE QUADROS|7|15|Atributo|N/D|N/D|N/D|
|&nbsp;&nbsp;&nbsp;  ÁUDIO|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  VÍDEO|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  LEGENDAS|5|9|Atributo|Sem suporte|Sem suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  LEGENDAS FECHADAS|6|12|Atributo|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
|4.3.5.  Marcas de mídia ou playlist mestre                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|OPCIONAL|Sem suporte|Com suporte|Com suporte|
| 4.3.5.2.  EXT-X-START  |6|12|OPCIONAL|Sem suporte|Com suporte parcial|Com suporte parcial|
|&nbsp;&nbsp;&nbsp;  DIFERENÇA DE HORÁRIO|6|12|Atributo|Sem suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  PRECISO|6|12|Atributo|Sem suporte|"SEM" suporte padrão|"SEM" suporte padrão|



## <a name="related-topics"></a>Tópicos relacionados

* [Reprodução de mídia](media-playback.md)
* [Streaming adaptável](adaptive-streaming.md)
 

 




