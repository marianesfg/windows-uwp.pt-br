---
title: Visão geral de iluminação
description: Ao usar a iluminação do Direct3D, você permite que o Direct3D processe os detalhes de iluminação para você. Os usuários avançados podem executar a iluminação por conta própria, se desejado.
ms.assetid: FCBF6A92-4EAC-4CCC-A76C-79985AF348AE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e90e460cf5f5bda7d90447440d76cf6898a83747
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7694985"
---
# <a name="lighting-overview"></a>Visão geral de iluminação

Ao usar a iluminação do Direct3D, você permite que o Direct3D processe os detalhes de iluminação para você. Os usuários avançados podem executar a iluminação por conta própria, se desejado.

Quando a iluminação está habilitada, o Direct3D calcula a cor de cada vértice de objeto com base em uma combinação das seguintes opções:

-   Os texels em um mapa de textura associado.
-   As cores difusas e especulares do vértice, se especificado.
-   A cor e a intensidade da luz produzida por fontes de iluminação de cena ou o nível de luz ambiente da cena.

A forma como você trabalha com a iluminação e os materiais faz uma grande diferença na aparência da cena renderizada. Os materiais definem como a luz é refletida por uma superfície. Níveis de luz direta e ambiente definem a luz refletida. Você deve usar materiais para renderizar uma cena se a iluminação estiver habilitada.

As luzes não são necessárias para renderizar uma cena, mas os detalhes em uma cena renderizada sem luz não estão visíveis. Na melhor das hipóteses, a renderização de uma cena apagada resulta em uma silhueta dos objetos nela. Isso não fornece detalhes suficientes para a maioria das finalidades.

## <a name="span-iddirectlightvsambientlightspanspan-iddirectlightvsambientlightspandirect-light-vs-ambient-light"></a><span id="direct_light_vs._ambient_light"></span><span id="DIRECT_LIGHT_VS._AMBIENT_LIGHT"></span>Luz direta e luz ambiente


Embora as luzes direta e ambiente iluminem objetos em uma cena, elas são independentes uma da outra, têm efeitos muito diferentes e exigem que você trabalhe com elas de formas completamente diferentes.

*Luz direta* atinge o objeto diretamente. A luz direta sempre tem direção e cor, além de ser um fator para algoritmos de sombreamento, como sombreamento de Gouraud. Diferentes tipos de luzes emitem luz direta de formas diferentes, o que cria efeitos especiais de atenuação.

*Luz ambiente* está efetivamente em qualquer lugar em uma cena. A luz ambiente é um nível geral de luz que preenche uma cena inteira, independentemente dos objetos e suas localizações nela. A luz ambiente não tem posição ou direção, somente cor e intensidade. Cada luz adiciona detalhes à luz ambiente geral em uma cena.

A cor de luz ambiente assume a forma de um valor RGBA, onde cada componente é um valor inteiro de 0 a 255. Isso é diferente da maioria dos valores de cores no Direct3D.

Os componentes vermelhos, verdes e azuis são combinados para formar a cor final da luz ambiente. O componente alfa controla a transparência da cor. Ao usar a aceleração de hardware ou a emulação de RGB, o componente alfa é ignorado.

## <a name="span-iddirect3dlightmodelvsnaturespanspan-iddirect3dlightmodelvsnaturespandirect3d-light-model-vs-nature"></a><span id="direct3d_light_model_vs._nature"></span><span id="DIRECT3D_LIGHT_MODEL_VS._NATURE"></span>Modelo de luz do Direct3D em comparação à natural


Na natureza, quando a luz é emitida de uma fonte, ela é refletido por centenas, se não milhares ou milhões de objetos antes de chegar aos olhos do usuário. Cada vez que ele é refletido, uma luz é absorvida por uma superfície, algumas estão espalhados em direções aleatórias e o restante prossegue para outra superfície ou a atenção do usuário. Esse processo continua até que a luz seja reduzida a nada ou um usuário perceba a luz.

Obviamente, os cálculos necessários para simular perfeitamente o comportamento natural de luz são muito demorados para uso em elementos gráficos de tempo real do Direct3D. Portanto, pensando na velocidade, o modelo de luz do Direct3D se aproxima da maneira como a luz funciona no mundo natural. O Direct3D descreve a luz em termos de componentes de vermelhos, verdes e azuis que são combinados para criar uma cor final.

