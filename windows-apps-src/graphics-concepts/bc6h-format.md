---
title: Formato BC6H
description: O formato BC6H é um formato de compactação de textura projetado para dar suporte a espaços de cores de intervalo altamente dinâmico (HDR) nos dados de origem.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- Formato BC6H
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: be88f06cd5893f2f67697a54754826440bdf7d18
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5989689"
---
# <a name="bc6h-format"></a>Formato BC6H


O formato BC6H é um formato de compactação de textura projetado para dar suporte a espaços de cores de intervalo altamente dinâmico (HDR) nos dados de origem.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>Sobre BC6H/DXGI\_FORMAT\_BC6H


O formato BC6H fornece compactação de alta qualidade para imagens que usam canais de cor HDR, com um valor de 16 bits para cada canal de cor do valor (16: 16:16). Não há nenhum suporte para um canal alfa.

BC6H é especificado pelos seguintes valores de enumeração DXGI\_FORMAT:

-   **DXGI\_FORMAT\_BC6H\_TYPELESS**.
-   **DXGI\_FORMAT\_BC6H\_UF16**. Esse formato BC6H não usa um bit de sinal nos valores de canal de cor da ponto flutuante de 16 bits.
-   **DXGI\_FORMAT\_BC6H\_SF16**. Esse formato BC6H usa um bit de sinal nos valores de canal de cor da ponto flutuante de 16 bits.

**Observação**  o flutuante de 16 formato de ponto para os canais de cor é geralmente conhecido como um "parcial" ponto flutuante formato. Esse formato tem o seguinte layout de bit:
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (float não assinado) | 5 bits de expoente + 11 bits de mantissa              |
| SF16 (float assinado)   | 1 bit de sinal + 5 bits de expoente + 10 bits de mantissa |

 

 

