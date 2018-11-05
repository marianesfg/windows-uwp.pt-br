---
title: Visores e recorte
description: O visor é um retângulo (2D) bidimensional no qual uma cena 3D é projetada.
ms.assetid: D0DD646E-13AE-452A-AD22-8C35000D0BA9
keywords:
- Visores e recorte
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4dd319c686bebf2a30431017f399f48b08618cb6
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6030065"
---
# <a name="viewports-and-clipping"></a>Visores e recorte


O *visor* é um retângulo (2D) bidimensional no qual uma cena 3D é projetada. No Direct3D, o retângulo existe como coordenadas dentro de uma superfície do Direct3D que o sistema usa como um destino de renderização. A transformação da projeção converte os vértices no sistema de coordenadas usado para o visor. Um visor também é usado para especificar o intervalo de valores de profundidade em uma superfície de destino de renderização na qual uma cena será renderizada (geralmente 0.0 a 1.0).

## <a name="span-idtheviewingfrustumspanspan-idtheviewingfrustumspanspan-idtheviewingfrustumspanthe-viewing-frustum"></a><span id="The_Viewing_Frustum"></span><span id="the_viewing_frustum"></span><span id="THE_VIEWING_FRUSTUM"></span>Tronco de exibição


Um tronco de exibição é o volume 3D em uma cena posicionado com relação à câmera do visor. A forma do volume afeta como os modelos são projetados do espaço da câmera na tela. O tipo mais comum de projeção, uma projeção de perspectiva, é responsável por fazer com que os objetos próximos da câmera pareçam maiores que os objetos distantes. Para a exibição de perspectiva, o tronco de exibição pode ser visualizado como uma pirâmide, com a câmera posicionada na ponta conforme mostrado na ilustração a seguir. Esta pirâmide é intersectada por um plano de recorte traseiro e frontal. O volume na pirâmide entre aos planos de recorte dianteiro e traseiro é o tronco de exibição. Os objetos ficam visíveis somente quando estão nesse volume.

![ilustração de um tronco de exibição com um plano de recorte traseiro e frontal](images/frustum.png)

Se você imaginar que está parado em uma sala escura e olhando por uma janela quadrada, estará visualizando um tronco de exibição. Nessa analogia, o plano de recorte próximo é a janela, e o plano de recorte de fundo é tudo o que finalmente interrompe sua visão – os arranha-céus do outro lado da rua, as montanhas distantes, ou nada. Você pode ver tudo dentro da pirâmide truncada que começa na janela e termina com o que quer que interrompa sua visão, impedindo sua visão.

O tronco de exibição é definido por fov (campo de visão) e por distâncias dos planos de recorte traseiro e frontal, especificados em coordenadas de z, conforme mostrado no diagrama a seguir.

![diagrama do tronco de exibição](images/fovdiag.png)

Neste diagrama, a variável D é a distância da câmera até a origem do espaço que foi definido na última parte do pipeline de geometria - a transformação de exibição. Este é o espaço em torno do qual você organiza os limites de seu tronco de exibição. Para obter informações sobre como essa variável D é usada para criar a matriz de projeção, consulte a [Transformação de projeção](projection-transform.md)

## <a name="span-idviewportrectanglespanspan-idviewportrectanglespanspan-idviewportrectanglespanviewport-rectangle"></a><span id="Viewport_Rectangle"></span><span id="viewport_rectangle"></span><span id="VIEWPORT_RECTANGLE"></span>Retângulo do visor


Uma estrutura de visor contém quatro membros (X, Y, largura, altura) que definem a área da superfície do destino de renderização na qual uma cena será renderizada. Esses valores correspondem ao retângulo de destino ou retângulo do visor, conforme mostrado no diagrama a seguir.

![diagrama do retângulo do visor](images/destrect.png)

Os valores que você especificar para os membros X, Y, largura, altura são coordenadas de tela relativas ao canto superior esquerdo da superfície do destino de renderização. A estrutura define dois membros adicionais (MinZ e MaxZ) que indicam os intervalos de profundidade nos quais a cena será renderizada.