No Direct3D, quando a luz é refletida por uma superfície, a cor da luz interage matematicamente com a superfície para criar a cor eventualmente exibida na tela. Para obter informações específicas sobre os algoritmos que o Direct3D usa, consulte [Matemática da iluminação](mathematics-of-lighting.md).

O modelo de luz do Direct3D generaliza a luz em dois tipos: luzes ambiente e direta. Cada uma tem atributos diferentes e interage com o material de uma superfície de maneiras diferentes. A luz ambiente é muito espalhada, portanto, sua direção e fonte são indeterminadas: ela mantém um nível baixo de intensidade em todos os lugares. A iluminação indireta usada por fotógrafos é um bom exemplo de luz ambiente.

Assim como na natureza, a luz ambiente no Direct3D não tem direção real ou fonte, apenas uma cor e uma intensidade. Na verdade, o nível de luz ambiente é independente de qualquer objeto em uma cena que gera luz. A luz ambiente não contribui para a reflexão especular.

A luz direta é gerada por uma fonte em uma cena; sempre tem cor e a intensidade, além de se deslocar em uma direção especificada. A luz direta interage com o material de uma superfície para criar realces especulares e sua direção é usada como um fator em algoritmos de sombreamento, incluindo sombreamento de Gouraud. Quando a luz direta é refletida, ela não contribui para o nível de luz ambiente em uma cena. As fontes em uma cena que geram luz direta têm características diferentes que afetam como iluminam uma cena.

Além disso, o material do polígono tem propriedades que afetam como ele reflete a luz recebida. Você define uma única característica de reflexão que descreve como o material reflete a luz ambiente, além de definir características individuais para determinar a reflexão especulares e difusa do material.

## <a name="span-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspancolor-values-for-lights-and-materials"></a><span id="Color_Values_for_Lights_and_Materials"></span><span id="color_values_for_lights_and_materials"></span><span id="COLOR_VALUES_FOR_LIGHTS_AND_MATERIALS"></span>Valores de cores para luzes e materiais


O Direct3D descreve as cores em termos de quatro componentes (vermelho, verde, azul e alfa), que são combinadas para compor uma cor final. Cada componente varia de 0,0 a 1,0. Embora as luzes e os materiais usem a mesma estrutura para descrever a cor, os valores são usados de modo um pouco diferente pelas luzes e pelos materiais.

Os valores de cor para fontes de iluminação representam a quantidade de um componente de luz específico emitido. Como as luzes não usam um componente alfa, apenas os componentes de vermelhos, verdes e azuis da cor são relevantes. Você pode visualizar os três componentes como lentes vermelhas, verdes e azuis em uma televisão de projeção. Cada lente pode estar deslocada (por um valor de 0,0 no membro apropriado), poderá ser o mais brilhante possível (um valor de 1,0) ou pode estão em algum nível no meio.

As cores das lentes são combinadas para criar a cor da luz final. Uma combinação como R(1,0), G(1,0), B(1,0) cria uma luz branca, onde R(0,0) G(0,0), B(0,0), não emite luz. Você pode fazer uma luz que emite um único componente, resultando em uma luz de vermelha, verde ou azul pura; ou, a luz pode usar combinações para emitir cores como amarelo ou roxo. Você pode até mesmo definir valores de componentes negativos para criar uma "luz escura" que remove a luz de uma cena. Ou, você pode definir os componentes com algum valor maior do que 1,0 para criar uma luz muito clara.

Por outro lado, com os materiais, os valores de cor representam o quanto um componente de luz é refletido por uma superfície renderizada com esse material. Um material cujos componentes de cor são R(1,0), G(1,0), B(1,0), A(1,0) reflete a luz recebida. Da mesma forma, um material com R(0,0), G(1,0), B(0,0), A(1,0) reflete toda a luz verde direcionada a ele. Os materiais têm diversos valores de reflexão para criar vários tipos de efeitos.

Consulte [Tipos de luzes](light-types.md) e [Propriedades de luzes](light-properties.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Luzes e materiais](lights-and-materials.md)

 

 




