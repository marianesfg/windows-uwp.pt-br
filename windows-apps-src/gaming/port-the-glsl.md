---
title: Fazer a portabilidade do GLSL
description: Quando você tiver movido o código que cria e configura os seus buffers e objetos de sombreador, será o momento de fazer a portabilidade do código dentro dos sombreadores da linguagem de sombreadores GL do OpenGL ES 2.0 (GLSL) para a linguagem de sombreadores de alto nível do Direct3D 11 (HLSL).
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, glsl, porta
ms.localizationpriority: medium
ms.openlocfilehash: 809440f9e77af19c01f4a050eee3b6f8d1c709b7
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8484449"
---
# <a name="port-the-glsl"></a>Fazer a portabilidade do GLSL




**APIs Importantes**

-   [Semântica HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574)
-   [Constantes de sombreador (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)

Quando você tiver movido o código que cria e configura os seus buffers e objetos de sombreador, será o momento de fazer a portabilidade do código dentro dos sombreadores da linguagem de sombreadores GL do OpenGL ES 2.0 (GLSL) para a linguagem de sombreadores de alto nível do Direct3D 11 (HLSL).

No OpenGL ES 2.0, os sombreadores devolvem dados depois da execução, usando intrínsecos como **gl\_Position**, **gl\_FragColor** ou **gl\_FragData\[n\]** (onde n é o índice de um destino de renderização específico). No Direct3D, não há intrínsecos específicos e os sombreadores devolvem dados como o tipo de retorno das duas respectivas funções principais().

Dados que você quer interpolados entre estágios de sombreador, como a posição do vértice ou normal, são manipulados pelo uso da declaração **varying**. Entretanto, o Direct3D não tem essa declaração. Em vez disso, cada dado que você quer que passe entre os estágios de sombreador devem ser marcados com uma [semântica HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574). A semântica específica selecionada indica (e é) a finalidade dos dados. Por exemplo, você deve declarar os dados que deseja interpolar entre o sombreador de fragmento como:

`float4 vertPos : POSITION;`

ou

`float4 vertColor : COLOR;`

Onde POSITION é a semântica usada para indicar dados de posição de vértice. POSITION também é um caso especial, pois, após a interpolação, ele não pode ser acessado pelo sombreador de pixel. Portanto, você deve especificar a entrada para o sombreador de pixel com SV\_POSITION e os dados de vértice interpolados serão posicionados naquela variável.

`float4 position : SV_POSITION;`

A semântica pode ser declarada nos métodos de corpo (principais) dos sombreadores. Para sombreadores de pixel, SV\_TARGET\[n\], que indicam um destino de renderização, é exigido no método de corpo. (SV\_TARGET sem sufixo numérico vai para o padrão, que é o índice de destino de renderização 0.)

Perceba também que sombreadores de vértice são exigidos para gerar a semântica de valor de sistema SV\_POSITION. Essa semântica resolve os dados de posição do vértice para coordenar valores em que x está entre -1 e 1, y está entre -1 e 1, z está dividido pelo valor homogêneo original da coordenada w (z/w) e w é 1 dividido pelo valor original de w (1/w). Sombreadores de pixel usam a semântica de valor de sistema SV\_POSITION para recuperar a localização de pixel na tela, onde x está entre 0 e a largura do destino de renderização e y está entre 0 e a altura do destino de renderização (cada deslocado em 0,5). Sombreadores de pixel de recursos nível 9\_x não podem ler a partir do valor SV\_POSITION.

Buffers constantes devem ser declarados com **cbuffer** e associados a um registro de inicialização específico para pesquisa.

Direct3D 11: declaração de buffer constante HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Aqui, o buffer constante usa registro b0 para colocar em espera o buffer em pacote. Todos os registros são referidos na forma b\#. Para saber mais sobre a implementação HLSL de buffers constantes, registros e remessa de dados, leia [Shader Constants (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581).

<a name="instructions"></a>Instruções
------------
### <a name="step-1-port-the-vertex-shader"></a>Etapa 1: compatibilizar o sombreador de vértice

No nosso exemplo simples de OpenGL ES 2.0, o sombreador de vértice tem três entradas: uma matriz 4x4 de projeção de exibição constante de modelo e dois vetores de 4 coordenadas. Esses dois vetores contêm a posição do vértice e sua cor. O sombreador transforma o vetor posição em coordenadas de perspectiva e o atribui ao intrínseco gl\_Position para rasterização. A cor de vértice é copiada para uma variável, que varia, para interpolação durante a rasterização também.

OpenGL ES 2.0: sombreador de vértice para objeto de cubo (GLSL)

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

No Direct3D, a matriz constante de modelo-exibição-projeção está contida em um buffer constante, empacotado no Registro b0, e a posição e cor do vértice são marcadas especificamente com a semântica HLSL apropriada: POSITION e COLOR. Como o layout de entrada indica uma ordem específica desses dois valores de vértice, crie uma estrutura para armazená-los e declare-a como o tipo do parâmetro de entrada na função (principal) do corpo do sombreador (Você também pode especificá-los como dois parâmetros individuais, mas isso pode ser problemático). Você também especifica um tipo de saída para esse estágio, que contém a cor e a posição interpoladas, e declará-la como o valor de retorno para a função do corpo do sombreador de vértice.

Direct3D 11: sombreador de vértice para objeto de cubo (HLSL)

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

### <a name="step-2-port-the-fragment-shader"></a>Etapa 2: compatibilizar o sombreador de fragmento

Nosso exemplo de sombreador de fragmento em GLSL é extremamente simples: fornecer o intrínseco gl\_FragColor com o valor de cor interpolado. O OpenGL ES 2.0 o reescreverá no destino de renderização padrão.

OpenGL ES 2.0: sombreador de fragmento para objeto de cubo (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

O Direct3D é quase tão simples quanto o OpenGL ES 2.0. A única diferença importante é que a função do corpo do sombreador de pixel deve retornar um valor. Já que a cor é um valor float de 4 coordenadas (RGBA), você indica float4 como o tipo de retorno, e, depois, especifica o destino de renderização padrão como a semântica de valor de sistema SV\_TARGET.

Direct3D 11: sombreador de pixel para o objeto cubo (HLSL)

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
Entender a semântica HLSL e o compactação de buffers constantes pode evitar certa dor de cabeça, e oferecer oportunidade de otimização. Caso tenha chance, leia [Sintaxe variável (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509706), [Introdução a buffers no Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898) e [Como: criar um buffer constante](https://msdn.microsoft.com/library/windows/desktop/ff476896). Caso não consiga fazer isso, consulte algumas dicas iniciais sobre semântica e buffers constantes para ter sempre em mente:

-   Sempre confira duas vezes o código da configuração Direct3D do renderizador para garantir que: as estruturas dos buffers constantes correspondam às declarações da estrutura cbuffer no HLSL; e que os tipos escalares do componente coincidam em ambas as declarações.
-   No código C++ do renderizador, use os tipos [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) nas declarações de buffer constante para garantir o empacotamento correto dos dados.
-   O melhor modo de usar buffers constantes de modo eficiente é organizar as variáveis de sombreador em buffers constantes com base na frequência de atualização. Por exemplo, caso tenha alguns dados uniformes atualizados uma vez por quadro e outros atualizados somente quando a câmera se move, talvez seja o caso de separar os dados em dois buffers constantes distintos.
-   Os primeiros erros de origem de compilação de sombreador (FXC) serão gerados pela semântica que você se esqueceu de aplicar ou aplicada incorretamente. Confira-as com atenção! Os documentos podem ser um pouco confusos, pois muitas páginas e exemplos antigos mencionam versões diferentes da semântica HLSL, anteriores ao Direct3D 11.
-   Certifique-se de que sabe qual nível de recurso do Direct3D você tem como objetivo para cada sombreador. A semântica para o nível de recursos 9\_\* é diferente da do 11\_1.
-   A semântica SV\_POSITION resolve os dados associados de posição de pós-interpolação para coordenar valores em que x está entre 0 e a largura do destino de renderização, y está entre 0 e a altura do destino de renderização e z está dividido pelo valor homogêneo original da coordenada w (z/w) e w é 1 dividido pelo valor original de w (1/w).

## <a name="related-topics"></a>Tópicos relacionados


[Como: compatibilizar um renderizador simples do OpenGL ES 2.0 ao Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Fazer a portabilidade de objetos de sombreador](port-the-shader-config.md)

[Fazer a portabilidade de dados e buffers de vértices](port-the-vertex-buffers-and-data-config.md)

[Desenhar na tela](draw-to-the-screen.md)

 

 




