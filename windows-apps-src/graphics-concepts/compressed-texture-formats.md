---
title: Formatos de textura compactada
description: "Esta seção contém informações sobre a organização interna de formatos de textura compactada."
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords: Formatos de textura compactada
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 1bf94307093913c3b89b1d2a80e1e77d8dec81eb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="compressed-texture-formats"></a>Formatos de textura compactada


Esta seção contém informações sobre a organização interna de formatos de textura compactada. Você não precisa esses detalhes para usar texturas compactadas, porque você pode usar as funções do Direct3D para conversão de e para formatos compactados. No entanto, essas informações são úteis se você quiser operar diretamente nos dados da superfície compactada.

O Direct3D usa um formato de compactação que divide mapas de textura em blocos de texel de 4 x 4. Se a textura não tiver transparência - é opaca - ou se a transparência for especificada por um alfa de 1 bit, um bloco de 8 bytes representa o bloco de mapa de textura. Se o mapa da textura contiver texels transparentes, usando um canal alfa, um bloco de 16 bits irá representá-lo.

Qualquer textura deve especificar que os dados sejam armazenados como 64 ou 128 bits por grupo de 16 texels. Se os blocos de 64 bits, ou seja, o formato BC1, forem usados para a textura, é possível combinar os formatos alfa opacos e de 1 bit por bloco dentro da mesma textura. Em outras palavras, a comparação da magnitude de inteiro sem sinal de color\_0 e color\_1 é realizada exclusivamente para cada bloco de 16 texels.

Quando os blocos de 128 bits forem usados, o canal alfa deve ser especificado no modo explícito (formato BC2) ou interpolado (formato BC3) para a textura inteira. Da mesma forma que ocorre com a cor, quando o modo interpolado é selecionado, o modo de seis ou oito alfas interpolados pode ser usado em uma base de bloco por bloco. Novamente, a comparação de magnitude de alpha\_0 e alpha\_1 é feita exclusivamente em uma base de bloco pelo bloco.

A densidade para formatos de BCn é medida em bytes (não blocos). Por exemplo, se você tiver uma largura de 16, então você terá uma densidade de quatro blocos (4\*8 para BC1, 4\*16 para BC2 ou BC3).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de textura compactada](compressed-texture-resources.md)

 

 



