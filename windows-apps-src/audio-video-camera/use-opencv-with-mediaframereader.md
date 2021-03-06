---
ms.assetid: ''
description: Este artigo mostra como usar a Biblioteca de visão de computador do código-fonte aberto (OpenCV) com a classe MediaFrameReader.
title: Usar OpenCV com MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, openCV
ms.localizationpriority: medium
ms.openlocfilehash: a6594898dff1bf5f2262034b10e262082335f1b2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255951"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Use a Biblioteca de visão do computador do código-fonte aberto (OpenCV) com MediaFrameReader

Este artigo mostra como usar a Biblioteca de visão do computador do código-fonte aberto (OpenCV), uma biblioteca de código nativo que fornece diversos algoritmos de processamento de imagem, com a classe [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) que pode ler diversos quadros de mídia de várias fontes simultaneamente. O código de exemplo neste artigo orienta você pelas etapas de criação de um aplicativo simples que obtém os quadros de um sensor de cor, desfoca cada quadro usando a biblioteca OpenCV e depois exibe a imagem processada em um controle de **imagem** XAML. 

>[!NOTE]
>OpenCV. win. Core e OpenCV. win. ImgProc não são atualizados regularmente e não passam pelas verificações de conformidade do repositório, portanto, esses pacotes são destinados apenas à experimentação.

Este artigo se baseia no conteúdo de dois outros artigos:

* [Processar quadros de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md): este artigo fornece informações detalhadas sobre como usar o **MediaFrameReader** para obter quadros de mídia de um ou mais quadros fontes e descreve detalhadamente a maior parte do código de amostra neste artigo. Em particular, **Processar quadros de mídia com MediaFrameReader** fornece o código de listagem para uma classe auxiliar, **FrameRenderer**, que lida com a apresentação dos quadros de mídia em um elemento de **imagem** XAML. O código de exemplo neste artigo também usa a classe auxiliar.

* [Processar bitmaps de software com o OpenCV](process-software-bitmaps-with-opencv.md) -este artigo orienta você pela criação de um código nativo Windows Runtime componente, **OpenCVBridge**, que ajuda a converter entre o objeto **SoftwareBitmap** , usado pelo **MediaFrameReader**e o tipo de **passe-partout** usado pela biblioteca OpenCV. O código de exemplo neste artigo presume que você seguiu as etapas para adicionar o componente **OpenCVBridge** à sua solução de aplicativo UWP.

Além desses artigos, para exibir e baixar um exemplo de funcional completo de ponta a ponta do cenário descrito neste artigo, consulte [Quadros de câmera + exemplo de OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) no repositório do GitHub de amostras universais do Windows.

Para começar a desenvolver rapidamente, você pode incluir a biblioteca OpenCV em um projeto de aplicativo UWP usando pacotes NuGet, mas esses pacotes podem não passar o processo certficication do aplicativo quando você envia seu aplicativo para a loja, portanto, é recomendável que você baixe o OpenCV código-fonte da biblioteca e crie os binários por conta própria antes de enviar seu aplicativo. As informações sobre o desenvolvimento com o OpenCV podem ser encontradas em [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implementar o componente de Windows Runtime nativo do OpenCVHelper
Siga as etapas em [processar bitmaps de software com OpenCV](process-software-bitmaps-with-opencv.md) para criar o componente auxiliar de OpenCV Windows Runtime e adicionar uma referência ao projeto de componente à sua solução de aplicativo UWP.

## <a name="find-available-frame-source-groups"></a>Localizar grupos de origem de quadro disponíveis
Primeiro, você deve encontrar um grupo de origem do quadro de mídia a partir do qual os quadros de mídia serão obtidos. Obtenha a lista de grupos de origem disponíveis no dispositivo atual chamando **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** . Em seguida, selecione os grupos de origem que fornecem os tipos de sensores necessários para o cenário do aplicativo. Neste exemplo, precisamos somente de um grupo de origem que fornece os quadros de uma câmera RGB.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Inicializar o objeto MediaCapture
Em seguida, é necessário inicializar o objeto **MediaCapture** para usar o grupo de origem de quadros selecionado na etapa anterior ao configurar a propriedade **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** de **MediaCaptureInitializationSettings**.

> [!NOTE] 
> A técnica usada pelo componente OpenCVHelper, descrita em detalhes em [Processar bitmaps de software com OpenCV](process-software-bitmaps-with-opencv.md), exige que os dados da imagem a ser processada estejam na memória da CPU em vez da GPU. Assim, você deve especificar **MemoryPreference.CPU** para o campo **[MemoryPreference](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** de **MediaCaptureInitializationSettings**.

Depois de inicializar o objeto **MediaCapture**, obtenha uma referência para a origem de quadro RGB acessando a propriedade **[MediaCapture.FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** .

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Inicializar o MediaFrameReader
Em seguida, crie um [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) para a origem de quadro RGB recuperado na etapa anterior. Para manter uma taxa de quadros ideal, você pode querer processar quadros com uma resolução menor do que a do sensor. Este exemplo fornece o argumento opcional **[BitmapSize](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapsize)** para o método **[MediaCapture.CreateFrameReaderAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** a fim de solicitar que os quadros fornecidos pelo leitor de quadros seja redimensionado para 640 x 480 pixels.

Depois de criar o leitor de quadros, registre um manipulador para o evento **[FrameArrived](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** . Em seguida, crie um novo objeto **[SoftwareBitmapSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** , que a classe auxiliar **FrameRenderer** usará para apresentar a imagem processada. Em seguida, chame o construtor para o **FrameRenderer**. Inicialize a instância da classe **OpenCVHelper** definida no componente de Windows Runtime OpenCVBridge. Essa classe auxiliar é usada no manipulador **FrameArrived** para processar cada quadro. Por fim, inicie o leitor de quadros ao chamar **[StartAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** .

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Manipular o evento FrameArrived
O evento **FrameArrived** é acionado sempre que um novo quadro está disponível no leitor de quadros. Chame **[TryAcquireLatestFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** para obter o quadro, se ele existir. Obtenha o **SoftwareBitmap** do **[MediaFrameReference](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference)** . Observe que a classe **CVHelper** usada neste exemplo exige imagens para usar o formato de pixels BRGA8 com alfa pré-multiplicado. Se o quadro passado para o evento tem um formato diferente, converta o **SoftwareBitmap** para o formato correto. Em seguida, crie um **SoftwareBitmap** para ser usado como o destino da operação de desfoque. As propriedades da imagem de origem são usadas como argumentos para o construtor criar um bitmap com o formato de correspondência. Chame o método de classe auxiliar **Desfocar** para processar o quadro. Por fim, passe a imagem de saída da operação de desfoque para **PresentSoftwareBitmap**, o método da classe auxiliar **FrameRenderer** que exibe a imagem no controle de **imagem** XAML com o qual foi inicializado.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Foto básica, vídeo e captura de áudio com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Processar quadros de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Processar bitmaps de software com OpenCV](process-software-bitmaps-with-opencv.md)
* [Exemplo de quadros de câmera](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Quadros de câmera + exemplo de OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 




