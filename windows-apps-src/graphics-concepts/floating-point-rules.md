---
title: Regras de ponto flutuante
description: O Direct3D dá suporte a várias representações de ponto flutuante. Todos os cálculos de ponto flutuante operam em um subconjunto das regras de ponto flutuante de precisão única de 32 bits 754 IEEE definido.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- Regras de ponto flutuante
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a29fbe49e45b819ddf4ffc3172445996d3622360
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370623"
---
# <a name="span-iddirect3dconceptsfloating-pointrulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>Regras de ponto flutuantes


O Direct3D dá suporte a várias representações de ponto flutuante. Todos os cálculos de ponto flutuante operam em um subconjunto das regras de ponto flutuante de precisão única de 32 bits 754 IEEE definido.

## <a name="span-idalpha32bitspanspan-idalpha32bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>regras de ponto flutuantes de 32 bits


Existem dois conjuntos de regras: aquele em conformidade com IEEE-754, e aquele que desvia do padrão.

### <a name="span-idalpha754rulesspanspan-idalpha754rulesspanspan-idalpha754rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>Regras de IEEE-754 honradas

Algumas dessas regras são uma única opção onde a IEEE-754 oferece opções.

-   Divisão por 0 produz + /-INF, exceto 0/0, o que resulta em NaN.
-   o log de (+/-) 0 produz -INF.  

    o log de um valor negativo (diferente de -0) produz NaN.
-   A raiz quadrada recíproca (rsq) ou raiz quadrada (sqrt) de um número negativo produz NaN.  

    A exceção é - 0; sqrt(-0) produz - 0 e rsq(-0) produz -INF.
-   INF - INF = NaN
-   (+/-)INF / (+/-)INF = NaN
-   (+/-)INF \* 0 = NaN
-   Qualquer valor NaN (qualquer OP) = NaN
-   As comparações EQ, GT, GE, LT e LE, quando um ou ambos operandos for NaN retornará **FALSE**.
-   As comparações ignoram o sinal de 0 (sendo assim, +0 igual -0).
-   A comparação NE, quando um ou ambos operandos for NaN retornará **TRUE**.
-   As comparações de qualquer valor não NaN com +/-INF retornam o resultado correto.

### <a name="span-idalpha754deviationsspanspan-idalpha754deviationsspanspan-idalpha754deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>Requisitos adicionais do IEEE-754 regras ou desvios

-   O IEEE-754 exibe que as operações de ponto flutuante produzam um resultado que seja o valor representável mais próximo a um resultado infinitamente preciso, conhecido como valor arredondado para o número par mais próximo.

    Direct3D 11 e até definir o mesmo requisito como IEEE-754: operações de ponto flutuante de 32 bits produzem um resultado que está dentro do 0,5 unidade-last-place (ULP) do resultado infinitamente precisos. Isso significa que, por exemplo, o hardware tem permissão de truncar resultados em 32 bits, em vez de executar o arredondamento para o número par mais próximo, uma vez que isso resultaria em um erro de no máximo 0,5 ULP. Esta regra se aplica somente à adição, subtração e multiplicação.

    Versões anteriores do Direct3D definem um requisito de menos rígido que IEEE-754: operações de ponto flutuante de 32 bits produzem um resultado que está dentro de uma unidade-last-place (1 ULP) do resultado infinitamente precisos. Isso significa que, por exemplo, o hardware tem permissão de truncar resultados em 32 bits, em vez de executar o arredondamento para o número par mais próximo, uma vez que isso resultaria em um erro de no máximo 1 ULP.

