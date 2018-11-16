---
title: Buffers de profundidade
description: Um buffer de profundidade, ou buffer z, armazena informações de profundidade para controlar quais áreas de polígonos são renderizadas em vez de serem ocultas da exibição.
ms.assetid: BE83A1D7-D43D-4013-8817-EFD2B24DC58E
keywords:
- Buffers de profundidade
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fb7ff4b23f9735347ce75e2e565c1b420ec936d6
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6660138"
---
# <a name="depth-buffers"></a>Buffers de profundidade


Um *buffer de profundidade*, ou *buffer z*, armazena informações de profundidade para controlar quais áreas de polígonos são renderizadas em vez de serem ocultas da exibição.

## <a name="span-idoverviewspanspan-idoverviewspanspan-idoverviewspanoverview"></a><span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>Visão geral


Um buffer de profundidade, geralmente chamado de buffer z ou buffer w, é uma propriedade do dispositivo que armazena informações de profundidade a serem usadas pelo Direct3D. Quando o Direct3D renderiza uma cena para uma superfície de destino, ele pode usar a memória em uma superfície de buffer de profundidade associado como um espaço de trabalho para determinar como os pixels dos polígonos rasterizados ocultam uns aos outros. O Direct3D usa uma superfície Direct3D fora da tela como o destino no qual os valores de cor finais são gravados. A superfície de buffer de profundidade que está associada com a superfície de destino de renderização é usada para armazenar informações de profundidade que indicam ao Direct3D qual a profundidade de cada pixel visível na cena.

Quando uma cena é rasterizada com o buffer de profundidade habilitado, cada ponto na superfície de renderização é testado. Os valores no buffer de profundidade podem ser uma coordenada z do ponto ou sua coordenada w homogênea - a partir da localização do ponto (x, y, z, w) no espaço de projeção. Um buffer de profundidade que usa valores z é geralmente chamado de buffer z e um que usa valores w é chamado de buffer w. Cada tipo de buffer de profundidade tem vantagens e desvantagens, que são discutidas posteriormente.

No início do teste, o valor de profundidade no buffer de profundidade é definido como o maior valor possível para a cena. O valor de cor na superfície de renderização é definido como o valor de cor de plano de fundo ou o valor da cor da textura em segundo plano nesse ponto. Cada polígono na cena é testado para verificar se cruza com a coordenada atual (x, y) na superfície de renderização.

Se ele cruza, o valor de profundidade - que será a coordenada z em um buffer z, e a coordenada w em um buffer-w - no ponto atual é testado para verificar se é menor do que o valor de profundidade armazenado no buffer de profundidade. Se a profundidade do valor do polígono for menor, ele é armazenado no buffer de profundidade e o valor de cor do polígono é gravado para o ponto atual na superfície de renderização. Se o valor de profundidade do polígono nesse ponto for maior, o próximo polígono na lista é testado. Esse processo está ilustrado no diagrama a seguir.

![Diagrama de teste de valores de profundidade](images/zbuffer.png)

## <a name="span-idbufferingtechniquesspanspan-idbufferingtechniquesspanspan-idbufferingtechniquesspanbuffering-techniques"></a><span id="Buffering_techniques"></span><span id="buffering_techniques"></span><span id="BUFFERING_TECHNIQUES"></span>Técnicas de buffer


Embora a maioria dos aplicativos não use esse recurso, você pode alterar a comparação que o Direct3D usa para determinar quais valores são colocados no buffer de profundidade e, posteriormente, na superfície de destino de renderização. Em alguns itens de hardware, alterar a função de comparação pode desabilitar o teste z hierárquico.

Praticamente todos os aceleradores do mercado são compatíveis com o buffer z, o que o transforma no tipo mais comum de buffer de profundidade hoje. Mesmo onipresentes, os buffers z têm suas desvantagens. Devido à matemática envolvida, os valores z gerados em um buffer z tendem a não ser distribuídos uniformemente no intervalo do buffer z (normalmente 0,0 a 1,0, inclusive).

Especificamente, a razão entre os planos de recorte distante e próximo afeta intensamente quanto os valores de z são distribuídos irregularmente. Usando uma razão de distância do plano distante para o plano próximo de 100, 90% do intervalo do buffer de profundidade são gastos nos primeiros 10% do intervalo de profundidade da cena. Os aplicativos típicos para diversão ou simulações visuais com cenas exteriores geralmente exigem razões de plano distante/plano próximo em algum ponto entre 1.000 e 10.000. Em uma razão de 1.000, 98% do intervalo são gastos nos primeiros 2% do intervalo de profundidade, e a distribuição fica pior com razões maiores. Isso pode causar artefatos de superfície ocultos em objetos distantes, especialmente ao usar buffers de profundidade de 16 bits, a profundidade de bits mais comumente compatível.

Um buffer de profundidade com base w, por outro lado, costuma ser distribuído mais uniformemente entre os planos de recorte próximo e distante do que um buffer z. O principal benefício é que a razão de distâncias para os planos de recorte distante e próximo não é mais um problema. Isso permite que os aplicativos sejam compatíveis com intervalos máximos grandes, ao mesmo tempo que ainda obtêm buffer de profundidade relativamente preciso perto do ponto ocular. Um buffer de profundidade com base w não é perfeito e, às vezes, pode apresentar artefatos de superfície ocultos para objetos próximos. Outra desvantagem na abordagem de buffer w está relacionada ao suporte de hardware: o buffer w não é tão amplamente compatível com itens de hardware como o buffer z.

O uso de um buffer z requer sobrecarga durante a renderização. Várias técnicas podem ser usadas para otimizar a renderização ao usar buffers z. Os aplicativos podem aumentar o desempenho ao usar o buffer z e texturização, garantindo que as cenas sejam renderizadas da frente para trás. Buffers z texturizados primitivos são pré-testados em relação ao buffer z em uma base de linha de varredura. Se uma linha de varredura for ocultada por um polígono renderizado anteriormente, o sistema a rejeita com rapidez e eficiência. O buffer Z pode melhorar o desempenho, mas a técnica é mais útil quando uma cena desenha os mesmos pixels mais de uma vez. Isso é difícil de calcular exatamente, mas geralmente, você pode obter uma boa aproximação. Se os mesmos pixels forem desenhados menos de duas vezes, você pode obter o melhor desempenho desativando o buffer z e renderizando a cena de trás para frente.

A interpretação real de um valor de profundidade é específica para o renderizador.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Buffers de profundidade e estêncil](depth-and-stencil-buffers.md)

 

 




