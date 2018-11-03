---
author: mtoepke
title: Renderizar a cena com teste de profundidade
description: Crie um efeito de sombra adicionando testes de profundidade ao sombreador de vértice (ou geometria) e ao sombreador de pixel.
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, renderização, cena, teste de profundidade, direct3d, sombras
ms.localizationpriority: medium
ms.openlocfilehash: dc776a60e771cc8d5961e8c7b9c67eb99fabea3a
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993384"
---
# <a name="render-the-scene-with-depth-testing"></a>Renderizar a cena com teste de profundidade




Crie um efeito de sombra adicionando testes de profundidade ao sombreador de vértice (ou geometria) e ao sombreador de pixel. Parte 3 do [Guia passo a passo: implementar volumes de sombra usando buffers de profundidade no Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="include-transformation-for-light-frustum"></a>Incluir transformação para tronco de luz


Seu sombreador de vértice precisa computar a posição transformada do espaço de luz para cada vértice. Forneça o modelo de espaço de luz e matrizes de projeção usando um buffer constante Você também pode usá-lo para fornecer a posição e a normal da luz em cálculos de iluminação. A posição transformada no espaço de luz é usada durante o teste de profundidade.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    float4 modelPos = mul(pos, model);
    pos = mul(modelPos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    // Transform the vertex position into projected space from the POV of the light.
    float4 lightSpacePos = mul(modelPos, lView);
    lightSpacePos = mul(lightSpacePos, lProjection);
    output.lightSpacePos = lightSpacePos;

    // Light ray
    float3 lRay = lPos.xyz - modelPos.xyz;
    output.lRay = lRay;
    
    // Camera ray
    output.view = eyePos.xyz - modelPos.xyz;

    // Transform the vertex normal into world space.
    float4 norm = float4(input.norm, 1.0f);
    norm = mul(norm, model);
    output.norm = norm.xyz;
    
    // Pass through the color and texture coordinates without modification.
    output.color = input.color;
    output.tex = input.tex;

    return output;
}
```

Em seguida, o sombreador de pixel usará a posição interpolada do espaço de luz fornecida pelo sombreador de vértice para testar se o pixel está na sombra.

## <a name="test-whether-the-position-is-in-the-light-frustum"></a>Testar se a posição está no tronco de luz


Primeiro, confira se o pixel está no tronco de exibição da luz, normalizando as coordenadas X e Y. Caso ambas estejam no intervalo \[0, 1\], é possível que o pixel esteja na sombra. Caso contrário, pule o teste de profundidade. Um sombreador pode fazer esse teste rapidamente chamando [Saturate](https://msdn.microsoft.com/library/windows/desktop/hh447231) e comparando o resultado com o valor original.

```cpp
// Compute texture coordinates for the current point's location on the shadow map.
float2 shadowTexCoords;
shadowTexCoords.x = 0.5f + (input.lightSpacePos.x / input.lightSpacePos.w * 0.5f);
shadowTexCoords.y = 0.5f - (input.lightSpacePos.y / input.lightSpacePos.w * 0.5f);
float pixelDepth = input.lightSpacePos.z / input.lightSpacePos.w;

float lighting = 1;

// Check if the pixel texture coordinate is in the view frustum of the 
// light before doing any shadow work.
if ((saturate(shadowTexCoords.x) == shadowTexCoords.x) &&
    (saturate(shadowTexCoords.y) == shadowTexCoords.y) &&
    (pixelDepth > 0))
{
```

## <a name="depth-test-against-the-shadow-map"></a>Teste de profundidade com base no mapa de sombra


Use um exemplo de função de comparação ([SampleCmp](https://msdn.microsoft.com/library/windows/desktop/bb509696) ou [SampleCmpLevelZero](https://msdn.microsoft.com/library/windows/desktop/bb509697)) para testar a profundidade do pixel no espaço de luz em relação ao mapa de profundidade. Calcule o valor de profundidade do espaço de luz normalizada, que é `z / w`, e passe o valor para a função de comparação. Como nós usamos um teste de comparação LessOrEqual para a amostra, a função intrínseca retorna zero quando o teste de comparação é transmitido; isso indica que o pixel está na sombra.

```cpp
// Use an offset value to mitigate shadow artifacts due to imprecise 
// floating-point values (shadow acne).
//
// This is an approximation of epsilon * tan(acos(saturate(NdotL))):
float margin = acos(saturate(NdotL));
#ifdef LINEAR
// The offset can be slightly smaller with smoother shadow edges.
float epsilon = 0.0005 / margin;
#else
float epsilon = 0.001 / margin;
#endif
// Clamp epsilon to a fixed range so it doesn't go overboard.
epsilon = clamp(epsilon, 0, 0.1);

// Use the SampleCmpLevelZero Texture2D method (or SampleCmp) to sample from 
// the shadow map, just as you would with Direct3D feature level 10_0 and
// higher.  Feature level 9_1 only supports LessOrEqual, which returns 0 if
// the pixel is in the shadow.
lighting = float(shadowMap.SampleCmpLevelZero(
    shadowSampler,
    shadowTexCoords,
    pixelDepth + epsilon
    )
    );
```

## <a name="compute-lighting-in-or-out-of-shadow"></a>Calcular a iluminação dentro ou fora da sombra


Caso o pixel não esteja na sombra, o sombreador de pixel calcula a iluminação direta e a adiciona ao valor do pixel.

```cpp
return float4(input.color * (ambient + DplusS(N, L, NdotL, input.view)), 1.f);
```

```cpp
float3 DplusS(float3 N, float3 L, float NdotL, float3 view)
{
    const float3 Kdiffuse = float3(.5f, .5f, .4f);
    const float3 Kspecular = float3(.2f, .2f, .3f);
    const float exponent = 3.f;

    // Compute the diffuse coefficient.
    float diffuseConst = saturate(NdotL);

    // Compute the diffuse lighting value.
    float3 diffuse = Kdiffuse * diffuseConst;

    // Compute the specular highlight.
    float3 R = reflect(-L, N);
    float3 V = normalize(view);
    float3 RdotV = dot(R, V);
    float3 specular = Kspecular * pow(saturate(RdotV), exponent);

    return (diffuse + specular);
}
```

Caso contrário, o sombreador de pixel deve calcular o valor do pixel com iluminação ambiente.

```cpp
return float4(input.color * ambient, 1.f);
```

Na próxima parte deste guia passo a passo, veremos como dar [Suporte a mapas de sombra em diversos hardwares](target-a-range-of-hardware.md).

 

 




