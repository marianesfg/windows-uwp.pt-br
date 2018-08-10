---
author: stevewhims
description: Este tópico descreve as várias categorias de valores que existem no C++. Você já deve ter ouvido de lvalues e rvalues, mas há outros tipos, muito.
title: Categorias de valor
ms.author: stwhi
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, mover, encaminhamento, categorias de valor, semântica de movimentação, encaminhamento perfeito, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: 75176334909e0e6bcf81763b543a19b70a4cba73
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/10/2018
ms.locfileid: "2739062"
---
# <a name="value-categories"></a>Categorias de valor
Este tópico descreve as várias categorias de valores que existem no C++. Você já deve ter ouvido de *lvalues* e *rvalues*, mas há outros tipos, muito. Cada expressão no C++ produz um valor que pertence a uma das categorias discutidas neste tópico. Há aspectos da linguagem C++, seu facilies e regras, que exigem uma compreensão adequada de categorias de valor.

## <a name="an-lvalue-has-identity"></a>Lvalue tem identidade
O que significa um valor ter *identidade*? Se você tiver (ou podem ser realizadas) o endereço de memória de um valor, o valor tem identity. Dessa forma, você pode fazer mais do que compare o conteúdo dos valores: você pode comparar ou diferenciá-los pelo identity.

Um *lvalue* tem uma identidade. Agora é uma questão de interesse apenas histórica que "l" em "lvalue" é uma abreviação de "left" (como à esquerda-à-lado de uma atribuição). No C++, lvalue pode aparecer na esquerda *ou* à direita de uma atribuição. "L" em "lvalues", em seguida, não realmente ajudá-lo a compreender nem definir quais são. Você só precisa entender que que chamamos lvalue é um valor que tem identity.

Agora, embora seja uma instrução true que possuem identidade lvalues, então, fazer xvalues. Falaremos um pouco mais no qual um *xvalue* é posteriormente neste tópico (embora, um tratamento completo deles não estiver no escopo aqui). No momento, basta esteja ciente de que haja uma categoria de valor chamada glvalue, para "generalizada lvalue". O subconjunto de glvalues contém lvalues (também conhecido como "clássico lvalues") e o xvalues. Portanto, enquanto "lvalue possui a identidade" for true, o conjunto completo de coisas que possuem identidade é o conjunto de glvalues, conforme mostrado nesta ilustração.

![Lvalue tem identidade](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Um rvalue é móvel; lvalue não é
Mas há valores que não são glvalues. Consequentemente, há valores que você *não pode* obter um endereço de memória para (ou se você não pode depender seja válido). Que SOA como uma desvantagem. Mas na realidade a vantagem de um valor, como isto é que você pode *Mover* (o que é geralmente barato), em vez de cópia it (que é geralmente caro). Movendo um valor significa que ele não está mais no lugar costumava ser. Então, tentar acessá-lo no lugar de ser é algo a ser evitada. Uma discussão sobre quando e *como* mover que um valor está fora do escopo deste tópico. Para este tópico, precisamos apenas saber que um valor que é móvel é conhecido como um *rvalue*.

O "r" em "rvalue" é uma abreviação de "direita" (como o direita-à-lado de uma atribuição). Mas você pode usar rvalues e referências para rvalues, fora de atribuições. O "r" em "rvalues", em seguida, não é a coisa para focalizar em. Você só precisa entender que que chamamos uma rvalue é um valor que é móvel.

Lvalue, de modo oposto, não movível, conforme mostrado nesta ilustração. Não é possível mover lvalue porque, se você pudesse, em seguida, seria não seguros (ou até mesmo desastrosos) para continuar para acessá-lo posteriormente. Lembre-se de que você tem sua identidade.

![Um rvalue é móvel; lvalue não é](images/is-movable.png)

Não é possível mover lvalue. Mas há *é* uma espécie de glvalue (o conjunto de coisas com identidade) que você pode mover&mdash;se você souber que você está fazendo&mdash;e que é o xvalue. Podemos vai rever essa ideia mais uma vez abaixo, quando procuramos a imagem completa das categorias de valor.

## <a name="an-lvalue-has-identity-a-prvalue-does-not"></a>Lvalue tem identidade; um prvalue não
Nesse estágio, nós sabemos o que possui a identidade. E sabemos o que é móvel e o que não está. Mas não temos ainda chamado o conjunto de valores que *não* possuem identidade. Esse conjunto é conhecido como o *prvalue*ou "rvalue puro".

![Lvalue tem identidade; um prvalue não](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>A imagem completa das categorias de valor
Ele permanece apenas combinar a info e as ilustrações acima em uma única imagem ganhar.

![A imagem completa das categorias de valor](images/value-categories.png)

- Um glvalue (generalizada lvalue) tem uma identidade.
- Lvalue (uma espécie de glvalue) tem identidade, mas não movível. Estes são os valores de leitura-gravação normalmente que você passa ao redor por referência ou por referência constante ou pelo valor se a cópia é barato.
- Um xvalue (uma espécie de glvalue, mas também um tipo de rvalue) tem identity e também é móvel. Isso pode ser lvalue mesma que você decidiu mover porque copiando é cara e você vai cuidado para não acessá-lo posteriormente. A maneira como você transforma lvalue em um xvalue e os motivos para mover um, será necessário aguardar a outro tópico. Mas você pode pensar no "x" no "xvalue" como significado "especialista" Se isto ajuda.
- Um prvalue (puro rvalue; um tipo de rvalue) não tem identidade, mas é móvel. Geralmente, essas são literais, temporaries, retornar valores&mdash;tudo o que é o resultado da avaliação de uma expressão (uma expressão que não seja um glvalue) ou qualquer coisa retornados pelo valor de uma função.
- Um rvalue é móvel.
