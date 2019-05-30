---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: Este artigo mostra como usar um MediaFrameReader com o MediaCapture para obter AudioFrames contendo dados de áudio de uma fonte de captura.
title: Processar quadros de áudio com o MediaFrameReader
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 60abc29ad4f9e16dc9d37e99f94c9f30039c0087
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360697"
---
# <a name="process-audio-frames-with-mediaframereader"></a>Processar quadros de áudio com o MediaFrameReader

Este artigo mostra como usar um [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) com [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) para obter dados de áudio de uma fonte de quadro de mídia. Para saber como usar um **MediaFrameReader** para obter dados de imagem, como cor, infravermelho ou profundidade da câmera, consulte [Processar quadros de mídia com o MediaFrameReader](process-media-frames-with-mediaframereader.md). Esse artigo fornece uma visão geral do padrão de uso do leitor de quadros e descreve alguns recursos adicionais da classe **MediaFrameReader**, como o uso de **MediaFrameSourceGroup** para recuperar os quadros de várias fontes ao mesmo tempo. 

> [!NOTE] 
> Os recursos abordados neste artigo só estão disponíveis a partir do Windows 10, versão 1803.

> [!NOTE] 
> Há um exemplo de aplicativo Universal do Windows que demonstra o uso do **MediaFrameReader** para exibir quadros de origens diferentes, incluindo câmeras em cores, de profundidade e infravermelho. Para obter mais informações, consulte [Exemplo de quadros de câmera](https://go.microsoft.com/fwlink/?LinkId=823230).

## <a name="setting-up-your-project"></a>Configurando seu projeto
O processo de aquisição dos quadros de áudio é basicamente o mesmo para outros tipos de quadros de mídia. Como com qualquer aplicativo que usa **MediaCapture**, você deve declarar que seu aplicativo usa a funcionalidade de *webcam* antes de tentar acessar qualquer dispositivo de câmera. Se seu aplicativo fizer a captura de um dispositivo de áudio, você deve declarar também a funcionalidade do dispositivo *microfone*. 

**Adicionar recursos ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.
4.  Para acessar as bibliotecas Imagens e Vídeos, marque as caixas para **Biblioteca de Imagens** e a caixa para **Biblioteca de Vídeos**.



## <a name="select-frame-sources-and-frame-source-groups"></a>Selecionar origens de quadro e grupos de origens de quadro

A primeira etapa na captura de quadros de áudio é inicializar um [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource), que representa a origem dos dados de áudio, como um microfone ou outro dispositivo de captura de áudio. Para fazer isso, você deve criar uma nova instância do objeto [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture). Nesse exemplo, a única configuração de inicialização do **MediaCapture** é [**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) para indicar que queremos fazer o streaming de áudio do dispositivo de captura. 

Depois de chamar [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync), você pode obter a lista de fontes de quadros de mídia acessíveis com a propriedade [**FrameSources**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.framesources). Esse exemplo usa uma consulta Linq para selecionar todas as fontes de quadros em que [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo) descrevendo a fonte de quadros tem um  [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) de **Áudio**, indicando que a fonte de mídia produz dados de áudio.

Se a consulta retorna uma ou mais fontes de quadros, você pode verificar a propriedade [**CurrentFormat**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) para ver se a fonte é compatível com o formato de áudio desejado. Nesse exemplo, os dados de áudio flutuantes. Verifique [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) para garantir que a codificação de áudio desejada seja compatível com a fonte.

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>Criar e iniciar o MediaFrameReader

Obtenha uma nova instância de **MediaFrameReader** ao chamar [**MediaCapture.CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_), passando o objeto **MediaFrameSource** selecionado na etapa anterior. Por padrão, os quadros de áudio são obtidos no modo de buffer, tornando menos provável o descarte dos quadros, embora isso ainda possa ocorrer se você não estiver processando quadros áudio rápidos o suficiente e preencher o buffer de memória alocado do sistema.

Registre um manipulador para o evento [**MediaFrameReader.FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived), que é acionado pelo sistema quando um novo quadro de dados de áudio está disponível. Chame [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) para começar a aquisição de quadro de áudio. Se o leitor de quadro não for iniciado, o valor do status retornado da chamada terá um valor diferente de [**Success**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus).

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

Na manipulador de evento **FrameArrived**, chame [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) no objeto **MediaFrameReader** passado como remetente para o manipulador tentar recuperar uma referência ao quadro de mídia mais recente. Observe que esse objeto pode ser nulo, portanto, você sempre deve verificar antes de usar o objeto. O tipo de quadro de mídia encapsulado no **MediaFrameReference** retornado de **TryAcquireLatestFrame** depende do tipo de fonte ou fontes de quadro que você configurou o leitor de quadro para obter. Como o leitor de quadro do exemplo foi configurado para obter quadros de áudio, ele obtém o quadro subjacente usando a propriedade [**AudioMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe). 

Esse método auxiliar **ProcessAudioFrame** no exemplo abaixo mostra como obter um [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) que fornece informações, como o carimbo de data e hora do quadro e se é descontínuo em relação ao objeto **AudioMediaFrame**. Para ler ou processar os dados de amostra de áudio, você precisa obter o objeto [**AudioBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer) do objeto **AudioMediaFrame**, criar um [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference) e chamar o método COM **IMemoryBufferByteAccess::GetBuffer** para recuperar os dados. Veja a nota abaixo da lista de código para obter mais informações sobre como acessar buffers nativos.

O formato dos dados depende da fonte de quadros. Neste exemplo, ao selecionar uma fonte de quadros de mídia, nos certificamos que a origem do quadro selecionado usou um único canal de dados de ponto flutuante. O restante do código de exemplo mostra como determinar a duração e a contagem da amostra para os dados de áudio no quadro.  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> Para operar os dados de áudio, você deve acessar um buffer de memória nativa. Para fazer isso, você deve usar a interface COM **IMemoryBufferByteAccess** ao incluir a lista de código abaixo. As operações no buffer nativo devem ser realizadas em um método que usa a palavra-chave **inseguro**. Você também precisa marcar a caixa para permitir código não seguro na guia **Compilar** da caixa de diálogo **Projeto-> Propriedades**.

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>Informações adicionais sobre como usar o MediaFrameReader com dados de áudio

Você pode recuperar o [**AudioDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.AudioDeviceController) associado à fonte do quadro de áudio ao acessar a propriedade [**MediaFrameSource.Controller**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.controller). Esse objeto pode ser usado para obter ou definir as propriedades de streaming do dispositivo de captura ou para controlar o nível de captura. O exemplo a seguir ativa o mudo no dispositivo de áudio  para que os quadros continuem a ser adquiridos pelo leitor de quadro, mas todas as amostras têm o valor 0.

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

Você pode usar um objeto [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) a fim de passar dados de áudio captador por uma fonte de quadro de mídia em um [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph). Passe o quadro para o método [**AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) de um [**AudioFrameInputNode**](https://docs.microsoft.com/en-us/uwp/api/windows.media.audio.audioframeinputnode). Para obter mais informações sobre como usar os gráficos de áudio para capturar, processar e misturar sinais de áudio, consulte [Gráficos de áudio](audio-graphs.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Quadros de processos de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Câmera](camera.md)
* [Básica de fotos, vídeo e áudio capturar com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Exemplo de quadros de câmera](https://go.microsoft.com/fwlink/?LinkId=823230)
* [Gráficos de áudio](audio-graphs.md)
 






