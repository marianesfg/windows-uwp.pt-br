---
title: Compactação de bloco
description: A compactação de bloco é uma técnica de compactação de textura com perdas para reduzir o volume de memória e o tamanho da textura, dando um aumento de desempenho. Uma textura compactada em bloco pode ser menor do que uma textura com 32 bits por cor.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- Compactação de bloco
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4c959ced5ada9145ca494dd023c9aa802d7dccc2
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695022"
---
# <a name="block-compression"></a>Compactação de bloco


A compactação de bloco é uma técnica de compactação de textura com perdas para reduzir o volume de memória e o tamanho da textura, dando um aumento de desempenho. Uma textura compactada em bloco pode ser menor do que uma textura com 32 bits por cor.

A compactação de bloco é uma técnica de compactação de textura para reduzir o tamanho da textura. Quando comparado com uma textura com 32 bits por cor, uma textura compactada em bloco pode ser até 75% menor. Os apps geralmente ganham um aumento no desempenho ao usar a compactação de bloco devido o menor volume de memória.

Mesmo com perdas, a compactação de bloco funciona bem e é recomendada para todas as texturas que são transformadas e filtradas pelo pipeline. Texturas que são mapeadas diretamente na tela (elementos de interface do usuário como ícones e texto) não são uma ótima opção para compactação porque os artefatos são mais perceptíveis.

Uma textura compactada em bloco deve ser criada como um múltiplo do tamanho de 4 em todas as dimensões e não pode ser usada como uma saída do pipeline.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>Como a compactação de bloco funciona


A compactação de bloco é uma técnica para reduzir a quantidade de memória necessária para armazenar dados de cor. Armazenando algumas cores no tamanho original e outras cores usando um esquema de codificação, você pode reduzir significativamente a quantidade de memória necessária para armazenar a imagem. Como o hardware decodifica automaticamente os dados compactados, não há nenhuma penalidade de desempenho para o uso de texturas compactadas.

Para ver como funciona a compactação, examine os dois exemplos a seguir. O primeiro exemplo descreve a quantidade de memória usada para armazenar dados não compactados; o segundo exemplo descreve a quantidade de memória usada para armazenar dados compactados.

