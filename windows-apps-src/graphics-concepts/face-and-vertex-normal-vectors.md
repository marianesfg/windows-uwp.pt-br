---
title: Face e vetores normais de vértice
description: Cada rosto em uma malha tem um vetor normal de unidade perpendicular. Direção do vetor é determinada pela ordem em que os vértices são definidos, e se o sistema de coordenadas é orientado à direita ou esquerda.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- Face e vetores normais de vértice
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2347efc5d68abd53442f52ecabdc060393ee561b
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929887"
---
# <a name="face-and-vertex-normal-vectors"></a>Face e vetores normais de vértice


Cada rosto em uma malha tem um vetor normal de unidade perpendicular. Direção do vetor é determinada pela ordem em que os vértices são definidos, e se o sistema de coordenadas é orientado à direita ou esquerda.

## <a name="span-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Vetor normal de unidade perpendicular para uma face frontal


Cada rosto em uma malha tem um vetor normal de unidade perpendicular. Direção do vetor é determinada pela ordem em que os vértices são definidos, e se o sistema de coordenadas é orientado à direita ou esquerda. A face normal aponta para longe do lado frontal da face. No Direct3D, somente a frente de um rosto fica visível. Uma face frontal é aquela na qual os vértices estão definidos em ordem no sentido horário.

A ilustração a seguir mostra um vetor normal para uma face frontal:

![um vetor normal para uma face frontal](images/nrmlvect.png)

## <a name="span-idcullingbackfacesspanspan-idcullingbackfacesspanspan-idcullingbackfacesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>Remoção de faces traseiras


Qualquer face que não é uma face frontal é uma face traseira. O Direct3D nem sempre renderiza faces traseiras; as faces traseiras normalmente são removidas. Remover a face traseira significa eliminar as faces traseiras da renderização. Você pode alterar o modo de remoção para renderizar faces traseiras se desejar. Consulte [Estado de Remoção](https://msdn.microsoft.com/library/windows/desktop/bb204882) para obter mais informações.

## <a name="span-idvertexunitnormalsspanspan-idvertexunitnormalsspanspan-idvertexunitnormalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>Normais de unidade de vértice


O Direct3D usa as normais de unidade de vértice para os efeitos de sombreamento Gouraud, iluminação e texturização.

A ilustração a seguir mostra normais de vértice:

![normais de vértice](images/vertnrml.png)

Ao aplicar o sombreamento Gourard em um polígono, o Direct3D usa as normais de vértice para calcular o ângulo entre a fonte de luz e a superfície. Ele calcula os valores de cor e intensidade para os vértices e os interpola para cada ponto entre todas as superfícies do primitivo. O Direct3D calcula o valor de intensidade de luz, usando o ângulo. Quanto maior o ângulo, menos a luz chega à superfície.

## <a name="span-idflatsurfacesspanspan-idflatsurfacesspanspan-idflatsurfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>Superfícies planas


Se você estiver criando um objeto plano, defina as normais de vértice para apontar perpendicular à superfície.

A ilustração a seguir mostra que uma superfície plana composta de dois triângulos com normais de vértice:

![superfície plana composto de dois triângulos com normais de vértice](images/flatvert.png)

## <a name="span-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>Sombreamento suave em um objeto não plano


Em vez de um objeto plano, é mais provável que seu objeto seja composto de faixas de triângulos e os triângulos não sejam coplanares. Uma maneira simples de alcançar o sombreamento suave entre todos os triângulos na faixa é primeiro calcular o vetor normal de superfície para cada face poligonal à qual o vértice está associado. A normal de vértice pode ser definida para fazer um ângulo igual com cada superfície normal. No entanto, esse método talvez não seja eficiente o suficiente para primitivos complexos.

Esse método é ilustrado pelo diagrama a seguir, que mostra duas superfícies, S1 e S2 vistas a partir de cima. Os vetores normais de S1 e S2 são mostrados em azul. O vetor normal de vértice é mostrado em vermelho. O ângulo que o vetor normal de vértice faz com a superfície normal de S1 é igual ao ângulo entre a normal de vértice e a normal de superfície de S2. Quando essas duas superfícies são iluminadas e sombreadas com sombreamento Gouraud, o resultado é uma borda suavemente sombreada, suavemente arredondada entre eles.

A ilustração a seguir mostra duas superfícies (S1 e S2) e seus vetores normais e o vetor normal de vértice:

![duas superfícies (s1 e s2) e seus vetores normais e o vetor normal de vértice](images/gvert.png)

Se a normal de vértice estiver em direção a uma das faces com a qual ela está associada, ela fará com que a intensidade de luz aumente ou diminua para os pontos nessa surface, dependendo do ângulo que ela faz com a fonte de luz. O diagrama a seguir mostra um exemplo. Essas superfícies são vistas de cima. A normal de vértice está na direção C1, fazendo com que ela tenha um ângulo menor com a fonte de luz do que se a normal de vértice tivesse ângulos iguais com as normais de superfície.

A ilustração a seguir mostra duas superfícies (S1 e S2) com um vetor normal de vértice que está na direção de uma face:

![duas superfícies (s1 e s2) com um vetor normal de vértice que está na direção de uma face](images/gvert2.png)

## <a name="span-idsharpedgesspanspan-idsharpedgesspanspan-idsharpedgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>Bordas afiadas


Você pode usar o sombreamento Gouraud para exibir alguns objetos em uma cena 3D com bordas afiadas. Para fazer isso, duplique os vetores de normal de vértice em qualquer interseção de faces onde uma borda afiada é necessária.

A ilustração a seguir mostra os vetores de normal de vértice duplicados nas bordas afiadas:

![vetores de normal de vértice duplicados nas bordas afiadas](images/shade1.png)

Se você usar os métodos DrawPrimitive para renderizar a cena, defina o objeto com bordas afiadas como uma lista de triângulos, em vez de uma faixa de triângulos. Quando você define um objeto como uma faixa triangular, o Direct3D trata isso como um único polígono composto de várias faces triangulares. O sombreamento Gouraud é aplicado em cada face do polígono e entre as faces adjacentes.

O resultado é um objeto que é suavemente sombreado de face para face. Como uma lista de triângulos é um polígono composto de uma série de faces triangulares separadas, o Direct3D aplica o sombreamento Gouraud em cada face do polígono. No entanto, ele não é aplicado de face para face. Se dois ou mais triângulos de uma lista de triângulos forem adjacentes, eles aparentam ter uma borda afiada entre eles.

Outra alternativa é alterar para o sombreamento simples ao renderizar objetos com bordas afiadas. Computacionalmente, este é o método mais eficiente, mas isso resultar em objetos na cena que não são renderizados de forma tão realista quanto os objetos que estão com sombreamento Gouraud.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




