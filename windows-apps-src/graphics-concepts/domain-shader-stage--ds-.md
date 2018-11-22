---
title: Estágio de Sombreador de Domínio (DS)
description: O estágio do Sombreador de Domínio (DS) calcula a posição de vértice de um ponto subdividido no patch saída; ele calcula a posição do vértice que corresponde a cada amostra de domínio.
ms.assetid: 673CC04A-A74F-495F-AFB7-49157538749C
keywords:
- Estágio de Sombreador de Domínio (DS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3bcad4a5e22249d4d7faed08fe9cc9af4c3fb338
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7561729"
---
# <a name="domain-shader-ds-stage"></a>Estágio de Sombreador de Domínio (DS)


O estágio do Sombreador de Domínio (DS) calcula a posição de vértice de um ponto subdividido no patch saída; ele calcula a posição do vértice que corresponde a cada amostra de domínio. Um sombreador de domínio é executado uma vez por ponto de saída do estágio de mosaico e tem acesso somente leitura às constantes do patch de saída, ao patch de saída do sombreador e às coordenadas UV de saída do estágio de mosaico.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


O estágio de Sombreador de Domínio (DS) gera a posição de vértice de um ponto subdividido no patch de saída, com base na entrada a partir do [estágio de Sombreador Hull (HS)](hull-shader-stage--hs-.md) e o [estágio de Mosaico (TS)](tessellator-stage--ts-.md).

![diagrama do estágio do sombreador de domínio](images/d3d11-domain-shader.png)

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


-   Um sombreador de domínio consome pontos de controle de saída do [estágio de Sombreador Hull (HS)](hull-shader-stage--hs-.md). As saídas de sombreador hull incluem:
    -   Pontos de controle.
    -   Dados da constante do patch.
    -   Fatores de mosaico. Os fatores de mosaico podem incluir os valores usados pelo gerador de mosaico de função fixa, bem como os valores brutos (antes do arredondamento por mosaico de números inteiros, por exemplo), que facilita o geomorfismo, por exemplo.
-   Um sombreador de domínio é chamado uma vez por coordenadas de saída do [estágio de Mosaico (TS)](tessellator-stage--ts-.md).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


-   O estágio de Sombreador de Domínio (DS) gera a posição de vértice de um ponto subdividido no patch de saída.

Depois que o sombreador de domínio é concluído, o mosaico é concluído e os dados de pipeline continuam para o próximo estágio de pipeline, como o [estágio do Sombreador de Geometria (GS)](geometry-shader-stage--gs-.md) e o [estágio do Sombreador de Pixel (PS)](pixel-shader-stage--ps-.md). Um sombreador de geometria que espera primitivos com adjacência (por exemplo, 6 vértices por triângulo) não é válido quando mosaico está ativo (isso resulta em um comportamento indefinido, sobre o qual a camada de depuração emitirá um aviso).

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


```
void main( out    MyDSOutput result, 
           float2 myInputUV : SV_DomainPoint, 
           MyDSInput DSInputs,
           OutputPatch<MyOutPoint, 12> ControlPts, 
           MyTessFactors tessFactors)
{
     ...

     result.Position = EvaluateSurfaceUV(ControlPoints, myInputUV);
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




