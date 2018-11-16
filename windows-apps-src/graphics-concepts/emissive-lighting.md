---
title: Iluminação emissiva
description: Iluminação emissive fica claro que é emitido por um objeto; por exemplo, um brilho.
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- Iluminação emissiva
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ce2082479261cd96fb1c5bafd5f2df06bf6f239
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7103104"
---
# <a name="emissive-lighting"></a>Iluminação emissiva


A *iluminação emissiva* é emitido por um objeto; por exemplo, um brilho. A emissão faz com que um objeto renderizado pareça ter iluminação própria. A emissão afeta a cor de um objeto e pode, por exemplo, tornar um material escuro mais claro e adotar parte da cor emitida.

A iluminação emissive é descrita por um termo único.

Iluminação emissiva = Cₑ

onde:

| Parâmetro | Valor padrão | Tipo                                                                 | Descrição     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | Transparência de vermelho, verde, azul e alfa (todos os valores de ponto flutuante) | Cor emissiva. |

 

O valor de Cₑ é cor 1 ou cor 2. Se a cor do vértice não for fornecida, a cor emissiva do material é usada.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


Neste exemplo, o objeto é colorido usando a luz ambiente da cena e uma cor ambiente de material.

De acordo com a equação, a cor resultante para os vértices de objeto é a cor do material.

A ilustração a seguir mostra a cor do material, que é verde. A luz emissiva destaca todos os vértices do objeto com a mesma cor. Não é dependente da normal de vértice ou da direção da luz. Como resultado, o círculo se parece com um círculo 2D porque não há nenhuma diferença na sombreamento ao redor a superfície do objeto.

![ilustração de uma esfera verde](images/lighte.jpg)

A ilustração a seguir mostra como a luz emissiva combina com os outros três tipos de luzes. No lado direito da esfera, há uma mistura de luz emissiva verde e luz ambiente vermelha. No lado esquerdo da esfera, a luz emissiva verde combina com a luz difusa e a luz ambiente vermelha, produzindo um gradiente vermelho. O realce especular é branco no centro e cria um anel de amarelo à medida que o valor da luz especular cai nitidamente deixando os valores de luz emissiva, difusa e ambiente se misturarem para formar o amarelo.

![ilustração de uma esfera verde com luz emissiva](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Matemática de iluminação](mathematics-of-lighting.md)

 

 




