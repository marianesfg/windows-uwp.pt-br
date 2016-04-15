---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: A classe AdvancedPhotoCapture permite que você capture fotos HDR (High Dynamic Range).
title: Captura de fotos HDR (High Dynamic Range)
---

# Captura de fotos HDR (High Dynamic Range)

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


A classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) permite que você capture fotos HDR (High Dynamic Range). Essa API também permite que você obtenha um quadro de referência da captura HDR antes que o processamento da imagem final seja concluído.

Outros artigos relacionados à captura HDR incluem:

-   Você pode usar o [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) para permitir que o sistema avalie o conteúdo de fluxo de visualização de captura de mídia para determinar se o processamento de HDR melhora o resultado da captura. Para obter mais informações, consulte [Análise de cena para captura de mídia](scene-analysis-for-media-capture.md).

-   Use [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) para capturar vídeos usando o algoritmo de processamento HDR interno do Windows. Para obter mais informações, consulte [Controles de captura do dispositivo para a captura de vídeo](capture-device-controls-for-video-capture.md).

-   Você pode usar o [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) para capturar uma sequência de fotos, cada uma com configurações diferentes de captura e implementar seu próprio HDR ou outro algoritmo de processamento. Para obter mais informações, consulte [Sequência de fotos variável](variable-photo-sequence.md).

**Observação**
-   Não há suporte para gravação de vídeo e captura de foto simultâneas usando **AdvancedPhotoCapture**.

-   Não há suporte para o uso de flash da câmera durante a captura avançada de fotos.

**Observação** Este artigo se baseia nos conceitos e códigos discutidos em [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md) que descreve as etapas para implementar uma captura básica de fotos e vídeos. É recomendável que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que seu aplicativo já tenha uma instância de MediaCapture inicializada corretamente.

## Namespaces de captura de fotos HDR

Para usar a captura de fotos HDR, seu aplicativo deve incluir os namespaces a seguir, além dos namespaces necessários para a captura de mídia básica.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]


## Determinar se a captura de fotos HDR é aceita no dispositivo atual

A técnica de captura HDR descrita neste artigo é executada usando o objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Nem todos os dispositivos oferecem suporte à **AdvancedPhotoCapture**. Determine se o dispositivo em que seu aplicativo está sendo executado oferece suporte à técnica obtendo o [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) do objeto **MediaCapture** e depois obtendo a propriedade [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Verifique a coleção de [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) do controlador do dispositivo de vídeo para ver se ele inclui [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Se incluir, há suporte para a captura HDR usando **AdvancedPhotoCapture**.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

## Configurar e preparar o objeto AdvancedPhotoCapture

Como você precisará acessar a instância de [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) em vários locais em seu código, será necessário declarar uma variável de membro para armazenar o objeto.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Em seu aplicativo, após a inicialização do objeto **MediaCapture**, crie um objeto [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) e defina o modo como [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Chame o método [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) do objeto [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) passando o objeto **AdvancedPhotoCaptureSettings** que você criou.

Chame o [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) do objeto **MediaCapture** passando um objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) especificando o tipo de codificação que a captura deve usar. A classe **ImageEncodingProperties** fornece métodos estáticos para criar as codificações de imagem que são suportadas pelo **MediaCapture**.

**PrepareAdvancedPhotoCaptureAsync** retorna o objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) que você usará para iniciar a captura de foto. Use esse objeto para registrar manipuladores para [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) e [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) que são discutidos posteriormente neste artigo.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

## Capturar uma foto HDR

Capture uma foto HDR, chame o método [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) do objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Esse método retorna um objeto [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) que fornece a foto capturada em sua propriedade [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382).

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

**ConvertOrientationToPhotoOrientation** e **ReencodeAndSavePhotoAsync** são métodos auxiliares discutidos como parte do cenário básico de captura de mídia no artigo [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Obter o quadro de referência opcional

O processo HDR captura vários quadros e, em seguida, compõe esses quadros em uma única imagem depois de todos os quadros serem capturados. Você pode acessar um quadro após sua captura, mas antes da conclusão do processo de HDR manipulando o evento [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392). Você não precisará fazer isso se só estiver interessado no resultado final de fotos HDR.

**Importante** [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) não é gerado nos dispositivos com suporte para HDR de hardware e, portanto, não gera quadros de referência. Seu aplicativo deve manipular o caso em que esse evento não for gerado.

Como o quadro de referência é decorrente do contexto da chamada para **CaptureAsync**, um mecanismo é fornecido para passar informações de contexto ao manipulador **OptionalReferencePhotoCaptured**. Primeiro você deve ter um objeto que contenha as informações de contexto. O nome e o conteúdo desse objeto dependem de você. Este exemplo define um objeto que tem membros para controlar o nome do arquivo e a orientação da câmera da captura.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Crie uma nova instância do seu objeto de contexto, popule seus membros e passe-os para a sobrecarga de [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) que aceita um objeto como parâmetro.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

No manipulador de eventos [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392), converta a propriedade [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) do objeto [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) na sua classe de objeto de contexto. Este exemplo modifica o nome do arquivo para distinguir a imagem do quadro de referência da imagem HDR final e depois chama o método auxiliar **ReencodeAndSavePhotoAsync** para salvar a imagem.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

## Receber uma notificação quando todos os quadros foram capturados

A captura de fotos HDR tem duas etapas. Primeiro, vários quadros são capturados e, em seguida, os quadros são processados na imagem HDR final. Você não pode iniciar outra captura quando os quadros HDR de origem ainda estiverem sendo capturados, mas pode iniciar uma captura depois que todos os quadros forem capturados, mas antes da conclusão do pós-processamento de HDR. O evento [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) é gerado quando as capturas HDR são concluídas, permitindo que você saiba que pode iniciar outra captura. Um cenário típico é desabilitar o botão de captura da sua interface do usuário quando a captura HDR começar e habilitá-la novamente quando **AllPhotosCaptured** for gerado.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

## Limpar o objeto AdvancedPhotoCapture

Quando seu aplicativo terminar de capturar, antes do descarte do objeto **MediaCapture**, você deve desligar o objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) chamando [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) e definindo sua variável de membro como nula.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## Tópicos relacionados

* [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md)

<!--HONumber=Mar16_HO1-->


