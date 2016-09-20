---
author: mtoepke
title: "Criar e exibir uma malha básica"
description: "Os jogos 3D da UWP (Plataforma Universal do Windows) geralmente usam polígonos para representar objetos e superfícies do jogo."
ms.assetid: bfe0ed5b-63d8-935b-a25b-378b36982b7d
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c082456d5eb0cf1c5c697a6af5bc1d4de1f5ada2

---

# Criar e exibir uma malha básica


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Os jogos 3D da UWP (Plataforma Universal do Windows) geralmente usam polígonos para representar objetos e superfícies do jogo. As listas de vértices que compõem a estrutura desses objetos e superfícies poligonais são chamadas de malhas. Aqui, vamos criar uma malha básica para um objeto cúbico e fornecê-lo no pipeline de sombreador para renderização e exibição.

> 
            **Importante**   O código de exemplo incluído aqui usa tipos (como DirectX::XMFLOAT3 e DirectX::XMFLOAT4X4) e métodos embutidos declarados no DirectXMath.h. Se você estiver recortando e colando esse código, \#include &lt;DirectXMath.h&gt; no seu projeto.

 

## O que você precisa saber


### Tecnologias

-   [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh769064)

### Pré-requisitos

-   Conhecimento básico de álgebra linear e sistemas de coordenadas 3D
-   Um modelo do Visual Studio 2015 Direct3D

## Instruções

### Etapa 1: construir a malha do modelo

Na maioria dos jogos, a malha de um objeto de jogo é carregada de um arquivo que contém os dados de vértice específicos. A ordenação desses vértices depende do aplicativo, mas eles geralmente são serializados como faixas ou leques. Os dados de vértice podem se originar em qualquer fonte de software ou podem ser criados manualmente. Cabe ao seu jogo interpretar os dados de maneira que o sombreador de vértice possa efetivamente processá-los.

No nosso exemplo, usamos uma malha simples para um cubo. O cubo, assim como qualquer malha de objeto nesse estágio do pipeline, é representado por meio de seu próprio sistema de coordenadas. O sombreador de vértice obtém as coordenadas e, aplicando as matrizes de transformação fornecidas por você, retorna a projeção final de exibição 2D em um sistema de coordenadas homogêneo.

Defina a malha para um cubo. (Ou carregue-a de um arquivo. Você decide.)

```cpp
SimpleCubeVertex cubeVertices[] =
{
    { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
    { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 0.0f) },
    { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 1.0f) },
    { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 1.0f) },

    { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
    { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 1.0f) },
    { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f) },
    { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f) },
};
```

O sistema de coordenadas do cubo coloca o centro do cubo na origem, com o eixo y sendo executado de cima para baixo, usando o sistema de coordenadas à esquerda. Os valores de coordenadas são expressos como valores flutuantes de 32 bits entre -1 e 1.

Em cada emparelhamento entre parênteses, o segundo grupo de valores DirectX::XMFLOAT3 especifica a cor associada ao vértice como um valor RGB. Por exemplo, o primeiro vértice em (-0.5, 0.5, -0.5) tem um cor verde completa (o valor G está definido como 1.0 e os valores "R" e "B" estão definidos como 0).

Portanto, você tem 8 vértices, cada um com uma cor específica. Cada vértice/emparelhamento entre parênteses representa os dados completos de um vértice, no nosso exemplo. Ao especificar o buffer do vértices, tenha em mente esse layout específico. Nós fornecemos esse layout de entrada para o sombreador de vértice, para que ele possa entender os seus dados de vértice.

### Etapa 2: configurar o layout de entrada

Agora, os vértices estão na memória. O dispositivo gráfico, porém, tem sua própria memória e você usa o Direct3D para acessá-la. Para que os seus dados sejam disponibilizados no dispositivo gráfico para processamento, é preciso limpar o caminho e deixá-lo como era: declare como os dados de vértice são organizados para que o dispositivo gráfico possa interpretá-los quando eles forem obtidos do seu jogo. Para isso, use [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575).

Declare e defina o layout de entrada para o buffer de vértices.

```cpp
const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

ComPtr<ID3D11InputLayout> inputLayout;
m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                vertexShaderBytecode->Data,
                vertexShaderBytecode->Length,
                &inputLayout)
);
```

