---
author: drewbatgit
ms.assetid: 
description: Este artigo lista as marcas de protocolo HLS (HTTP Live Streaming) com suporte para aplicativos UWP.
title: Suporte para marcas HLS (HTTP Live Streaming)
translationtype: Human Translation
ms.sourcegitcommit: 3d61f5272e4d11acfb7e0a85436ca60ba458dcae
ms.openlocfilehash: a561f11a1638d5fea21d1d3b3f8bc47f71271f3f

---

# Suporte para marcas HLS (HTTP Live Streaming)
A tabela a seguir lista as marcas HLS que têm suporte para aplicativos UWP.

> [!NOTE] 
> As marcas personalizadas que começam com "X-" podem ser acessadas como metadados programados conforme descrito no artigo [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

|Marca |Introduzida na versão do protocolo HLS|Versão de rascunho do documento do protocolo HLS|Obrigatória no cliente|Versão de julho do Windows 10|Windows 10, versão 1511|Windows 10, versão 1606 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  Marcas básicas                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte|
| 4.3.1.2.  EXT-X-VERSION |2|3|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte
|4.3.2.  Marcas do segmento de mídia                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|OPCIONAL|Com suporte|Com suporte|Com suporte|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|OPCIONAL|Com suporte|Com suporte|Com suporte|
| 4.3.2.4.  EXT-X-KEY |1|0|OPCIONAL|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp; METHOD|1|0|Atributo|"NONE, AES-128"|"NONE, AES-128"|"NONE, AES-128, SAMPLE-AES"|
|&nbsp;&nbsp;&nbsp; URI|1|0|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp; IV|2|3|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
| 4.3.2.5.  EXT-X-MAP |5|9|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp; URI|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
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
|&nbsp;&nbsp;&nbsp;  TYPE|4|7|Atributo|"AUDIO, VIDEO"|"AUDIO, VIDEO"|"AUDIO, VIDEO, SUBTITLES"|
|&nbsp;&nbsp;&nbsp;  URI|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  GROUP-ID|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  LANGUAGE|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  NAME|4|7|Atributo|Sem suporte|Sem suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  DEFAULT|4|7|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  AUTOSELECT|4|7|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  FORCED|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  CHARACTERISTICS|5|9|Atributo|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|OBRIGATÓRIA|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  BANDWIDTH|1|0|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  PROGRAM-ID|1|0|Atributo|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AVERAGE-BANDWIDTH|7|14|Atributo|Sem suporte|Sem suporte|Sem suporte|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  RESOLUTION|2|3|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  FRAME-RATE|7|15|Atributo|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AUDIO|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  VIDEO|4|7|Atributo|Com suporte|Com suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  SUBTITLES|5|9|Atributo|Sem suporte|Sem suporte|Com suporte|
|&nbsp;&nbsp;&nbsp;  CLOSED-CAPTIONS|6|12|Atributo|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|OPCIONAL|Sem suporte|Sem suporte|Sem suporte|




## Tópicos relacionados

* [Reprodução de mídia](media-playback.md)
* [Streaming adaptável](adaptive-streaming.md)
 

 







<!--HONumber=Nov16_HO1-->


