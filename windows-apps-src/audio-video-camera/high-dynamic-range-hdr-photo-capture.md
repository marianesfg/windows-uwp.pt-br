---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: Este artigo mostra como usar a classe AdvancedPhotoCapture para capturar fotos HDR (High Dynamic) e de pouca luz.
title: Captura de foto de pouca luz e HDR (High Dynamic Range)
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cc81fdc03096dd8cac5e844a496b38da78eb6136
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5936489"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>Captura de foto de pouca luz e HDR (High Dynamic Range)



Este artigo mostra como usar a classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) para capturar fotos HDR (High Dynamic Range). Essa API também permite que você obtenha um quadro de referência da captura HDR antes que o processamento da imagem final seja concluído.

Outros artigos relacionados à captura HDR incluem:

-   Você pode usar o [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) para permitir que o sistema avalie o conteúdo de fluxo de visualização de captura de mídia para determinar se o processamento de HDR melhora o resultado da captura. Para obter mais informações, consulte [Análise de cena para MediaCapture](scene-analysis-for-media-capture.md).

-   Use [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) para capturar vídeos usando o algoritmo de processamento HDR interno do Windows. Para obter mais informações, consulte [Controles de captura do dispositivo para a captura de vídeo](capture-device-controls-for-video-capture.md).

-   Você pode usar o [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) para capturar uma sequência de fotos, cada uma com configurações diferentes de captura e implementar seu próprio HDR ou outro algoritmo de processamento. Para obter mais informações, consulte [Sequência de fotos variável](variable-photo-sequence.md).



