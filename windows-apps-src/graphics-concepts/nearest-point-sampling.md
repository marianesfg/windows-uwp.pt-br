---
title: Exemplo do ponto mais próximo
description: Os apps não são obrigados a usar filtragem de textura.
ms.assetid: D7F88320-2C61-47E9-9B92-EC31D48DB079
keywords:
- Exemplo do ponto mais próximo
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 965660f51efc914f7ca21a5bf747fe809319eff9
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6274171"
---
# <a name="span-iddirect3dconceptsnearest-pointsamplingspannearest-point-sampling"></a><span id="direct3dconcepts.nearest-point_sampling"></span>Exemplo do ponto mais próximo


Os apps não são obrigados a usar filtragem de textura. O Direct3D pode ser configurado para calcular o endereço do texel, que geralmente não é avaliado como inteiro, e copia a cor de texel com o endereço do inteiro mais próximo. Esse processo é chamado de *exemplo do ponto mais próximo*. A amostragem do ponto próximo pode ser uma maneira rápida e eficiente para processar texturas se o tamanho da textura for semelhante ao tamanho da imagem do primitivo na tela. Caso contrário, a textura deve ser ampliada ou minificada. O resultado de incompatibilidade de tamanhos de textura para o tamanho da imagem do primitivo pode ser uma imagem truncada, com alias ou desfocada.

Use o exemplo do ponto mais próximo com cuidado, pois, às vezes, podem causar artefatos gráficos quando a textura é amostrada com o limite entre os dois texels. Esse limite é a posição ao longo da textura (u ou v) em que as transições de texel amostradas fazem as transições de um texel para o próximo. Quando a amostragem de pontos é usada, o sistema escolhe uma amostra texel ou a outra, e o resultado pode mudar abruptamente de um texel para o outro à medida que o limite é ultrapassado. Esse efeito pode aparecer como artefatos gráficos indesejados na textura exibida. Quando a filtragem linear for usada, o texel resultante é calculado a partir de ambos os texels adjacentes e se combina perfeitamente entre eles à medida que o índice de textura percorre o limite.

Esse efeito pode ser observado ao mapear uma textura muito pequena para um polígono muito grande: uma operação geralmente chamada de ampliação. Por exemplo, ao usar uma textura que se parece com um quadriculado, o exemplo de ponto mais próximo resulta em um quadriculado maior com bordas distintas. Por outro lado, a filtragem de textura linear resulta em uma imagem em que o quadriculado varia no polígono.

Na maioria dos casos, os apps recebem os melhores resultados ao evitar o exemplo de ponto mais próximo sempre que possível. A maioria do hardware atual é otimizado para filtragem linear, portanto, o app não deve ter o desempenho prejudicado. Se o efeito desejado exigir o uso do exemplo de ponto próximo, como ao usar texturas para exibir caracteres de texto legível, então o app deve ter cuidado para evitar a amostragem nos limites de texel, o que pode resultar em efeitos indesejados. A ilustração a seguir mostra a possível aparência desses artefatos.

![ilustração de uma caixa divididas em seções seis com linhas horizontais não contínuas nos dois quadrados superiores à direita](images/ptrtfct.png)

Dois quadrados no canto superior direito do grupo parecem diferentes de seus vizinhos, com deslocamentos diagonais neles. Para evitar artefatos gráficos como estes, você deve estar familiarizado com as regras de amostragem de textura do Direct3D para filtragem do ponto mais próximo. O Direct3D mapeia uma coordenada de textura de ponto flutuante desde \[0,0; 1,0\] (0,0 a 1,0 inclusive) com um valor de espaço de texel inteiro desde \ [- 0,5; n - 0,5\], onde n é o número de texels em uma determinada dimensão na textura. O índice de textura resultante é arredondado para o inteiro mais próximo. Esse mapeamento pode introduzir imprecisões de amostragem nos limites de texel.

Para obter um exemplo simples, imagine um app que renderiza polígonos com o Modo de endereçamento de textura de encapsulamento. Usando o mapeamento empregado por Direct3D, o índice de textura u é mapeado como mostrado no diagrama a seguir para uma textura com 4 texels de largura.

![diagrama de coordenadas de textura 0,0 e 1,0 no limite entre texels](images/ptsmpprb.png)

As coordenadas de textura 0,0 e 1,0 da ilustração estão exatamente no limite entre texels. Usando o método pelo qual o Direct3D mapeia os valores, as coordenadas de textura variam de \ [- 0,5; 4 - 0,5\], onde 4 é a largura da textura. Nesse caso, a amostra de texel é o texel 0 para um índice de textura de 1,0. No entanto, se a coordenada de textura foi somente um pouco menor do que 1,0, a amostra de texel seria o texel n em vez do texel 0.

A implicação disso é que ao ampliar uma pequena textura usando coordenadas de textura de 0,0 e 1,0 com filtragem de ponto mais próximo ponto triângulo alinhado no espaço da tela resulta em pixels para os quais é feita a amostragem do mapa de textura com o limite entre texels do limite. Qualquer imprecisões no cálculo de coordenadas de textura, porém pequenos, resulta em artefatos ao longo as áreas em que a imagem renderizada que correspondem até as bordas de texel do mapa de textura.

O mapeamento de coordenadas de textura de ponto flutuante em texels inteiros com precisão é difícil, requer tempo para os cálculos e, em geral, não é necessário. A maioria das implementações de hardware usa uma abordagem iterativa para calcular as coordenadas de textura em cada local de pixel em um triângulo. As abordagens iterativas tendem a ocultar esses imprecisões porque os erros são acumulados de modo uniforme durante a iteração.

O rasterizador de referência do Direct3D usa uma abordagem de avaliação direta para o cálculos dos índices de textura em cada local de pixel. A avaliação direta difere da abordagem iterativa em que qualquer imprecisão na operação apresenta uma distribuição de erro mais aleatória. O resultado é que os erros de amostragem nos limites podem ser mais perceptíveis, pois o rasterizador de referência não executa essa operação com precisão.

A melhor abordagem é usar a filtragem de ponto mais próximo somente quando necessário. Quando for necessário deve usá-la, é recomendável deslocar as coordenadas de textura das posições de limite para evitar artefatos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Filtragem de textura](texture-filtering.md)

 

 




