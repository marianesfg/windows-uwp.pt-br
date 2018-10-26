---
title: Texturas opacas e Alfa de 1 bit
description: O formato de textura BC1 destina-se a texturas que são opaca ou que têm uma única cor transparente.
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords:
- Texturas opacas e Alfa de 1 bit
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 672d7b2ddc913ea3a966fbd0a095367521a27d7c
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5550188"
---
# <a name="span-iddirect3dconceptsopaqueand1-bitalphatexturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>Texturas opacas e Alfa de 1 bit


O formato de textura BC1 destina-se a texturas que são opaca ou que têm uma única cor transparente.

Para cada bloco opaco ou de alfa de 1 bit são armazenados dois valores de 16 bits (formato RGB 5:6:5) e um bitmap de 4 x 4 com 2 bits por pixel. Isso resulta em um total de 64 bits de 16 texels ou quatro bits por texel. No bitmap de bloco, há 2 bits por texel para selecionar entre as quatro cores, dois dos quais são armazenados em dados codificados. As outras duas cores são derivadas dessas cores armazenadas por interpolação linear. Esse layout está ilustrado no diagrama a seguir.

![diagrama do layout de bitmap](images/colors1.png)

O formato de alfa de 1 bit é diferenciado do formato opaco ao comparar os dois valores de cor de 16 bits armazenados no bloco. Eles são tratados como inteiros não designados. Se a primeira cor for maior que a segunda, isso significa que somente texels opacos estão definidos. Isso significa que as quatro cores são usadas para representar os texels. Na codificação de quatro cores, existem duas cores derivadas e todas as quatro cores são distribuídas igualmente no espaço de cores RGB. Esse formato é análogo ao RGB 5:6:5. Caso contrário, para a transparência alfa de 1 bit, as três cores são usadas e a quarta é reservada para representar texels transparentes.

Na codificação de três cores, há uma cor derivada e o quarto código de 2 bits é reservado para indicar um texel transparente (informações de alfa). Esse formato é análogo ao RGBA 5:5:5:1, onde a parte final é usada para codificar a máscara alfa.

O exemplo de código a seguir ilustra o algoritmo para decidir se a codificação de três ou quatro cores está selecionada:

```
if (color_0 > color_1) 
{
    // Four-color block: derive the other two colors. 
    
    // 00 = color_0, 01 = color_1, 10 = color_2, 11 = color_3
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block.
    color_2 = (2 * color_0 + color_1 + 1) / 3;
    color_3 = (color_0 + 2 * color_1 + 1) / 3;
}    
else
{ 
    // Three-color block: derive the other color.
    // 00 = color_0,  01 = color_1,  10 = color_2,  
    // 11 = transparent.
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block. 
    color_2 = (color_0 + color_1) / 2;    
    color_3 = transparent;    

}
```

É recomendável que você defina os componentes RGBA do pixel transparência como zero antes da mesclagem.

As tabelas a seguir mostram o layout de memória para o bloco de 8 bytes. Presume-se que o primeiro índice corresponde à coordenada y e o segundo à coordenada x. Por exemplo, Texel\[1\]\[2\] refere-se ao pixel de mapa de textura em (x,y) = (2,1).

Este é o layout de memória para o bloco de 8 bytes (64 bits):

| Endereço da palavra | palavra de 16 bits    |
|--------------|----------------|
| 0            | Cor\_0       |
| 1            | Cor\_1       |
| 2            | Palavra de bitmap\_0 |
| 3            | Palavra de bitmap\_1 |

 

Cor\_0 e Cor\_1, as cores nos dois extremos, estão dispostas da seguinte maneira:

| Bits        | Cor                 |
|-------------|-----------------------|
| 4:0 (LSB\*) | Componente de cor azul  |
| 10:5        | Componente de cor verde |
| 15:11       | Componente de cor vermelha   |

 

\*bit menos importante

Palavra de bitmap\_0 disposto da seguinte forma:

| Bits          | Texel           |
|---------------|-----------------|
| 1:0 (LSB)     | Texel\[0\]\[0\] |
| 3:2           | Texel\[0\]\[1\] |
| 5:4           | Texel\[0\]\[2\] |
| 7:6           | Texel\[0\]\[3\] |
| 9:8           | Texel\[1\]\[0\] |
| 11:10         | Texel\[1\]\[1\] |
| 13:12         | Texel\[1\]\[2\] |
| 15:14 (MSB\*) | Texel\[1\]\[3\] |

 

\*bit mais importante (MSB)

Palavra de bitmap\_1 disposto da seguinte forma:

| Bits        | Texel           |
|-------------|-----------------|
| 1:0 (LSB)   | Texel\[2\]\[0\] |
| 3:2         | Texel\[2\]\[1\] |
| 5:4         | Texel\[2\]\[2\] |
| 7:6         | Texel\[2\]\[3\] |
| 9:8         | Texel\[3\]\[0\] |
| 11:10       | Texel\[3\]\[1\] |
| 13:12       | Texel\[3\]\[2\] |
| 15:14 (MSB) | Texel\[3\]\[3\] |

 

## <a name="span-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>Exemplo de codificação de cor opaca


Como um exemplo de codificação opaca, considere que as cores vermelho e preto são os extremos. Vermelho é cor\_0 e preto é cor\_1. Existem quatro cores interpoladas que formam o gradiente distribuído uniformemente entre elas. Para determinar os valores do bitmap de 4 x 4, os seguintes cálculos são usados:

```
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

Em seguida, o bitmap se parece com o diagrama a seguir.

![diagrama do layout expandido do bitmap](images/colors2.png)

Isso é semelhante à seguinte série ilustrada de cores.

**Observação**  em uma imagem, o pixel (0,0) é exibido na parte superior esquerda.

 

![ilustração de um gradiente codificado opaco](images/redsquares.png)

## <a name="span-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>Exemplo de codificação alfa de 1 bit


Esse formato é selecionado quando o inteiro de 16 bits não designado, cor\_0, for menor do que o inteiro de 16 bits não designado, cor\_1. Um exemplo de onde esse formato pode ser usado é em folhas em uma árvore, mostrado em comparação a um céu azul. Alguns texels pode ser marcados como transparentes enquanto três tons de verde ainda estão disponíveis para as folhas. Duas cores corrigem os extremos e a terceira é uma cor interpolada.

A ilustração a seguir é um exemplo dessa imagem.

![ilustração de codificação alfa de 1 bit](images/greenthing.png)

Onde a imagem aparece como branco, o texel deve ser codificado como transparente. Os componentes RGBA de texels o transparente devem ser definidos como zero antes de mesclagem.

A codificação de bitmap para as cores e a transparência é determinada usando os seguintes cálculos.

```
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

Em seguida, o bitmap se parece com o diagrama a seguir.

![diagrama do layout expandido do bitmap](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de textura compactada](compressed-texture-resources.md)

 

 