Nesse código, você define um layout para os vértices; especificamente, quais dados cada elemento da lista de vértices contém. Aqui, na **basicVertexLayoutDesc**, são especificados dois componentes de dados:

-   
            **POSITION**: uma semântica HLSL para dados de posição fornecidos a um sombreador. Nesse código, é um DirectX::XMFLOAT3 ou, mais especificamente, uma estrutura com 3 valores de ponto de flutuação de 32 bits correspondente a uma coordenada 3D (x, y, z). Também será possível usar um float4 se você fornecer a coordenada homogênea "w" e, nesse caso, especifique DXGI\_FORMAT\_R32G32B32A32\_FLOAT. A escolha de um DirectX::XMFLOAT3 ou de um float4 depende das necessidades inerentes ao jogo. Portanto, verifique se os dados de vértice da sua malha correspondem corretamente ao formato que você usa.

    Cada valor de coordenada é expresso como um valor de ponto de flutuação entre -1 e 1 no espaço de coordenadas do objeto. Quando o sombreador de vértice é concluído, o vértice transformado está no espaço de projeção de exibição (perspectiva corrigida) homogênea.

    "Porém, o valor de enumeração indica RGB, não XYZ!" você observa. Muito bem! Nos dois casos de dados de cor e dados de coordenada, você geralmente usa 3 ou 4 valores de componentes, então, por que não usar o mesmo formato em ambos? A semântica HLSL, não o nome do formato, indica como o sombreador trata os dados.

-   
            **COLOR**: uma semântica HLSL para dados de cor. Como em **POSITION**, ela consiste em três valores de ponto de flutuação de 32 bits (DirectX::XMFLOAT3). Cada valor contém um componente de cor: vermelho (r), azul (b) ou verde (g), expressos como um número flutuante entre 0 e 1.

    
            Os valores **COLOR** são tipicamente retornados como um valor RGBA de componente 4 no final do pipeline de sombreador. Para esse exemplo, você definirá o valor alfabético "A" como 1.0 (opacidade máxima) no pipeline de sombreador para todos os pixels.