O formato BC6H pode ser usado para o recursos de textura [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (incluindo matrizes), Texture3D ou TextureCube (incluindo matrizes). Da mesma forma, esse formato se aplica a qualquer superfície de MIP-map associada a esses recursos.

O BC6H usa um tamanho de bloco fixo de 16 bytes (128 bits) e um tamanho de bloco fixo de 4 x 4 texels. Como com os formatos BC anteriores, as imagens de textura maiores que o tamanho de bloco com suporte (4 x 4) são compactadas usando vários blocos. Essa identidade endereçamento se aplica também a imagens tridimensionais, mapas MIP, mapas de cubo e matrizes de textura. Todos os blocos de imagem devem ser do mesmo formato.

Advertências com o formato BC6H:

-   O BC6H suporta a denormalization de ponto flutuante, mas não suporta INF (infinito) e NaN (não numérico). A exceção é o modo assinado de BC6H (DXGI\_FORMAT\_BC6H\_SF16), que dá suporte a -INF (infinito negativo). Esse suporte para -INF é simplesmente um artefato do formato em si e não é especificamente permitido pela codificadores para esse formato. Em geral, quando um codificador encontra dados de sinal INF (positivo ou negativo) ou NaN, o codificador deve converter os dados para o valor de representação não INF máximo permitido, e mapear NaN como 0 antes da compactação.
-   O BC6H não oferece suporte a um canal alfa.
-   O decodificador BC6H executa descompactação antes de realizar a filtragem de textura.
-   A descompactação do BC6H deve ter precisão de bit, ou seja, o hardware deve retornar resultados que são idênticos ao decodificador descrito nesta documentação.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>Implementação do BC6H


Um bloco de BC6H consiste em bits de modo, pontos de extremidade compactados, índices compactados e um índice de partição opcional. Esse formato especifica 14 modos diferentes.

Uma cor de ponto de extremidade é armazenada como um trio RGB. O BC6H define uma paleta de cores em uma linha aproximada em um número de pontos de extremidade de cores definidos. Além disso, dependendo do modo, um bloco pode ser dividido em duas regiões ou tratado como uma única região, onde um bloco de duas regiões tem um conjunto separado de pontos de extremidade de cor para cada região. O BC6H armazena um índice de paleta por texel.

No caso de duas regiões, há 32 partições possíveis.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>Decodificar o formato BC6H


O pseudocódigo a seguir mostra as etapas para descompactar o pixel em (x, y) que recebe o bloco de 16 bytes BC6H.

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

A tabela a seguir contém os valores e a contagem de bit de cada um dos 14 formatos possíveis para blocos de BC6H.

| Modo | Índices de partição | Partition | Pontos de extremidade de cor                  | Bits de modo      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 bits           | 5 bits    | 75 bits (10.555, 10.555, 10.555) | 2 bits (00)    |
| 2    | 46 bits           | 5 bits    | 75 bits (7666, 7666, 7666)       | 2 bits (01)    |
| 3    | 46 bits           | 5 bits    | 72 bits (11.555, 11.444, 11.444) | 5 bits (00010) |
| 4    | 46 bits           | 5 bits    | 72 bits (11.444, 11.555, 11.444) | 5 bits (00110) |
| 5    | 46 bits           | 5 bits    | 72 bits (11.444, 11.444, 11.555) | 5 bits (01010) |
| 6    | 46 bits           | 5 bits    | 72 bits (9555, 9555, 9555)       | 5 bits (01110) |
| 7    | 46 bits           | 5 bits    | 72 bits (8666, 8555, 8555)       | 5 bits (10010) |
| 8    | 46 bits           | 5 bits    | 72 bits (8555, 8666, 8555)       | 5 bits (10110) |
| 9    | 46 bits           | 5 bits    | 72 bits (8555, 8555, 8666)       | 5 bits (11010) |
| 10   | 46 bits           | 5 bits    | 72 bits (6666, 6666, 6666)       | 5 bits (11110) |
| 11   | 63 bits           | 0 bits    | 60 bits (10.10, 10.10, 10.10)    | 5 bits (00011) |
| 12   | 63 bits           | 0 bits    | 60 bits (11.9, 11.9, 11.9)       | 5 bits (00111) |
| 13   | 63 bits           | 0 bits    | 60 bits (12.8, 12.8, 12.8)       | 5 bits (01011) |
| 14   | 63 bits           | 0 bits    | 60 bits (16.4, 16.4, 16.4)       | 5 bits (01111) |

 

Cada formato nesta tabela pode ser identificado exclusivamente pelos bits de modo. Os dez primeiros modos são usados para blocos de duas regiões e o campo de bit de modo pode ter dois ou cinco bits de comprimento. Esses blocos também têm campos para os pontos de extremidade de cor compactados (72 ou 75 bits), a partição (5 bits) e os índices de partição (46 bits).

Para os pontos de extremidade de cor compactados, os valores na tabela anterior denotam a precisão dos pontos de extremidade armazenados RGB e o número de bits usados para cada valor de cor. Por exemplo, o modo 3 especifica um nível de precisão de ponto de extremidade de cor de valor 11, e o número de bits usados para armazenar os valores delta dos pontos de extremidade transformados para as cores vermelho, azul e verde (5, 4 e 4 respectivamente). O modo 10 não usa compactação delta e armazena em vez disso todos os pontos de extremidade de cor explicitamente.

Os últimos quatro modos de bloco são usados para blocos de uma região, onde o campo de modo é 5 bits. Esses blocos têm campos para os pontos de extremidade (60 bits) e os índices compactados (63 bits). O modo 11 (assim como o modo 10) não usa compactação delta e armazena em vez disso ambos os pontos de extremidade de cor explicitamente.

Os modos 10011, 10111, 11011 e 11111 (não mostrados) são reservados. Não use-os em seu codificador. Se blocos forem passados ao hardware com um desses modos especificados, o bloco descompactado resultante deverá conter todos os zeros em todos os canais, exceto para o canal alfa.

Para o BC6H, o canal alfa sempre deve retornar 1.0, independentemente do modo.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>Conjunto de partições BC6H

Há 32 conjuntos de partição possíveis para um bloco de duas regiões, e que estão definidos na tabela a seguir. Cada bloco de 4 x 4 representa uma única forma.

![tabela dos conjuntos de partição bc6h](images/bc6h-partition-sets.png)

Nesta tabela de conjuntos de partição, a entrada em negrito e sublinhada é o local do índice correção para o subconjunto 1 (que é especificado com um bit a menos). O índice de correção de subconjunto 0 é sempre o índice 0, uma vez que o particionamento é sempre organizado de tal forma que o índice 0 esteja sempre o subconjunto 0. A ordem de partição vai do canto superior esquerdo para o canto inferior direito e, em seguida, move-se da esquerda para a direita e de cima para baixo.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>Formato de ponto de extremidade compactado BC6H


![campos de bit para formatos de ponto de extremidade bc6h compactado](images/bc6h-headers-med.png)

Esta tabela mostra os campos de bit para os pontos de extremidade compactados como uma função de formato de ponto de extremidade, com cada coluna especificando uma codificação e cada linha especificando um campo de bit. Essa abordagem ocupa 82 bits para blocos de duas regiões e 65 bits para blocos de uma região. Por exemplo, os primeiros 5 bits para a codificação de uma região \[16 4\] acima (especificamente, a coluna mais à direita) são bits m\[4:0\], os próximos 10 bits são bits rw\[9:0\] e assim por diante com os últimos bits 6 contendo bw\[10:15\].

Os nomes dos campos na tabela acima são definidos da seguinte maneira:

| Campo | Variável          |
|-------|-------------------|
| m     | modo              |
| d.     | índice de forma       |
| rw    | endpt\[0\].A\[0\] |
| rx    | endpt\[0\].B\[0\] |
| ry    | endpt\[1\].A\[0\] |
| rz    | endpt\[1\].B\[0\] |
| gw    | endpt\[0\].A\[1\] |
| gx    | endpt\[0\].B\[1\] |
| gy    | endpt\[1\].A\[1\] |
| gz    | endpt\[1\].B\[1\] |
| bw    | endpt\[0\].A\[2\] |
| bx    | endpt\[0\].B\[2\] |
| by    | endpt\[1\].A\[2\] |
| bz    | endpt\[1\].B\[2\] |

 

Endpt\[i\], onde i é 0 ou 1, refere-se ao conjunto de pontos de extremidade de número 0 ou 1 respectivamente.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>Extensão de sinal para valores de ponto de extremidade


Para os blocos de duas regiões, há quatro valores de ponto de extremidade que pode ter seu sinal estendido. Endpt\[0\].A é assinado somente se o formato for um formato assinado; os outros pontos de extremidade são assinados somente se o ponto de extremidade foi transformado, ou se o formato é um formato assinado. O código a seguir demonstra o algoritmo para estender o sinal dos valores de ponto de extremidade de duas regiões.

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

Para blocos de uma região, o comportamento é o mesmo, apenas com endpt\[1\] removido.

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>Transformar inversão para valores de ponto de extremidade


Para os blocos de duas regiões, a transformação aplica o inverso da codificação de diferença, adicionando o valor base em endpt\[0\].A a três outras entradas para um total de 9 operações de adição. Na imagem abaixo, o valor base é representado como "A0" e tem a maior precisão de ponto flutuante. "A1", "B0" e "B1" são todos os deltas calculados a partir do valor de âncora, e esses valores delta são representados com precisão mais baixa. (A0 corresponde ao endpt\[0\].A, B0 corresponde ao endpt\[0\].B, A1 corresponde ao endpt\[1\].A e B1 corresponde ao endpt\[1\].B.)

![cálculo dos valores de ponto de extremidade de inversão de transformação](images/bc6h-transform-inverse.png)

Para blocos de uma região, há apenas um deslocamento delta e, portanto, apenas 3 operações de adição.

O decompressor deve garantir que os resultados da transformação inversa não irão exceder a precisão de endpt\[0\].a. No caso de um estouro, os valores resultantes da transformação inversa devem ser encapsulados dentro do mesmo número de bits. Se a precisão de A0 for "p" bits, o algoritmo de transformação é:

`B0 = (B0 + A0) & ((1 << p) - 1)`

Para formatos assinados, os resultados do cálculo do delta também devem ter seu sinal estendido. Se a operação de extensão do sinal considerar estender ambos os sinais, onde 0 é positivo e 1 é negativo, a extensão de sinal de 0 se encarregará da vinculação acima. De maneira equivalente, após a vinculação acima, apenas um valor de 1 (negativo) precisa ter o sinal estendido.

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Desquantização de pontos de extremidade de cor


Considerando os pontos de extremidade não compactados, a próxima etapa é executar uma desquantização inicial dos pontos de extremidade de cor. Isso envolve três etapas:

-   Um desquantização de paletas de cores
-   Interpolação de paletas
-   Finalização da desquantização

Dividir o processo de desquantização em duas partes (desquantização da paleta de cores antes da interpolação e desquantização final após a interpolação) reduz o número de operações de multiplicação necessárias quando comparado a um processo completo de desquantização antes da interpolação de paleta.

O código a seguir ilustra o processo para recuperar estimativas dos valores de cor de 16 bits original e, em seguida, usando os valores de peso fornecidos para adicionar 6 valores de cor adicional à paleta. A mesma operação é executada em cada canal.

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

O exemplo de código a seguir demonstra o processo de interpolação, com as seguintes observações:

-   Como a gama completa de valores de cor para a função **unquantize** (abaixo) fica entre -32768 e 65535, o interpolador é implementado usando aritmética assinada de 17 bits.
-   Após a interpolação, os valores são passados para a função **finish\_unquantize** (descrita no terceiro exemplo nesta seção), que aplica a escala final.
-   Todos os descompactadores de hardware são obrigatórios para retornar resultados precisos de bit com essas funções.

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

**finish\_unquantize** é chamada após a interpolação de paleta. A função **unquantize** adia o dimensionamento por 31/32 para assinado, 31/64 para não assinado. Esse comportamento é necessário para obter o valor final no intervalo de metade válido (-0x7BFF ~ 0x7BFF) após a interpolação de paleta para reduzir o número de multiplicações necessárias. **finish\_unquantize** aplica a escala final e retorna um valor **unsigned curto** que é reinterpretado em **half**.

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Compactação de bloco de textura](texture-block-compression.md)

 

 




