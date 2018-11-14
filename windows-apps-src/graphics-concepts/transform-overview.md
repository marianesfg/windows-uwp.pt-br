---
title: Visão geral da transformação
description: As transformações de matriz lidam com muita a matemática de nível inferior dos gráficos 3D.
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32f55b0a387221b792e37072f129edddf285195b
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6656979"
---
# <a name="transform-overview"></a>Visão geral da transformação


As transformações de matriz lidam com muita a matemática de nível inferior dos gráficos 3D.

O pipeline de geometria leva vértices como entrada. O mecanismo de transformação aplica as transformações do mundo, modo de exibição e projeção para os vértices, recorta o resultado e passa tudo para o rasterizador.

| Transformação e espaço                           | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Coordenadas de modelo no espaço do modelo              | No topo do pipeline, os vértices de um modelo são declarados em relação a um sistema de coordenadas local. Isso é uma origem local e uma orientação. Essa orientação de coordenadas geralmente é conhecida como *espaço do modelo*. Coordenadas individuais são chamadas de *coordenadas de modelo*.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Transformação do mundo no espaço do mundo              | O primeiro estágio do pipeline de geometria transforma os vértices de um modelo de seu sistema de coordenadas local em um sistema de coordenadas usado por todos os objetos em uma cena. O processo de reorientação dos vértices é chamado de [transformação de mundo](world-transform.md), que converte do espaço do modelo para uma nova orientação chamada de *espaço do mundo*. Cada vértice no espaço do mundo é declarado usando *coordenadas do mundo*.                                                                                                                                                                                                                                                                                                                           |
| Transformação da exibição no espaço de exibição (espaço da câmera) | No próximo estágio, os vértices que descrevem o mundo 3D são orientados com relação a uma câmera. Ou seja, seu aplicativo escolhe um ponto de vista da cena, e as coordenadas de espaço do mundo são realocadas e rotacionadas em torno do modo de exibição da câmera, convertendo o espaço do mundo em *espaço de exibição* (também conhecido como *espaço da câmera*). Esta é a [Transformação de exibição](view-transform.md), que converte faz a conversão do espaço do mundo em espaço de exibição.                                                                                                                                                                                                                                                                                                                        |
| Transformação da projeção em espaço de projeção    | O próximo estágio é a [Transformação da projeção](projection-transform.md), que converte o espaço do modo de exibição em espaço de projeção. Nesta parte do pipeline, os objetos são geralmente dimensionados em relação à sua distância do espectador para dar a ilusão de profundidade para uma cena; objetos mais próximos parecem estar maiores que os objetos distantes. Para fins de simplicidade, esta documentação refere-se ao espaço no qual os vértices existem após a transformação de projeção como *espaço de projeção*. Alguns livros de elementos gráficos podem se referir ao espaço de projeção como *espaço homogêneo de pós-perspectiva*. Nem todas as transformações de projeção dimensionam o tamanho dos objetos em uma cena. Uma projeção como essa às vezes é chamada de um *afim* ou *projeção ortogonal*. |
| Recorte no espaço da tela                      | Na parte final do pipeline, qualquer vértice que não ficará visível na tela é removido, para que o rasterizador não reserve um tempo para calcular as cores e o sombreamento para algo que jamais será visto. Esse processo é denominado *recorte*. Depois de recorte, os vértices restantes são dimensionados de acordo com os parâmetros do visor e convertidos em coordenadas da tela. Os vértices resultantes, vistos na tela quando a cena é rasterizada, existem no *espaço de tela*.                                                                                                                                                                                                                                                    |

 

As transformações são usadas para converter a geometria de objeto de um espaço de coordenadas em outro. O Direct3D usa matrizes para realizar transformações 3D. As matrizes criar transformações 3D. Você pode combinar as matrizes para produzir uma matriz única que abrange várias transformações.

Você pode transformar coordenadas entre o espaço do modelo, espaço do mundo e o modo de exibição de espaço.

-   [Transformação de mundo](world-transform.md) - Converte o espaço do modelo em espaço do mundo.
-   [Transformação de exibição](view-transform.md) - Converte o espaço do mundo em espaço do modo de exibição.
-   [Transformação de projeção](projection-transform.md) - Converte o espaço do modo de exibição em espaço de projeção.

## <a name="span-idmatrixtransformsspanspan-idmatrixtransformsspanspan-idmatrixtransformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>Transformações de matriz


Em aplicativos que trabalham com elementos gráficos 3D, você pode usar transformações geométricas para fazer o seguinte:

-   Expressar a localização de um objeto em relação a outro objeto.
-   Rotacionar e dimensionar objetos
-   Alterar as posições de visualização, trajetos e perspectivas.

