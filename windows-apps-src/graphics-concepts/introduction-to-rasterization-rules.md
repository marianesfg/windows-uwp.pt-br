---
title: "Introdução às regras de rasterização"
description: "Muitas vezes, os pontos especificados para vértices não correspondem exatamente os pixels na tela. Quando isso ocorre, o Direct3D aplica as regras de rasterização de triângulo para decidir quais pixels se aplicam a um determinado triângulo."
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- "Introdução às regras de rasterização"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b7814c01d18e7a5ef121d151438a25921f35aaf2
ms.lasthandoff: 02/07/2017

---

# <a name="introduction-to-rasterization-rules"></a>Introdução às regras de rasterização


Muitas vezes, os pontos especificados para vértices não correspondem exatamente os pixels na tela. Quando isso ocorre, o Direct3D aplica as regras de rasterização de triângulo para decidir quais pixels se aplicam a um determinado triângulo.

Esta é uma introdução simplificada às regras de rasterização. Para obter mais detalhes, consulte [Regras de rasterização](rasterization-rules.md). Consulte também [Estágio do rasterizador Rasterizer (RS)](rasterizer-stage--rs-.md).

## <a name="span-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>Regras de rasterização de triângulo


O Direct3D usa uma convenção de preenchimento superior esquerdo para preencher a geometria. Essa é a mesma convenção utilizada para retângulos em GDI e OpenGL. No Direct3D, o centro do pixel é o ponto determinante. Se o centro está dentro de um triângulo, o pixel é parte do triângulo. Os centros de pixel estão localizados em coordenadas de números inteiros.

Essa descrição das regras de rasterização de triângulos utilizada pelo Direct3D não se aplica necessariamente a todo o hardware disponível. O teste pode revelar pequenas variações na implementação dessas regras.

A ilustração a seguir mostra um retângulo cujo canto superior esquerdo está em (0, 0) e o canto inferior direito em (5, 5). Esse retângulo preenche 25 pixels, como esperado. A largura do retângulo é definida como direita menos esquerda. A altura é definida como parte inferior menos superior.

![um quadrado numerado dividido em seis linhas e colunas](images/pixmap.png)

Na convenção de preenchimento superior esquerdo, *superior* refere-se à localização vertical dos trechos horizontais e *esquerdo* refere-se à localização horizontal dos pixels em uma extensão. Uma borda não pode ser superior, a menos que seja horizontal. Em geral, a maioria dos triângulos tem somente as margens esquerda e direita. A ilustração a seguir mostra uma borda superior e uma borda direita.

![um quadrado numerado com dois triângulos](images/triedge.png)

A convenção de preenchimento superior esquerdo determina a ação executada pelo Direct3D quando um triângulo passa pelo centro de um pixel. A ilustração a seguir mostra dois triângulos, um em (0, 0), (5, 0) e (5, 5) e o outro em (0, 5), (0, 0) e (5, 5). O primeiro triângulo nesse caso tem 15 pixels (mostrado em preto), enquanto o segundo tem apenas 10 pixels (mostrado em cinza), pois a borda compartilhada é a esquerda do primeiro triângulo.

![um quadrado numerado que mostra dois triângulos](images/twotris.png)

Se você definir um retângulo com o canto superior esquerdo em (0,5; 0,5) e o canto inferior direito em (2,5; 4,5), o ponto central desse retângulo é em (2,5; 1,5). Quando o rasterizador Direct3D criar mosaicos com esse retângulo, o centro de cada pixel está especificamente dentro de cada um dos quatro triângulos e não requer a convenção de preenchimento superior esquerdo. A ilustração a seguir mostra isso. Os pixels do retângulo são identificados de acordo com o triângulo no qual o Direct3D os inclui.

![um quadrado numerado com um retângulo dividido em quatro triângulos](images/noambig.png)

Se você mover o retângulo na ilustração anterior para que o canto superior esquerdo fique em (1,0; 1,0), o canto inferior direito em (3,0; 5,0) e o centro aponte para (2,0; 3,0), o Direct3D aplica a convenção de preenchimento superior esquerdo. A maioria dos pixels desse retângulo amplia a fronteira entre dois ou mais triângulos, como demonstrado na ilustração a seguir.

![um quadrado numerado com um retângulo dividido em quatro triângulos](images/fillrule.png)

Para os dois retângulos, os mesmos pixels são afetados, conforme mostrado na ilustração a seguir.

![pixels afetados pelos dois quadrados numerados anteriores](images/samepix.png)

## <a name="span-idpointandlinerulesspanspan-idpointandlinerulesspanspan-idpointandlinerulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>Regras de ponto e linha


Os pontos são renderizados igual aos sprites de ponto, que são renderizado como quadrilaterais alinhado à tela e, portanto, seguem as mesmas regras de renderização do polígono.

As regras de renderização de linha não as mesmas para [linhas de GDI](https://msdn.microsoft.com/library/windows/desktop/dd145027).

## <a name="span-idpointspriterulesspanspan-idpointspriterulesspanspan-idpointspriterulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Regras de sprite de ponto


Os sprites de ponto e os primitivos de patch são rasterizados como se os primitivos fossem transformados em triângulos antes e os triângulos resultante fossem rasterizados.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Dispositivos](devices.md)

[Estágio do rasterizador (RS)](rasterizer-stage--rs-.md)

[Regras de rasterização](rasterization-rules.md)

 

 