> [!NOTE] 
> A partir do Windows 10, versão 1709, há suporte para a gravação de vídeo e o uso de **AdvancedPhotoCapture** simultaneamente.  Isso não é possível em versões anteriores. Essa alteração significa que você pode ter uma **[LowLagMediaRecording](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording)** preparada e uma **[AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture)** ao mesmo tempo. Você pode iniciar ou parar a gravação de vídeo entre chamadas para **[MediaCapture.PrepareAdvancedPhotoCaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** e **[AdvancedPhotoCapture.FinishAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)**. Você também pode chamar **[AdvancedPhotoCapture.CaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** enquanto o vídeo é gravado. No entanto, alguns cenários de **AdvancedPhotoCapture**, como capturar uma foto HDR, enquanto a gravação de vídeo resulta na alteração de alguns quadros de vídeo pela captura de HDR, resultando em uma experiência de usuário negativa. Por esse motivo, a lista de modos retornada pelo **[AdvancedPhotoControl.SupportedModes](https://docs.microsoft.com/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** será diferente enquanto o vídeo é gravado. Você deve verificar esse valor imediatamente depois de iniciar ou parar a gravação de vídeo para garantir que o modo desejado seja suportado no estado de gravação de vídeo atual.


> [!NOTE] 
> A partir do Windows 10, versão 1709, quando a **AdvancedPhotoCapture** é definida como o modo HDR, a configuração da propriedade [**FlashControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Enabled) é ignorada e o flash nunca é acionado. Para outros modos de captura, se **FlashControl.Enabled**, isso substitui as configurações de **AdvancedPhotoCapture** e resulta na captura de uma foto normal com flash. Se [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Auto) estiver configurado como true, a **AdvancedPhotoCapture** pode ou não usar o flash, dependendo do comportamento padrão do driver da câmera para as condições na cena atual. Em versões anteriores, a configuração de flash de **AdvancedPhotoCapture** sempre substitui a configuração de **FlashControl.Enabled**.

> [!NOTE] 
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. Recomendamos que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que seu aplicativo já tenha uma instância do MediaCapture inicializada corretamente.

Há um exemplo Universal do Windows que demonstra o uso da classe **AdvancedPhotoCapture** que você pode usar para ver a API usada no contexto ou como ponto de partida para seu próprio aplicativo. Para obter mais informações, consulte o [exemplo de captura avançada com câmera](http://go.microsoft.com/fwlink/?LinkID=620517).

## <a name="advanced-photo-capture-namespaces"></a>Namespaces de captura de foto avançada

Os exemplos de código neste artigo usam APIs nos namespaces a seguir, além dos namespaces necessários para captura de mídia básica.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]

## <a name="hdr-photo-capture"></a>Captura de foto HDR

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>Determinar se a captura de fotos HDR é aceita no dispositivo atual

A técnica de captura HDR descrita neste artigo é executada usando o objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Nem todos os dispositivos oferecem suporte à **AdvancedPhotoCapture**. Determine se o dispositivo em que seu aplicativo está sendo executado oferece suporte à técnica obtendo o [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) do objeto **MediaCapture** e depois obtendo a propriedade [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Verifique a coleção [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) do controlador de dispositivo de vídeo para verificar se inclui [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Caso inclua, a capture HDR usando **AdvancedPhotoCapture** é suportada.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>Configurar e preparar o objeto AdvancedPhotoCapture

Como você precisará acessar a instância de [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) em vários locais em seu código, será necessário declarar uma variável de membro para armazenar o objeto.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

No aplicativo, depois de inicializar o objeto **MediaCapture**, crie um objeto [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) e defina o modo como [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Faça uma chamada com o objeto [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) usando o método [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841), passando o objeto **AdvancedPhotoCaptureSettings** criado.

Chame o [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) do objeto **MediaCapture** passando um objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) especificando o tipo de codificação que a captura deve usar. A classe **ImageEncodingProperties** fornece métodos estáticos para criar as codificações de imagem que são suportadas pelo **MediaCapture**.

**PrepareAdvancedPhotoCaptureAsync** retorna o objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) que você usará para iniciar a captura de foto. Use esse objeto para registrar manipuladores para [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) e [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) que são discutidos posteriormente neste artigo.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

### <a name="capture-an-hdr-photo"></a>Capturar uma foto HDR

Capture uma foto HDR, chame o método [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) do objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Esse método retorna um objeto [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) que fornece a foto capturada em sua propriedade [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382).

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

A maioria dos aplicativos de fotografia vai querer codificar a rotação da foto capturada no arquivo de imagem para que ela possa ser exibida corretamente por outros aplicativos e dispositivos. Este exemplo mostra o uso da classe auxiliar **CameraRotationHelper** para calcular a orientação adequada para o arquivo. Essa classe é descrita e listada na íntegra no artigo [**Tratar a orientação do dispositivo com MediaCapture**](handle-device-orientation-with-mediacapture.md).

O método auxiliar **SaveCapturedFrameAsync**, que salva a imagem em disco, será abordado posteriormente neste artigo.

### <a name="get-optional-reference-frame"></a>Obter o quadro de referência opcional

O processo HDR captura vários quadros e, em seguida, compõe esses quadros em uma única imagem depois de todos os quadros serem capturados. Você pode acessar um quadro após sua captura, mas antes da conclusão do processo de HDR manipulando o evento [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392). Você não precisará fazer isso se só estiver interessado no resultado final de fotos HDR.

> [!IMPORTANT]
> [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) não é gerado nos dispositivos com suporte para HDR de hardware e, portanto, não gera quadros de referência. Seu aplicativo deve manipular o caso em que esse evento não for gerado.

Como o quadro de referência é decorrente do contexto da chamada para **CaptureAsync**, um mecanismo é fornecido para passar informações de contexto ao manipulador **OptionalReferencePhotoCaptured**. Primeiro você deve chamar um objeto que contenha as informações de contexto. O nome e o conteúdo desse objeto dependem de você. Este exemplo define um objeto que tem membros para controlar o nome de arquivo e a orientação de câmera da captura.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Crie uma nova instância do seu objeto de contexto, popule seus membros e passe-os para a sobrecarga de [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) que aceita um objeto como parâmetro.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

No manipulador de eventos [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392), converta a propriedade [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) do objeto [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) na sua classe de objeto de contexto. Este exemplo modifica o nome do arquivo para distinguir a imagem do quadro de referência da imagem HDR final e depois chama o método auxiliar **SaveCapturedFrameAsync** para salvar a imagem.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>Receber uma notificação quando todos os quadros foram capturados

A captura de fotos HDR tem duas etapas. Primeiro, vários quadros são capturados e, em seguida, os quadros são processados na imagem HDR final. Você não pode iniciar outra captura quando os quadros HDR de origem ainda estiverem sendo capturados, mas pode iniciar uma captura depois que todos os quadros forem capturados, mas antes da conclusão do pós-processamento de HDR. O evento [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) é gerado quando as capturas HDR são concluídas, permitindo que você saiba que pode iniciar outra captura. Um cenário típico é desabilitar o botão de captura da sua interface do usuário quando a captura HDR começar e habilitá-la novamente quando **AllPhotosCaptured** for gerado.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

### <a name="clean-up-the-advancedphotocapture-object"></a>Limpar o objeto AdvancedPhotoCapture

Quando seu aplicativo terminar de capturar, antes do descarte do objeto **MediaCapture**, você deve desligar o objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) chamando [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) e definindo sua variável de membro como nula.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]


## <a name="low-light-photo-capture"></a>Captura de fotos com pouca luz
A partir do Windows 10, versão 1607, o **AdvancedPhotoCapture** pode ser usado para capturar fotos usando um algoritmo interno que melhora a qualidade das fotos capturadas nas configurações de pouca luz. Quando você usar o recurso de pouca luz da classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture), o sistema avaliará a cena atual e, se necessário, aplicará um algoritmo para compensar as condições de pouca luz. Se o sistema determinar que o algoritmo não é necessário, uma captura normal será realizada.

Antes de usar a captura com pouca luz, determine se o dispositivo em que seu aplicativo está sendo executado oferece suporte à técnica obtendo o [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) do objeto **MediaCapture** e depois obtendo a propriedade [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Verifique a coleção de [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) do controlador do dispositivo de vídeo para ver se ele inclui [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845). Se incluir, há suporte para a captura com pouca luz usando **AdvancedPhotoCapture**. 
[!code-cs[LowLightSupported1](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported1)]

[!code-cs[LowLightSupported2](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported2)]

Em seguida, declare uma variável membro para armazenar o objeto **AdvancedPhotoCapture**. 

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Em seu aplicativo, após a inicialização do objeto **MediaCapture**, crie um objeto [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) e defina o modo como [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845). Chame o método [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) do objeto [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) passando o objeto **AdvancedPhotoCaptureSettings** que você criou.

Chame o [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) do objeto **MediaCapture** passando um objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) especificando o tipo de codificação que a captura deve usar. 

[!code-cs[CreateAdvancedCaptureLowLightAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureLowLightAsync)]

Para capturar uma foto, chame [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync).

[!code-cs[CaptureLowLight](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureLowLight)]

Como o exemplo de HDR acima, este exemplo usa uma classe auxiliar chamada **CameraRotationHelper** para determinar o valor de rotação que deve ser codificado na imagem para que ela possa ser exibida corretamente por outros aplicativos e dispositivos. Essa classe é descrita e listada na íntegra no artigo [**Tratar a orientação do dispositivo com MediaCapture**](handle-device-orientation-with-mediacapture.md).

O método auxiliar **SaveCapturedFrameAsync**, que salva a imagem em disco, será abordado posteriormente neste artigo.

Você pode capturar várias fotos com pouca luz sem precisar reconfigurar o objeto **AdvancedPhotoCapture**, mas quando terminar a captura, deverá chamar [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) para limpar o objeto e os recursos associados.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## <a name="working-with-advancedcapturedphoto-objects"></a>Trabalhando com objetos AdvancedCapturedPhoto
[**AdvancedPhotoCapture.CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync) retorna um objeto [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto) que representa a foto capturada. Esse objeto expõe a propriedade [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto.Frame) que retorna um objeto [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) que representa a imagem. O evento [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.OptionalReferencePhotoCaptured) também fornece um objeto **CapturedFrame** em seus argumentos de evento. Depois de obter um objeto desse tipo, há uma série de coisas que você poderá fazer com ele, inclusive criar um [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) ou salvar a imagem em um arquivo. 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>Obter um SoftwareBitmap de um CapturedFrame
É muito simples obter um **SoftwareBitmap** de um objeto **CapturedFrame** simplesmente acessando a propriedade [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) do objeto. No entanto, a maioria dos formatos de codificação não dá suporte para **SoftwareBitmap** com **AdvancedPhotoCapture**. Portanto, você deve verificar se a propriedade não é nula antes de usá-la.

[!code-cs[SoftwareBitmapFromCapturedFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapFromCapturedFrame)]

Na versão atual, o único formato de codificação que dá suporte a **SoftwareBitmap** para **AdvancedPhotoCapture** é o NV12 descompactado. Portanto, se você quiser usar esse recurso, especifique essa codificação quando chamar [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403). 

[!code-cs[UncompressedNv12](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUncompressedNv12)]

É claro que você pode sempre salvar a imagem em um arquivo e, em seguida, carregar o arquivo em um **SoftwareBitmap** em uma etapa separada. Para obter mais informações sobre como trabalhar com **SoftwareBitmap**, consulte [**Criar, editar e salvar imagens de bitmap**](imaging.md).

## <a name="save-a-capturedframe-to-a-file"></a>Salvar um CapturedFrame em um arquivo
A classe [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) implementa a interface IInputStream, para que possa ser usada como entrada para um [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) e depois um [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) pode ser usado para gravar os dados da imagem em disco.

No exemplo a seguir, uma nova pasta na biblioteca de imagens do usuário é criada e um arquivo é criado dentro dessa pasta. Observe que seu aplicativo precisará incluir a funcionalidade **Biblioteca de Imagens** no arquivo de manifesto do aplicativo para acessar esse diretório. Depois, um fluxo do arquivo é aberto no arquivo especificado. Em seguida, o [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder.CreateAsync) é chamado para criar o decodificador do **CapturedFrame**. Em seguida, [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214) cria um codificador a partir do fluxo do arquivo e o decodificador.

As próximas etapas codificam a orientação da foto no arquivo de imagem usando os valores de [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.BitmapProperties) do codificador. Para obter mais informações sobre como lidar com a orientação ao capturar imagens, consulte [ **Tratar a orientação do dispositivo com MediaCapture**](handle-device-orientation-with-mediacapture.md).

Por fim, a imagem é gravada em um arquivo com uma chamada para [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync).

[!code-cs[SaveCapturedFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSaveCapturedFrameAsync)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
