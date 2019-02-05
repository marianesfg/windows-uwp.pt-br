---
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: Este artigo mostra como usar a interface IMediaEncodingProperties para definir a resolução e a taxa de quadro do fluxo de visualização da câmera e de fotos e vídeo capturados.
title: Definir o formato, a resolução e a taxa de quadros para o MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 77b8f075e0eac02722c29eddddb6f188575ca18f
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9047501"
---
# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>Definir o formato, a resolução e a taxa de quadros para o MediaCapture



Este artigo mostra como usar a interface [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) para definir a resolução e a taxa de quadro do fluxo de visualização da câmera e de fotos e vídeo capturados. Ele também mostra como garantir que a taxa de proporção do fluxo de visualização corresponda ao da mídia capturada.

Perfis de câmera oferecem uma maneira mais avançada de descobrir e definir as propriedades de fluxo da câmera, mas eles não têm suporte em todos os dispositivos. Para obter mais informações, consulte [Perfis de câmera](camera-profiles.md).

O código neste artigo foi adaptado da [amostra CameraResolution](https://go.microsoft.com/fwlink/p/?LinkId=624252&clcid=0x409). Você pode baixar a amostra para ver o código usado no contexto ou usar a amostra como ponto de partida para seu próprio aplicativo.

> [!NOTE] 
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. É recomendável que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que seu aplicativo já tenha uma instância do MediaCapture inicializada corretamente.

## <a name="a-media-encoding-properties-helper-class"></a>Uma classe auxiliar de propriedades de codificação de mídia

Criar uma classe auxiliar simples para encapsular a funcionalidade da interface [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) torna mais fácil selecionar um conjunto de propriedades de codificação que atendem aos critérios específicos. Essa classe auxiliar é particularmente útil devido ao seguinte comportamento do recurso de propriedades de codificação:

**Aviso**  o método [**VideoDeviceController.GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) usa um membro da enumeração [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640) , como **VideoRecord** ou **fotos**e retorna uma lista de qualquer [** ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) ou objetos [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) que transmitem o fluxo de codificação de configurações, como a resolução da foto capturada ou vídeo. Os resultados da chamada a **GetAvailableMediaStreamProperties** podem incluir **ImageEncodingProperties** ou **VideoEncodingProperties**, independentemente do valor de **MediaStreamType** especificado. Por esse motivo, você sempre deve verificar o tipo de cada valor retornado e convertê-lo para o tipo apropriado antes de tentar acessar qualquer um dos valores de propriedade.

A classe auxiliar definida a seguir manipula a verificação e a conversão de tipo para [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) ou [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217), para que o código do aplicativo não precise distinguir entre os dois tipos. Além disso, a classe auxiliar expõe propriedades para a taxa de proporção das propriedades, a taxa de quadros (somente para propriedades de codificação de vídeo) e um nome amigável que facilita a exibição das propriedades de codificação na interface do usuário do aplicativo.

Você deve incluir o namespace [**Windows.Media.MediaProperties**](https://msdn.microsoft.com/library/windows/apps/hh701296) no arquivo de origem para a classe auxiliar.

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>Determinar se os fluxos de visualização e captura são independentes

Em alguns dispositivos, o mesmo pin de hardware é usado para fluxos de visualização e captura. Nesses dispositivos, a definição das propriedades de codificação de um também define o outro. Em dispositivos que usam pins de hardware diferentes para captura e visualização, as propriedades podem ser definidas para cada fluxo de forma independente. Use o código a seguir para determinar se os fluxos de visualização e captura são independentes. Você deve ajustar sua interface do usuário para habilitar ou desabilitar a configuração dos fluxos de forma independente com base no resultado desse teste.

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## <a name="get-a-list-of-available-stream-properties"></a>Obter uma lista de propriedades de fluxo disponíveis

Obtenha uma lista das propriedades de fluxo disponíveis para um dispositivo de captura obtendo [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) para o objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md) do seu aplicativo e depois chamando [**GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) e transmitindo um dos valores de [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640): **VideoPreview**, **VideoRecord** ou **Photo**. Neste exemplo, a sintaxe Linq é usada para criar uma lista de objetos **StreamPropertiesHelper**, definidos anteriormente neste artigo, para cada um dos valores [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) retornados de **GetAvailableMediaStreamProperties**. Este exemplo usa primeiro os métodos de extensão Linq para ordenar as propriedades retornadas com base, em primeiro lugar, na resolução e, depois, na taxa de quadros.

Se seu aplicativo tiver requisitos de resolução ou de taxa de quadros específicos, você poderá selecionar um conjunto de propriedades de codificação de mídia de forma programática. Um aplicativo de câmera comum expõe a lista de propriedades disponíveis na interface do usuário e permite que o usuário selecione as configurações desejadas. Um **ComboBoxItem** é criado para cada item na lista de objetos **StreamPropertiesHelper** na lista. O conteúdo é definido como o nome amigável retornado pela classe auxiliar, e a marca é definida como a própria classe auxiliar para que ela possa ser usada posteriormente para recuperar as propriedades de codificação associadas. Cada **ComboBoxItem** é adicionado à **ComboBox** transmitida ao método.

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## <a name="set-the-desired-stream-properties"></a>Definir as propriedades de fluxo desejadas

Instrua o controlador de dispositivo de vídeo a usar as propriedades de codificação desejadas chamando [**SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895), transmitindo o valor **MediaStreamType**, que indica se as propriedades de foto, vídeo ou visualização devem ser definidas. Este exemplo define as propriedades de codificação solicitadas quando o usuário seleciona um item em um dos objetos **ComboBox** populados com o método auxiliar **PopulateStreamPropertiesUI**.

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>Corresponder a taxa de proporção dos fluxos de visualização e captura

Um aplicativo de câmera típico fornecerá a interface do usuário para o usuário selecionar a resolução de captura de vídeo ou foto, mas definirá de forma programática a resolução da visualização. Há algumas estratégias diferentes para selecionar a melhor resolução de fluxo de visualização para seu aplicativo:

-   Selecione a melhor resolução de visualização disponível, permitindo que a estrutura da IU execute qualquer dimensionamento necessário da visualização.

-   Selecione a resolução de visualização mais próxima à resolução de captura para que a visualização mostre a representação mais próxima à mídia capturada final.

-   Selecione a resolução da visualização mais próxima ao tamanho do [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278), para que não passem mais pixels do que o necessário pelo pipeline de fluxo de visualização.

**Importante**  é possível, em alguns dispositivos, definir uma taxa de proporção diferente para o fluxo de visualização da câmera e fluxo de captura. O corte de quadro causado por essa incompatibilidade pode resultar em conteúdo presente na mídia capturada que não estava visível na visualização, o que pode resultar em uma experiência de usuário negativa. É altamente recomendável que você use a mesma taxa de proporção, dentro de uma pequena janela de tolerância, para os fluxos de visualização e captura. Não há problemas em ter resoluções totalmente diferentes habilitadas para captura e visualização, desde que a taxa de proporção tenha correspondência aproximada.


Para garantir que os fluxos de captura de foto ou de vídeo correspondam à taxa de proporção do fluxo de visualização, este exemplo chama [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) e transmite o valor de enumeração **VideoPreview** para solicitar as propriedades de fluxo atuais para o fluxo de visualização. Em seguida, uma pequena janela de tolerância de taxa de proporção é definida para que possamos incluir taxas de proporção que não sejam exatamente iguais ao fluxo de visualização, desde que sejam aproximadas. Em seguida, um método de extensão Linq é usado para selecionar apenas os objetos **StreamPropertiesHelper** em que a taxa de proporção esteja dentro do intervalo de tolerância definido do fluxo de visualização.

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 




