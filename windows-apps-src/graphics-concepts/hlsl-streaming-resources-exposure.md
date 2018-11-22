---
title: Exposição de recursos de streaming HLSL
description: Uma sintaxe específica da Microsoft High Level Shader Language (HLSL) é necessária para dar suporte a recursos de streaming no Modelo de Sombreador 5.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- Exposição de recursos de streaming HLSL
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8523f4895c541ffb3b92ee00d5b62c57343ae00
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7572128"
---
# <a name="hlsl-streaming-resources-exposure"></a>Exposição de recursos de streaming HLSL


Uma sintaxe específica da Microsoft High Level Shader Language (HLSL) é necessária para dar suporte a recursos de streaming no [Modelo de Sombreador 5](https://msdn.microsoft.com/library/windows/desktop/ff471356).

A sintaxe HLSL para o Modelo de Sombreador 5 é permitida apenas em dispositivos com suporte a recursos de streaming. Cada método HLSL relevante para streaming de recursos na tabela a seguir aceita um (feedback) ou dois (vinculação e feedback nesta ordem) parâmetros opcionais adicionais. Por exemplo, um método de **Amostra** é:

**Amostra(amostra, local \[, deslocamento \[, vinculação \[, feedback\] \] \])**

Um exemplo de um método de **Amostra** é [**Texture2D.Sample(S,float,int,float,uint)**](https://msdn.microsoft.com/library/windows/desktop/dn393787).

Os parâmetros de deslocamento, vinculação e feedback são opcionais. Você deve especificar todos os parâmetros opcionais até aquele que você precisa, que é consistente com as regras do C++ para argumentos de função padrão. Por exemplo, se for necessário o status de feedback, parâmetros de compensação e vinculação precisam ser fornecidos explicitamente à **Amostra**, mesmo que eles talvez não sejam logicamente necessários.

O parâmetro de vinculação é um valor flutuante escalar. O valor literal de vinculação = 0.0f indica que a operação de vinculação não é executada.

O parâmetro de feedback é uma variável **uint** que você pode fornecer para o acesso à memória consultando a função intrínseca [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083). Você não deve modificar ou interpretar o valor do parâmetro de feedback; porém, o compilador não fornece qualquer análise avançada e diagnóstico para detectar se você modificou o valor.

Veja aqui a sintaxe de [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083):

**bool CheckAccessFullyMapped(em uint FeedbackVar);**

[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) interpreta o valor de *FeedbackVar* e retorna true se todos os dados que estão sendo acessados foram mapeados no recurso; caso contrário, **CheckAccessFullyMapped** retorna false.

Se o parâmetro de feedback ou vinculação estiver presente, o compilador emite uma variante de instrução básica. Por exemplo, o exemplo de um recurso de streaming gera a instrução `sample_cl_s`.

Se nem a vinculação nem o feedback forem especificados, o compilador emite a instrução básica, para que não haja nenhuma alteração do comportamento atual.

O valor de vinculação de 0.0f indica que nenhuma vinculação é realizada; dessa forma, o compilador de driver adicional pode continuar adaptando a instrução para o hardware de destino. Se o feedback for um registro NULL em uma instrução, o feedback não é utilizado; dessa forma, o compilador de driver pode continuar adaptando a instrução para a arquitetura de destino.

Se o compilador HLSL infere que vinculação é 0.0f e feedback não é utilizado, o compilador emite a instrução básica correspondente (por exemplo, `sample` em vez de `sample_cl_s`).

Se um acesso a recursos streaming consiste em várias instruções de código de bytes constituintes, por exemplo, para recursos estruturados, o compilador agrega valores de feedback individuais através da operação OR para produzir o valor de feedback final. Portanto, você pode ver um único valor de feedback para tal acesso complexo.

Esta é a tabela resumida dos métodos HLSL que são alterados para dar suporte a feedback e/ou vinculação. Todos esses trabalham em recursos em bloco e não streaming de todas as dimensões. Os recursos não streaming sempre aparentam estar totalmente mapeados.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471359">Objetos HLSL</a> </th>
<th align="left">Métodos intrínsecos com opção de feedback (*) - também tem a opção de vinculação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Coletar</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>[RW] Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>[RW]Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Amostra*</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>Texture2DMS</p>
<p>[RW]Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>[RW]Texture3D</p>
<p>[RW]Buffer</p>
<p>[RW]ByteAddressBuffer</p>
<p>[RW]StructuredBuffer</p></td>
<td align="left">Carga</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Acesso pipeline aos recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




