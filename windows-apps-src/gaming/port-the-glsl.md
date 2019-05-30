---
title: Fazer a portabilidade do GLSL
description: Quando você tiver movido o código que cria e configura os seus buffers e objetos de sombreador, será o momento de fazer a portabilidade do código dentro dos sombreadores da linguagem de sombreadores GL do OpenGL ES 2.0 (GLSL) para a linguagem de sombreadores de alto nível do Direct3D 11 (HLSL).
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, glsl, porta
ms.localizationpriority: medium
ms.openlocfilehash: 210f98476a06b88e7d3d543006a6d4ec886cfd45
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368250"
---
# <a name="port-the-glsl"></a>Fazer a portabilidade do GLSL




**APIs importantes**

-   [Semântica HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps)
-   [Shader Constants (HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)

Quando você tiver movido o código que cria e configura os seus buffers e objetos de sombreador, será o momento de fazer a portabilidade do código dentro dos sombreadores da linguagem de sombreadores GL do OpenGL ES 2.0 (GLSL) para a linguagem de sombreadores de alto nível do Direct3D 11 (HLSL).

Em OpenGL ES 2.0, sombreadores retornam dados após a execução usando intrínsecos, como **gl\_posição**, **gl\_FragColor**, ou **gl\_FragData \[n\]**  (em que n é o índice para um destino de renderização específico). No Direct3D, não há intrínsecos específicos e os sombreadores devolvem dados como o tipo de retorno das duas respectivas funções principais().

Dados que você quer interpolados entre estágios de sombreador, como a posição do vértice ou normal, são manipulados pelo uso da declaração **varying**. Entretanto, o Direct3D não tem essa declaração. Em vez disso, cada dado que você quer que passe entre os estágios de sombreador devem ser marcados com uma [semântica HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps). A semântica específica selecionada indica (e é) a finalidade dos dados. Por exemplo, você deve declarar os dados que deseja interpolar entre o sombreador de fragmento como:

`float4 vertPos : POSITION;`

ou

`float4 vertColor : COLOR;`

Onde POSITION é a semântica usada para indicar dados de posição de vértice. POSITION também é um caso especial, pois, após a interpolação, ele não pode ser acessado pelo sombreador de pixel. Portanto, você deve especificar a entrada para o sombreador de pixel com VA\_posição e os dados de vértice interpolados serão colocados nessa variável.

`float4 position : SV_POSITION;`

A semântica pode ser declarada nos métodos de corpo (principais) dos sombreadores. Para sombreadores de pixel, SV\_alvo\[n\], que indica um destino de renderização, é necessário no método de corpo. (VA\_sem um sufixo numérico assume como padrão para renderizar o índice de destino 0 de destino.)

Observe também que os sombreadores de vértices são necessárias para a saída a VA\_semântica de valor do sistema de posição. Essa semântica resolve os dados de posição de vértice para valores de coordenadas nos casos em que x fica entre -1 e 1, y fica entre -1 e 1, z é dividido pelo valor de w da coordenada homogênea original (z/w) e w é 1 dividido pelo valor original de w (1/w). Sombreadores de pixel usam a VA\_valor semântico para recuperar o local de pixel na tela, onde x é entre 0 e a largura de destino de renderização e y da posição sistema está entre 0 e a altura de destino de renderização (cada deslocamento por 0,5). 9 de nível de recurso\_x não é possível ler a VA sombreadores de pixel\_valor da posição.

Buffers constantes devem ser declarados com **cbuffer** e associados a um registro de inicialização específico para pesquisa.

Direct3D 11: Uma declaração de buffer de constantes HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Aqui, o buffer constante usa registro b0 para colocar em espera o buffer em pacote. Todos os registros são chamados no formulário b\#. Para saber mais sobre a implementação HLSL de buffers constantes, registros e remessa de dados, leia [Shader Constants (HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants).

<a name="instructions"></a>Instruções
------------
### <a name="step-1-port-the-vertex-shader"></a>Etapa 1: O sombreador de vértices da porta

No nosso exemplo simples de OpenGL ES 2.0, o sombreador de vértice tem três entradas: uma matriz 4x4 de projeção de exibição constante de modelo e dois vetores de 4 coordenadas. Esses dois vetores contêm a posição do vértice e sua cor. O sombreador transforma o vetor da posição em coordenadas de ponto de Vista e o atribui ao gl\_intrínseco para a rasterização de posição. A cor de vértice é copiada para uma variável, que varia, para interpolação durante a rasterização também.

OpenGL ES 2.0: Sombreador de vértice para o objeto de cubo (GLSL)

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

Agora, no Direct3D, a matriz de projeção do modelo-exibição constante está contida em um buffer de constantes empacotada no registro b0 e a posição do vértice e a cor especificamente são marcados com a semântica HLSL respectiva apropriada: POSIÇÃO e cor. Como o layout de entrada indica uma ordem específica desses dois valores de vértice, crie uma estrutura para armazená-los e declare-a como o tipo do parâmetro de entrada na função (principal) do corpo do sombreador (Você também pode especificá-los como dois parâmetros individuais, mas que poderia obter complicado.) Você também especifica um tipo de saída para este estágio, que contém a posição interpolada e a cor, e declare-o como o valor de retorno para a função do corpo do sombreador de vértice.

Direct3D 11: Sombreador de vértice para o objeto de cubo (HLSL)

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

O tipo de dados de saída, PixelShaderInput, é populado durante a rasterização e fornecido ao sombreador de fragmento (pixel).

### <a name="step-2-port-the-fragment-shader"></a>Etapa 2: O sombreador de fragmentos da porta

Nosso sombreador de fragmento de exemplo no GLSL é extremamente simple: forneça o gl\_FragColor intrínseco com o valor de cor interpolados. O OpenGL ES 2.0 o reescreverá no destino de renderização padrão.

OpenGL ES 2.0: Sombreador de fragmentos para o objeto de cubo (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

O Direct3D é quase tão simples quanto o OpenGL ES 2.0. A única diferença importante é que a função do corpo do sombreador de pixel deve retornar um valor. Como a cor é um valor de float 4 coordenada (RGBA), você indicar float4 como o tipo de retorno e, em seguida, especifique o destino de renderização padrão como a VA\_semântica de valor do sistema de destino.

Direct3D 11: Sombreador de pixel para o objeto de cubo (HLSL)

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

A cor do pixel na posição é gravada no destino de renderização. Agora, vamos ver como exibir os conteúdos daquele destino de renderização em [Desenhar na tela](draw-to-the-screen.md)!

## <a name="previous-step"></a>Etapa anterior


[Fazer a portabilidade de buffers de vértices e dados](port-the-vertex-buffers-and-data-config.md) Próxima etapa
---------
[Desenhar na tela](draw-to-the-screen.md) Observações
-------
Entender a semântica HLSL e o compactação de buffers constantes pode evitar certa dor de cabeça, e oferecer oportunidade de otimização. Se você receber uma chance, leia [variável sintaxe HLSL ()](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-syntax), [Introdução aos Buffers no Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro), e [como: Criar um Buffer de constantes](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to). Caso não consiga fazer isso, consulte algumas dicas iniciais sobre semântica e buffers constantes para ter sempre em mente:

-   Sempre confira duas vezes o código da configuração Direct3D do renderizador para garantir que: as estruturas dos buffers constantes correspondam às declarações da estrutura cbuffer no HLSL; e que os tipos escalares do componente coincidam em ambas as declarações.
-   No código C++ do renderizador, use os tipos [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) nas declarações de buffer constante para garantir o empacotamento correto dos dados.
-   O melhor modo de usar buffers constantes de modo eficiente é organizar as variáveis de sombreador em buffers constantes com base na frequência de atualização. Por exemplo, caso tenha alguns dados uniformes atualizados uma vez por quadro e outros atualizados somente quando a câmera se move, talvez seja o caso de separar os dados em dois buffers constantes distintos.
-   Os primeiros erros de origem de compilação de sombreador (FXC) serão gerados pela semântica que você se esqueceu de aplicar ou aplicada incorretamente. Confira-as com atenção! Os documentos podem ser um pouco confusos, pois muitas páginas e exemplos antigos mencionam versões diferentes da semântica HLSL, anteriores ao Direct3D 11.
-   Certifique-se de que sabe qual nível de recurso do Direct3D você tem como objetivo para cada sombreador. A semântica de recurso de nível 9\_ \* são diferentes para 11\_1.
-   A VA\_resolve semântico de posição os dados da posição de pós-interpolação associado para coordenar os valores em que x é entre 0 e a largura de destino de renderização, y estão entre 0 e a renderização de altura de destino, z é dividida por l coordenada homogênea original valor (z/w) e w é 1 dividido pelo valor w original (1/w).

## <a name="related-topics"></a>Tópicos relacionados


[Como: um renderizador simple do OpenGL ES 2.0 ao Direct3D 11 da porta](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Fazer a portabilidade de objetos de sombreador](port-the-shader-config.md)

[Realizar a portabilidade de dados e buffers de vértice](port-the-vertex-buffers-and-data-config.md)

[Desenhar na tela](draw-to-the-screen.md)

 

 




