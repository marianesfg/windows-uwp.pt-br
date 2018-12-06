---
title: Elementos gráficos 2D para jogos DirectX
description: Falaremos sobre o uso de elementos gráficos e efeitos de bitmap 2D e como usá-los em seu jogo.
ms.assetid: ad69e680-d709-83d7-4a4c-7bbfe0766bc7
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, 2d, elementos gráficos
ms.localizationpriority: medium
ms.openlocfilehash: 1154abc4305307d87f15fbe0c0e5461e3a15e27e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8757801"
---
# <a name="2d-graphics-for-directx-games"></a>Elementos gráficos 2D para jogos DirectX



Falaremos sobre o uso de elementos gráficos e efeitos de bitmap 2D e como usá-los em seu jogo.

Os gráficos 2D são um subconjunto dos gráficos 3D que lidam com primitivas ou bitmaps 2D. De modo mais geral, eles não usam uma coordenada z do modo como ocorre em um jogo 3D, pois a jogabilidade está normalmente restrita ao plano x-y. Às vezes, eles usam técnicas de gráficos 3D para criar seus componentes visuais e, geralmente, são mais simples de desenvolver. Se você é novo na criação de jogos, um jogo 2D é um ótimo ponto para começar, e o desenvolvimento de gráficos 2D pode ser um bom modo de aprender a lidar com DirectX.

