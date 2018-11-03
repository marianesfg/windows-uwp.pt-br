---
title: Usando valores gerados pelo sistema
description: Os valores gerados pelo sistema são gerados pelo estágio de Assembler de Entrada (IA) (com base na semântica de entrada fornecida pelo usuário) para permitir certas eficiências nas operações do sombreador.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- Usando valores gerados pelo sistema
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f187495568892f5b489f6e109669811f4c45ab1
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5988629"
---
# <a name="span-iddirect3dconceptsusingsystem-generatedvaluesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>Usando valores gerados pelo sistema


Os valores gerados pelo sistema são gerados pelo [estágio de Assembler de Entrada (IA)](input-assembler-stage--ia-.md) (com base na [semântica de entrada fornecida pelo usuário](https://msdn.microsoft.com/library/windows/desktop/bb509647)) para permitir certas eficiências nas operações do sombreador. Ao anexar dados, como uma ID de instância (visível para o [estágio do Sombreador de Vértice (VS)](vertex-shader-stage--vs-.md)), uma ID de vértice (visível para o VS) ou uma ID de primitivo (visível para o [estágio do Sombreador de Geometria (GS)](geometry-shader-stage--gs-.md)/[estágio do Sombreador de Pixel (PS)](pixel-shader-stage--ps-.md)), pode parecer um estágio do sombreador subsequente para esses valores do sistema, para otimizar o processamento nesse estágio.

Por exemplo, o estágio do VS pode procurar a ID de instância para coletar dados por vértice adicionais para o sombreador ou para realizar outras operações; os estágios GS e PS podem usar a ID do primitivo para coletar dados por primitiva da mesma forma.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


Uma ID de vértice é usada por cada estágio de sombreador para identificar cada vértice. É um inteiro sem sinal de 32 bits, cujo valor padrão é 0. Ele é atribuído a um vértice quando o primitivo é processada pelo [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md). Anexar a semântica de ID de vértice para a declaração de entrada do sombreador para informar o estágio de IA para gerar uma ID por vértice.

O IA adicionará uma ID de vértice para cada vértice para uso por estágios do sombreador. Para cada chamada de desenho, a ID de vértice é incrementada em 1. Entre as chamadas desenho indexadas, a contagem é redefinida de volta para o valor inicial. Se a ID de vértice estourar (exceder 2³² – 1), ela é encapsulada como 0.

Para todos os tipos primitivos, os vértices têm uma ID de vértice associada a eles (independentemente da adjacência).

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


Uma ID de primitivo é usada por cada estágio de sombreador para identificar cada primitivo. É um inteiro sem sinal de 32 bits, cujo valor padrão é 0. Ele é atribuído a um primitivo quando o primitivo é processada pelo [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md). Para informar ao estágio de IA para gerar um ID de primitivo, anexe a semântica de ID do primitivo à declaração de entrada do sombreador.

O estágio de IA adicionará uma ID de primitivo para cada primitivo para uso pelo [estágio do Sombreador de Geometria (GS)](geometry-shader-stage--gs-.md) ou [estágio do Sombreador de Vértice (VS)](vertex-shader-stage--vs-.md) (aquela que é o primeiro estágio ativado após o estágio de IA). Para cada chamada de desenho indexada, a ID de primitivo é incrementada em 1, no entanto, a ID de primitivo é redefinida para 0 sempre que uma nova instância começa. Todas as outras chamadas de desenho não alteram o valor da ID da instância. Se a ID de instância estourar (exceder 2³² – 1), ela é encapsulada como 0.

O [estágio do Sombreador de Pixel (PS)](pixel-shader-stage--ps-.md) não tem uma entrada separada para uma ID de primitivo; no entanto, qualquer entrada de sombreador de pixel que especifica uma ID de primitivo usa um modo de interpolação constante.

Não há nenhum suporte para gerar automaticamente uma ID de primitivo para primitivos adjacentes. Para tipos primitivos com adjacência, como uma faixa de triângulos com arredores, uma ID de primitivo é mantida somente para os primitivos interiores (os primitivos não adjacentes), assim como o conjunto de primitivos em uma faixa de triângulos sem arredores.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


Uma ID de instância é usada por cada estágio de sombreador para identificar a instância da geometria que está sendo processada. É um inteiro sem sinal de 32 bits, cujo valor padrão é 0.

O [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md) adicionará uma ID de instância para cada vértice, se a declaração de entrada de sombreador de vértice inclui a semântica de ID de instância. Para cada chamada de desenho indexada, a ID da instância é incrementada em 1. Todas as outras chamadas de desenho não alteram o valor da ID da instância. Se a ID de instância estourar (exceder 2³² – 1), ela é encapsulada como 0.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


A ilustração a seguir mostra como os valores do sistema são anexados a uma faixa de triângulos instanciada no [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md).

![Ilustração de valores do sistema para uma faixa de triângulos instanciada](images/d3d10-ia-example.png)

Estas tabelas mostram os valores do sistema gerados para ambas as instâncias da mesma faixa de triângulos. A primeira instância (instância U) é mostrada em azul, a segunda instância (instância V) é mostrada em verde. As linhas sólidas conectam os vértices aos primitivos, as linhas tracejadas conectam os vértices adjacentes.

As tabelas a seguir mostram os valores gerados pelo sistema para a instância U.

| Dados de vértice    | C,U | D,U | E,U | F,U | G,U | H,U | I,U | J,U | K,U | L,U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

A instância de faixa de triângulos U tem 3 primitivos de triângulo, com os seguintes valores gerados pelo sistema:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

As tabelas a seguir mostram os valores gerados pelo sistema para a instância V.

| Dados de vértice    | C,V | D,V | E,V | F,V | G,V | H,V | I,V | J,V | K,V | L,V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

A instância de faixa de triângulos V tem 3 primitivos de triângulo, com os seguintes valores gerados pelo sistema:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

O [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md) gera as IDs (vértice, primitivo e instância); observe também que cada instância recebe uma ID de instância única. Os dados terminam com o recorte de faixa, que separa cada instância da faixa de triângulos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Estágio do assembler de entrada (IA)](input-assembler-stage--ia-.md)

 

 




