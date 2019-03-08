---
title: InitialUploadRequest (JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
description: " InitialUploadRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2fbb2417f743d97b487ad16abd241fcb5eea62bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646631"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest (JSON)
O corpo de um clipe de jogo de POSTAGEM carregue a solicitação. 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
O objeto InitialUploadRequest tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| cadeia de caracteres| A ID de cadeia de caracteres para o texto a ser usado como o nome para o clipe. Isso é gerenciado e localizado no arquivo de configuração para o título pelo desenvolvedor do título.| 
| <b>userCaption</b>| cadeia de caracteres| Opcional. Nome alternativo inserido pelo usuário para o clipe de jogo até um comprimento máximo de 250 caracteres.| 
| <b>sessionRef</b>| cadeia de caracteres| Opcional. Referência de sessão de jogo durante o qual a gravação foi feita.| 
| <b>dateRecorded</b>| DateTime| A hora em que a gravação foi iniciada, em UTC. Marshaling como uma cadeia de caracteres no formato ISO 8601 (consulte <a href="https://www.w3.org/TR/NOTE-datetime">formatos de data e hora</a> para obter mais informações).| 
| <b>durationInSeconds</b>| inteiro sem sinal de 32 bits| O comprimento do clipe, em segundos.| 
| <b>expectedBlocks</b>| inteiro sem sinal de 32 bits| Opcional. Número de blocos em que o arquivo será dividido. Omita se o arquivo será transmitido em uma única solicitação.| 
| <b>fileSize</b>| inteiro sem sinal de 32 bits| Tamanho do arquivo em bytes do vídeo que será carregado.| 
| <b>type</b>| [Enumeração GameClipType](../enums/gvr-enum-gamecliptypes.md)| O tipo de clipe, empacotado como um valor de cadeia de caracteres de enumeração que é delimitada por vírgulas.| 
| <b>source</b>| [Enumeração GameClipSource](../enums/gvr-enum-gameclipsource.md)| Especifica como o clipe foi originado, empacotado como um valor de cadeia de caracteres da enumeração.| 
| <b>Visibilidade</b>| [Enumeração GameClipVisibility](../enums/gvr-enum-gameclipvisibility.md)| Especifica a visibilidade do clipe jogo quando ela for publicada no sistema.| 
| <b>titleData</b>| cadeia de caracteres| Opcional. Recipiente de propriedades para as propriedades de título específicas associadas a este clipe. Armazenados e retornados como-está. Os desenvolvedores de título podem usar este campo para manter seus próprios metadados sobre um clipe.| 
| <b>titleData</b>| cadeia de caracteres| Opcional. Conjunto de propriedades para propriedades de console específicas associadas a este clipe. Armazenados e retornados como-está. Console de plataforma pode usar este campo para manter seus próprios metadados sobre um clipe.| 
| <b>systemProperties</b>| cadeia de caracteres| Opcional. Conjunto de propriedades para propriedades de console específicas associadas a este clipe. Armazenados e retornados como está. Console de plataforma pode usar este campo para manter seus próprios metadados sobre um clipe.| 
| <b>usersInSession</b>| matriz de cadeia de caracteres| Opcional. Uma lista dos usuários na sessão atual.| 
| <b>thumbnailSource</b>| [Enumeração ThumbnailSource](../enums/gvr-enum-thumbnailsource.md)| Opcional. A origem da miniatura.| 
| <b>thumbnailOffsetMillseconds</b>| inteiro com sinal de 32 bits| Especifica o deslocamento (em milissegundos) para deslocamento miniaturas geradas. Somente especificado quando <b>thumbnailSource</b> é definido como o deslocamento.| 
| <b>savedByUser</b>| Valor booliano| Opcional. Define o clipe para ser salvo para a cota do usuário em vez de armazenamento de PEPS. Padrão é false.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "greatestMomentId": "123abc",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   