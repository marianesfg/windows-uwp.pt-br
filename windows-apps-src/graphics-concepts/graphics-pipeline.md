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
ms.localizationpriority: medium
ms.openlocfilehash: d4a362a3a7be06e48c64ce3e4d43ff917b9b24c5
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5554087"
---
# <a name="graphics-pipeline"></a>Pipeline de elementos gráficos


O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Os dados fluem da entrada para a saída por meio de cada um dos estágios configuráveis ou programáveis.

Todos os estágios de podem ser configurados usando a API do Direct3D. Estágios que apresentam núcleos comuns de sombreador (os blocos retangulares arredondados) são programáveis usando a linguagem de programação [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Isso torna o pipeline extremamente flexível e adaptável.

Os mais utilizados são o estágio de sombreador de vértice (VS) e o estágio de sombreador de Pixel (PS). Se você não fornecer esses estágios de sombreador, um sombreador de pixel e vértice de passagem no-op padrão será utilizado.

![diagrama do fluxo de dados no pipeline programável do direct3d 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Estágio do Assembler de Entrada

|-|-| | Finalidade | O [estágio de Assembler de entrada (IA)](input-assembler-stage--ia-.md) fornece dados primitivos e adjacência ao pipeline, como triângulos, linhas e pontos, incluindo IDs de semântica para ajudar a tornar os sombreadores mais eficientes, reduzindo o processamento para primitivas que ainda não tiver sido já processado. | | Entrada | Dados primitivos (triângulos, linhas, e/ou pontos), de buffers preenchidos pelo usuário na memória. E possivelmente dados de adjacência. Um triângulo seria 3 vértices para cada triângulo e possivelmente 3 vértices para dados de adjacência por triângulo. | | Saída | Primitivos com valores anexados gerados pelo sistema (como uma ID de primitivo, uma ID de instância ou uma ID de vértice). |

## <a name="vertex-shader-stage"></a>Estágio do Sombreador de Vértice

|-|-| | Finalidade | O [estágio do sombreador de vértice (VS)](vertex-shader-stage--vs-.md) processa vértices, normalmente realizando operações como transformações, aplicação de capas e iluminação. Um sombreador de vértice usa um único vértice de entrada para produzir um vértice de saída. Operações por vértice individuais, como transformações, aplicação, metamorfose e iluminação por vértice. | | Entrada | Um único vértice, com valores VertexID e InstanceID gerados pelo sistema. Cada vértice de entrada de sombreador de vértice pode ser composto de até 16 vetores de 32 bits (até 4 componentes cada). | | Saída | Um único vértice. Cada vértice de saída pode ser composto de até 16 vetores de 4 componentes de 32 bits. |
 
## <a name="hull-shader-stage"></a>Estágio de sombreador Hull
 
|-|-| | Finalidade | O [estágio de sombreador Hull (HS)](hull-shader-stage--hs-.md) é um dos estágios de mosaico, que divide uma única superfície de um modelo em vários triângulos. Um sombreador hull é chamado uma vez por patch e transforma os pontos de controle de entrada que definem uma superfície de ordem baixa em pontos de controle que formam um patch. Ela também faz alguns cálculos por patch para fornecer dados para o estágio de mosaico (TS) e o estágio de sombreador de domínio (DS). | | Entrada | Entre 1 e pontos de controle de entrada 32, que juntos definem uma superfície de ordem baixa. | | Saída | Entre pontos de controle de saída 1 e 32, que juntos formam um patch. O sombreador hull declara o estado para o estágio de mosaico (TS), incluindo o número de pontos de controle, o tipo da face do patch e o tipo de particionamento para usar ao criar o mosaico. |

## <a name="tessellator-stage"></a>Estágio de mosaico

|-|-| | Finalidade | O [estágio de mosaico (TS)](tessellator-stage--ts-.md) cria um padrão de amostragem do domínio que representa o patch de geometria e gera um conjunto de objetos menores (triângulos, pontos ou linhas) que se conectam a essas amostras. | | Entrada | O mosaico opera uma vez por patch usando os fatores de mosaico (que especificam o quão delicadamente o domínio será transformado em mosaico) e o tipo de particionamento (que especifica o algoritmo usado para dividir um patch) que são transmitidos do estágio de sombreador hull. | | Saída | O mosaico gera uv (e, opcionalmente, w) coordenadas e a topologia de superfície para o estágio de sombreador de domínio. |

## <a name="domain-shader-stage"></a>Estágio de sombreador de domínio

|-|-| | Finalidade | O [estágio do sombreador de domínio (DS)](domain-shader-stage--ds-.md) calcula a posição do vértice de um ponto subdividido no patch saída; Ele calcula a posição do vértice que corresponde a cada amostra de domínio. Um sombreador de domínio é executado uma vez por mosaico ponto de saída do estágio e tem acesso somente leitura para o sombreador hull patch de saída e constantes de patch de saída, e o estágio de mosaico coordenadas UV de saída. | | Entrada | Um sombreador de domínio consome pontos de controle de saída do [sombreador Hull (hs)](hull-shader-stage--hs-.md). As saídas do sombreador hull incluem: pontos de controle, dados da constante de patch, e fatores de mosaico (os fatores de mosaico podem incluir os valores usados pelo gerador de mosaico de função fixa, bem como os valores brutos, antes do arredondamento por mosaico inteiro, o que facilita o geomorfismo, por exemplo). Um sombreador de domínio é chamado uma vez por coordenadas de saída do [estágio de mosaico (TS)](tessellator-stage--ts-.md). | | Saída | O estágio de sombreador de domínio (DS) gera a posição do vértice de um ponto subdividido no patch de saída. |

## <a name="geometry-shader-stage"></a>Estágio de sombreador de geometria

|-|-| | Finalidade | O [estágio de sombreador de geometria (GS)](geometry-shader-stage--gs-.md) processa primitivos inteiros: triângulos, linhas e pontos, juntamente com seus vértices adjacentes. Ele suporta amplificação e cancelamento de amplificação de geometria. Ele é útil para algoritmos incluindo expansão de sprites de ponto, sistemas de partículas dinâmicas, geração de pele/barbatanas, geração de volume de sombra, passagem única de renderização para Cubemap, troca de material por primitivo, a configuração de material por primitivo — incluindo geração de coordenadas baricêntricas como dados primitivos para que um sombreador de pixel possa executar a interpolação de atributo personalizada. | | Entrada | Ao contrário de sombreadores de vértice que operam em um único vértice, entradas do sombreador de geometria são os vértices de um primitivo completo (três vértices para triângulos, dois vértices para linhas ou único vértice para ponto). | | Saída | O estágio de sombreador de geometria (GS) é capaz de gerar vários vértices formando uma única topologia selecionada. As topologias de saída do sombreador de geometria disponíveis são <strong>tristrip</strong>, <strong>linestrip</strong>, e <strong>pointlist</strong>. O número de primitivos emitidos pode variar livremente dentro de qualquer chamada do sombreador de geometria, embora o número máximo de vértices que podem ser emitidos deva ser declarado estaticamente. Tamanhos de faixa emitidos a partir de uma chamada de sombreador de geometria podem ser arbitrários, e novas faixas podem ser criadas por meio da função HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) . |

## <a name="stream-output-stage"></a>Estágio de Saída de Fluxo

|-|-| | Finalidade | Continuamente o [estágio de saída de fluxo (SO)](stream-output-stage--so-.md) gera (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU. | | Entrada | Dados de vértice de um estágio de pipeline anterior. | | Saída | O estágio de saída de fluxo (SO) continuamente gera (ou transmite) dados de vértice do estágio ativo anterior, como o estágio de sombreador de geometria (GS), para um ou mais buffers na memória. Se o estágio de sombreador de geometria (GS) estiver inativo, e o estágio de saída de fluxo (SO) estiver ativo, ele gera continuamente dados de vértice do estágio do sombreador de domínio (DS) para buffers na memória (ou se o DS também estiver inativo, do estágio do sombreador de vértice (VS)). |

## <a name="rasterizer-stage"></a>Estágio do rasterizador

|-|-| | Finalidade | O [estágio do rasterizador (RS)](rasterizer-stage--rs-.md) recorta primitivas que não estão exibidas, prepara-as para o estágio de sombreador de Pixel (PS) e determina como invocar os sombreadores de pixel. Converte informações de vetor (compostas de formas ou primitivos) em uma imagem de rasterização (composta de pixels) para fins de exibição de gráficos 3D em tempo real. | | Entrada | Vértices (x, y, z, w) chegando ao rasterizador estágio são consideradas no espaço de recorte homogêneo. Neste espaço de coordenadas do eixo X aponta para a direita, Y aponta para cima e Z aponta para longe da câmera. | | Saída | Os pixels reais que precisam ser renderizados. Inclui alguns atributos de vértice para uso em interpolação pelo sombreador de Pixel. |

## <a name="pixel-shader-stage"></a>Estágio do sombreador de pixel
 
|-|-| | Finalidade | O [estágio de sombreador de Pixel (PS)](pixel-shader-stage--ps-.md) recebe dados interpolados para uma primitiva e gera dados por pixel, como cor. | | Entrada | Quando o pipeline é configurado sem um sombreador de geometria, um sombreador de pixel é limitado a 16 entradas de 32 bits e 4 componentes. Caso contrário, um sombreador de pixel pode levar até 32 entradas de 32 bits de 4 componentes. Os dados de entrada de sombreador de pixel incluem atributos de vértice (que podem ser interpolados com ou sem correção de perspectiva) ou podem ser tratados como constantes por primitivo. As entradas do sombreador de pixel são interpoladas dos atributos de vértice do primitivo que está sendo rasterizado, com base no modo de interpolação declarado. Se um primitivo for recortado antes da rasterização, o modo de interpolação também será aplicado durante o processo de recorte. | | Saída | Um sombreador de pixel pode gerar até 8, 32 bits, cores de 4 componentes, ou nenhuma cor se o pixel for descartado. Componentes de registro de saída de sombreador de pixel devem ser declarados para que possam ser usados; uma máscara de gravação de saída distinta é permitida para cada registro. |

## <a name="output-merger-stage"></a>Estágio de fusão de saída
 
|-|-| | Finalidade | O [estágio de fusão de saída (OM)](output-merger-stage--om-.md) combina vários tipos de dados de saída (valores de sombreador de pixel, informações de profundidade e estêncil) com o conteúdo dos buffers de destino e de profundidade/estêncil de renderização para gerar o resultado do pipeline final. | | Entrada | As entradas de fusão de saída são o estado do Pipeline, os dados de pixel gerados pelos sombreadores de pixel, o conteúdo dos destinos de renderização e o conteúdo dos buffers de profundidade/estêncil. | | Saída | A cor do pixel renderizado final. |

## <a name="related-topics"></a>Tópicos relacionados

- [Guia de aprendizagem de Gráficos do Direct3D](index.md)

- [Calcular o pipeline](compute-pipeline.md)
