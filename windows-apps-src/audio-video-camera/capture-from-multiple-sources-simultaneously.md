---
ms.assetid: ''
description: Este artigo mostra como capturar vídeo de várias fontes simultaneamente em um único arquivo com várias faixas de vídeo incorporadas.
title: Capturar de várias fontes usando o MediaFrameSourceGroup
ms.date: 09/12/2017
ms.topic: article
keywords: windows 10, uwp, captura, vídeo
ms.localizationpriority: medium
ms.openlocfilehash: a654739490043b9f821e7906fa8cf9e3e7259fed
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7706416"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Capturar de várias fontes usando o MediaFrameSourceGroup

Este artigo mostra como capturar vídeo de várias fontes simultaneamente em um único arquivo com várias faixas de vídeo incorporadas. A partir da RS3, você pode especificar vários objetos **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** para um único **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**. Isso permite que você codifique vários fluxos simultaneamente em um único arquivo. Os fluxos de vídeo codificados nesta operação devem ser incluídos em um único **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)**, que especifica um conjunto de câmeras no dispositivo atual que pode ser usado ao mesmo tempo. 

Para obter informações sobre como usar o **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** com a classe **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** para habilitar cenários de computador em tempo real que usam várias câmeras, consulte [Processar quadros de mídia com o MediaFrameReader](process-media-frames-with-mediaframereader.md).

O restante deste artigo conduzirá você pelas etapas de gravação de vídeo com duas câmeras coloridas em um único arquivo com várias faixas de vídeo.

## <a name="find-available-sensor-groups"></a>Localizar grupos de sensores disponíveis
O **MediaFrameSourceGroup** é uma coleção de fontes de quadro, normalmente câmeras, que podem ser acessadas simultaneamente. O conjunto de grupos de fontes de quadro disponíveis é diferente para cada dispositivo; portanto, a primeira etapa neste exemplo é obter a lista de grupos de fontes de quadro disponíveis e localizar uma que contenha as câmeras necessárias para o cenário, que, neste caso, requer duas câmeras coloridas.

O método **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** retorna todos os grupos de fontes disponíveis do dispositivo atual. Cada **MediaFrameSourceGroup** retornado tem uma lista de objetos **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** que descreve cada fonte de quadro do grupo. Uma consulta Linq é usada para localizar um grupo de fontes que contenha duas câmeras coloridas, uma no painel frontal e outra na parte posterior. É retornado um objeto anônimo contendo o **MediaFrameSourceGroup** selecionado e o **MediaFrameSourceInfo** para cada câmera colorida. Em vez de usar a sintaxe Linq, você pode percorrer cada grupo e, em seguida, cada **MediaFrameSourceInfo** para procurar um grupo que atenda às suas necessidades.

Observe que nem todo dispositivo conterá um grupo de fontes com duas câmeras coloridas; portanto, você deve certificar-se de que um grupo de fontes foi encontrado antes de tentar capturar o vídeo.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>Inicializar o objeto MediaCapture
**[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** é a principal classe usada na maioria das operações de captura de áudio, vídeo e fotos nos aplicativos UWP. Inicialize o objeto chamando **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.InitializeAsync)**, passando um objeto **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** que contenha parâmetros de inicialização. Neste exemplo, a única configuração especificada é a propriedade **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)**, que é definida como o **MediaFrameSourceGroup** recuperado no exemplo de código anterior.