-   Não há nenhum suporte para exceções de ponto flutuante, bits de status ou interrupções.
-   Denorms são liberadas para zero com sinal preservado na entrada e saída de qualquer operação matemática de ponto flutuante. Exceções são feitas para qualquer operação de movimentação de E/S ou dados que não manipula os dados.
-   Os estados que contêm valores de ponto flutuante, como valores de visor MinDepth/MaxDepth ou BorderColor, podem ser fornecidos como valores denorm e podem ou não ser liberados antes que o hardware utilize-os.
-   As operações de mínimo ou máximo liberam denorms para a comparação, mas o resultado pode ou não ser um denorm liberado.
-   A entrada NaN para uma operação sempre produz NaN na saída. Mas o padrão de bit exato do NaN não é necessário para manter o mesmo (a menos que a operação seja uma instrução de movimento bruta - que não altera os dados.)
-   As operações de mínimo ou máximo para as quais somente um operando é NaN retornam o outro operando como resultado (ao contrário das regras de comparação que vimos anteriormente). Isso é uma regra de IEEE 754R.

    A especificação IEEE 754R para operações de mínimo e máximo de ponto flutuante diz que se uma das entradas para mínimo ou máximo for um valor de QNaN silencioso, o resultado da operação será outro parâmetro. Por exemplo: 

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    Uma revisão da especificação IEEE 754R adotou um comportamento diferente para mínimo e máximo quando uma entrada é um valor SNaN de "sinalização" em relação a um valor QNaN:

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    Em geral, o Direct3D segue os padrões para a aritmética: IEEE-754 e 754R IEEE. Mas, nesse caso, temos um desvio.

    As regras aritméticas no Direct3D 10 e posterior não fazem qualquer distinção entre valores NaN de sinalização e silenciosos (QNaN versus SNaN). Todos os valores NaN são tratados da mesma maneira. No caso de mínimo e máximo, o comportamento do Direct3D para qualquer valor NaN é semelhante à forma que QNaN é manipulado na definição de IEEE 754R. (Como complemento - se ambas as entradas forem NaN, qualquer valor NaN será retornado.)

-   Outra regra IEEE 754R é que min(-0,+0) == min(+0,-0) == -0, and max(-0,+0) == max(+0,-0) == +0, o que respeita o sinal, diferente das regras de comparação para zero com sinal (como vimos anteriormente). O Direct3D recomenda o comportamento do IEEE 754R aqui, mas não o impõe; é permitido que o resultado da comparação de zeros seja dependente da ordem dos parâmetros, usando uma comparação que ignora os sinais.
-   x\*1.0f sempre resulta em x (exceto denorm liberado).
-   x/1.0f sempre resulta em x (exceto denorm liberados).
-   x +/- 0.0f sempre resulta em x (exceto denorm liberados). Mas -0 + 0 = +0.
-   As operações fundidas (como mad, dp3) produzem resultados que não são menos precisos do que a pior ordem serial possível da avaliação da expansão não fundida da operação. A definição de pior ordem possível, para fins de tolerância, não é uma definição fixa para uma determinada operação fundida; depende dos valores específicos das entradas. As etapas individuais na expansão não fundida têm tolerância de 1 ULP (ou para qualquer instrução que o Direct3D chamar com uma tolerância lax maior que 1 ULP, mais a tolerância lax será permitida).
-   As operações fundidas cumprem as mesmas regras NaN que as operações não fundidas.
-   sqrt e rcp têm tolerância de 1 ULP. As instruções de raiz quadrada recíproca e recíproca de sombreador, [**rcp**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/hh447205(v=vs.85)) e [**rsq**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/rsq--sm4---asm-), têm seu próprios requisitos separados de precisão relaxada.
-   Multiplique e divide cada operação no nível de precisão de ponto flutuante de 32 bits (precisão de até 0,5 ULP para multiplicação e 1,0 ULP para recíproca). Se x/y é implementado diretamente, os resultados devem ter precisão maior ou igual à de um método de duas etapas.

## <a name="span-iddoubleprec64bitspanspan-iddoubleprec64bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64-bit (precisão dupla) regras de ponto flutuantes


