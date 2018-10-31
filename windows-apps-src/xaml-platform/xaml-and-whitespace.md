---
author: jwmsft
description: Conheça as regras de processamento de espaços em branco usadas por XAML.
title: XAML e espaço em branco
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 560f820ec2ecc7f28145ec29c31a60c1e4573d7e
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821774"
---
# <a name="xaml-and-whitespace"></a>XAML e espaço em branco


Conheça as regras de processamento de espaços em branco usadas por XAML.

## <a name="whitespace-processing"></a>Processamento de espaços em branco

Consistente com XML, caracteres de espaço em branco em XAML são espaço, avanço de linha e tabulação. Elas correspondem aos valores Unicode 0020, 000A e 0009 respectivamente. Por padrão, ocorre a seguinte normalização de espaços em branco quando um processador XAML encontra qualquer texto interno localizado entre elementos de um arquivo XAML:

-   Os caracteres de avanço de linha entre caracteres do leste asiático são removidos.
-   Todos os caracteres de espaço em branco (espaço. avanço de linha, tabulação) são convertidos em espaços.
-   Todos os espaços consecutivos são excluídos e substituídos por um espaço.
-   Um espaço imediatamente posterior à marca inicial é excluído.
-   Um espaço imediatamente anterior à marca inicial é excluído.
-   *Caracteres do leste asiático* são definidos como um conjunto de caracteres Unicode que vai de U+20000 até U+2FFFD e de U+30000 até U+3FFFD. Às vezes, esse subconjunto também é chamado de *ideogramas CJK*. Para obter mais informações, consulte http://www.unicode.org.

"Padrão" corresponde ao estado denotado pelo valor padrão do atributo **xml:space**.

### <a name="whitespace-in-inner-text-and-string-primitives"></a>Espaço em branco em texto interno e primitivas de cadeia

As regras de normalização acima aplicam-se a textos internos em elementos XAML. Após a normalização, o processador XAML converte qualquer texto interno em um tipo apropriado, da seguinte maneira:

-   Se o tipo da propriedade não for uma coleção, mas não for diretamente um tipo **Object**, o processador XAML tentará fazer uma conversão naquele tipo usando o conversor de tipos. Aqui, uma falha de conversão gerará um erro de análise XAML.
-   Se o tipo de propriedade for uma coleção e o texto interno for contíguo (sem marcas de elementos de divisão), o texto interno será analisado como uma única **String**. Se o tipo de coleção não puder aceitar **String**, isso também gerará um erro de análise XAML.
-   Se o tipo de propriedade for **Object**, o texto interno será analisado como uma única **String**. Se houver marcas de elementos de divisão, isso gerará um erro de análise XAML, pois o tipo **Object** implica apenas em um objeto (**String** ou outro).
-   Se o tipo da propriedade for uma coleção e o texto interno não for contíguo, a primeira subcadeia será convertida em uma **String** e adicionada como um item de coleção, o elemento de divisão será adicionado como item de coleção e, por fim, a subcadeia à direita (se houver) será adicionada à coleção como um terceiro item **String**.

### <a name="whitespace-and-text-content-models"></a>Espaço em branco e modelos de conteúdo de texto

Na prática, a preservação de espaços em branco só é importante para um subconjunto de todos os modelos de conteúdo possíveis. Esse subconjunto é composto por modelos de conteúdo que podem pegar um tipo **String** e singleton em algum formulário, coleção **String** dedicada ou uma mistura de **String** e outros tipos em listas, coleções ou dicionários.

Mesmo para modelos de conteúdo que podem pegar cadeias, o comportamento padrão nesses modelos de conteúdo é tratar qualquer espaço em branco restante como insignificante.

### <a name="preserving-whitespace"></a>Preservando espaços em branco

Várias técnicas de preservação de espaços em branco no código XAML de origem para eventuais apresentações não são afetadas pela normalização de espaços em branco do processador XAML.

`xml:space="preserve"`: especifique esse atributo no nível do elemento em que a preservação de espaços em branco é desejada. Observe que isso preserva todos os espaços em branco, inclusive os espaços que podem ser adicionados por editores de código ou áreas de design, para alinhar elementos de marcação como um aninhamento visualmente intuitivo. Se esses espaços são renderizados ou não é, mais uma vez, uma questão do modelo de conteúdo do respectivo elemento. Não recomendamos que você especifique `xml:space="preserve"` no nível da raiz, pois a maioria dos modelos de objeto não considera os espaços em branco como um meio significativo. É prática recomendada somente definir o atributo especificamente no nível dos elementos que renderizam espaços em branco em cadeias de caracteres ou que sejam coleções significativas de espaços em branco.

Entidades e espaços sem quebra: o XAML dá suporte à inserção de qualquer entidade Unicode em um modelo de objeto de texto. Você pode usar entidades dedicadas, como espaços sem quebras (na codificação UTF-8). Você também pode usar controles de texto avançados com suporte a caracteres de espaço sem quebras. Seja cauteloso se estiver usando entidades para simular características de layout, como recuos, pois a saída das entidades em tempo de execução vai variar com base em um número maior de fatores do que as unidades de layout gerais, como uso adequado de painéis e margens.

