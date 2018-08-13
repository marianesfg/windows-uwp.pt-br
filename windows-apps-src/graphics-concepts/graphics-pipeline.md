---
title: Pipeline de elementos gráficos
description: O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Os dados fluem da entrada para a saída por meio de cada um dos estágios configuráveis ou programáveis.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Pipeline de elementos gráficos
- Estágios de pipeline
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6cb50af5facdbf4271c9911f0e1f536dead0b8b8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044905"
---
# <a name="graphics-pipeline"></a>Pipeline de elementos gráficos


O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Os dados fluem da entrada para a saída por meio de cada um dos estágios configuráveis ou programáveis.

Todos os estágios de podem ser configurados usando a API do Direct3D. Estágios que apresentam núcleos comuns de sombreador (os blocos retangulares arredondados) são programáveis usando a linguagem de programação [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Isso torna o pipeline extremamente flexível e adaptável.

Os mais utilizados são o estágio de sombreador de vértice (VS) e o estágio de sombreador de Pixel (PS). Se você não fornecer esses estágios de sombreador, um sombreador de pixel e vértice de passagem no-op padrão será utilizado.

![diagrama do fluxo de dados no pipeline programável do direct3d 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Estágio do Assembler de Entrada

|-|-| | Finalidade | As fontes de [entrada Assembler (IA) estágio](input-assembler-stage--ia-.md) primitivas e os dados de proximidade no pipeline, como triângulos, linhas e pontos, incluindo semântica IDs para ajudar a tornar sombreadores mais eficiente, reduzindo o processamento para primitivos que já não tiver sido processadas. | | Entrada | Dados primitivos (triângulos, linhas e/ou pontos), dos buffers preenchida pelo usuário na memória. E possivelmente dados de adjacência. Um triângulo seria 3 vértices para cada triângulo e possivelmente 3 vértices para dados de proximidade por triângulo. | | Saída | Primitivos com valores de gerada pelo sistema anexados (por exemplo, uma ID primitiva, uma ID da instância ou uma ID de vértice). |

## <a name="vertex-shader-stage"></a>Estágio do Sombreador de Vértice

|-|-| | Finalidade | O [estágio de vértice sombreador (VS)](vertex-shader-stage--vs-.md) processa vértices, normalmente executar operações como transformações, atribuição de capa e de iluminação. Um sombreador de vértice usa um único vértice de entrada para produzir um vértice de saída. Operações de vértice de individuais, transformações, aparência, Morphing e iluminação por vértice. | | Entrada | Um único vértice, com valores de VertexID e a identificação da instância gerada pelo sistema. Cada vértice entrada do vértice sombreador pode ser composto de até 16 vetores de 32 bits (até 4 componentes cada). | | Saída | Um único vértice. Cada vértice de saída pode ser composto de até 16 vetores de componente de 4 de 32 bits. |
 
## <a name="hull-shader-stage"></a>Estágio de sombreador Hull
 
|-|-| | Finalidade | O [estágio Hull sombreador (HS)](hull-shader-stage--hs-.md) é um dos estágios de mosaico, divida com eficiência uma única superfície de um modelo em muitos triângulos. Um sombreador hull é chamado uma vez por patch e transforma os pontos de controle de entrada que definem uma superfície de ordem baixa em pontos de controle que formam um patch. Ele também faz alguns cálculos por patch para fornecer dados para o estágio de Tessellator (TS) e o estágio de sombreador de domínio (DS). | | Entrada | Entre 1 e pontos de controle de entrada 32, que juntas definem a uma superfície de ordem baixa. | | Saída | Entre os pontos de controle de saída 1 e 32, que juntas, constituem um patch. Sombreador hull declara o estado de estágio Tessellator (TS), incluindo o número de pontos de controle, o tipo da face do patch e o tipo de particionamento para usar quando tessellating. |

## <a name="tessellator-stage"></a>Estágio de mosaico

|-|-| | Finalidade | O [estágio de Tessellator (TS)](tessellator-stage--ts-.md) cria um padrão de amostragem do domínio que representa o patch geometria e gera um conjunto de objetos menores (triângulos, pontos ou linhas) que se conectam essas amostras. | | Entrada | O tessellator opera uma vez por usando os fatores de mosaico (que especificam como levemente o domínio será tessellated) e o tipo de particionamento (que especifica o algoritmo usado para um patch de fatia) que sejam passados desde o estágio hull-sombreador de patch. | | Saída | O tessellator produz uv (e, opcionalmente, w) coordenadas e a topologia de superfície para o estágio de domínio-sombreador. |

## <a name="domain-shader-stage"></a>Estágio de sombreador de domínio

|-|-| | Finalidade | O [estágio de sombreador de domínio (DS)](domain-shader-stage--ds-.md) calcula a posição de vértice de um ponto subdivided no patch saída; calcula a posição de vértice que corresponde a amostra de cada domínio. Um sombreador de domínio é executado uma vez por tessellator ponto de saída do estágio e tem acesso somente leitura para o sombreador hull patch de saída e de saída de constantes de patch e o estágio tessellator coordenadas UV de saída. | | Entrada | Um sombreador de domínio consome pontos de controle de saída do [estágio Hull sombreador (HS)](hull-shader-stage--hs-.md). As saídas do sombreador hull incluem: pontos de controle, dados da constante de patch, e fatores de mosaico (os fatores de mosaico podem incluir os valores usados pelo gerador de mosaico de função fixa, bem como os valores brutos, antes do arredondamento por mosaico inteiro, o que facilita o geomorfismo, por exemplo). Um sombreador de domínio é invocado uma vez por coordenadas de saída do [estágio Tessellator (TS)](tessellator-stage--ts-.md). | | Saída | O estágio de sombreador de domínio (DS) produz a posição de vértice de um ponto subdivided no patch de saída. |

## <a name="geometry-shader-stage"></a>Estágio de sombreador de geometria

|-|-| | Finalidade | O [estágio Geometry Shader (GS)](geometry-shader-stage--gs-.md) processa primitivos inteiros: triângulos, linhas e pontos, juntamente com seus vértices adjacentes. Ele suporta amplificação e cancelamento de amplificação de geometria. Ele é útil para algoritmos incluindo expansão de sprites de ponto, sistemas de partículas dinâmicas, geração de pele/barbatanas, geração de volume de sombra, passagem única de renderização para Cubemap, troca de material por primitivo, a configuração de material por primitivo — incluindo geração de coordenadas baricêntricas como dados primitivos para que um sombreador de pixel possa executar a interpolação de atributo personalizada. | | Entrada | Ao contrário de sombreadores de vértice, que operam em um único vértice, entradas do sombreador geometria são os vértices de um primitivo completo (três vértices para triângulos, dois vértices para linhas ou vértice único ponto). | | Saída | O estágio Geometry Shader (GS) é capaz de saindo vários vértices formando uma única topologia selecionada. As topologias de saída do sombreador de geometria disponíveis são <strong>tristrip</strong>, <strong>linestrip</strong>, e <strong>pointlist</strong>. O número de primitivos emitidos pode variar livremente dentro de qualquer chamada do sombreador de geometria, embora o número máximo de vértices que podem ser emitidos deva ser declarado estaticamente. Comprimentos de faixa emitidos a partir de uma invocação de sombreador geometria podem ser arbitrários e novos recortes podem ser criados por meio da função [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL. |

## <a name="stream-output-stage"></a>Estágio de Saída de Fluxo

|-|-| | Finalidade | O [estágio de fluxo de saída (etc)](stream-output-stage--so-.md) continuamente produz (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Dados transmitidos check-out para memória podem ser recirculated volta à pipeline como dados de entrada ou leitura-back da CPU. | | Entrada | Dados de vértice de um estágio de pipeline anterior. | | Saída | O estágio de saída do fluxo (etc) continuamente produz (ou transmite) dados de vértice do estágio de ativo anterior, como o estágio Geometry Shader (GS), para um ou mais buffers na memória. Se o estágio Geometry Shader (GS) está inativo e o estágio de saída do fluxo (etc) está ativo, ele emite continuamente dados de vértice do estágio sombreador de domínio (DS) para buffers de memória (ou se DS também está inativo, desde o estágio de vértice sombreador (VS)). |

## <a name="rasterizer-stage"></a>Estágio do rasterizador

|-|-| | Finalidade | Os primitivos de clipes de [estágio rasterizador (RS)](rasterizer-stage--rs-.md) que não estão no modo de exibição, prepara primitivas para o estágio de Pixel Shader (PS) e determina como invocar sombreadores pixel. Converte de vetor informações (compostas de formas ou primitivos) em uma imagem de varredura (composta de pixels) para fins de exibir gráficos 3D em tempo real. | | Entrada | Os vértices (x, y, z, w) chegam ao rasterizador estágio serão considerados como equivalentes no espaço de clip homogêneo. Neste espaço de coordenadas do eixo X apontando para a direita, Y apontando para cima e Z aponta para longe de câmera. | | Saída | Os pixels reais que precisam ser renderizado. Inclui alguns atributos de vértice para uso em interpolação pelo sombreador de Pixel. |

## <a name="pixel-shader-stage"></a>Estágio do sombreador de pixel
 
|-|-| | Finalidade | O [estágio de Pixel Shader (PS)](pixel-shader-stage--ps-.md) recebe dados interpolados para um primitivo e gera dados por pixel como cor. | | Entrada | Quando o pipeline estiver configurado sem um sombreador de geometria, um sombreador de pixel é limitado a 16, entradas de 32 bits, 4-componente. Caso contrário, um sombreador de pixel pode levar até 32 entradas de 32 bits de 4 componentes. Os dados de entrada de sombreador de pixel incluem atributos de vértice (que podem ser interpolados com ou sem correção de perspectiva) ou podem ser tratados como constantes por primitivo. As entradas do sombreador de pixel são interpoladas dos atributos de vértice do primitivo que está sendo rasterizado, com base no modo de interpolação declarado. Se um primitivo for recortado antes da rasterização, o modo de interpolação também será aplicado durante o processo de recorte. | | Saída | Um sombreador de pixel pode saída até 8, 32 bits, 4-componente cores ou sem cor se pixel é desconsiderado. Componentes de registrador do pixel shader saída deverá ser declaradas antes que eles possam ser usados; cada registro é permitido uma máscara de saída-write distinta. |

## <a name="output-merger-stage"></a>Estágio de fusão de saída
 
|-|-| | Finalidade | O [estágio de saída fusão (OM)](output-merger-stage--om-.md) combina os vários tipos de dados de saída (os valores de sombreador de pixel, informações de profundidade e estêncil) com o conteúdo dos buffers de profundidade/estêncil e de destino do renderização para gerar o resultado final do pipeline. | | Entrada | Entradas de fusão de saída são o estado de Pipeline, os dados de pixel gerados pelos sombreadores pixel, o conteúdo de destinos de renderização e o conteúdo dos buffers de profundidade/estêncil. | | Saída | O final renderizados cor pixel. |

## <a name="related-topics"></a>Tópicos relacionados

- [Guia de aprendizagem de Gráficos do Direct3D](index.md)

- [Calcular o pipeline](compute-pipeline.md)
