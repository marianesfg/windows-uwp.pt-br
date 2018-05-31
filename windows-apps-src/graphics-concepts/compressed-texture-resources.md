---
title: Recursos de textura compactada
description: Os mapas de textura são imagens digitalizadas desenhadas em formas tridimensionais para adicionar detalhes visuais.
ms.assetid: 2DD5FF94-A029-4694-B103-26946C8DFBC1
keywords:
- Recursos de textura compactada
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a26679bab35590d61f9188f64df977fbaf78d90
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653225"
---
# <a name="compressed-texture-resources"></a>Recursos de textura compactada


Os mapas de textura são imagens digitalizadas desenhadas em formas tridimensionais para adicionar detalhes visuais. Elas são mapeadas para essas formas durante a rasterização, e o processo pode consumir grandes quantidades de memória e barramento de sistema. Para reduzir a quantidade de memória consumida pelas texturas, o Direct3D dá suporte à compactação de superfícies de textura. Alguns dispositivos Direct3D têm suporte nativo às superfícies de textura compactadas. Em tais dispositivos, quando você cria uma superfície compactada e carrega os dados nela, a superfície pode ser usada no Direct3D como qualquer outra superfície de textura. O Direct3D manipula a descompactação quando a textura é mapeada para um objeto 3D.

## <a name="span-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanstorage-efficiency-and-texture-compression"></a><span id="Storage-Efficiency-and-Texture-Compression"></span><span id="storage-efficiency-and-texture-compression"></span><span id="STORAGE-EFFICIENCY-AND-TEXTURE-COMPRESSION"></span>Compactação de textura e eficiência de armazenamento


Todos os formatos de compactação de textura são potências de dois. Embora isso não signifique que uma textura é necessariamente quadrada, significa que x e y são potências de dois. Por exemplo, se uma textura for originalmente de 512 por 128 bytes, o próximo mapeamento de MIP seria de 256 por 64 e assim por diante, com cada nível diminuindo por uma potência de dois. Em níveis inferiores, onde a textura é filtrada como 16 por 2 e 8 por 1, haverá desperdício de bits porque o bloco de compactação é sempre um bloco de 4 por 4 texels. As partes não utilizadas do bloco são preenchidas.

Embora existam bits desperdiçados nos níveis inferiores, o ganho geral é ainda significativo. Teoricamente, o pior caso é uma textura de 2K por 1 (potência de 2⁰). Aqui, apenas uma única linha de pixels é codificada por bloco, enquanto o restante do bloco é não utilizado.

## <a name="span-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanmixing-formats-within-a-single-texture"></a><span id="Mixing-Formats-Within-a-Single-Texture"></span><span id="mixing-formats-within-a-single-texture"></span><span id="MIXING-FORMATS-WITHIN-A-SINGLE-TEXTURE"></span>Mistura de formatos dentro de uma textura


É importante observar que qualquer textura deve especificar que os dados sejam armazenados como 64 ou 128 bits por grupo de 16 texels. Se os blocos de 64 bits, ou seja, o formato de compactação de bloco BC1, forem usados para a textura, é possível combinar os formatos alfa opacos e de 1 bit por bloco dentro da mesma textura. Em outras palavras, a comparação da magnitude de inteiro sem sinal de color\_0 e color\_1 é realizada exclusivamente para cada bloco de 16 texels.

Depois que os blocos de 128 bits forem usados, o canal alfa deve ser especificado no modo explícito (formato BC2) ou interpolado (formato BC3) para a textura inteira. Da mesma forma que ocorre com a cor, quando o modo interpolado (formato BC3) é selecionado, então o modo de seis ou oito alfas interpolados pode ser usado em uma base de bloco por bloco. Novamente, a comparação de magnitude de alpha\_0 e alpha\_1 é feita exclusivamente em uma base de bloco pelo bloco.

O Direct3D fornece serviços para compactar superfícies que são usadas para a texturização de modelos 3D. Esta seção fornece informações sobre como criar e tratar os dados em uma superfície de textura compactada.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Nesta seção


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="opaque-and-1-bit-alpha-textures.md">Texturas opacas e alfa de 1 bit</a></p></td>
<td align="left"><p>O formato de textura BC1 destina-se a texturas que são opacas ou que têm uma única cor transparente.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures-with-alpha-channels.md">Texturas com canais alfa</a></p></td>
<td align="left"><p>Há duas maneiras de codificar mapas de textura que exibem transparência mais complexa. Em cada caso, um bloco que descreve a transparência precede o bloco de 64 bits já descrito. A transparência é representada como um bitmap de 4 x 4 com 4 bits por pixel (codificação explícita), ou com menos bits e interpolação linear parecida com a que é usada para a codificação de cores.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="block-compression.md">Compactação de bloco</a></p></td>
<td align="left"><p>A compactação de bloco é uma técnica de compactação de textura com perdas para reduzir o volume de memória e o tamanho da textura, dando um aumento de desempenho. Uma textura compactada em bloco pode ser menor do que uma textura com 32 bits por cor.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="compressed-texture-formats.md">Formatos de textura compactada</a></p></td>
<td align="left"><p>Esta seção contém informações sobre a organização interna de formatos de textura compactada. Você não precisa esses detalhes para usar texturas compactadas, porque você pode usar as funções do Direct3D para conversão de e para formatos compactados. No entanto, essas informações são úteis se você quiser operar diretamente nos dados da superfície compactada.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




