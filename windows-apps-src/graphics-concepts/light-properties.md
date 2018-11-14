---
title: Propriedades de luz
description: As propriedades de luz descrevem o tipo (ponto, direcional, destaque), a atenuação, a cor, a direção, a posição e o intervalo da fonte de luz.
ms.assetid: E832C3FD-9921-41C4-87B8-056E16B61B77
keywords:
- Propriedades de luz
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 07a465d8fdcd1d425ed62e8d83cadd261f316da2
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6255396"
---
# <a name="light-properties"></a>Propriedades de luz


As propriedades de luz descrevem o tipo (ponto, direcional, destaque), a atenuação, a cor, a direção, a posição e o intervalo da fonte de luz. Dependendo do tipo de luz usado, uma luz pode ter propriedades de atenuação e intervalo ou para efeitos de destaque. Nem todos os tipos de luzes usam todas as propriedades.

As propriedades de posição, intervalo e atenuação definem o local da luz no espaço do mundo e como a iluminação emitida se comporta à distância.

## <a name="span-idlightattenuationspanspan-idlightattenuationspanspan-idlightattenuationspanlight-attenuation"></a><span id="Light_Attenuation"></span><span id="light_attenuation"></span><span id="LIGHT_ATTENUATION"></span>Atenuação de luz


A atenuação controla como a intensidade da luz diminui em direção à distância máxima especificada pela propriedade do intervalo. Às vezes, três valores de ponto flutuante são usados para representar a atenuação de luz: Atenuação0, Atenuação1 e Atenuação2. Esses valores de ponto flutuante variando de 0,0 ao infinito controlam a atenuação da luz. Alguns apps definem o membro do Atenuação1 como 1,0 e a outros como 0,0, o que resulta em intensidade de luz que muda como 1 / D, onde D é a distância da fonte de luz para o vértice. A intensidade da luz máxima está na origem, diminuindo a distância da luz para 1 / (intervalo da luz).

Embora normalmente um app defina Atenuação0 como 0,0, Atenuação1 como um valor constante e Atenuação2 como 0,0, é possível obter diversos efeitos de luz por essa alteração. Você pode combinar valores de atenuação para obter efeitos de atenuação mais complexos. Ou você pode defini-los como valores fora do intervalo normal para criar efeitos de atenuação ainda mais incomuns. Entretanto, não é permitido usar valores de atenuação negativos. Consulte [Atenuação e fator de destaque](attenuation-and-spotlight-factor.md).

## <a name="span-idlightcolorspanspan-idlightcolorspanspan-idlightcolorspanlight-color"></a><span id="Light_Color"></span><span id="light_color"></span><span id="LIGHT_COLOR"></span>Cor da luz


As luzes no Direct3D emitem três cores que são usadas de forma independente em cálculos de iluminação do sistema: uma cor difusa, uma ambiente e uma especular. Cada uma é incorporada pelo módulo de iluminação do Direct3D e interage com uma parte do material atual a fim de produzir uma cor final usada na renderização. A cor difusa interage com a propriedade de reflexão difusa do material atual, a cor especular com a propriedade de reflexão especular do material e assim por diante. Para obter informações específicas sobre como o Direct3D aplica essas cores, consulte [Matemática da iluminação](mathematics-of-lighting.md).

Em um app Direct3D, normalmente existem três valores de cores: difusas, ambiente e especular, que definem a cor emitida.

O tipo de cor que se aplica mais intensamente aos cálculos do sistema é o difuso. A cor difusa mais comum é o branco (R:1,0 G:1,0 B:1,0), mas você pode criar cores conforme necessário para obter os efeitos desejados. Por exemplo, você pode usar a luz vermelha para uma lareira ou verde para um sinal de tráfego definido como "Avançar".

Em geral, você define os componentes de cor da luz como valores entre 0,0 e 1,0, inclusive, mas isso não é um requisito. Por exemplo, você pode definir todos os componentes como 2,0, o que criar uma luz "mais brilhante do que o branco." Esse tipo de configuração pode ser útil ao usar as configurações de atenuação diferentes da constante.

Embora o Direct3D use valores RGBA de luzes, o componente de cor alfa não é usado.

Em geral, as cores materiais são usadas para iluminação. No entanto, você pode especificar que as cores materiais de emissão, ambiente, difusas e especulares devem ser substituídas pelas cores de vértice difusas ou especulares.

O valor de alfa/transparência sempre vem somente do canal alfa da cor difusa.

O valor de nevoeiro sempre vem somente do canal alfa da cor especular.

## <a name="span-idlightdirectionspanspan-idlightdirectionspanspan-idlightdirectionspanlight-direction"></a><span id="Light_Direction"></span><span id="light_direction"></span><span id="LIGHT_DIRECTION"></span>Direção da luz


A propriedade de direção da luz determina a direção em que a luz emitida pelo objeto se desloca no espaço do mundo. A direção é usada apenas por luzes direcionais e destaques, e é descrita com um vetor.

Defina a direção da luz como um vetor. Os vetores de direção são descritos como distâncias de uma origem lógica, independentemente da posição da luz em uma cena. Portanto, um destaque que aponta diretamente para uma cena no eixo z positivo tem um vetor de direção de &lt;0,0,1&gt; independentemente da posição definida. Da mesma forma, você pode simular a luz do sol brilhando diretamente em uma cena usando uma luz direcional cuja direção é &lt;0,-1,0&gt;. Não é necessário criar luzes que brilham nos eixos de coordenadas; você pode misturar e corresponder valores para criar luzes que brilham em ângulos mais interessantes.

Embora não seja necessário normalizar um vetor de direção da luz, verifique sempre se tem magnitude. Ou seja, não use um vetor de direção &lt;0,0,0&gt;.

## <a name="span-idlightpositionspanspan-idlightpositionspanspan-idlightpositionspanlight-position"></a><span id="Light_Position"></span><span id="light_position"></span><span id="LIGHT_POSITION"></span>Posição da luz


A posição da luz é descrita usando uma estrutura de vetor. As coordenadas x, y e z são consideradas no espaço do mundo. As luzes direcionais são o único tipo de luz que não usa a propriedade de posição.

## <a name="span-idlightrangespanspan-idlightrangespanspan-idlightrangespanlight-range"></a><span id="Light_Range"></span><span id="light_range"></span><span id="LIGHT_RANGE"></span>Intervalo de luz


A propriedade de intervalo da luz determina a distância, no espaço do mundo, em que malhas de uma cena não recebem mais luz emitida pelo objeto. As luzes direcionais não usam a propriedade do intervalo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Luzes e materiais](lights-and-materials.md)

 

 




