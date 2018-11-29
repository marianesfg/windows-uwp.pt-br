---
title: Filtragem bilinear de textura
description: A filtragem bilinear calcula a média ponderada dos 4 texels mais próximos do ponto de amostragem.
ms.assetid: 0851AD28-8246-4547-A663-47884DDDFC3E
keywords:
- Filtragem bilinear de textura
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 437650883b4782ca02c0daf24cc8ebed01d954f6
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7973999"
---
# <a name="bilinear-texture-filtering"></a>Filtragem bilinear de textura


A *filtragem bilinear* calcula a média ponderada dos 4 texels mais próximos do ponto de amostragem. Essa abordagem de filtragem é mais precisa e comum que a filtragem por ponto mais próximo. Essa abordagem é eficiente porque ela é implementado no hardware de gráficos moderno.


## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


Texturas são endereçadas sempre linearmente de (0,0, 0,0) no canto superior esquerdo até (1,0, 1,0) no canto inferior direito. O endereçamento linear de uma textura é mostrado na ilustração a seguir.

![ilustração de textura 4 x 4 com blocos sólidas de cor](images/bilinear-fig7a.png)

As texturas geralmente são representadas como se fossem compostas de blocos sólidos de cor, mas é mais correto pensar nas texturas da mesma maneira que você pensaria na exibição de rasterização: cada texel é definido no centro exato de uma célula de grade, conforme mostrado na ilustração a seguir.

![ilustração de textura 4 x 4 com texels definidos no centro das células de grade](images/bilinear-fig7b.png)

Se você solicitar a amostra de textura para a cor dessa textura nas coordenadas UV (0,375, 0,375), você receberá vermelho sólido (255, 0, 0). Isso faz sentido porque é o centro da célula texel vermelho em UV (0,375, 0,375). E se você pedir a amostra de cor da textura UV (0,25, 0,25)? Isso não é tão fácil, pois o ponto em UV (0,25, 0,25) está no canto exato de 4 texels.

O esquema mais simples é simplesmente pedir à amostra que retorne a cor do texel mais próximo; isso é chamado de ponto de filtragem (veja [Amostragem por ponto mais próximo](nearest-point-sampling.md)) e é geralmente indesejável devido a resultados granulados ou sólidos. A amostragem de pontos de nossa textura em UV (0,25, 0,25) mostra outro problema sutil com filtragem por ponto mais próximo: há quatro texels equidistantes do ponto de amostra, por isso não há um único texel mais próximo. Um desses quatro texels será escolhido como a cor retornada, mas a seleção depende de como a coordenada é arredondada, o que pode introduzir artefatos que causam danos (consulte o artigo sobre amostragem por ponto mais próximo no SDK).

Um esquema de filtragem ligeiramente mais preciso e comum é calcular a média ponderada dos 4 texels mais próximos ao ponto amostragem; isso é chamado de *filtragem bilinear*. O custo extra computacional para a filtragem bilinear é geralmente insignificante porque esta rotina é implementada em hardwares de gráfico modernos. Veja as cores que obtemos em alguns pontos de amostragem diferentes usando a filtragem bilinear:

```
UV: (0.5, 0.5)
```

Esse ponto fica localizado na borda exata entre os texels vermelhos, verdes, azuis e brancos. A cor retornada pela amostragem é cinza:

```
  0.25 * (255, 0, 0)
  0.25 * (0, 255, 0) 
  0.25 * (0, 0, 255) 
## + 0.25 * (255, 255, 255) 
------------------------
= (128, 128, 128)
```

```
UV: (0.5, 0.375)
```

Esse ponto fica localizado no ponto intermediário da borda entre os texels vermelhos e verdes. A cor retornada pela amostragem é amarelo-cinza (observe que as contribuições de texels azuis e brancos são dimensionadas como 0):

```
  0.5 * (255, 0, 0)
  0.5 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (128, 128, 0)
```

```
UV: (0.375, 0.375)
```

Esse é o endereço do texel vermelho, que é a cor retornada (todos os outros texels do cálculo de filtragem são ponderados para 0):

```
  1.0 * (255, 0, 0)
  0.0 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (255, 0, 0)
```

Compare esses cálculos com a ilustração a seguir, que mostra o que acontece se o cálculo de filtragem bilinear for executado em cada endereço de textura na textura de 4 x 4.

![Ilustração de textura 4 x 4 com filtragem bilinear realizada em todos os endereços de textura](images/bilinear-fig7c.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Filtragem de textura](texture-filtering.md)

 

 




