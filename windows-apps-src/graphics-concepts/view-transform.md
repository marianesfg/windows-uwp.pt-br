---
title: Transformação da exibição
description: Uma transformação de exibição localiza o visualizador no espaço do mundo, transformando vértices em espaço da câmera.
ms.assetid: DA4C2051-4C28-4ABF-8C06-241C8CB87F2F
keywords:
- Transformação da exibição
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63660883f327547a82eac4a3accec475995a651a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8462619"
---
# <a name="view-transform"></a>Transformação da exibição


Uma *transformação de exibição* localiza o visualizador no espaço do mundo, transformando vértices em espaço da câmera. No espaço de câmera, a câmera ou visualizador está na origem, olhando na direção z positiva. A matriz de exibição realoca os objetos no mundo ao redor de uma posição de câmera - a origem do espaço da câmera - e orientação. O Direct3D usa um sistema de coordenadas à esquerda, portanto, z é positivo em uma cena.

Há muitas maneiras de criar uma matriz de exibição. Em todos os casos, a câmera tem algumas posições e orientações lógicas no espaço do mundo que são usadas como ponto de partida para criar uma matriz de exibição que será aplicada aos modelos em uma cena. A matriz de visualização é convertida e gira objetos para colocá-los no espaço da câmera, onde a câmera está na origem. Uma maneira de criar uma matriz de exibição é combinar uma matriz de translação com matrizes de rotação para cada eixo. Nesta abordagem, a seguinte equação de matriz geral se aplica.

![equação de transformação de exibição](images/viewtran.png)

Nessa fórmula, V é a matriz de visualização que está sendo criada, T é uma matriz de tradução que reposiciona objetos no mundo e Rₓ por R<sub>z</sub> são matrizes de rotação que giram objetos ao longo do eixo x, y e z. As matrizes de translação e rotação se baseiam na posição e orientação lógicas da câmera no espaço do mundo. Portanto, se a posição lógica da câmera no mundo for &lt;10,20,100&gt;, o objetivo da matriz de translação é mover objetos -10 unidades ao longo do eixo x, -20 unidades ao longo do eixo y e -100 unidades ao longo do eixo z. As matrizes de rotação na fórmula são baseadas na orientação da câmera, em termos de quanto os eixos do espaço da câmera são girados para fora do alinhamento com o espaço do mundo. Por exemplo, se a câmera mencionada anteriormente estiver apontando diretamente para baixo, o seu eixo z é de 90 graus (pi/2 radianos) desalinhado com o eixo z do espaço do mundo, conforme mostrado na ilustração a seguir.

![Ilustração do espaço de exibição da câmera em comparação com o espaço do mundo](images/camtop.png)

As matrizes de rotação aplicam rotações de magnitude igual, porém oposta, aos modelos na cena. A matriz de visualização de câmera inclui uma rotação de -90 graus em torno do eixo x. A matriz de rotação é combinada com a matriz de translação para criar uma matriz de exibição que ajusta a posição e a orientação dos objetos na cena para que sua parte superior fique voltada para a câmera, dando a impressão de que a câmera está acima do modelo.

## <a name="span-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspansetting-up-a-view-matrix"></a><span id="Setting_Up_a_View_Matrix"></span><span id="setting_up_a_view_matrix"></span><span id="SETTING_UP_A_VIEW_MATRIX"></span>Configurando uma matriz de exibição


O Direct3D usa as matrizes de mundo e modo de exibição para configurar várias estruturas de dados internas. Cada vez que você define um novo mundo ou uma matriz de exibição, o sistema recalcula as estruturas internas associadas. Definir essas matrizes com frequência é algo computacionalmente demorado. Você pode minimizar o número de cálculos necessários concatenando as matrizes de mundo e modo de exibição em uma matriz de exibição de mundo que você define como matriz de mundo, e, em seguida, definindo a matriz de visualização para a identidade. Mantenha cópias em cache das matrizes individuais de mundo e modo de exibição para que você possa modificar, concatenar e restaurar a matriz de mundo conforme necessário.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Transformações](transforms.md)

 

 