Drivers de hardware e exibição opcionalmente suportam ponto flutuante de precisão dupla. Para indicar o suporte, quando você chama [ **ID3D11Device::CheckFeatureSupport** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) com [ **D3D11\_recurso\_DUPLOS** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_feature), os conjuntos de driver **DoublePrecisionFloatShaderOps** de [ **D3D11\_recurso\_dados\_DUPLOS** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_doubles) como TRUE. Em seguida, o driver e o hardware devem dar suporte a todas as instruções de ponto flutuante de precisão dupla.

As instruções de precisão dupla seguem os requisitos de comportamento IEEE 754R.

O suporte à geração de valores desnormalizados é necessário para os dados de precisão dupla (nenhum comportamento flush-para-zero). Da mesma forma, as instruções não fazem a leitura de dados desordenados como um zero com sinal, elas respeitam o valor denorm.

## <a name="span-idalpha16bitspanspan-idalpha16bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>regras de ponto flutuantes de 16 bits


O Direct3D também dá suporte a representações de 16 bits de números de ponto flutuante.

Formato:

-   Sinal de 1 bit (s) na posição MSB bit
-   5 bits de expoente ajustado (e)
-   10 bits de fração (f), com um bit oculto adicional

Um valor de float16 (v) segue estas regras:

-   Se e == 31 e f != 0, então v é NaN independente de s
-   Se e = = 31 e f = = 0 e, em seguida, v = (-1) s\*infinito (com sinal infinito)
-   Se e estiver entre 0 e 31, v = (-1) s\*2(e-15)\*(1.f)
-   Se e = = 0 e f! = 0 e, em seguida, v = (-1) s\*2(e-14)\*(0.f) (números desnormalizados)
-   Se e = = 0 e f = = 0 e, em seguida, v = (-1) s\*0 (zero com sinal)

As regras de ponto flutuante de 32 bits também esperam por números de ponto flutuante de 16 bits, ajustados para o layout de bit descrito anteriormente. Algumas exceções:

-   Precisão: Operações não fundidas em números de ponto flutuante de 16 bits produzem um resultado que é o mais próximo valor representável em um resultado preciso infinitamente (round mesmo mais próximo, por IEEE-754, aplicada aos valores de 16 bits). As regras de ponto flutuante de 32 bits aderem à tolerância de 1 ULP, as regras de ponto flutuante de 16 bits aderem a 0,5 ULP para as operações não fundidas e 0,6 ULP para operações fundidas.
-   Os números de ponto flutuante de 16 bits preservam denorms.

## <a name="span-idalpha11bitspanspan-idalpha11bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>regras de ponto flutuantes 11 e 10 bits


O Direct3D também dá suporte a formatos de ponto flutuante de 11 e 10 bits.

Formato:

-   Nenhum bit de sinal
-   5 bits de expoente ajustado (e)
-   6 bits de fração (f) para um formato de 11 bits, 5 bits de fração (f) para um formato de 10 bits, com um bit oculto adicional em ambos os casos.

Um valor de float11/float10 (v) segue as regras a seguir:

-   se e == 31 and f != 0, então v é NaN
-   se e == 31 e f == 0, então v = +infinito
-   Se e estiver entre 0 e 31, v, em seguida, = 2(e-15)\*(1.f)
-   Se e = = 0 e f! = 0 e, em seguida, v = \*2(e-14)\*(0.f) (números desnormalizados)
-   se e == 0 e f == 0, então v = 0 (zero)

As regras de ponto flutuante de 32 bits também esperam por números de ponto flutuante de 11 e 10 bits, ajustados para o layout de bit descrito anteriormente. As exceções incluem:

-   Precisão: seguem as regras de ponto flutuantes de 32 bits ULP 0,5.
-   Os números de ponto flutuante de 10/11 bits preservam denorms.
-   Qualquer operação que poder resultar em um número menor que zero é vinculada com zero.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Apêndices](appendix.md)

[Recursos](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources)

[Texturas](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 




