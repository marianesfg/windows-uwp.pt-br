---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: Este tópico mostra como obter um único quadro de visualização do fluxo de visualização de captura de mídia.
title: Obter um quadro de visualização
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7faa018dad336b6e22dd236e57585cade38f8a94
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8748448"
---
# <a name="get-a-preview-frame"></a>Obter um quadro de visualização


Este tópico mostra como obter um único quadro de visualização do fluxo de visualização de captura de mídia.

> [!NOTE] 
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. Recomendamos que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código neste artigo presume que seu aplicativo já tem uma instância de MediaCapture que foi inicializada corretamente e que você tem um [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) com um fluxo de visualização de vídeo ativo.

Além dos namespaces necessários para captura de mídia básica, a captura de um quadro de visualização exige o namespace a seguir.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

Quando solicitar um quadro de visualização, você poderá especificar o formato em que gostaria de receber o quadro criando um objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) com o formato desejado. Este exemplo cria um quadro de vídeo com a mesma resolução do fluxo de visualização chamando [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) e especificando [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) para solicitar as propriedades do fluxo de visualização. A largura e a altura do fluxo de visualização são usadas para criar o novo quadro de vídeo.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

Se seu objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) for inicializado e você tiver um fluxo de visualização ativo, chame [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926711) para obter um fluxo de visualização. Passe o quadro de vídeo criado na última etapa para especificar o formato do quadro retornado.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

Obtenha uma representação de [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) do quadro de visualização acessando a propriedade [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) do objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917). Para obter informações sobre como salvar, carregar e modificar bitmaps de software, consulte [Imagens](imaging.md).

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Você também poderá obter uma representação de [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) do quadro de visualização se quiser usar a imagem com APIs do Direct3D.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> As propriedades [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) ou [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) do **VideoFrame** retornado podem ser nulas dependendo da forma como você chama **GetPreviewFrameAsync** e do dispositivo no qual o aplicativo está sendo executado.

> - Se você chamar a sobrecarga de [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926713) que aceita um argumento **VideoFrame**, o **VideoFrame** retornado terá um **SoftwareBitmap** não nulo e a propriedade **Direct3DSurface** será nula.
> - Se você chamar a sobrecarga de [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) que não tem argumentos em um dispositivo que usa uma superfície Direct3D para representar o quadro internamente, a propriedade **Direct3DSurface** será diferente de nula e a propriedade **SoftwareBitmap** será nula.
> - Se você chamar a sobrecarga de [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) que não tem argumentos em um dispositivo que não usa uma superfície Direct3D para representar o quadro internamente, a propriedade **SoftwareBitmap** será diferente de nula e a propriedade **Direct3DSurface** será nula.

Seu aplicativo sempre deverá verificar se há um valor nulo antes de tentar operar nos objetos retornados pelas propriedades **SoftwareBitmap** ou **Direct3DSurface**.

Quando terminar de usar o quadro de visualização, certifique-se de chamar seu método [**Close**](https://msdn.microsoft.com/library/windows/apps/dn930918) (projetado para Descarte em C#) para liberar os recursos usados pelo quadro. Ou use o padrão **using**, que descarta automaticamente o objeto.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