Você pode transformar qualquer ponto (x, y, z) em outro ponto (x', y', z'), usando uma matriz 4x4, conforme mostrado na seguinte equação.

![equação para transformar qualquer ponto em outro ponto](images/matmult.png)

Execute as seguintes equações em (x, y, z) e na matriz para produzir o ponto (x', y', z').

![equações para o novo ponto](images/matexpnd.png)

As transformações mais comuns são conversão, rotação e dimensionamento. Você pode combinar as matrizes que produzem esses efeitos em uma única matriz para calcular várias transformações ao mesmo tempo. Por exemplo, você pode criar uma matriz única para traduzir e girar uma série de pontos.

Matrizes são escritas na ordem das colunas de linha. Uma matriz que dimensiona uniformemente os vértices em cada eixo, conhecida como dimensionamento uniforme, é representada pela matriz de seguir usando a notação matemática.

![equação de uma matriz de dimensionamento uniforme](images/matrix.png)

No C++, o Direct3D declara matrizes como uma matriz bidimensional, usando uma estrutura de matriz. O exemplo a seguir mostra como inicializar uma estrutura [**D3DMATRIX**](https://msdn.microsoft.com/library/windows/desktop/bb172573) para atuar como uma matriz de dimensionamento uniforme (fator de escala "s").

```
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>Translação


A seguinte equação traduz o ponto (x, y, z) para um novo ponto (x', y', z').

![equação de uma matriz de translação para um novo ponto](images/transl8.png)

Você pode criar manualmente uma matriz de translação em C++. O exemplo a seguir mostra o código-fonte para uma função que cria uma matriz para traduzir vértices.

```
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>Escala


A seguinte equação dimensiona o ponto (x, y, z) por valores arbitrários nas direções x, y e z para um novo ponto (x', y', z').

![equação de uma matriz de escala para um novo ponto](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>Girar


As transformações descritas aqui destinam-se aos sistemas de coordenadas à esquerda e, portanto, podem ser diferentes das matrizes de transformação que você já viu em outro lugar.

A seguinte equação rotaciona o ponto (x, y, z) em torno do eixo x, produzindo um novo ponto (x', y', z').

![equação de uma matriz de rotação x para um novo ponto](images/matxrot.png)

A seguinte equação gira o ponto em torno do eixo y.

![equação de uma matriz de rotação y para um novo ponto](images/matyrot.png)

A seguinte equação gira o ponto em torno do eixo z.

![equação de uma matriz de rotação z para um novo ponto](images/matzrot.png)

Nessas matrizes de exemplo, a letra grega "teta" significa o ângulo de rotação, em radianos. Os ângulos são medidos no sentido horário olhando ao longo do eixo de rotação em direção à origem.

O código a seguir mostra uma função para manipular a rotação sobre o eixo X.

```
    // Inputs are a pointer to a matrix (pOut) and an angle in radians.
    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## <a name="span-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>Concatenação de matrizes


Uma vantagem de usar matrizes é que você pode combinar os efeitos de duas ou mais matrizes multiplicando-as. Isso significa que, para girar um modelo e, em seguida, convertê-lo em um local, você não precisa aplicar duas matrizes. Em vez disso, multiplique as matrizes de rotação e conversão para produzir uma matriz de composição que contém todos os seus efeitos. Esse processo, chamado de concatenação de matriz, pode ser escrito com a seguinte equação.

![equação da concatenação de matriz](images/matrxcat.png)

Nesta equação, C é a matriz de composição que está sendo criada e M₁ por Mₙ são as matrizes individuais. Na maioria dos casos, apenas duas ou três matrizes são concatenadas, mas não há nenhum limite.

A ordem em que a multiplicação da matriz é realizada é essencial. A fórmula anterior reflete a regra da esquerda para a direita da concatenação de matriz. Ou seja, os efeitos visíveis das matrizes que você usa para criar uma matriz composta ocorrem na ordem da esquerda para a direita. Uma matriz de mundo típica é mostrada no exemplo a seguir. Imagine que você está criando a matriz do mundo para um disco voador estereotipado. Provavelmente, você desejaria girar o disco voador em torno de seu centro - eixo y do espaço de modelo - e levá-lo para algum outro local em sua cena. Para realizar esse efeito, você primeiro cria uma matriz de rotação e, em seguida, multiplica ela pela matriz de translação, conforme mostrado na seguinte equação.

![equação de rotação com base em uma matriz de rotação e uma matriz de translação](images/wrldexpl.png)

Nessa fórmula, R<sub>y</sub> é uma matriz de rotação sobre o eixo y e T<sub>w</sub> é uma translação para alguma posição nas coordenadas do mundo.

A ordem na qual você multiplica as matrizes é importante porque, ao contrário da multiplicação de dois valores escalares, a multiplicação de matrizes não é comutativa. Multiplicar as matrizes na ordem oposta tem o efeito visual de levar o disco voador para a sua posição do espaço de mundo e, em seguida, girando-o em torno da origem do mundo.

Não importa qual tipo de matriz você está criando, lembre-se da regra da esquerda para a direita para garantir que você obterá os efeitos esperados.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Transformações](transforms.md)

 

 




