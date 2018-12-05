---
description: Lista o suporte no nível da linguagem em XAML para o Windows Runtime para determinados tipos de dados em CLR (Common Language Runtime) e em outras linguagens de programação, por exemplo, C++.
title: Tipos de dados XAML intrínsecos
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 26f4153b59c618a4559549ba7fa9ca0f99c4ab64
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8731049"
---
# <a name="xaml-intrinsic-data-types"></a>Tipos de dados XAML intrínsecos


O XAML para Windows Runtime dá suporte no nível de linguagem para vários tipos de dados que são primitivos, frequentemente usados em CLR (common language runtime) e outras linguagens de programação, como C++.

O local mais comum em que você verá usos de tipos de dados intrínsecos XAML é quando os recursos são definidos em um dicionário de recursos XAML. Você pode definir constantes nele, por exemplo, números que você usa para vários valores. Ou então, você pode usar uma animação de storyboard que é animada usando uma cadeia de caracteres ou um valor booliano e, então, você precisará de um elemento de objeto XAML representando a cadeia de caracteres ou o booliano para preencher o quadro-chave de sua definição de [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320). Os modelos XAML padrão do Windows Runtime usam ambas as técnicas.

O XAML para o Windows Runtime fornece suporte no nível de linguagem para estes tipos.

| Primitivo XAML | Descrição |
|-------|-------------|
| **x:Boolean**  | Para suporte a CLR, corresponde a [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx). O XAML analisa valores para **x:Boolean** sem diferenciar maiúsculas e minúsculas. Observe que "x:Bool" não é uma alternativa aceita. |
| **x:String**   | Para suporte a CLR, corresponde a [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx). Codificação dos padrões de cadeias de caracteres para a codificação XML ao redor. |
| **x:Double**   | Para suporte a CLR, corresponde a [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Além dos valores numéricos, a sintaxe de texto de **x:Double** permite o token "NaN", que é como o comportamento de layout "Auto" pode ser armazenado como valor de recurso. Os tokens são tratados como elementos que diferenciam maiúsculas e minúsculas. Você pode usar a notação científica, por exemplo "1+E06" para `1,000,000`. |
| **x:Int32**    | Para suporte a CLR, corresponde a [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx). **x:Int32** é considerado um elemento com sinal, e você pode incluir o símbolo de subtração ("-") para um inteiro negativo. Em XAML, a ausência de um sinal na sintaxe de texto indica que o valor tem sinal positivo. |

Geralmente, esses primitivos da linguagem XAML são os únicos casos em que você define um elemento de objeto que usa o prefixo **x:** em XAML. Todos os outros recursos da linguagem XAML são tipicamente usados na forma de atributos ou como extensão de marcação.

**Observação**por convenção, os primitivos de linguagem para XAML e todos os outros elementos da linguagem XAML são mostrados com o prefixo "x:". É assim que os elementos da linguagem XAML são normalmente usados em situações reais de marcação. Essa convenção é seguida na documentação de XAML e também na especificação XAML.

## <a name="other-xaml-primitives"></a>Outras primitivas XAML

A especificação XAML 2009 destaca outros primitivos no nível da linguagem XAML **x:Uri** e **x:Single**. A não ser que estejam listados na tabela neste tópico, outros primitivos de linguagem XAML, como definidos por outros vocabulários XAML ou pela especificação XAML 2009, não têm suporte atualmente em XAML para o Windows Runtime.

**Observação**datas e horas (propriedades que usam [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) ou [**DateTimeOffset**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996) ou [**System. TimeSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/system.timespan.aspx)) não são configuráveis com um primitivo XAML. Essas propriedades geralmente não são configuráveis em XAML, pois não há conversão de cadeia de caracteres padrão no analisador XAML do Windows Runtime para datas e horas. Para valores de inicialização de quaisquer propriedades de data e hora, você precisará usar code-behind que é executado quando uma página ou um elemento é carregado.

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do XAML](xaml-overview.md)
* [Guia de sintaxe do XAML](xaml-syntax-guide.md)
* [Animações de storyboard](https://msdn.microsoft.com/library/windows/apps/mt187354)
 

