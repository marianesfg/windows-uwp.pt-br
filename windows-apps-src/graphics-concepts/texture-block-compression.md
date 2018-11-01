---
title: Compactação de bloco de textura
description: O suporte para Compactação de Bloco (BC) para texturas foi estendido no Direct3D 11 para incluir os algoritmos BC6H e BC7.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- Compactação de bloco de textura
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 46c58da3dbe425b055855423aa9e9cebaa06f929
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5924162"
---
# <a name="texture-block-compression"></a>Compactação de bloco de textura


O suporte para texturas de compactação de bloco (BC) foi estendido no Direct3D 11 para incluir os algoritmos de BC6H e BC7. O BC6H dá suporte para dados de origem de cor de alto alcance dinâmico, e o BC7 fornece compactação de qualidade acima da média com menos artefatos para dados de origem RGB padrão.

Para obter informações mais específicas sobre o suporte do algoritmo de compactação de bloco antes do Direct3D 11, incluindo o suporte para os formatos BC1 a BC5, consulte [Compactação de bloco (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb694531).

**Observação sobre formatos de arquivo:** Os formatos de compactação de textura de BC6H e BC7 usam o formato de arquivo DDS para armazenar os dados de textura compactada. Para obter mais informações, consulte o [Guia de programação para DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991) para obter detalhes.

## <a name="span-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Formatos de compactação de bloco com suporte no Direct3D 11


| Dados de origem                                  | Resolução de compactação de dados mínima necessária                              | Formato recomendado | Nível mínimo de recursos com suporte |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| Cor de três canais com canal alfa       | Três canais de cores (5 bits:6 bits:5 bits), com 0 ou 1 bit de alfa  | BC1                | Direct3D 9.1                    |
| Cor de três canais com canal alfa       | Três canais de cores (5 bits:6 bits:5 bits), com 4 bits de alfa         | BC2                | Direct3D 9.1                    |
| Cor de três canais com canal alfa       | Três canais de cores (5 bits:6 bits:5 bits) com 8 bits de alfa          | BC3                | Direct3D 9.1                    |
| Cor de um canal                            | Um canal de cor (8 bits)                                                | BC4                | Direct3D 10                     |
| Cor de dois canais                            | Dois canais de cores (8 bits:8 bits)                                        | BC5                | Direct3D 10                     |
| Cor de alto alcance dinâmico (HDR) de três canais | Três canais de cores (16 bits:16 bits:16 bits) em "meio" ponto flutuante\* | BC6H               | Direct3D 11                     |
| Cor de três canais, canal alfa opcional  | Três canais de cores (4 a 7 bits por canal) com 0 a 8 bits de alfa  | BC7                | Direct3D 11                     |

 

\*"Meio" ponto flutuante é um valor de 16 bits que consiste em um bit de sinal opcional, um exponente baseado em 5 bits e uma mantissa de 10 ou 11 bits.
## <a name="span-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>Formatos BC1, BC2 e B3


Os formatos BC1, BC2 e BC3 são equivalentes aos formatos de compactação de textura 9 DXTn do Direct3D e são iguais aos formatos BC1, BC2 e BC3 correspondentes do Direct3D 10. O suporte para esses três formatos é exigido por todos os níveis de recurso (D3D\_FEATURE\_LEVEL\_9\_1, D3D\_FEATURE\_LEVEL\_9\_2, D3D\_FEATURE\_LEVEL\_9\_3, D3D\_FEATURE\_LEVEL\_10\_0, D3D\_FEATURE\_LEVEL\_10\_1 e D3D\_FEATURE\_LEVEL\_11\_0).

| Formato de compactação de bloco | Formato DXGI                                                                           | Formato equivalente do Direct3D 9                               | Bytes por bloco de pixels 4x4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI\_FORMAT\_BC1\_UNORM, DXGI\_FORMAT\_BC1\_UNORM\_SRGB, DXGI\_FORMAT\_BC1\_TYPELESS | D3DFMT\_DXT1, FourCC="DXT1"                                | 8                         |
| BC2                      | DXGI\_FORMAT\_BC2\_UNORM, DXGI\_FORMAT\_BC2\_UNORM\_SRGB, DXGI\_FORMAT\_BC2\_TYPELESS | D3DFMT\_DXT2\*, FourCC="DXT2", D3DFMT\_DXT3, FourCC="DXT3" | 16                        |
| BC3                      | DXGI\_FORMAT\_BC3\_UNORM, DXGI\_FORMAT\_BC3\_UNORM\_SRGB, DXGI\_FORMAT\_BC3\_TYPELESS | D3DFMT\_DXT4\*, FourCC="DXT4", D3DFMT\_DXT5, FourCC="DXT5" | 16                        |

 

\*Esses esquemas de compactação (DXT2 e DXT4) não fazem distinção entre os formatos alfa pré-multiplicados do Direct3D 9 e os formatos alfa padrão. Essa distinção deve ser processada pelos sombreadores programáveis no momento da renderização.

## <a name="span-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>Formatos BC4 e BC5


| Formato de compactação de bloco | Formato DXGI                                                                     | Formato equivalente do Direct3D 9 | Bytes por bloco de pixels 4x4 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI\_FORMAT\_BC4\_UNORM, DXGI\_FORMAT\_BC4\_SNORM, DXGI\_FORMAT\_BC4\_TYPELESS | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI\_FORMAT\_BC5\_UNORM, DXGI\_FORMAT\_BC5\_SNORM, DXGI\_FORMAT\_BC5\_TYPELESS | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6hformatspanspan-idbc6hformatspanspan-idbc6hformatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>Formato BC6H


Para obter mais informações sobre esse formato, consulte a documentação do [formato BC6H](https://msdn.microsoft.com/library/windows/desktop/hh308952).

| Formato de compactação de bloco | Formato DXGI                                                                      | Formato equivalente do Direct3D 9 | Bytes por bloco de pixels 4x4 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI\_FORMAT\_BC6H\_UF16, DXGI\_FORMAT\_BC6H\_SF16, DXGI\_FORMAT\_BC6H\_TYPELESS | N/D                          | 16                        |

 

O formato BC6H pode selecionar diferentes modos de codificação para cada bloco de pixels 4x4. Um total de 14 modos de codificação diferentes estão disponíveis, cada um com compensações ligeiramente diferentes na qualidade visual resultante da textura exibida. A escolha dos modos permite a decodificação rápida pelo hardware com o nível de qualidade selecionado ou adaptado de acordo com o conteúdo de origem, mas também aumenta a complexidade do espaço de pesquisa.

## <a name="span-idbc7formatspanspan-idbc7formatspanspan-idbc7formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>Formato BC7


Para obter mais informações sobre esse formato, consulte a documentação do [formato BC7](https://msdn.microsoft.com/library/windows/desktop/hh308953).

| Formato de compactação de bloco | Formato DXGI                                                                           | Formato equivalente do Direct3D 9 | Bytes por bloco de pixels 4x4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI\_FORMAT\_BC7\_UNORM, DXGI\_FORMAT\_BC7\_UNORM\_SRGB, DXGI\_FORMAT\_BC7\_TYPELESS | N/D                          | 16                        |

 

O formato BC7 pode selecionar diferentes modos de codificação para cada bloco de pixels 4x4. Um total de 8 modos de codificação diferentes estão disponíveis, cada um com compensações ligeiramente diferentes na qualidade visual resultante da textura exibida. A escolha dos modos permite a decodificação rápida pelo hardware com o nível de qualidade selecionado ou adaptado de acordo com o conteúdo de origem, mas também aumenta a complexidade do espaço de pesquisa.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Apêndices](appendix.md)

[Texturas](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