Para obter informações sobre outras operações que você pode realizar com **MediaCapture** e outros recursos do aplicativo UWP para captura de mídia, consulte [Câmera](camera.md).

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>Criar um MediaEncodingProfile
A classe **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** informa ao pipeline de captura de mídia como o áudio e o vídeo capturados devem ser codificados à medida que são gravados em um arquivo. Nos cenários comuns de transcodificação e captura, essa classe fornece um conjunto de métodos estáticos para criar perfis comuns, como **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** e **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**. Neste exemplo, um perfil de codificação é criado manualmente usando um contêiner Mpeg4 e a codificação de vídeo H264. As configurações de codificação de vídeo são especificadas por meio de um objeto **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)**. Para cada câmera colorida usada neste cenário, um objeto **VideoStreamDescriptor** é configurado. O descritor é construído com o objeto **VideoEncodingProperties** que especifica a codificação. A propriedade **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor.Label)** do **VideoStreamDescriptor** deve ser definida como a ID da fonte de quadros de mídia que será capturada no fluxo. É assim que o pipeline de captura sabe qual descritor de fluxo e propriedades de codificação devem ser usadas em cada câmera. A ID da fonte de quadros é exposta pelos objetos **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** localizados na seção anterior, quando um **MediaFrameSourceGroup** foi selecionado.


A partir do Windows 10, versão 1709, você pode definir várias propriedades de codificação em um **MediaEncodingProfile** chamando **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)**. Você pode recuperar a lista de descritores de fluxo de vídeo chamando **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)**. Observe que, se você definir a propriedade **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)**, que armazena um descritor de fluxo único, a lista de descritores que você definiu chamando **SetVideoTracks** será substituída por uma lista que contém o descritor único especificado.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

### <a name="encode-timed-metadata-in-media-files"></a>Codificar metadados com tempo nos arquivos de mídia

A partir do Windows 10, versão 1803, além de áudio e vídeo, você pode codificar metadados com tempo em um arquivo de mídia compatível com o formato de dados. Por exemplo, os metadados da GoPro (gpmd) podem ser armazenados em arquivos MP4 para transmitir a localização geográfica correlacionada com um streaming de vídeo. 

A codificação de metadados usa um padrão paralelo à codificação de áudio ou vídeo. A classe [**TimedMetadataEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) descreve o tipo, o subtipo e as propriedades de codificação dos metadados, como **VideoEncodingProperties** faz para vídeo. O [**TimedMetadataStreamDescriptor**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) identifica um streaming de metadados, assim como o **VideoStreamDescriptor** faz para streaming de vídeo.  

O exemplo a seguir mostra como inicializar um objeto **TimedMetadataStreamDescriptor**. Primeiro, um objeto **TimedMetadataEncodingProperties** é criado e o **Subtipo** é definido em um GUID que identifica o tipo de metadados que será incluído no fluxo. Esse exemplo usa a GUID de metadados da GoPro (gpmd). O método [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) é chamado para definir dados específicos do formato. Para arquivos MP4, os dados específicos de formato são armazenados na caixa SampleDescription (stsd). Em seguida, um novo **TimedMetadataStreamDescriptor** é criado a partir das propriedades de codificação. As propriedades **Label** e **Name** são definidas para identificar o fluxo para codificação. 

[!code-cs[GetStreamDescriptor](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetStreamDescriptor)]

Chame [MediaEncodingProfile.SetTimedMetadataTracks](**https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks**) para adicionar o descritor de streaming de metadados ao perfil de codificação. O exemplo a seguir mostra um método auxiliar que usa dois descritores de streaming de vídeo, um descritor de streaming de áudio e descritor de streaming de metadados e retorna um **MediaEncodingProfile** que pode ser usado para codificar os fluxos.

[!code-cs[GetMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Gravar usando o MediaEncodingProfile de vários fluxos
A etapa final deste exemplo é iniciar a captura de vídeo chamando **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)**, passando o **StorageFile** no qual a mídia capturada é gravada e o **MediaEncodingProfile** criado no exemplo de código anterior. Após alguns segundos, a gravação é interrompida com uma chamada para **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)**.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

Quando a operação for concluída, um arquivo de vídeo terá sido criado, contendo o vídeo capturado de cada câmera codificada como um fluxo separado no arquivo. Para obter informações sobre a reprodução de arquivos de mídia que contém várias faixas de vídeo, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Processar quadros de mídia com o MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md)


 

 




