---
title: Texturas com canais alfa
description: Há duas maneiras de codificar mapas de texturas que exibem uma transparência mais complexa.
ms.assetid: 768A774A-4F21-4DDE-B863-14211DA92926
keywords:
- Texturas com canais alfa
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88d150383d2be219e7f382e0e690771acbc9d2ee
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8825166"
---
# <a name="textures-with-alpha-channels"></a>Texturas com canais alfa


Há duas maneiras de codificar mapas de textura que exibem transparência mais complexa. Em cada caso, um bloco que descreve a transparência precede o bloco de 64 bits já descrito. A transparência é representada como um bitmap 4x4 com 4 bits por pixel (codificação explícita) ou com menos bits e interpolação linear, semelhante ao que é usado para a codificação de cores.

O bloco de transparência e o bloco de cores são organizados conforme mostrado na tabela a seguir.

| Endereço da palavra | Bloco de 64 bits                      |
|--------------|-----------------------------------|
| 3:0          | Bloco de transparência                |
| 7:4          | Bloco de 64 bits descrito anteriormente |

 

## <a name="span-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanexplicit-texture-encoding"></a><span id="Explicit-Texture-Encoding"></span><span id="explicit-texture-encoding"></span><span id="EXPLICIT-TEXTURE-ENCODING"></span>Codificação de textura explícita


Para a codificação de textura explícita (formato BC2), os componentes alfa do texels que descrevem a transparência são codificados em um bitmap 4x4 com 4 bits por texel. Esses quatro bits podem ser alcançados por uma variedade de meios, como pontilhado ou usando os quatro bits mais significativos dos dados alfa. Independentemente de como são produzidos, eles são usados como estão, sem qualquer forma de interpolação.

O diagrama a seguir mostra um bloco de transparência de 64 bits.

![diagrama de um bloco de transparência de 64 bits](images/colors4.png)

**Observação**  o método de compactação do Direct3D usa os quatro bits mais significativos.

 

As tabelas a seguir ilustram como as informações alfa são dispostas na memória, para cada palavra de 16 bits.

Layout da palavra 0:

| Bits          | Alfa      |
|---------------|------------|
| 3:0 (LSB\*)   | \[0\]\[0\] |
| 7:4           | \[0\]\[1\] |
| 11:8          | \[0\]\[2\] |
| 15:12 (MSB\*) | \[0\]\[3\] |

 

\*bit menos significativo, bit mais significativo (MSB)

Layout da palavra 1:

| Bits        | Alfa      |
|-------------|------------|
| 3:0 (LSB)   | \[1\]\[0\] |
| 7:4         | \[1\]\[1\] |
| 11:8        | \[1\]\[2\] |
| 15:12 (MSB) | \[1\]\[3\] |

 

Layout da palavra 2:

| Bits        | Alfa      |
|-------------|------------|
| 3:0 (LSB)   | \[2\]\[0\] |
| 7:4         | \[2\]\[1\] |
| 11:8        | \[2\]\[2\] |
| 15:12 (MSB) | \[2\]\[3\] |

 

Layout da palavra 3:

| Bits        | Alfa      |
|-------------|------------|
| 3:0 (LSB)   | \[3\]\[0\] |
| 7:4         | \[3\]\[1\] |
| 11:8        | \[3\]\[2\] |
| 15:12 (MSB) | \[3\]\[3\] |

 

A comparação de cores usada em BC1 para determinar se o texel é transparente não é usada nesse formato. Presume-se que, sem a comparação de cores, os dados de cor sejam sempre tratados como se estivessem modo de 4 cores.

## <a name="span-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanthree-bit-linear-alpha-interpolation"></a><span id="Three-Bit-Linear-Alpha-Interpolation"></span><span id="three-bit-linear-alpha-interpolation"></span><span id="THREE-BIT-LINEAR-ALPHA-INTERPOLATION"></span>Interpolação alfa linear de três bits


A codificação de transparência para o formato BC3 é baseada em um conceito semelhante ao da codificação linear usada para cor. Dois valores alfa de 8 bits e um bitmap 4x4 com três bits por pixel são armazenados nos primeiros oito bytes do bloco. Os valores alfa representativos são usados para interpolar os valores alfa intermediários. Há mais informações disponíveis sobre como os dois valores alfa são armazenados. Se alpha\_0 for maior que alpha\_1, seis valores alfa intermediários serão criados pela interpolação. Caso contrário, os quatro valores alfa intermediários são interpolados entre os extremos alfa especificados. Os dois valores alfa implícitos adicionais são 0 (totalmente transparente) e 255 (totalmente opaco).

O exemplo de código a seguir ilustra esse algoritmo.

```
// 8-alpha or 6-alpha block?    
if (alpha_0 > alpha_1) {    
    // 8-alpha block:  derive the other six alphas.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (6 * alpha_0 + 1 * alpha_1 + 3) / 7;    // bit code 010
    alpha_3 = (5 * alpha_0 + 2 * alpha_1 + 3) / 7;    // bit code 011
    alpha_4 = (4 * alpha_0 + 3 * alpha_1 + 3) / 7;    // bit code 100
    alpha_5 = (3 * alpha_0 + 4 * alpha_1 + 3) / 7;    // bit code 101
    alpha_6 = (2 * alpha_0 + 5 * alpha_1 + 3) / 7;    // bit code 110
    alpha_7 = (1 * alpha_0 + 6 * alpha_1 + 3) / 7;    // bit code 111  
}    
else {  
    // 6-alpha block.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (4 * alpha_0 + 1 * alpha_1 + 2) / 5;    // Bit code 010
    alpha_3 = (3 * alpha_0 + 2 * alpha_1 + 2) / 5;    // Bit code 011
    alpha_4 = (2 * alpha_0 + 3 * alpha_1 + 2) / 5;    // Bit code 100
    alpha_5 = (1 * alpha_0 + 4 * alpha_1 + 2) / 5;    // Bit code 101
    alpha_6 = 0;                                      // Bit code 110
    alpha_7 = 255;                                    // Bit code 111
}
```

O layout da memória do bloco de alfa é o seguinte:

| Byte | Alfa                                                          |
|------|----------------------------------------------------------------|
| 0    | Alpha\_0                                                       |
| 1    | Alpha\_1                                                       |
| 2    | \[0\]\[2\] (2 MSBs), \[0\]\[1\], \[0\]\[0\]                    |
| 3    | \[1\]\[1\] (1 MSB), \[1\]\[0\], \[0\]\[3\], \[0\]\[2\] (1 LSB) |
| 4    | \[1\]\[3\], \[1\]\[2\], \[1\]\[1\] (2 LSBs)                    |
| 5    | \[2\]\[2\] (2 MSBs), \[2\]\[1\], \[2\]\[0\]                    |
| 6    | \[3\]\[1\] (1 MSB), \[3\]\[0\], \[2\]\[3\], \[2\]\[2\] (1 LSB) |
| 7    | \[3\]\[3\], \[3\]\[2\], \[3\]\[1\] (2 LSBs)                    |

 

A comparação de cores usada em BC1 para determinar se o texel é transparente não é usada com esses formatos. Presume-se que, sem a comparação de cores, os dados de cor sejam sempre tratados como se estivessem modo de 4 cores.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de textura compactada](compressed-texture-resources.md)

 

 




