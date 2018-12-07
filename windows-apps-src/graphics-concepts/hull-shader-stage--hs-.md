---
title: Estágio de sombreador Hull (HS)
description: O estágio de Sombreador Hull (HS) é um dos estágios de mosaico, que divide uma única superfície de um modelo em vários triângulos com eficiência.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- Estágio de sombreador Hull (HS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9137f7ef46da1b861976dbac680327febf315dac
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8788584"
---
# <a name="hull-shader-hs-stage"></a>Estágio de sombreador Hull (HS)


O estágio de Sombreador Hull (HS) é um dos estágios de mosaico, que divide uma única superfície de um modelo em vários triângulos com eficiência. O estágio de sombreador Hull (HS) produz um patch de geometria (e constantes de patch) que corresponde a cada entrada de patch (quad, triângulo ou linha). Um sombreador hull é chamado uma vez por patch e transforma os pontos de controle de entrada que definem uma superfície de ordem baixa em pontos de controle que formam um patch. Ela também faz alguns cálculos por patch para fornecer dados para o [estágio de Mosaico (TS)](tessellator-stage--ts-.md) e o [estágio do Sombreador de Domínio (DS)](domain-shader-stage--ds-.md).

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


![diagrama da fase de sombreadores hull](images/d3d11-hull-shader.png)

Os três estágios de mosaico trabalham juntos para converter superfícies de ordem superior (que mantêm o modelo simples e eficiente) em vários triângulos para uma renderização detalhada dentro do pipeline de elementos gráficos. Os estágios de mosaico incluem o estágio de Sombreador Hull (HS), [estágio de Mosaico (TS)](tessellator-stage--ts-.md), e [estágio do Sombreador de Domínio (DS)](domain-shader-stage--ds-.md).

O estágio de Sombreador Hull (HS) é um estágio de sombreador programável. Um sombreador hull é implementado com uma função HLSL.

Um sombreador hull opera em duas fases: uma fase de ponto de controle e uma fase de patch constante, que é executada em paralelo pelo hardware. O compilador HLSL extrai o paralelismo em um sombreador hull e o codifica em código de bytes que orienta o hardware.

-   A fase de ponto de controle opera uma vez para cada ponto de controle, lendo os pontos de controle para um patch e gerando um ponto de controle de saída (identificado por um **ControlPointID**).
-   A fase de patch constante opera uma vez por patch para gerar fatores de mosaico de borda e outras constantes por patch. Internamente, muitas fases de constante de patch podem ser executadas ao mesmo tempo. A fase de patch constante tem acesso somente leitura a todos os pontos de controle de entrada e saída.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Entre 1 e 32 pontos de controle de entrada, que juntos definem uma superfície de ordem baixa.

-   s sombreador hull declara o estado exigido pelo [estágio de Mosaico (TS)](tessellator-stage--ts-.md). Isso inclui informações como o número de pontos de controle, o tipo da face do patch e o tipo de particionamento a ser usado durante a criação do mosaico. Essas informações são exibidas como declarações normalmente na frente do código do sombreador.
-   Os fatores de mosaico determinam quanto subdividir cada patch.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


Entre 1 e 32 pontos de controle de saída, que juntos formam um patch.

-   A saída do sombreador é entre 1 e 32 pontos de controle, independentemente do número de fatores de mosaico. Os pontos de controle de saída de um sombreador hull podem ser consumidos pelo estágio do sombreador de domínio. Dados constantes de patch podem ser consumidos por um sombreador de domínio. Fatores de mosaico podem ser consumidos pelo [estágio de Mosaico (TS)](tessellator-stage--ts-.md) e o [estágio do Sombreador de Domínio (DS)](domain-shader-stage--ds-.md).
-   Se o sombreador hull definir qualquer fator de mosaico de borda para ≤ 0 ou NaN, o patch será removido (omitido). Como resultado, o estágio de mosaico pode ou não ser executado, o sombreador de domínio não será executado e nenhuma saída visível será produzida para esse patch.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


```
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

Consulte [como: criar um sombreador Hull](https://msdn.microsoft.com/library/windows/desktop/ff476338).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




