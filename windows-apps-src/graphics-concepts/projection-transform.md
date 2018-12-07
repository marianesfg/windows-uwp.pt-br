---
title: Transformação da projeção
description: A transformação da projeção controla o interior da câmera, como a escolha de uma lente para uma câmera. Esse é o mais complicado dos três tipos de transformação.
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- Transformação da projeção
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f0806c0aa7a130a080457f4361d17f64451846f9
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8800851"
---
# <a name="projection-transform"></a>Transformação da projeção


A *transformação da projeção* controla as partes internas da câmera, como escolher uma lente de uma câmera. Esse é o mais complicado dos três tipos de transformação.

A matriz de projeção costuma ser uma projeção de perspectiva e escala. A transformação da projeção converte o tronco de exibição em uma forma cuboide. Como a parte perto do final do tronco de exibição é menor do que a extremidade oposta, isso tem o efeito de expandir objetos próximos da câmera; é assim que a perspectiva é aplicada à cena.

No [tronco de exibição](viewports-and-clipping.md), a distância entre a câmera e a origem do espaço de transformação da exibição é definido arbitrariamente como D. Portanto, a matriz de projeção se parece com a ilustração a seguir.

![ilustração da matriz de projeção](images/projmat1.png)

A matriz de visualização converte a câmera para a origem por meio da translação na direção z por - D. A matriz de translação é como a ilustração a seguir.

![ilustração da matriz de translação](images/projmat2.png)

Multiplicar a matriz de translação pela matriz de projeção (T\*P) resulta na matriz de projeção composta, conforme mostrado na ilustração a seguir.

![ilustração da matriz de projeção composta](images/projmat3.png)

A transformação de perspectiva converte um tronco de exibição em um novo espaço de coordenadas. Observe que o tronco se torna cuboide e também que a origem se move do canto superior direito da cena para o centro, conforme mostrado no diagrama a seguir.

![diagrama de como a transformação de perspectiva muda o tronco de exibição em um novo espaço de coordenadas](images/cuboid.png)

Na transformação de perspectiva, os limites das direções x e y são -1 e 1. Os limites da direção z são 0 para o plano frontal e 1 para o plano de fundo.

Essa matriz faz a translação e dimensiona objetos com base em uma distância especificada da câmera ao plano de recorte próximo, mas não considera o campo de visão (fov), e os valores de z que produz para objetos na distância podem ser praticamente idênticos, dificultando comparações de profundidade. A tabela a seguir aborda esses problemas e ajusta vértices para levar em conta a taxa de proporção do visor, sendo uma boa opção para a projeção de perspectiva.

![ilustração de uma matriz para a projeção de perspectiva](images/prjmatx1.png)

Nessa matriz, Zₙ é o valor de z do plano de recorte próximo. As variáveis w, h e Q têm os significados a seguir. Observe que fov<sub>w</sub> e fovₖ representam os campos de visão horizontal e vertical do visor em radianos.

![equações dos significados variáveis](images/prjmatx2.png)

Para seu app, usar os ângulos de campo de visão para definir os coeficientes dimensionamento de x e y pode não ser tão conveniente quanto usar as dimensões horizontais e verticais do visor (no espaço da câmera). Após resolução da matemática, as duas equações a seguir para w e h usam dimensões do visor e são equivalentes às equações anteriores.

![equações dos significados variáveis de w e h](images/prjmatx3.png)

Nessas fórmulas, Zₙ representa a posição do plano de recorte próximo e o V<sub>w</sub> e variáveis Vₕ representam a largura e altura do visor no espaço da câmera.

Seja qual fórmula você decidir usar, lembre-se de definir Zₙ para o maior valor possível, pois os valores de z extremamente próximos da câmera não variam muito. Isso dificulta comparações de profundidade usando buffers de z de 16 bits um pouco complicados.

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>Uma matriz de projeção fácil para w


O Direct3D pode usar o componente de w de um vértice que foi transformado pelas matrizes de mundo, exibição e projeção para realizar cálculos baseados em profundidade em buffer de profundidade ou efeitos de nevoeiro. Cálculos como esses exigem que sua matriz de projeção normalize w para equivaler ao z do espaço do mundo. Em poucas palavras, se sua matriz de projeção inclui um coeficiente (3,4) que não é 1, todos os coeficientes devem ser dimensionados pelo inverso do coeficiente (3,4) para criar uma matriz adequada. Se você não fornecer uma matriz em conformidade, efeitos de nevoeiro e buffering de profundidade não serão aplicados corretamente.

A ilustração a seguir mostra uma matriz de projeção incompatível e a mesma matriz dimensionada de forma que o nevoeiro relativo aos olhos seja habilitado.

![ilustrações de uma matriz de projeção incompatível e uma matriz com nevoeiro relativo aos olhos](images/eyerlmx.png)

Nas matrizes anteriores, todas as variáveis são consideradas diferente de zero. Para obter informações sobre o buffer de profundidade com base em w, consulte [Buffers de profundidade](depth-buffers.md).

Direct3D usa matriz de projeção atual definida em seus cálculos de profundidade com base em w. Como resultado, os apps devem definir uma matriz de projeção em conformidade para receber os recursos desejados com base em w, mesmo que eles não usem Direct3D para transformações.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Transformações](transforms.md)

 

 




