---
title: Formato BC7
description: O formato BC7 é um formato de compactação de textura usado para compactação de alta qualidade de dados de RGB e RGBA.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- Formato BC7
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 70380dd0bd07cfe0c81e8339f8606029663b47d4
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6673039"
---
# <a name="bc7-format"></a>Formato BC7


O formato BC7 é um formato de compactação de textura usado para compactação de alta qualidade de dados de RGB e RGBA.

Para obter informações sobre os modos de bloco do formato BC7, consulte [Referência do Modo de Formato BC7](https://msdn.microsoft.com/library/windows/desktop/hh308954).

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgiformatbc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>Sobre BC7/DXGI\_FORMAT\_BC7


BC7 é especificado pelos seguintes valores de enumeração DXGI\_FORMAT:

-   **DXGI\_FORMAT\_BC7\_TYPELESS**.
-   **DXGI\_FORMAT\_BC7\_UNORM**.
-   **DXGI\_FORMAT\_BC7\_UNORM\_SRGB**.

O formato BC7 pode ser usado para o recursos de textura [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (incluindo matrizes), Texture3D ou TextureCube (incluindo matrizes). Da mesma forma, esse formato se aplica a qualquer superfície de MIP-map associada a esses recursos.

O BC7 usa um tamanho de bloco fixo de 16 bytes (128 bits) e um tamanho de bloco fixo de 4 x 4 texels. Como com os formatos BC anteriores, as imagens de textura maiores que o tamanho de bloco com suporte (4 x 4) são compactadas usando vários blocos. Essa identidade endereçamento se aplica também a imagens tridimensionais, mapas MIP, mapas de cubo e matrizes de textura. Todos os blocos de imagem devem ser do mesmo formato.

O BC7 compacta imagens de dados de ponto fixo de três canais (RGB) e quatro canais (RGBA). Normalmente, os dados de origem tem 8 bits por componente de cor (canal), embora o formato seja capaz de codificar dados de origem com maior quantidade de bits por componente de cor. Todos os blocos de imagem devem ser do mesmo formato.

O decodificador BC7 executa descompactação antes da filtragem de textura ser aplicada.

O hardware de descompactação BC7 deve ter precisão de bit, ou seja, o hardware deve retornar resultados que são idênticos aos resultados retornados pelo decodificador descrito neste documento.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>Implementação do BC7


Uma implementação de BC7 pode especificar um dos 8 modos, com o modo especificado no bit menos significativo do bloco de 16 bytes (128 bits). O modo é codificado por zero ou mais bits com um valor de 0 seguido por um 1.

Um bloco BC7 pode conter vários pares de ponto de extremidade. O conjunto de índices que corresponde a um par de ponto de extremidade pode ser chamado de "subconjunto". Além disso, em alguns modos de bloco, a representação de ponto de extremidade é codificada em um formato que pode ser chamado de "RGBP", onde o bit "P" representa um bit menos significativo compartilhado para os componentes de cor do ponto de extremidade. Por exemplo, se a representação de ponto de extremidade para o formato for "RGB 5.5.5.1", então o ponto de extremidade é interpretado como um valor RGB 6.6.6, onde o estado do bit P define o bit menos significativo de cada componente. Da mesma forma, para os dados de origem com um canal alfa, se a representação para o formato for "RGBAP 5.5.5.5.1", então o ponto de extremidade será interpretados como RGBA 6.6.6.6. Dependendo do modo de bloco, você pode especificar o bit menos significativo compartilhado para um ambos os pontos de extremidade de um subconjunto individualmente (2 bits P por subconjunto), ou compartilhados entre os pontos de extremidade de um subconjunto (1 bit P por subconjunto).

Para os blocos de BC7 que não codificam explicitamente o componente alfa, um bloco BC7 consiste em bits de modo, bits de partição, pontos de extremidade compactados, índices compactados e um bit P opcional. Nesses blocos, os pontos de extremidade têm uma representação somente RGB e o componente alfa é decodificado como 1.0 para todos os texels nos dados de origem.

Para os blocos de BC7 que tem componentes de cor e alfa combinados, um bloco consiste em bits de modo, pontos de extremidade compactados, índices compactados, bits de partição opcionais e um bit P. Nesses blocos, as cores de ponto de extremidade são expressas em formato RGBA e os valores de componente alfa são interpolados junto com os valores de componente de cor.

Para os blocos BC7 que têm componentes de cor e alfa separados, um bloco consiste em bits de modo, bits de rotação, pontos de extremidade compactados, índices compactados, e um bit seletor de índice opcional. Esses blocos têm um vetor RGB eficaz \[R, G, B\] e um canal alfa escalar \[A\] codificado separadamente.

A tabela a seguir lista os componentes de cada tipo de bloco.

| O bloco BC7 contém...     | bits de modo | bits de rotação | bits de seletor de índice | bits de partição | pontos de extremidade compactados | bit P    | índices compactados |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| somente os componentes de cor     | obrigatórios  | N/A           | N/A                | obrigatórios       | obrigatórios             | opcional | obrigatórios           |
| cor + alfa combinado    | obrigatórios  | N/A           | N/A                | opcional       | obrigatórios             | opcional | obrigatórios           |
| cor e alfa separados | obrigatórios  | obrigatórios      | opcional           | N/A            | obrigatórios             | N/A      | obrigatórios           |

 

O BC7 define uma paleta de cores em uma linha aproximada entre dois pontos de extremidade. O valor de modo determina o número de pares de ponto de extremidade de interpolação por bloco. O BC7 armazena um índice de paleta por texel.

Para cada subconjunto de índices que corresponde a um par de pontos de extremidade, o codificador corrige o estado de um bit dos dados de índice compactado para esse subconjunto. Ele faz isso, escolhendo uma ordem de ponto de extremidade que permite que o índice do índice de "correção" designado defina o bit mais significativo como 0, e que pode então ser descartada, salvando um bit por subconjunto. Para os modos de bloco com apenas um subconjunto único, o índice de correção será sempre o índice 0.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>Decodificar o formato BC7


O pseudocódigo a seguir mostra as etapas para descompactar o pixel em (x, y) que recebe o bloco de 16 bytes BC7

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

O pseudocódigo a seguir descreve as etapas para decodificar totalmente os componentes de cor e alfa do ponto de extremidade para cada subconjunto dado um bloco de BC7 16 bytes.

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

Para gerar cada componente interpolado para cada subconjunto, use o seguinte algoritmo: deixe "c" ser o componente a ser gerado; deixe "e0" ser o componente de ponto de extremidade 0 do subconjunto; e deixe "e1" ser o componente de ponto de extremidade 1 do subconjunto.

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

O seguinte pseudocódigo ilustra como extrair índices e contagens de bit de componentes de cor e alfa. Os blocos com cor e alfa separados também têm dois conjuntos de dados de índice: um para o canal de vetor e outro para o canal escalar. Para o modo de 4, esses índices são de larguras diferentes (2 ou 3 bits), e há um seletor de um bit que especifica se o vetor ou dados escalares usam os índices de 3 bits. (A extração da contagem de bits alfa é semelhante à extração de contagem de bits de cor, mas com comportamento inverso com base no bit **idxMode**.)

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>Referência de modo de formato BC7


Esta seção contém uma lista de 8 modos de bloco e alocações de bit para os blocos de formato de compactação de textura BC7.

As cores para cada subconjunto dentro de um bloco são representadas por duas cores de ponto de extremidade explícitas e um conjunto de cores interpoladas entre elas. Dependendo da precisão de índice do bloco, cada subconjunto pode ter 4, 8 ou 16 cores possíveis.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>Modo 0

O modo 0 do BC7 tem as seguintes características:

-   Componentes de cor apenas (nenhum alfa)
-   3 subconjuntos por bloco
-   Pontos de extremidade RGBP 4.4.4.1 com um bit P exclusivo por ponto de extremidade
-   índices de 3 bits
-   16 partições

![Layout de bit do modo 0](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>Modo 1

O modo 1 do BC7 tem as seguintes características:

-   Componentes de cor apenas (nenhum alfa)
-   2 subconjuntos por bloco
-   Pontos de extremidade RGBP 6.6.6.1 com um bit P compartilhado por subconjunto)
-   índices de 3 bits
-   64 partições

![Layout de bit do modo 1](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>Modo 2

O modo 2 do BC7 tem as seguintes características:

-   Componentes de cor apenas (nenhum alfa)
-   3 subconjuntos por bloco
-   Pontos de extremidade RGB 5.5.5
-   índices de 2 bits
-   64 partições

![Layout de bit do modo 2](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>Modo 3

O modo 3 do BC7 tem as seguintes características:

-   Componentes de cor apenas (nenhum alfa)
-   2 subconjuntos por bloco
-   Pontos de extremidade RGBP 7.7.7.1 com um bit P único por subconjunto)
-   índices de 2 bits
-   64 partições

![Layout de bit do modo 3](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>Modo 4

O modo 4 do BC7 tem as seguintes características:

-   Componentes de cor com o componente alfa separado
-   1 subconjunto por bloco
-   pontos de extremidade de cor RGB 5.5.5
-   pontos de extremidade alfa 6 bits
-   índices de 16 x 2 bits
-   índices de 16 x 3 bits
-   rotação de componente de 2 bits
-   seletor de índice de 1 bit (se os índices de 2 ou 3 bits forem usados)

![layout de bit do modo 4](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>Modo 5

O modo 5 do BC7 tem as seguintes características:

-   Componentes de cor com o componente alfa separado
-   1 subconjunto por bloco
-   pontos de extremidade de cor RGB 7.7.7
-   pontos de extremidade alfa 6 bits
-   índices de cores de 16 x 2 bits
-   índices alfa de 16 x 2 bits
-   rotação de componente de 2 bits

![Layout de bit do modo 5](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>Modo 6

O modo 6 do BC7 tem as seguintes características:

-   Componentes de cor e alfa combinados
-   Um subconjunto por bloco
-   Pontos de extremidade de cor RGBAP 7.7.7.7.1 (e alfa) (bit P exclusivo por ponto de extremidade)
-   índices de 16 x 4 bits

![layout de bit do modo 6](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>Modo 7

O modo 7 do BC7 tem as seguintes características:

-   Componentes de cor e alfa combinados
-   2 subconjuntos por bloco
-   Pontos de extremidade de cor RGBAP 5.5.5.5.1 (e alfa) (bit P exclusivo por ponto de extremidade)
-   índices de 2 bits
-   64 partições

![layout de bit do modo 7](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Comentários

O modo 8 (o byte menos significativo está definido como 0x00) é reservado. Não use-o em seu codificador. Se você passar esse modo para o hardware, será retornado um bloco inicializado para todos os zeros.

No BC7, você pode codificar o componente alfa de uma das seguintes maneiras:

-   Tipos de bloco sem codificação explícita de componente alfa. Nesses blocos, os pontos de extremidade de cor têm uma codificação somente RGB, com o componente alfa decodificado em 1.0 para todos os texels.
-   Os tipos de bloco com componentes de cor e alfa combinados. Nesses blocos, os valores de cor de ponto de extremidade são especificados no formato RGBA, e os valores de componente alfa são interpolados juntamente com os valores de cor.
-   Os tipos de bloco com componentes de cor e alfa separados. Nesses blocos, os valores de cor e alfa são especificados separadamente, cada um com seu próprio conjunto de índices. Como resultado, eles têm um vetor efetivação e um canal escalar codificado separadamente, onde o vetor comumente especifica os canais de cor \[R, G, B\] e o escalar especifica o alfa canal \[A\]. Para dar suporte a essa abordagem, um campo de 2 bits separado é fornecido na codificação, que permite a especificação da codificação de canal separado como um valor escalar. Como resultado, o bloco pode ter uma das quatro representações diferentes a seguir dessa codificação alfa (conforme indicado pelo campo de 2 bits):
    -   RGB|A: canal alfa separado
    -   AGB|R: canal de cor "vermelha" separado
    -   RAB|G: canal de cor "verde" separado
    -   RGA|B: canal de cor "azul" separado

    O decodificador reordena a ordem de canal para RGBA após a decodificação, portanto, o formato de bloco interno é invisível para o desenvolvedor. Tons de preto com cor separada e componentes alfa também têm dois conjuntos de dados de índice: um para o conjunto em vetor de canais e outro para o canal escalar. (No caso de modo 4, esses índices possuem larguras diferentes \[2 ou 3 bits\]. O modo 4 também contém um seletor de 1 bit que especifica se o vetor ou o canal escalar usa os índices de 3 bits.)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Compactação de bloco de textura](texture-block-compression.md)

 

 




