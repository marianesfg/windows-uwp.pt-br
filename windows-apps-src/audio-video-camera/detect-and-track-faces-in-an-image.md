---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: Este tópico mostra como usar FaceDetector para detectar rostos em uma imagem. FaceTracker é otimizado para acompanhamento facial ao longo do tempo em uma sequência de quadros de vídeo.
title: Detectar rostos em imagens ou vídeos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 970fbcef984f28e779ea7c133de95f7f7f99be8d
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469273"
---
# <a name="detect-faces-in-images-or-videos"></a>Detectar rostos em imagens ou vídeos



\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não fornece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

Este tópico mostra como usar [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) para detectar rostos em uma imagem. [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) é otimizado para acompanhamento facial ao longo do tempo em uma sequência de quadros de vídeo.

Para um método alternativo de acompanhamento facial que usa [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776), consulte [Análise de cena para captura de mídia](scene-analysis-for-media-capture.md).

O código neste artigo foi adaptado dos exemplos [Detecção básica de rostos](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409) e [Acompanhamento facial básico](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409). Você pode baixar esses exemplos para ver o código usado em contexto ou usar o exemplo como ponto de partida para seu próprio aplicativo.

## <a name="detect-faces-in-a-single-image"></a>Detectar rostos em uma única imagem

A classe [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) permite detectar um ou mais rostos em uma imagem estática.

Esse exemplo usa APIs dos namespaces a seguir.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

Declarar uma variável de membro de classe para o objeto [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) e para a lista de objetos [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) que serão detectados na imagem.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

A detecção de rostos opera em um objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) que pode ser criado de diversas maneiras. Neste exemplo, [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) é usado para permitir que o usuário selecione um arquivo de imagem no qual rostos serão detectados. Para obter mais informações sobre como trabalhar com bitmaps de software, consulte [Geração de imagens](imaging.md).

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

Use a classe [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) para decodificar o arquivo de imagem em **SoftwareBitmap**. O processo de detecção de rostos é mais rápido com uma imagem menor, portanto você pode reduzir verticalmente a imagem de origem para um tamanho menor. Você pode fazer isso durante a decodificação criando um objeto [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254), definindo as propriedades [**ScaledWidth**](https://msdn.microsoft.com/library/windows/apps/br226261) e [**ScaledHeight**](https://msdn.microsoft.com/library/windows/apps/br226260) passando-o para a chamada a [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332), que retorna o **SoftwareBitmap** decodificado e dimensionado.

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

Na versão atual, a classe **FaceDetector** dá suporte somente a imagens em Gray8 ou Nv12. A classe **SoftwareBitmap** fornece o método [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362), que converte um bitmap de um formato em outro. Este exemplo converterá a imagem de origem no formato de pixel Gray8 caso ainda não esteja nesse formato. Se você quiser, poderá usar os métodos [**GetSupportedBitmapPixelFormats**](https://msdn.microsoft.com/library/windows/apps/dn974140) e [**IsBitmapPixelFormatSupported**](https://msdn.microsoft.com/library/windows/apps/dn974142) para determinar, em tempo de execução, se há suporte para um formato de pixel, caso o conjunto de formatos com suporte seja expandido em futuras versões.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

Instancie o objeto **FaceDetector** chamando [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974132) e chame [**DetectFacesAsync**](https://msdn.microsoft.com/library/windows/apps/dn974134), passando o bitmap que foi dimensionado para um tamanho razoável e convertido em um formato de pixel com suporte. Esse método retorna uma lista de objetos [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123). **ShowDetectedFaces** é um método auxiliar, mostrado abaixo, que desenha quadrados ao redor dos rostos na imagem.

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

Certifique-se de descartar os objetos que foram criados durante o processo de detecção de rostos.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

Para exibir a imagem e desenhar caixas em torno dos rostos detectados, adicione um elemento [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) à página XAML.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

Defina algumas variáveis de membro para definir o estilo dos quadrados que serão desenhados.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

No método auxiliar **ShowDetectedFaces**, um novo [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) é criado, e a origem é definida como [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) criada com base no **SoftwareBitmap** que representa a imagem de origem. A tela de fundo do controle XAML **Canvas** é definida como o pincel de imagem.

Se a lista de rostos passada para o método auxiliar não estiver vazia, execute um loop de cada rosto na lista e use a propriedade [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126) da classe [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) para determinar a posição e o tamanho do retângulo dentro da imagem que contém o rosto. Uma vez que o controle **Canvas** muito provavelmente terá um tamanho diferente da imagem de origem, você deve multiplicar as coordenadas X e Y e a largura e a altura de **FaceBox** por um valor de escala que seja a proporção do tamanho da imagem de origem e o tamanho real do controle **Canvas**.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>Acompanhar rostos em uma sequência de quadros

Se você quiser detectar rostos em vídeos, é mais eficiente usar a classe [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) em vez da classe [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129), embora as etapas de implementação sejam muito semelhantes. **FaceTracker** usa informações sobre quadros processados anteriormente para otimizar o processo de detecção.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

Declare uma variável de classe para o objeto **FaceTracker**. Este exemplo usa [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) para iniciar o acompanhamento facial em um intervalo definido. [SemaphoreSlim](https://msdn.microsoft.com/library/system.threading.semaphoreslim.aspx) é usado para garantir que somente uma operação de acompanhamento facial seja executada por vez.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

Para iniciar a operação de acompanhamento facial, crie um novo objeto **FaceTracker** chamando [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974151). Inicie o intervalo desejado do temporizador e crie o temporizador. O método auxiliar **ProcessCurrentVideoFrame** será chamado sempre que o intervalo especificado tiver decorrido.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

O método auxiliar **ProcessCurrentVideoFrame** é chamado de forma assíncrona pelo temporizador, portanto o método chama primeiro o método **Wait** do semáforo para verificar se uma operação de acompanhamento está em andamento e se o método é retornado sem tentar detectar rostos. Ao final desse método, o método **Release** do semáforo é chamado, o que permite que a chamada subsequente a **ProcessCurrentVideoFrame** continue.

A classe [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) opera em objetos [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917). Há várias maneiras de obter **VideoFrame**, incluindo a captura de um quadro de visualização de um objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md) em execução ou a implementação do método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) de [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). Este exemplo usa um método auxiliar indefinido que retorna um quadro de vídeo, **GetLatestFrame**, como um espaço reservado para essa operação. Para obter informações sobre como obter quadros de vídeo do fluxo de visualização de um dispositivo de captura de mídia em execução, consulte [Obter um quadro de visualização](get-a-preview-frame.md).

Assim como acontece com **FaceDetector**, **FaceTracker** dá suporte a um conjunto limitado de formatos de pixel. Este exemplo abandonará a detecção de rostos se o quadro fornecido não estiver no formato Nv12.

Chame [**ProcessNextFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn974157) para recuperar uma lista de objetos [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) que representam os rostos no quadro. Depois que você tiver a lista de rostos, poderá exibi-los da mesma maneira descrita acima para detecção de rostos. Observe que, como o método auxiliar de acompanhamento facial não é chamado no thread da interface do usuário, você deve fazer as atualizações da interface do usuário dentro de uma chamada [**CoredDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>Tópicos relacionados

* [Análise de cena para captura de mídia](scene-analysis-for-media-capture.md)
* [Exemplo de detecção básica de rostos](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [Exemplo de acompanhamento facial básico](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Reprodução de mídia](media-playback.md)
