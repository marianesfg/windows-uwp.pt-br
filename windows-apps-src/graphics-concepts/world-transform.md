---
title: "Transformação do mundo"
description: "Uma transformação de mundo muda as coordenadas do espaço do modelo, onde os vértices são definidos em relação à origem local de um modelo, para o espaço do mundo."
ms.assetid: 767B032C-69D0-4583-8FEB-247F4C41E31D
keywords: "Transformação do mundo"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 6218747e0522235f1d71510a4ad4b6ce60def4a3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="world-transform"></a>Transformação do mundo


Uma transformação de mundo muda as coordenadas do espaço do modelo, onde os vértices são definidos em relação à origem local de um modelo, para o espaço do mundo. No espaço do mundo, os vértices são definidos em relação a uma origem comum a todos os objetos em uma cena. A transformação do mundo coloca um modelo no mundo.

## <span id="What_Is_a_World_Transform"></span><span id="what_is_a_world_transform"></span><span id="WHAT_IS_A_WORLD_TRANSFORM"></span>


O diagrama a seguir mostra a relação entre o sistema de coordenadas do mundo e o sistema de coordenadas local do modelo.

![diagrama de como as coordenadas de mundo e locais estão relacionadas](images/worldcrd.png)

A transformação do mundo pode incluir qualquer combinação de traduções, rotações e escalas.

## <a name="span-idsettingupaworldmatrixxmlspansetting-up-a-world-matrix"></a><span id="SETTING_UP_A_WORLD_MATRIX.XML"></span>Configurando uma matriz de mundo


Assim como com qualquer outra transformação, crie a transformação do mundo concatenando uma série de matrizes em uma única matriz que contém a soma total de seus efeitos. No caso mais simples, quando um modelo está na origem de mundo e seus eixos de coordenada locais são orientados da mesma forma que o espaço do mundo, a matriz de mundo é a matriz de identidade. Mais comumente, a matriz de mundo é uma combinação de uma translação no espaço do mundo e possivelmente uma ou mais rotações para girar o modelo conforme necessário.

O Direct3D usa as matrizes de mundo e modo de exibição que você definiu para configurar várias estruturas de dados internas. Cada vez que você define um novo mundo ou uma matriz de exibição, o sistema recalcula as estruturas internas associadas. Definir essas matrizes frequentemente, por exemplo, milhares de vezes por quadro, é computacionalmente demorado. Você pode minimizar o número de cálculos necessários concatenando as matrizes de mundo e modo de exibição em uma matriz de exibição de mundo que você define como matriz de mundo, e, em seguida, definindo a matriz de visualização para a identidade. Mantenha cópias em cache das matrizes individuais de mundo e modo de exibição para que você possa modificar, concatenar e restaurar a matriz de mundo conforme necessário.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Transformações](transforms.md)

 

 




