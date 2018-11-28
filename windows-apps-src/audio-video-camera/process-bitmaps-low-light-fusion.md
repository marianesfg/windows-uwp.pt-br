---
description: Este artigo explica como usar a classe LowLightFusion para processar bitmaps.
title: Processar bitmaps com a API Low Light Fusion
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, low light fusion, bitmaps, processamento de imagens
ms.localizationpriority: medium
ms.openlocfilehash: e0677d2e4ce5e412c158b8a00da3a6338bae6c46
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7964086"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Processar bitmaps com a API LowLightFusion

É difícil capturar as imagens de pouca luz com boa qualidade, especialmente em dispositivos móveis com abertura fixa e tamanho de sensor. Para compensar a baixa luminosidade, os dispositivos podem aumentar o tempo de exposição ou o ganho do sensor, o que pode resultar no desfoque do movimento e no aumento do ruído nas imagens. 

A [classe LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion) melhora a qualidade das imagens de pouca luz por meio da amostragem das informações de pixel em vários quadros em curta proximidade temporal, por exemplo, imagens de baixa intermitência, para reduzir o ruído e o desfoque do movimento. Este é um recurso útil a ser adicionado a um app de edição de fotos, por exemplo.

Esse recurso também é disponibilizado por meio da [classe AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture), que aplica o algoritmo Low Light Fusion a uma sequência de imagens diretamente, depois que as imagens são capturadas, se necessário. Consulte [Captura de fotos de pouca luz](https://docs.microsoft.com/windows/uwp/audio-video-camera/high-dynamic-range-hdr-photo-capture#low-light-photo-capture) para saber como implementar esse recurso.

## <a name="prepare-the-images-for-processing"></a>Preparar as imagens para processamento

Neste exemplo, demonstraremos como usar a [classe LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion), bem como o [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker), para permitir que um usuário selecione várias imagens nas quais serão executadas o Low Light Fusion.

Primeiro, precisaremos determinar quantas imagens (também denominadas quadros) o algoritmo aceita e criar uma lista para armazenar esses quadros.

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

Depois que determinarmos quantos quadros o algoritmo Low Light Fusion aceitará, poderemos usar o [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir que o usuário escolha as imagens a serem usadas no algoritmo.

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

Agora que temos o número correto de quadros selecionados, precisamos decodificá-los em [SoftwareBitmaps](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) e garantir que os SoftwareBitmaps estejam no formato correto para LowLightFusion.

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Fundir os bitmaps em um único bitmap

Agora que temos um número correto de quadros em um formato aceitável, podemos usar o método **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion.fuseasync)** para aplicar o algoritmo Low Light Fusion. O resultado será a imagem processada, com mais clareza, no formato de um SoftwareBitmap. 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

Por fim, limparemos o SoftwareBitmap resultante codificando-o e salvando-o em uma imagem "normal" e amigável, como as imagens de entrada com as quais começamos.

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>Antes e depois

Aqui está um exemplo de uma imagem de entrada e da imagem de saída resultante da aplicação do algoritmo Low Light Fusion.

> [!div class="mx-tableFixed"] 
| Quadro de entrada | Saída do Low Light Fusion | 
|-------------|-------------------------|
| ![Quadro de entrada no algoritmo Low Light Fusion](./images/LLF-Input.png) | ![Quadro resultante do algoritmo Low Light Fusion](./images/LLF-Output.png) |

Você pode ver no quadro de entrada que a iluminação e clareza das sombras ao redor da faixa foram aprimoradas.

## <a name="related-topics"></a>Tópicos relacionados 
[Classe LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[Classe LowLightFusionResult](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)