Você pode desenvolver gráficos para jogos 2D em DirectX usando Direct2D, Direct3D ou uma combinação deles. Muitas das classes mais úteis para o desenvolvimento de jogos 2D são criadas em Direct3D, como a classe [**Sprite**](https://msdn.microsoft.com/library/windows/desktop/bb205601). Direct2D é um conjunto de APIs direcionado principalmente a interfaces de usuário e aplicativos que exigem suporte a primitivas de desenho (como círculos, linhas e formas poligonais planas). Tendo isso em mente, ele também fornece um conjunto de classes e métodos avançado e de bom desempenho para criar gráficos de jogos, especialmente na criação de sobreposições, interfaces e HUDs do jogo, ou na criação de diversos jogos 2D, desde os mais simples até alguns consideravelmente detalhados. Porém, a abordagem mais eficiente na criação de jogos 2D é usar elementos dos dois binários, e é desse modo que veremos o desenvolvimento de gráficos 2D neste tópico.

## <a name="concepts-at-a-glance"></a>Conceitos básicos


Antes do advento de gráficos 3D modernos e do hardware para dar suporte a eles, os jogos eram em 2D e muitas de suas técnicas gráficas envolviam a movimentação de blocos de memória (normalmente, matrizes de dados de cores que eram convertidas ou transformadas em pixels na tela em uma proporção de 1:1).

No DirectX, os gráficos 2D fazem parte do pipeline 3D. Há uma variedade muito maior de resoluções de tela e hardware gráfico disponível, e sua engine de gráficos 2D deve ser capaz de dar suporte a essa variedade sem mudanças significativas na fidelidade.

Consulte alguns conceitos básicos que você deve conhecer ao iniciar o desenvolvimento de gráficos 2D.

-   Pixels e coordenadas de rasterização. Um pixel é um ponto único em uma exibição de rasterização e tem seu próprio para de coordenadas (x, y) indicando seu local na exibição (O termo "pixel" é frequentemente usado alternadamente entre os pixels físicos que compõem a exibição e os elementos de memória endereçáveis usados para manter os valores de cor e alfa dos pixels antes de serem enviados para a exibição). A rasterização é tratada pelas APIs como uma grade retangular de elementos de pixel, que muitas vezes tem uma correspondência de 1:1 com a grade de pixels física de uma exibição. Os sistemas de coordenadas de rasterização começam na parte superior esquerda, com um pixel em (0, 0), no canto superior mais à esquerda na grade.
-   Os gráficos de bitmap (às vezes chamados de gráficos de rasterização) são elementos gráficos representados como uma grade retangular de valores de pixel. Os sprites (matrizes de pixels processadas, gerenciadas de modo independente da rasterização) são um tipo de gráfico de bitmap. Normalmente, são usados para personagens ativos ou objetos animados independentes do plano de fundo em um jogo. Os vários quadros de animação de um sprite são representados como coleções de bitmaps chamadas de "folhas" ou "lotes". Os planos de fundo são objetos de bitmap maiores com uma resolução igual ou maior à da rasterização da tela. Muitas vezes, eles atuam como cenário(s) do ambiente de um jogo.
-   Os gráficos vetoriais são aqueles que usam primitivas geométricas, como pontos, linhas, círculos e polígonos, para definir objetos 2D. Eles são representados não como matrizes de pixels, mas como equações matemáticas que os definem em um espaço 2D. Eles não possuem necessariamente uma correspondência de 1:1 com a grade de pixels da exibição e devem ser transformados (a partir do sistema de coordenadas no qual você os renderizou) no sistema de coordenadas de rasterização da exibição.
-   Conversão é pegar um ponto ou vértice e calcular seu novo local no mesmo sistema de coordenadas.
-   Dimensionar é ampliar ou reduzir um objeto com base em um fator de escala especificado. Com uma imagem vetorial, os elementos ampliados ou reduzidos são os vértices que a compõem. No caso de bitmaps, ocorre a ampliação ou redução dos elementos de pixel. Com imagens em bitmap, você perde dados de pixels quando a imagem é reduzida e amplia pixels individuais quando a imagem é aproximada. Nesse caso, é possível usar operações de interpolação de cores de pixels, como filtragem bilinear, para suavizar os limites de cores afetados entre pixels ampliados.
-   Rotação é o ato de girar um objeto em um eixo ou eixos específicos. Com uma imagem vetorial, os vértices da geometria são multiplicados por uma matriz de rotação para obter o vértice rotacionado. Com uma imagem em bitmap, é possível aplicar algoritmos diferentes, cada um com um nível de fidelidade menor ou maior nos resultados. Em relação ao dimensionamento e à conversão, há APIs específicas para operações de rotação.
-   Transformação é o ato de pegar um ponto ou vértice em um sistema de coordenadas e calcular o ponto ou vértice correspondente em outro sistema. Isso inclui conversão, dimensionamento e rotação, bem como outras operações de cálculo de coordenadas.
-   Recortar é remover partes de bitmaps da geometria que não estejam na área de visualização da exibição ou que estejam ocultos por objetos com prioridade de visualização maior.
-   O buffer de quadros é uma área na memória (geralmente na memória do próprio hardware gráfico) que contém o mapa de rasterização final que você desenhará na tela. A cadeia de permuta é uma coleção de buffers onde você desenha um buffer de fundo e, quando a imagem está pronta, realiza uma "permuta", colocando-a na frente e exibindo-a.

## <a name="design-considerations"></a>Considerações de design


O desenvolvimento de gráficos 2D é uma ótima maneira de se acostumar com o desenvolvimento em Direct3D e permitirá que você dedique mais tempo a aspectos essenciais do desenvolvimento de jogos: áudio, controles e mecânica do jogo.

Sempre desenhe em um buffer de fundo. Desenhar diretamente no buffer de fundo significa que sua imagem será exibida quando o sinal de exibição for recebido (normalmente a cada 1/60 de segundo), mesmo que a operação de desenho não tenha sido concluída.

Desenvolva sua engine gráfica para dar suporte a uma boa seleção de resoluções, de 1024x600 a 1920x1080 (ou superior). Seu público agradecerá se você dar suporte à resolução nativa de seus monitores LCD, principalmente com gráficos 2D.

Com relação ao visual, um trabalho de arte bem feito será seu maior trunfo. Embora os gráfico em bitmap não tenham o apelo de visuais fotorrealistas em 3D que utilizam os últimos recursos de modelo de sombreador, muitas vezes um trabalho de arte primoroso em alta resolução pode trazer até mais estilo e personalidade, além de prejudicar muito menos o desempenho.

## <a name="reference"></a>Referência


-   [Visão geral de Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987)
-   [Guia de início rápido de Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd535473)
-   [Visão geral de interoperabilidade entre Direct2D e Direct3D](https://msdn.microsoft.com/library/windows/desktop/dd370966)
