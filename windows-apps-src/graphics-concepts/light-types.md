---
title: Tipos de luz
description: 'A propriedade do tipo de luz define qual o tipo de fonte de luz que você está usando. Existem três tipos de luzes no Direct3D: pontos iluminados, destaques e luzes direcionais.'
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- Tipos de luz
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9898a050131813b7b2431f8fc11397eee7c7942c
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6183937"
---
# <a name="light-types"></a>Tipos de luz


A propriedade do tipo de luz define qual o tipo de fonte de luz que você está usando. Existem três tipos de luzes no Direct3D: pontos iluminados, destaques e luzes direcionais. Cada tipo ilumina objetos em uma cena de forma diferente, com diferentes níveis de sobrecarga.

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>Luz pontual


As luzes pontuais têm cor e posição em uma cena, mas nenhum direção única. Elas emitem luz igualmente em todas as direções, como mostrado na ilustração a seguir.

![Ilustração de luz pontual](images/ptlight.png)

Uma lâmpada é um bom exemplo de luz pontual. As luzes pontuais são afetadas pela atenuação e pelo intervalo, além de iluminarem uma malha com base em vértices. Durante a iluminação, o Direct3D usa a posição da luz pontual no espaço do mundo e as coordenadas do vértice iluminado para derivar um vetor de direção de luz e a distância que a luz já percorreu. Ambos são usados, juntamente com o vértice normal, para calcular a contribuição da luz para a iluminação da superfície.

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>Luz direcional


As luzes direcionais têm somente cor e direção, mas não têm posição. Elas emitem luz paralela. Ou seja, todas as luzes geradas pelas luzes direcionais percorrem uma cena na mesma direção. Imagine uma luz direcional como uma fonte de luz a uma distância quase infinita, como o sol. As luzes direcionais não são afetadas por atenuação ou intervalos, portanto, a direção e a cor especificadas são fatores considerados somente quando o Direct3D calcula cores de vértice. Devido ao pequeno número de fatores de iluminação, essas são as luzes menos intensas para usar.

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>Destaque


Os destaques têm cor, posição e direção para emissão de luz. A luz emitida de um destaque é composta por um cone interno brilhante e um cone externo maior, com a intensidade de luz entre os dois diminuindo conforme mostrado na ilustração a seguir.

![Ilustração de um destaque com um cone interno e cone um externo](images/spotlt.png)

Destaques são afetados por quedas, atenuação e intervalo. Esses fatores, bem como a distância de deslocamento da luz em cada vértice, são elaborados ao calcular os efeitos de iluminação para objetos em uma cena. Esses efeitos de cada vértice fazem dos destaques uma das luzes mais demoradas de calcular no Direct3D.

Valores de Queda, Teta e Phi são usados somente por destaques. Esses valores controlam o tamanho dos cones internos e externos dos objetos do destaque, além da forma como a luz diminui entre eles.

Teta é o ângulo radiano do cone interno do destaque e o valor de Phi é o ângulo do cone de luz externo. A queda controla como a luz diminui a intensidade entre a borda externa de cone interno e a borda interna do cone externo. A maioria dos apps define a Queda como 1,0 para criar uma queda uniforme entre os dois cones, mas você pode definir outros valores conforme necessário.

A ilustração a seguir mostra a relação entre esses valores e como eles podem afetar os cones internos e externos do destaque da luz.

![ilustração de como os valores de phi e teta se relacionam com os cones de destaque](images/spotlt2.png)

Destaques emitem um cone de luz com duas partes: um cone interno brilhante e um cone externo. A luz é mais brilhante em de cone interno e não está presente fora do cone externo, com intensidade de luz atenuada entre as duas áreas. Em geral, esse tipo de atenuação é conhecido como queda.

A quantidade de luz recebida por um vértice tem por base a localização do vértice nos cones internos ou externos. O Direct3D calcula o produto de pontos do vetor de direção do destaque (L) e o vetor de luz para o vértice (D). Esse valor é igual ao cosseno do ângulo entre os dois vetores e serve como um indicador de posição do vértice que pode ser comparado a ângulos de cone da luz para determinar onde o vértice pode estar nos cones internos ou externos. A ilustração a seguir fornece uma representação gráfica da associação entre esses dois vetores.

![ilustração do vetor de direção de destaque e o vetor do vértice de destaque](images/spotalg1.png)

O sistema compara esse valor ao cosseno dos ângulos dos cones internos e externos do destaque. Os valores de Teta e Phi da luz representam os ângulos totais de cone para os cones internos e externos. Como a atenuação ocorre à medida que o vértice fica mais distante do Centro de iluminação (em vez de entre o ângulo total do cone), o tempo de execução divide esses ângulos de cone na metade antes de calcular seus cossenos.

Se o produto de pontos dos vetores L e D for menor ou igual ao cosseno do ângulo cone externo, o vértice está além do cone externo e não recebe nenhuma luz. Se o produto de pontos de L e D for maior do que o cosseno do ângulo de cone interno, o vértice está dentro do cone interno e recebe a quantidade máxima de luz, considerando ainda a atenuação em vez da distância. Se o vértice estiver em algum lugar entre duas regiões, a queda é calculada com a seguinte equação.

![fórmula de intensidade da luz em vértices depois da queda](images/falloff.png)

onde:

-   I<sub>f</sub> é a intensidade da luz após a queda
-   Alfa é o ângulo entre os vetores L e D
-   Teta é o ângulo do cone interno
-   Phi é o ângulo do cone externo
-   p é a queda

Essa fórmula gera um valor entre 0,0 e 1,0 que dimensiona a intensidade da luz no vértice para contabilizar a queda. A atenuação como um fator da distância do vértice para a luz também é aplicada. O gráfico a seguir mostra como valores de queda diferentes podem afetar a curva de queda.

![gráfico de intensidade de luz em comparação à distância do vértice para a luz](images/fallgraf.png)

O efeito de diversos valores de queda sobre a iluminação real é sutil e uma penalidade de desempenho pequena é incorrida ao modelar a curva de queda com valores de queda diferentes de 1,0. Por esses motivos, esse valor geralmente é definido como 1,0.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Luzes e materiais](lights-and-materials.md)

 

 




