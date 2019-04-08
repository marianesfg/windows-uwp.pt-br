---
title: Pipeline de elementos gráficos
description: O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Os dados fluem da entrada para a saída por meio de cada um dos estágios configuráveis ou programáveis.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Pipeline de elementos gráficos
- Estágios de pipeline
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 55621cec768e0aac680c3a84fd803e591459a97d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605431"
---
# <a name="graphics-pipeline"></a>Pipeline de elementos gráficos


O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Os dados fluem da entrada para a saída por meio de cada um dos estágios configuráveis ou programáveis.

Todos os estágios de podem ser configurados usando a API do Direct3D. Estágios que apresentam núcleos comuns de sombreador (os blocos retangulares arredondados) são programáveis usando a linguagem de programação [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Isso torna o pipeline extremamente flexível e adaptável.

Os mais utilizados são o estágio de sombreador de vértice (VS) e o estágio de sombreador de Pixel (PS). Se você não fornecer esses estágios de sombreador, um sombreador de pixel e vértice de passagem no-op padrão será utilizado.

![diagrama do fluxo de dados no pipeline programável do direct3d 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Estágio do Assembler de Entrada

|-|-| |Finalidade|O [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md) fornece dados primitivos e de adjacência ao pipeline, como triângulos, linhas e pontos, incluindo IDs de semântica que tornam os sombreadores mais eficientes ao reduzir o processamento a primitivos que ainda não foram processados.| |Entrada|Dados primitivos (triângulos, linhas e/ou pontos), nos buffers preenchidos pelo usuário na memória. E possivelmente dados de adjacência. Um triângulo seria três vértices para cada triângulo e possivelmente três vértices para dados de proximidade por triângulo.| |Saída|Primitivos com valores gerados pelo sistema anexados (como uma ID de primitivo, uma ID de instância ou uma ID de vértice).|

## <a name="vertex-shader-stage"></a>Estágio do Sombreador de Vértice

|-|-| |Finalidade|O [estágio do Sombreador de Vértices (VS)](vertex-shader-stage--vs-.md) processa vértices, normalmente realizando operações como transformações, aplicação de capas e iluminação. Um sombreador de vértice usa um único vértice de entrada para produzir um vértice de saída. Operações por vértice individuais, como transformações, aplicação de capas, metamorfose e iluminação por vértice.| |Entrada|Um único vértice, com valores VertexID e InstanceID gerados pelo sistema. Cada vértice de entrada de sombreador de vértice pode ser composto por até 16 vetores de 32 bits (até quatro componentes cada um).| |Saída|Um único vértice. Cada vértice de saída pode ser composto por até 16 vetores de 32 bits de quatro componentes.|
 
## <a name="hull-shader-stage"></a>Estágio do Sombreador de Envoltória
 
|-|-| |Finalidade|O [estágio do Sombreador de Envoltória (HS)](hull-shader-stage--hs-.md) é um dos estágios de mosaico, que fragmenta com eficiência uma única superfície de modelo em vários triângulos. Um sombreador hull é chamado uma vez por patch e transforma os pontos de controle de entrada que definem uma superfície de ordem baixa em pontos de controle que formam um patch. Ela também faz alguns cálculos por patch para fornecer dados para o estágio do Mosaico (TS) e o estágio do Sombreador de Domínio (DS)| |Entrada|Entre 1 e 32 pontos de controle de entrada, que juntos definem uma superfície de ordem inferior.| |Saída|Entre 1 e 32 pontos de controle de saída, que juntos formam um patch. O sombreador de envoltória declara o estado do estágio do Mosaico (TS), incluindo o número de pontos de controle, o tipo de face do patch e o tipo de particionamento a ser usado ao criar o mosaico.|

## <a name="tessellator-stage"></a>Estágio do Mosaico

|-|-| |Finalidade|O [estágio do Mosaico (TS)](tessellator-stage--ts-.md) cria um padrão de amostragem do domínio que representa o patch de geometria e gera um conjunto de objetos menores (triângulos, pontos ou linhas) que conectam essas amostras.| |Entrada|O mosaico opera uma vez por patch usando os fatores de mosaico (que especificam o nível de minuciosidade do mosaico do domínio) e o tipo de particionamento (que especifica o algoritmo usado para dividir um patch), que são transmitidos pelo estágio do Sombreador de Envoltória. | |Saída|O mosaico transmite coordenadas uv (e, opcionalmente, w) e a topologia de superfície para o estágio do sombreador de domínio.|

## <a name="domain-shader-stage"></a>Estágio do Sombreador de Domínio

|-|-| |Finalidade|O [estágio do Sombreador de Domínio (DS)](domain-shader-stage--ds-.md) calcula a posição de vértice de um ponto subdividido no patch de saída; ele calcula a posição de vértice que corresponde a cada amostra de domínio. Um sombreador de domínio é executado uma vez por ponto de saída do estágio do mosaico e tem acesso somente leitura ao patch de saída do sombreador de envoltória, às constantes do patch de saída e às coordenadas UV de saída do estágio do mosaico.| |Entrada|Um sombreador de domínio consome pontos de controle de saída no [estágio do Sombreador de Envoltória (HS)](hull-shader-stage--hs-.md). As saídas de sombreador hull incluem: Fatores de mosaico (os fatores de mosaico podem incluir os valores usados pelo tessellator a função fixa, bem como os valores brutos - antes do arredondamento de mosaico de inteiro - o que facilita a geomorphing, por exemplo, dados constantes do Patch e pontos de controle ). Um sombreador de domínio é chamado uma vez por coordenada de saída no [estágio do Mosaico (TS)](tessellator-stage--ts-.md).| |Saída|O estágio do Sombreador de Domínio (DS) gera a posição de vértice de um ponto subdividido no patch de saída.|

## <a name="geometry-shader-stage"></a>Estágio do Sombreador de Geometria

|-|-| |Finalidade|O [estágio do Sombreador de Geometria (GS)](geometry-shader-stage--gs-.md) processa primitivos inteiros: triângulos, linhas e pontos, juntamente com seus vértices adjacentes. Ele suporta amplificação e cancelamento de amplificação de geometria. Ele é útil para algoritmos incluindo expansão de sprites de ponto, sistemas de partículas dinâmicas, geração de pele/barbatanas, geração de volume de sombra, passagem única de renderização para Cubemap, troca de material por primitivo, a configuração de material por primitivo — incluindo geração de coordenadas baricêntricas como dados primitivos para que um sombreador de pixel possa executar a interpolação de atributo personalizada. | |Entrada|Diferente dos sombreadores de vértice, que operam em um único vértice, as entradas do sombreador de geometria são os vértices de um primitivo completo (três vértices para triângulos, dois vértices para linhas ou um único vértice para ponto).| |Saída|O estágio do Sombreador de Geometria (GS) é capaz de gerar vários vértices que formam uma única topologia selecionada. As topologias de saída do sombreador de geometria disponíveis são <strong>tristrip</strong>, <strong>linestrip</strong>, e <strong>pointlist</strong>. O número de primitivos emitidos pode variar livremente dentro de qualquer chamada do sombreador de geometria, embora o número máximo de vértices que podem ser emitidos deva ser declarado estaticamente. Os tamanhos de faixa emitidos a partir de uma chamada de sombreador de geometria podem ser arbitrários, e novas faixas podem ser criadas por meio da função HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660).|

## <a name="stream-output-stage"></a>Estágio da Saída de Fluxo

|-|-| |Finalidade|O [estágio da Saída de Fluxo (SO)](stream-output-stage--so-.md) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos da CPU.| |Entrada|Dados de vértice de um estágio de pipeline anterior.| |Saída|O estágio da Saída de Fluxo (SO) gera (ou transmite) continuamente dados de vértice do estágio ativado anterior, como o estágio do Sombreador de Geometria (GS), para um ou mais buffers na memória. Se o estágio do Sombreador de Geometria (GS) estiver inativo e o estágio da Saída de Fluxo (SO) estiver ativo, ele gerará continuamente dados de vértice do estágio do Sombreador de Domínio (DS) para buffers na memória (ou se o DS também estiver inativo, do estágio do Sombreador de Vértice (VS)).|

## <a name="rasterizer-stage"></a>Estágio do Rasterizador

|-|-| |Finalidade|O [estágio do Rasterizador (RS)](rasterizer-stage--rs-.md) recorta primitivos que não estão em exibição, prepara-os para o estágio do Sombreador de Pixel (PS) e determina como chamar os sombreadores de pixel. Converte as informações de vetor (compostas por formas ou primitivos) em uma imagem de varredura (composta por pixels) para fins de exibição de gráficos 3D em tempo real.| |Entrada|Pressupõe-se que os vértices (x, y, z, w) que entram no estágio do Rasterizador estejam no espaço de recorte homogêneo. Nesse espaço de coordenada, o eixo X aponta para a direita, o eixo Y aponta para cima e o eixo Z aponta para longe da câmera.| |Saída|Os pixels reais que precisam ser renderizados. Inclui alguns atributos de vértice para uso em interpolação pelo Sombreador de Pixel.|

## <a name="pixel-shader-stage"></a>Estágio do Sombreador de Pixel
 
|-|-| |Finalidade|O [estágio do Sombreador de Pixel (PS)](pixel-shader-stage--ps-.md) recebe dados interpolados de um primitivo e gera dados por pixel, como a cor.| |Entrada|Quando o pipeline é configurado sem um sombreador de geometria, um sombreador de pixel limita-se a 16 entradas de 32 bits de quatro componentes. Caso contrário, um sombreador de pixel pode levar até 32 entradas de 32 bits de 4 componentes. Os dados de entrada de sombreador de pixel incluem atributos de vértice (que podem ser interpolados com ou sem correção de perspectiva) ou podem ser tratados como constantes por primitivo. As entradas do sombreador de pixel são interpoladas dos atributos de vértice do primitivo que está sendo rasterizado, com base no modo de interpolação declarado. Se um primitivo for recortado antes da varredura, o modo de interpolação também será aplicado durante o processo de recorte. | |Saída|Um sombreador de pixel pode gerar até 8 cores de 32 bits de quatro componentes ou nenhuma cor se o pixel for descartado. Os componentes de registro de saída do sombreador de pixel devem ser declarados para que possam ser usados; uma máscara de gravação de saída distinta é permitida para cada registro.|

## <a name="output-merger-stage"></a>Estágio da Fusão de Saída
 
|-|-| |Finalidade|O [estágio da Fusão de Saída (OM)](output-merger-stage--om-.md) combina vários tipos de dados de saída (valores de sombreador de pixel, informações de profundidade e estêncil) com o conteúdo dos buffers de profundidade/estêncil e de destino de renderização para gerar o resultado final do pipeline.| |Entrada|As entradas da Fusão de Saída são o estado do pipeline, os dados de pixel gerados pelos sombreadores de pixel, o conteúdo dos destinos de renderização e o conteúdo dos buffers de profundidade/estêncil.| |Saída|A cor final do pixel renderizado.|

## <a name="related-topics"></a>Tópicos relacionados

- [Guia de aprendizado de gráficos do Direct3D](index.md)

- [Computar o pipeline](compute-pipeline.md)