-   [Armazenando dados compactados](#storing-uncompressed-data)
-   [Armazenando dados compactados](#storing-compressed-data)

### <a name="span-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>Armazenando dados compactados

A ilustração a seguir representa uma textura de 4 × 4 não compactada. Suponha que cada cor contenha um componente único de cor (vermelho, por exemplo) e é armazenado em um byte de memória.

![uma textura de 4 x 4 não compactada](images/d3d10-block-compress-1.png)

Os dados não compactados são dispostos na memória sequencialmente e exigem 16 bytes, conforme mostrado na ilustração a seguir.

![dados não compactados na memória sequencial](images/d3d10-block-compress-2.png)

### <a name="span-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>Armazenando dados compactados

Agora que você já viu quanta memória uma imagem descompactada utiliza, dê uma olhada na quantidade de memória poupada por uma imagem compactada. O formato de compactação [BC1](#bc1) armazena 2 cores (1 byte cada) e 16 índices de 3 bits (48 bits, ou 6 bytes) que são usados para interpolar as cores originais na textura, conforme mostrado na ilustração a seguir.

![o formato de compactação bc1](images/d3d10-block-compress-3.png)

O espaço total obrigatório para armazenar os dados compactados é de 8 bytes, que é uma economia de memória de 50% em relação ao exemplo não compactado. A economia é ainda maior quando mais de um componente de cor é usado.

A economia de memória substancial proporcionada pelo compactação de bloco pode causar um aumento no desempenho. Esse desempenho impacta a qualidade da imagem (devido à interpolação de cores); no entanto, a baixa qualidade geralmente não é perceptível.

A próxima seção mostra como o Direct3D possibilita o uso de compactação de bloco em um app.

## <a name="span-idusingblockcompressionspanspan-idusingblockcompressionspanspan-idusingblockcompressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>Usando a compactação de bloco


Crie uma textura compactada em bloco da mesma forma que uma textura descompactada, exceto pelo fato de você especificar um formato compactado em bloco.

Em seguida, crie um modo de exibição para associar a textura ao pipeline. Uma vez que uma textura compactada em bloco pode ser usada apenas como uma entrada para um estágio de sombreador, convém criar um modo de exibição de recurso do sombreador.

Use uma textura compactada em bloco da mesma maneira que você usaria uma textura não compactada. Se seu aplicativo receberá um ponteiro de memória para dados compactados em bloco, você precisa levar em conta a memória preenchimento em uma minimapa que faz com que o tamanho declarado ser diferente do tamanho real.

-   [Tamanho virtual versus tamanho físico](#virtual-size-versus-physical-size)

### <a name="span-idvirtualsizespanspan-idvirtualsizespanspan-idvirtualsizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>Tamanho virtual versus tamanho físico

Se você tiver o código do app que usa um ponteiro de memória para percorrer a memória de um bloco de textura compactado, há uma consideração importante que pode exigir uma modificação no código do seu app. Uma textura compactada em bloco deve ser um múltiplo de 4 em todas as dimensões porque os algoritmos de compactação de bloco operam em blocos de texel de 4 x 4. Isso será um problema para um mipmap cujas dimensões iniciais são divisíveis por 4, mas os níveis subdivididos não são. O diagrama a seguir mostra a diferença na área entre o tamanho virtual (declarado) e o tamanho físico (real) de cada nível de mipmap.

![níveis de mipmap compactados e não compactados](images/d3d10-block-compress-pad.png)

O lado esquerdo do diagrama mostra os tamanhos de nível de mipmap que são gerados para uma textura de 60 × 40 não compactada. O tamanho de nível superior é retirado da chamada de API que gera a textura; cada nível subsequente é metade do tamanho do nível anterior. Para uma textura descompactada, não há nenhuma diferença entre o tamanho virtual (declarado) e o tamanho físico (real).

O lado direito do diagrama mostra os tamanhos de nível de mipmap que são gerados para a mesma textura de 60 × 40 com compactação. Observe que o segundo e o terceiro nível possuem preenchimento de memória para gerar os fatores de tamanho 4 em cada nível. Isso é necessário para que os algoritmos possam operar em blocos de 4 × 4 texel. Isso é evidente se você considerar os níveis de mipmap menores que 4 × 4; o tamanho desses níveis muito pequenos de mipmap será arredondado para cima para o fator mais próximo de 4 quando a memória de textura for alocada.

O hardware de amostragem usa o tamanho virtual; quando é feito uma amostragem da textura, o preenchimento de memória é ignorado. Para níveis de mipmap menores que 4 × 4, somente os quatro primeiros texels são usados para um mapa 2 x 2, e apenas o primeiro texel é usado por um bloco 1 × 1. No entanto, não há nenhuma estrutura de API que expõe o tamanho físico (incluindo o preenchimento de memória).

Resumindo, tome cuidado ao usar blocos de memória alinhados ao copiar regiões que contêm dados compactados em bloco. Para fazer isso em um app que obtém um ponteiro de memória, certifique-se de que o ponteiro usa a densidade de superfície para levar em conta o tamanho da memória física.

## <a name="span-idcompressionalgorithmsspanspan-idcompressionalgorithmsspanspan-idcompressionalgorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>Algoritmos de compactação


As técnicas de compactação de bloco no Direct3D dividem os dados de textura descompactados em blocos de 4 × 4, compactam cada bloco e armazenam os dados. Por esse motivo, as texturas a serem compactadas devem ter dimensões de textura que são múltiplas de 4.

![compactação de bloco](images/d3d10-compression-1.png)

O diagrama acima mostra uma textura particionada em blocos de texel. O primeiro bloco mostra o layout de 16 texels rotulado como a-p, mas todos os blocos tem a mesma organização dos dados.

O Direct3D implementa vários esquemas de compactação, cada um implementa um equilíbrio diferente entre os vários componentes armazenados, o número de bits por componente e a quantidade de memória consumida. Use esta tabela para ajudar a escolher o formato que funciona melhor com o tipo de dados e a resolução de dados que melhor se adapta ao seu app.

| Dados de origem                     | Resolução de compactação de dados (em bits) | Escolha esse formato de compactação |
|---------------------------------|---------------------------------------|--------------------------------|
| Alfa e cor de três componentes | Cor (5:6:5), alfa (1) ou nenhum alfa  | [BC1](#bc1)                    |
| Alfa e cor de três componentes | Cor (5:6:5), alfa (4)              | [BC2](#bc2)                    |
| Alfa e cor de três componentes | Cor (5:6:5), alfa (8)              | [BC3](#bc3)                    |
| Cor de um componente             | Um componente (8)                     | [BC4](#bc4)                    |
| Cor de dois componentes             | Dois componentes (8:8)                  | [BC5](#bc5)                    |

 

-   [BC1](#bc1)
-   [BC2](#bc2)
-   [BC3](#bc3)
-   [BC4](#bc4)
-   [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

Use o primeiro formato de compactação de bloco (BC1) (DXGI\_FORMAT\_BC1\_TYPELESS, DXGI\_FORMAT\_BC1\_UNORM, ou DXGI\_BC1\_UNORM\_SRGB) para armazenar dados de cor de três componentes usando cor 5:6:5 (5 bits de vermelho, 6 bits de verde, e 5 bits de azul). Isso vale até mesmo quando os dados também contêm alfa de 1 bit. Pressupondo uma textura de 4 × 4 usando o formato de dados maior possível, o formato BC1 reduz a memória necessária de 48 bytes (16 cores × 3 componentes/cor × 1 byte/componente) para 8 bytes de memória.

O algoritmo funciona em blocos de texels de 4 × 4. Em vez de armazenar 16 cores, o algoritmo salva 2 cores de referência (color\_0 e color\_1) e 16 índices de cor 2 bits (blocos a–p), conforme mostrado no diagrama a seguir.

![o layout de compactação bc1](images/d3d10-compression-bc1.png)

Os índices de cor (a–p) são usados para procurar as cores originais em uma tabela de cores. A tabela de cores contém 4 cores. As duas primeiras cores, color\_0 e color\_1, são as cores mínima e máxima. As outras duas cores, color\_2 e color\_3, são cores intermediárias calculadas com uma interpolação linear.

```
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

As quatro cores recebem valores de índice de 2 bits que serão salvos em blocos a–p.

```
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

Por fim, todas as cores nos blocos a–p são comparadas com as quatro cores na tabela de cores e o índice para a cor mais próxima é armazenado nos blocos de 2 bits.

Esse algoritmo se compromete a dados que também contêm alfa de 1 bit. A única diferença é que color\_3 é definida como 0 (que representa uma cor transparente) e color\_2 é uma mistura linear de color\_0 e color\_1.

```
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

Use o formato BC2 (DXGI\_FORMAT\_BC2\_TYPELESS, DXGI\_FORMAT\_BC2\_UNORM, ou DXGI\_BC2\_UNORM\_SRGB) para armazenar dados que contêm dados de cor e alfa com baixa coerência (use [BC3](#bc3) para dados alfa altamente coerentes). O formato BC2 armazena dados RGB como uma cor 5:6:5 (5 bits de vermelho, 6 bits de verde, 5 bits de azul) e alfa como um valor de 4 bits separado. Pressupondo uma textura de 4 × 4 usando o formato de dados maior possível, essa técnica de compactação reduz a memória necessária de 64 bytes (16 cores × 4 componentes/cor × 1 byte/componente) para 16 bytes de memória.

O formato BC2 armazena cores com o mesmo número de bits e dados de layout como o formato [BC1](#bc1), no entanto, o BC2 requer 64-bits adicionais de memória para armazenar os dados alfa, conforme mostrado no diagrama a seguir.

![o layout de compactação bc2](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

Use o formato BC3 (DXGI\_FORMAT\_BC3\_TYPELESS, DXGI\_FORMAT\_BC3\_UNORM, ou DXGI\_BC3\_UNORM\_SRGB) para armazenar dados de cores altamente coerentes (use [BC2](#bc2) com dados alfa menos coerentes). O formato BC3 armazena dados de cor usando cor 5:6:5 (5 bits de vermelho, 6 bits de verde, 5 bits de azul) e dados alfa usando um byte. Pressupondo uma textura de 4 × 4 usando o formato de dados maior possível, essa técnica de compactação reduz a memória necessária de 64 bytes (16 cores × 4 componentes/cor × 1 byte/componente) para 16 bytes de memória.

O formato BC3 armazena cores com o mesmo número de bits e layout de dados que o formato [BC1](#bc1), no entanto, o BC3 requer 64 bits adicionais de memória para armazenar os dados alfa. O formato BC3 manipula o alfa armazenando dois valores de referência e interpolando entre eles (da mesma forma como o BC1 armazena cor RGB).

O algoritmo funciona em blocos de texels de 4 × 4. Em vez de armazenar 16 valores alfa, o algoritmo armazena 2 alfas de referência (alpha\_0 e alpha\_1) e 16 índices de cor de 3 bits (alfa a-p), conforme mostrado no diagrama a seguir.

![o layout de compactação bc3](images/d3d10-compression-bc3.png)

O formato BC3 usa os índices de alfa (a–p) para procurar as cores originais em uma tabela de pesquisa que contém 8 valores. Os dois primeiros valores, alpha\_0 e alpha\_1, são os valores mínimo e máximo, os outros seis valores intermediários são calculados usando interpolação linear.

O algoritmo determina o número de valores alfa interpolados examinando os dois valores alfa de referência. Se alpha\_0 for maior que alpha\_1, o BC3 interpolará 6 valores alfa, caso contrário, ele interpolará 4. Quando o BC3 interpola apenas 4 valores alfa, ele define dois valores alfa adicionais (0 para totalmente transparente e 255 para totalmente opaco). O BC3 compacta os valores alfabéticos na área de texel de 4 × 4, armazenando o código de bit correspondente aos valores alfa interpolados que melhor correspondem ao alfa original para um determinado texel.

```
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>BC4

Use o formato BC4 para armazenar dados de cor de um componente usando 8 bits para cada cor. Como resultado da precisão maior (em comparação com [BC1](#bc1)), o BC4 é ideais para armazenar dados de ponto flutuante no intervalo de \[0 a 1\] usando o formato DXGI\_FORMAT\_BC4\_UNORM e \[-1 a + 1\] usando o formato de DXGI\_FORMAT\_BC4\_SNORM. Pressupondo uma textura de 4 × 4 usando o formato de dados maior possível, essa técnica de compactação reduz a memória necessária de 16 bytes (16 cores × 1 componentes/cor × 1 byte/componente) para 8 bytes.

O algoritmo funciona em blocos de texels de 4 × 4. Em vez de armazenar 16 cores, o algoritmo armazena 2 cores de referência (red\_0 e red\_1) e 16 índices de cor de 3 bits (vermelho a até vermelho p), conforme mostrado no diagrama a seguir.

![o layout de compactação bc4](images/d3d10-compression-bc4.png)

O algoritmo usa os índices de 3 bits para procurar as cores em uma tabela de cores que contém 8 cores. As duas primeiras cores, red\_0 e red\_1, são as cores mínima e máxima. O algoritmo calcula as cores restantes usando interpolação linear.

O algoritmo determina o número de valores de cor interpolados examinando os dois valores de referência. Se red\_0 for maior que red\_1, o BC4 interpolará 6 valores de cor, caso contrário, ele interpolará 4. Quando o BC4 interpola apenas 4 valores de cor, ele define dois valores de cor adicional (0.0f para totalmente transparente e 1.0f para totalmente opaco). O BC4 compacta os valores alfabéticos na área de texel de 4 × 4, armazenando o código de bit correspondente aos valores alfa interpolados que melhor correspondem ao alfa original para um determinado texel.

-   [BC4\_UNORM](#bc4-unorm)
-   [BC4\_SNORM](#bc4-snorm)

### <a name="span-idbc4unormspanspan-idbc4unormspanspan-idbc4-unormspanbc4unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>BC4\_UNORM

A interpolação dos dados de componente único é feita como no exemplo de código a seguir.

```
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

As cores de referência recebem índices de 3 bits (000 – 111, uma vez que há 8 valores), que serão salvos em blocos na cor vermelha a até a cor vermelha p durante a compactação.

### <a name="span-idbc4snormspanspan-idbc4snormspanspan-idbc4-snormspanbc4snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>BC4\_SNORM

O DXGI\_FORMAT\_BC4\_SNORM é exatamente o mesmo, exceto que os dados são codificados no intervalo SNORM e quando 4 valores de cor são interpolados. A interpolação dos dados de componente único é feita como no exemplo de código a seguir.

```
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                     // bit code 110
  red_7 =  1.0f;                     // bit code 111
}
```

As cores de referência recebem índices de 3 bits (000 – 111, uma vez que há 8 valores), que serão salvos em blocos na cor vermelha a até a cor vermelha p durante a compactação.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

Use o formato BC5 para armazenar dados de cor de dois componentes usando 8 bits para cada cor. Como resultado da precisão maior (em comparação com [BC1](#bc1)), o BC5 é ideais para armazenar dados de ponto flutuante no intervalo de \[0 a 1\] usando o formato DXGI\_FORMAT\_BC5\_UNORM e \[-1 a + 1\] usando o formato de DXGI\_FORMAT\_BC5\_SNORM. Pressupondo uma textura de 4 × 4 usando o formato de dados maior possível, essa técnica de compactação reduz a memória necessária de 32 bytes (16 cores × 2 componentes/cor × 1 byte/componente) para 16 bytes.

-   [BC5\_UNORM](#bc5-unorm)
-   [BC5\_SNORM](#bc5-snorm)

O algoritmo funciona em blocos de texels de 4 × 4. Em vez de armazenar 16 cores para ambos os componentes, o algoritmo armazena 2 cores de referência para cada componente (red\_0, red\_1, green\_0 e green\_1) e 16 índices de cor de 3 bits para cada componente (vermelho a até vermelho p, e verde a até verde p), conforme mostrado no diagrama a seguir.

![o layout de compactação bc5](images/d3d10-compression-bc5.png)

O algoritmo usa os índices de 3 bits para procurar as cores em uma tabela de cores que contém 8 cores. As duas primeiras cores, red\_0 e red\_1 (ou green\_0 e green\_1), são as cores mínima e máxima. O algoritmo calcula as cores restantes usando interpolação linear.

O algoritmo determina o número de valores de cor interpolados examinando os dois valores de referência. Se red\_0 for maior que red\_1, o BC5 interpolará 6 valores de cor, caso contrário, ele interpolará 4. Quando o BC5 interpola apenas 4 valores de cor, ele define os dois valores de cor restantes em 0.0f e 1.0f.

### <a name="span-idbc5unormspanspan-idbc5unormspanspan-idbc5-unormspanbc5unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5\_UNORM

A interpolação dos dados de componente único é feita como no exemplo de código a seguir. Os cálculos para os componentes verdes são semelhantes.

```
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

As cores de referência recebem índices de 3 bits (000 – 111, uma vez que há 8 valores), que serão salvos em blocos na cor vermelha a até a cor vermelha p durante a compactação.

### <a name="span-idbc5snormspanspan-idbc5snormspanspan-idbc5-snormspanbc5snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5\_SNORM

O DXGI\_FORMAT\_BC5\_SNORM é exatamente o mesmo, exceto que os dados são codificados no intervalo SNORM e quando 4 valores de dados são interpolados, os dois valores adicionais são -1.0f e 1.0f. A interpolação dos dados de componente único é feita como no exemplo de código a seguir. Os cálculos para os componentes verdes são semelhantes.

```
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

As cores de referência recebem índices de 3 bits (000 – 111, uma vez que há 8 valores), que serão salvos em blocos na cor vermelha a até a cor vermelha p durante a compactação.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>Conversão de formato


O Direct3D permite cópias entre texturas pré-estruturadas e texturas compactadas por bloco com as mesmas larguras de bit.

Você pode copiar recursos entre alguns tipos de formato. Esse tipo de operação de cópia executa um tipo de conversão de formato que reinterpreta os dados de recursos como um tipo de formato diferente. Considere este exemplo que mostra a diferença entre a reinterpretação de dados com o comportamento de um tipo mais comum de conversão:

```
FLOAT32 f = 1.0f;
UINT32 u;
```

Para reinterpretar 'f' como o tipo de 'u', use [memcpy](http://msdn.microsoft.com/library/dswaw1wk.aspx):

```
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

Na reinterpretação anterior, o valor subjacente dos dados não muda; [memcpy](http://msdn.microsoft.com/library/dswaw1wk.aspx) reinterpreta o flutuante como um inteiro sem sinal.

Para executar o tipo mais comum de conversão, use a atribuição:

```
u = f; // 'u' becomes 1.
```

Na conversão anterior, o valor subjacente dos dados muda.

A tabela a seguir lista os formatos de destino e origem permitidos que você pode usar nesse tipo de reinterpretação de conversão de formatos. Você deve codificar os valores corretamente para que a reinterpretação funcione conforme esperado.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Largura de bit</th>
<th align="left">Recurso não compactado</th>
<th align="left">Recurso compactado em bloco</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de textura compactada](compressed-texture-resources.md)