Para obter uma lista completa de formatos, consulte [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059). Para ver uma lista completa de semânticas HLSL, consulte [Semânticas](https://msdn.microsoft.com/library/windows/desktop/bb509647).

Chame [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) e crie o layout de entrada no dispositivo Direct3D. Agora, você precisa criar um buffer que possa realmente manter os dados.

### Etapa 3: popular os buffers de vértices

Os buffers de vértices contêm a lista de vértices de cada triângulo da malha. Cada vértice deve ser exclusivo nessa lista. Em nosso exemplo, há 8 vértices para o cubo. O sombreador de vértice é executado no dispositivo gráfico e lê o buffer de vértices, e interpreta os dados com base no layout de entrada especificado na etapa anterior.

No próximo exemplo, você fornecerá uma descrição e um sub-recurso para o buffer, o que informará o Direct3D sobre vários aspectos do mapeamento físico dos dados de vértice e como tratá-lo na memória, no dispositivo gráfico. Isso é necessário porque você usa um [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) genérico, que pode conter qualquer coisa! As estruturas [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) e [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) são fornecidas para garantir que o Direct3D entenda o layout da memória física do buffer, incluindo o tamanho de cada elemento de vértice no buffer, e também o tamanho máximo da lista de vértices. Aqui, também é possível controlar o acesso à memória do buffer e a forma como ela é partilhada, mas isso está um pouco além do escopo deste tutorial.

Depois de configurar o buffer, você chama [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) para criá-lo efetivamente. Claro, se você tiver mais de um objeto, crie buffers para cada modelo exclusivo.

Declare e crie o buffer de vértices.

```cpp
D3D11_BUFFER_DESC vertexBufferDesc = {0};
vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
vertexBufferDesc.CPUAccessFlags = 0;
vertexBufferDesc.MiscFlags = 0;
vertexBufferDesc.StructureByteStride = 0;

D3D11_SUBRESOURCE_DATA vertexBufferData;
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;

ComPtr<ID3D11Buffer> vertexBuffer;
m_d3dDevice->CreateBuffer(
                &vertexBufferDesc,
                &vertexBufferData,
                &vertexBuffer);
```

Vértices carregados. Mas, qual é a ordem de processamento desses vértices? Esse aspecto é tratado quando você fornece uma lista de índices para os vértices: a ordenação desses índices é a ordem em que o sombreador de vértice os processa.

### Etapa 4: popular os buffers de índices

Agora, forneça uma lista dos índices de cada um dos vértices. Esses índices correspondem à posição do vértice no buffer de vértices, começando com 0. Para ajudar você a visualizar isso, considere que cada vértice exclusivo na sua malha tem um número exclusivo atribuído a ele, como uma ID. Essa ID é a posição inteira do vértice no buffer de vértices.

![Um cubo com oito vértices numerados](images/cube-mesh-1.png)

No nosso exemplo de cubo, há 8 vértices, os quais criam 6 quadrupletos para os lados. Você divide os quadrupletos em triângulos, para um total de 12 triângulos que usam nossos 8 vértices. Nos 3 vértices por triângulo, há 36 entradas no buffer de índices. No nosso exemplo, esse padrão de índice é conhecido como uma lista de triângulos e você a indica para o Direct3D como uma **D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLELIST**, ao definir a topologia primitiva.

Está é, provavelmente, a maneira mais ineficiente de listar índices, pois há muitas redundâncias quando os triângulos compartilham pontos e lados. Por exemplo, quando um triângulo compartilha um lado em uma sombra de losango, você lista 6 índices para os quatro vértices, assim:

![ordem de índices ao construir um losango](images/rhombus-surface-1.png)

-   Triângulo 1: \[0, 1, 2\]
-   Triângulo 2: \[0, 2, 3\]

Em uma topologia de faixa ou leque, você ordena os vértices de forma a eliminar muitos lados redundantes durante a passagem (por exemplo, o lado do índice 0 até o índice 2 da imagem). Para malhas maiores, isso reduz significativamente o número de vezes em que o sombreador de vértice é executado e melhora muito o desempenho. Entretanto, vamos manter isso simples e compatível com a lista de triângulos.

Declare os índices do buffer de vértices de maneira tão simples quanto a topologia da lista de triângulos.

```cpp
unsigned short cubeIndices[] =
{   0, 1, 2,
    0, 2, 3,

    4, 5, 6,
    4, 6, 7,

    3, 2, 5,
    3, 5, 4,

    2, 1, 6,
    2, 6, 5,

    1, 7, 6,
    1, 0, 7,

    0, 3, 4,
    0, 4, 7 };
```

Trinta e seis elementos de índice no buffer indicam redundância em excesso, pois há apenas 8 vértices. Se você optar por eliminar algumas redundâncias e usar outro tipo de lista de vértices, como faixa ou leque, especifique esse tipo ao fornecer um valor [**D3D11\_PRIMITIVE\_TOPOLOGY**](https://msdn.microsoft.com/library/windows/desktop/ff476189) específico para o método [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455).

Para obter mais informações sobre as diferentes técnicas de lista de índices, consulte [Topologias primitivas](https://msdn.microsoft.com/library/windows/desktop/bb205124).

### Etapa 5: criar um buffer constante para as matrizes de transformação

Para dar início ao processamento de vértices, você precisa fornecer as matrizes de transformação que serão aplicadas (multiplicadas) em cada vértice, quando ele for executado. Para a maioria dos jogos em 3D, há três delas:

-   A matriz 4x4, que faz a transformação do sistema de coordenadas (modelo) de objeto para o sistema de coordenadas de mundo
-   A matriz 4x4, que faz a transformação do sistema de coordenadas mundial para o sistema de coordenadas (exibição) de câmera.
-   A matriz 4x4, que faz a transformação do sistema de coordenadas de câmera para o sistema de coordenadas de projeção da exibição 2D.

Essas matrizes são passadas para o sombreador em um *buffer constante*. Um buffer constante é uma região da memória que permanece constante durante toda a execução do próximo passo do pipeline do sombreador, e que pode ser acessa diretamente pelos sombreadores com base no seu código HLSL. O buffer constante é definido duas vezes: primeiramente, no código C++ do jogo e (pelo menos) uma vez na sintaxe HLSL compatível com C para o código de sombreador. A correspondência entre as duas declarações deve ser direta no que se refere aos tipos e ao alinhamento de dados. É fácil introduzir hardware par localizar erros quando o sombreador usa a declaração HLSL para interpretar dados declarados em C++ e os tipos não são correspondentes ou o alinhamento de dados está desativado.

Buffers constantes não são alterados por HLSL. Você pode alterá-los quando o jogo atualizar dados específicos. Em geral, os desenvolvedores de jogos criam 4 classes de buffers constantes: um tipo para atualizações por quadro; um tipo para atualizações por modelo/objeto; um tipo para atualizações por atualização de estado do jogo; e um tipo para os dados que jamais são modificados pela vida útil do jogo.

Neste exemplo, temos apenas a classe que nunca é modificada: os dados DirectX::XMFLOAT4X4 das três matrizes.

> 
            **Observação**   O exemplo de código apresentado aqui usa matrizes principais de colunas. É possível usar matrizes principais de linhas, usando a palavra-chave **row\_major** em HLSL e garantindo que os dados da matriz de origem também sejam principais de linha. O DirectXMath usa matrizes principais de linhas e pode ser usado com matrizes HLSL definidas com a palavra-chave **row\_major**.

 

Declare e crie um buffer constante para as três matrizes que você usa para transformar cada vértice.

```cpp
struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};
ComPtr<ID3D11Buffer> m_constantBuffer;
ConstantBuffer m_constantBufferData;

// ...

// Create a constant buffer for passing model, view, and projection matrices
// to the vertex shader.  This allows us to rotate the cube and apply
// a perspective projection to it.

D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags = 0;
constantBufferDesc.MiscFlags = 0;
constantBufferDesc.StructureByteStride = 0;
m_d3dDevice->CreateBuffer(
                &constantBufferDesc,
                nullptr,
                &m_constantBuffer
             );

m_constantBufferData.model = DirectX::XMFLOAT4X4( // Identity matrix, since you are not animating the object
            1.0f, 0.0f, 0.0f, 0.0f,
            0.0f, 1.0f, 0.0f, 0.0f,
            0.0f, 0.0f, 1.0f, 0.0f,
            0.0f, 0.0f, 0.0f, 1.0f);

);
// Specify the view (camera) transform corresponding to a camera position of
// X = 0, Y = 1, Z = 2.  

m_constantBufferData.view = DirectX::XMFLOAT4X4(
            -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
             0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
             0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
             0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f);
```

> 
            **Observação**  Geralmente, você declara a matriz de projeção ao configurar recursos específicos do dispositivo, porque os resultados dessa multiplicação devem coincidir com os parâmetros de tamanho do visor atual em 2D (que normalmente correspondem à altura e à largura de pixel da exibição). Se isso mudar, dimensione os valores das coordenadas x e y de acordo com as alterações.

 

```cpp
// Finally, update the constant buffer perspective projection parameters
// to account for the size of the application window.  In this sample,
// the parameters are fixed to a 70-degree field of view, with a depth
// range of 0.01 to 100.  

float xScale = 1.42814801f;
float yScale = 1.42814801f;
if (backBufferDesc.Width > backBufferDesc.Height)
{
    xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
}
else
{
    yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
}
m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

Enquanto está aqui, defina os buffers de vértices e índices no  [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476149) e também a topologia que está usando.

```cpp
// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(SimpleCubeVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset);

m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0);

 m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
```

Muito bem! Assembly de entrada concluído. Está tudo pronto para renderização. Vamos acionar o sombreador de vértice.

### Etapa 6: processar a malha com o sombreador de vértice

Agora que você tem um buffer de vértices contendo os vértices definidos para a malha e o buffer de índices que define a ordem de processamento dos vértices, envie-os para o sombreador de vértice. O código do sombreador de vértice, expresso como linguagem compilada de sombreador de alto nível, é executado uma vez para ada vértice no buffer de vértices, permitindo que você execute suas transformações por vértice. O resultado final é tipicamente uma projeção 2D.

(Você carregou o sombreador de vértice? Não? Então, examine [Como carregar recursos no jogo em DirectX](load-a-game-asset.md).)

Aqui você cria o sombreador de vértice...

``` syntax
// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0);
```

...e define os buffers constantes.

``` syntax
m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf());
```

Consulte o código de sombreador de vértice que lida com a transformação de coordenadas de objeto para coordenadas de mundo e, depois, para o sistema de coordenadas de projeção de exibição 2D. Também é possível aplicar alguma iluminação por vértice para agregar beleza. Isso é feito no arquivo HLSL do seu sombreador de vértices (SimplerVertexShader.hlsl, neste exemplo).

``` syntax
cbuffer simpleConstantBuffer : register( b0 )
{
    matrix model;
    matrix view;
    matrix projection;
};

struct VertexShaderInput
{
    DirectX::XMFLOAT3 pos : POSITION;
    DirectX::XMFLOAT3 color : COLOR;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
    float4 color : COLOR;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projection space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    vertexShaderOutput.pos = pos;

    // Pass the vertex color through to the pixel shader.
    vertexShaderOutput.color = float4(input.color, 1.0f);

    return vertexShaderOutput;
}
```

Está vendo **cbuffer** na parte superior? É o HLSL análogo para o mesmo buffer constante que declaramos antes no código C++. E o **VertexShaderInputstruct**? Ora, aparentemente é o layout de entrada e a declaração de dados de vértice. É importante que o buffer constante e as declarações de dados de vértice do código C++ correspondam às declarações do código HLSL, e isso inclui sinais, tipos e alinhamento de dados.


            **PixelShaderInput** especifica o layout dos dados que são retornados pela função principal do sombreador de vértice. Ao concluir o processamento de um vértice, você retornará uma posição de vértice no espaço de projeção 2D e uma cor usada para iluminação por vértice. A placa gráfica usa a saída de dados do sombreador para calcular os "fragmentos" (pixels possíveis) que deverão ser coloridos quando o sombreador de pixel for executado no próximo estágio do pipeline.

### Etapa 7: passar a malha pelo sombreador de pixel

Nesse estágio do pipeline gráfico, geralmente você executa operações por pixel nas superfícies projetadas visíveis dos objetos. (As pessoas gostam de texturas.) Para fins de amostragem, porém, basta passar isso por meio desse estágio.

Primeiro, vamos criar uma instância do sombreador de pixel. O sombreador de pixel é executado para cada pixel da projeção 2D do cenário, atribuindo uma cor a esse pixel. Nesse caso, passamos a cor do pixel retornado pela passagem direta do sombreador de vértice.

Defina o sombreador de pixel.

``` syntax
m_d3dDeviceContext->PSSetShader( pixelShader.Get(), nullptr, 0 );
```

Defina um sombreador de pixel de passagem em HLSL.

``` syntax
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

Coloque esse código em um arquivo HLSL separado do HLSL de sombreador de vértice (por exemplo, SimplePixelShader.hlsl). Esse código é executado uma vez para cada pixel visível no visor (uma representação na memória da parte da tela em que você está desenhando), o que, nesse caso, mapeia toda a tela. Agora, o pipeline gráfico está totalmente definido.

### Etapa 8: rasterizar e exibir a malha

Vamos executar o pipeline. É fácil: chame [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/bb173565).

Desenhe o cubo.

```cpp
// Draw the cube.
m_d3dDeviceContext->DrawIndexed( ARRAYSIZE(cubeIndices), 0, 0 );
            
```

Dentro da placa gráfica, cada vértice é processado na ordem especificada no buffer de índices. Depois que o código é executado no sombreador de vértices e os fragmentos 2D são definidos, o sombreador de pixel é invocado e os triângulos são coloridos.

Agora, coloque o cubo na tela.

Apresente o buffer de quadros para a exibição.

```cpp
// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop is generally  throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the app on Present until the screen is refreshed.

m_swapChain->Present(1, 0);
```

E pronto! Para um cenário repleto de modelos, use vários buffers de vértices e índices. É possível ter diferentes sombreadores para diferentes tipos de modelo. Lembre-se: cada modelo tem seu próprio sistema de coordenadas. Você precisa transformá-lo no sistema compartilhado de coordenadas de mundo usando as matrizes definidas no buffer constante.

## Comentários

Este tópico abrange a criação e a exibição de geometria simples que você mesmo cria. Para saber mais sobre o carregamento de geometria complexa de um arquivo e a conversão dessa geometria no formato do objeto de buffer de vértices específico do exemplo (.vbo), consulte [Como carregar recursos no jogo em DirectX](load-a-game-asset.md).

> **Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados


* [Como carregar recursos no jogo em DirectX](load-a-game-asset.md)

 

 







<!--HONumber=Jun16_HO4-->