O Direct3D pressupõe que o volume de recorte do visor varia de -1,0 a 1,0 em X e de 1,0 a -1,0 em Y. Essas eram as configurações usadas mais frequentemente por aplicativos no passado. Você pode ajustar a taxa de proporção do visor antes de recortar usando a [transformação da projeção](projection-transform.md).

**Observação**  MinZ e MaxZ indicam os intervalos de profundidade nos quais a cena será renderizada e não são usados para recorte. A maioria dos aplicativos irá definir esses valores para 0,0 e 1,0 para permitir que o sistema renderize todo o intervalo de valores de profundidade no buffer de profundidade. Em alguns casos, você pode conseguir efeitos especiais usando outros intervalos de profundidade. Por exemplo, para renderizar um painel transparente em um jogo, você pode definir os dois valores como 0,0 para forçar o sistema a renderizar objetos em uma cena em primeiro plano, ou você pode defini-los como 1,0 para renderizar um objeto que sempre deve estar em segundo plano.

 

A dimensão usadas nos membros X, Y, largura e altura de uma estrutura de visor definem o local e as dimensões do visor na superfície do destino de renderização. Esses valores são dados em coordenadas de tela, relativas ao canto superior esquerdo da superfície.

O Direct3D usa a localização do visor e as dimensões para dimensionar os vértices para inserir uma cena renderizada no local apropriado na superfície de destino. Internamente, o Direct3D insere esses valores na matriz a seguir, que é aplicada a cada vértice.

![equação da matriz que é aplicada a cada vértice](images/vpscale.png)

Essa matriz dimensiona os vértices de acordo com a dimensão do visor e o intervalo de profundidade desejado, e os move para o local apropriado na superfície do destino de renderização. A matriz também gira a coordenada y para refletir uma origem de tela no canto superior esquerdo com y aumentando para baixo. Depois que essa matriz é aplicada, os vértices continuam homogêneos - ou seja, eles ainda existem como vértices \[x,y,z,w\] - e eles devem ser convertidos em coordenadas não homogêneas antes de serem enviados para o rasterizador.

**Observação**  aplicativos normalmente definem MinZ e MaxZ para 0,0 e 1,0, respectivamente, para fazer com que o sistema renderize o intervalo de profundidade inteiro. No entanto, você pode usar outros valores para alcançar determinados efeitos. Por exemplo, você pode definir os dois valores como 0,0 para forçar todos os objetos no primeiro plano ou definir ambos como 1,0 para renderizar todos os objetos em segundo plano.

 

## <a name="span-idclearingaviewportspanspan-idclearingaviewportspanspan-idclearingaviewportspanclearing-a-viewport"></a><span id="Clearing_a_Viewport"></span><span id="clearing_a_viewport"></span><span id="CLEARING_A_VIEWPORT"></span>Limpar um visor


Limpar o visor redefine o conteúdo do retângulo do visor na superfície do destino de renderização. Ele também pode limpar o retângulo nas superfícies de buffer de estêncil e de profundidade.

## <a name="span-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanset-up-the-viewport-for-clipping"></a><span id="Set_Up_the_Viewport_for_Clipping"></span><span id="set_up_the_viewport_for_clipping"></span><span id="SET_UP_THE_VIEWPORT_FOR_CLIPPING"></span>Configurar o visor para o recorte


Os resultados da matriz de projeção determinam o volume de recorte no espaço de projeção:

-w<sub>c</sub>&lt;= x<sub>c</sub>&lt;= w<sub>c</sub>

-w<sub>c</sub>&lt;= y<sub>c</sub>&lt;= w<sub>c</sub>

0 &lt;= z<sub>c</sub>&lt;= w<sub>c</sub>

Onde: x, y, z e w representam as coordenadas de vértice depois que a transformação da projeção é aplicada. Qualquer vértice que tiver um componente x, y ou z fora desses intervalos será recortado, se o recorte estiver habilitado (comportamento padrão).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




