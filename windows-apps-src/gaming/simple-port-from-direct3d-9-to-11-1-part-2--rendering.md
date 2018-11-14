---
author: mtoepke
title: Converter a estrutura de renderização
description: Veja como converter uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11. Saiba também como fazer a portabilidade de buffers de geometria, como compilar e carregar programas sombreadores HLSL e como implementar a cadeia de renderização no Direct3D 11.
ms.assetid: f6ca1147-9bb8-719a-9a2c-b7ee3e34bd18
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, estrutura de renderização, conversão, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: 044a0dc7bf264a82b849623a53d00268d7b30fd9
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6260510"
---
# <a name="convert-the-rendering-framework"></a>Converter a estrutura de renderização



**Resumo**

-   [Parte 1: inicializar o Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   Parte 2: converter a estrutura de renderização
-   [Parte 3: fazer a portabilidade do loop do jogo](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Veja como converter uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11. Saiba também como fazer a portabilidade de buffers de geometria, como compilar e carregar programas sombreadores HLSL e como implementar a cadeia de renderização no Direct3D 11. Parte 2 do guia passo a passo de [portabilidade de um aplicativo simples em Direct3D 9 para o DirectX 11 e a Plataforma Universal do Windows (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="convert-effects-to-hlsl-shaders"></a>Converter efeitos em sombreadores HLSL


O exemplo a seguir é uma técnica simples de D3DX, escrita para a API herdada de efeitos, voltada à transformação de vértices de hardware e passagem de dados de cores.

Código do sombreador do Direct3D 9

```cpp
// Global variables
matrix g_mWorld;        // world matrix for object
matrix g_View;          // view matrix
matrix g_Projection;    // projection matrix

// Shader pipeline structures
struct VS_OUTPUT
{
    float4 Position   : POSITION;   // vertex position
    float4 Color      : COLOR0;     // vertex diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : COLOR0;  // Pixel color    
};

// Vertex shader
VS_OUTPUT RenderSceneVS(float3 vPos : POSITION, 
                        float3 vColor : COLOR0)
{
    VS_OUTPUT Output;
    
    float4 pos = float4(vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, g_mWorld);
    pos = mul(pos, g_View);
    pos = mul(pos, g_Projection);

    Output.Position = pos;
    
    // Just pass through the color data
    Output.Color = float4(vColor, 1.0f);
    
    return Output;
}

// Pixel shader
PS_OUTPUT RenderScenePS(VS_OUTPUT In) 
{ 
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}

// Technique
technique RenderSceneSimple
{
    pass P0
    {          
        VertexShader = compile vs_2_0 RenderSceneVS();
        PixelShader  = compile ps_2_0 RenderScenePS(); 
    }
}
```

No Direct3D 11, ainda podemos usar os sombreadores HLSL. Colocamos cada sombreador em seu próprio arquivo HLSL, de modo que o Visual Studio os compile em arquivos separados. Depois, nós os carregaremos como recursos Direct3D separados. Definimos o nível de destino como [Modelo de sombreador 4 de nível 9\_1 (/4\_0\_level\_9\_1)](https://msdn.microsoft.com/library/windows/desktop/ff476876) pois esses sombreadores são escritos para GPUs do DirectX 9.1.

Quando definimos o layout de entrada, garantimos que ele representasse a mesma estrutura de dados usada para armazenar dados por vértice na memória do sistema e da GPU. De modo semelhante, a saída de um sombreador de vértice deve corresponder à estrutura usada como entrada do sombreador de pixel. As regras não são iguais às de passagem de dados de uma função para outra no C++; você pode omitir variáveis que não foram usadas no fim da estrutura. Mas a ordem não pode ser mudada e você não pode ignorar conteúdo no meio da estrutura de dados.

> **Observação**  as regras de associação dos sombreadores de vértice aos sombreadores de pixel no Direct3D 9 eram mais flexíveis que as regras no Direct3D 11. A organização do Direct3D 9 era flexível, mas ineficiente.

 

É possível que seus arquivos HLSL usem uma sintaxe mais antiga para a semântica do sombreador (por exemplo, COLOR em vez de SV\_TARGET. Se isso for verdade, você precisará habilitar o modo de compatibilidade com HLSL (opção de compilador /Gec) ou atualizar a [semântica](https://msdn.microsoft.com/library/windows/desktop/bb509647) do sombreador de acordo com a sintaxe atual. Neste exemplo, o sombreador de vértice foi atualizado com a sintaxe atual.

Aqui está o sombreador de vértice de transformação de hardware, desta vez definido em seu próprio arquivo.

> **Observação**os sombreadores de vértice são necessários para gerar o valor de sistema sv\_position.. Essa semântica resolve os dados de posição de vértice para valores de coordenadas nos casos em que x fica entre -1 e 1, y fica entre -1 e 1, z é dividido pelo valor de w da coordenada homogênea original (z/w) e w é 1 dividido pelo valor original de w (1/w).

 

Sombreador de vértice HLSL (nível de recursos 9.1)

```cpp
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
    matrix mWorld;       // world matrix for object
    matrix View;        // view matrix
    matrix Projection;  // projection matrix
};

struct VS_INPUT
{
    float3 vPos   : POSITION;
    float3 vColor : COLOR0;
};

struct VS_OUTPUT
{
    float4 Position : SV_POSITION; // Vertex shaders must output SV_POSITION
    float4 Color    : COLOR0;
};

VS_OUTPUT main(VS_INPUT input) // main is the default function name
{
    VS_OUTPUT Output;

    float4 pos = float4(input.vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, mWorld);
    pos = mul(pos, View);
    pos = mul(pos, Projection);
    Output.Position = pos;

    // Just pass through the color data
    Output.Color = float4(input.vColor, 1.0f);

    return Output;
}
```

Isso é tudo o que precisamos para o sombreador de pixel de passagem. Embora nós chamemos de passagem, na verdade trata-se da obtenção de dados de cores interpoladas com a perspectiva correta para cada pixel. Observe que a semântica de valores do sistema SV\_TARGET é aplicada à saída do valor de cor pelo sombreador de pixel, conforme exige a API.

> **Observação**sombreadores de pixel de nível 9 \ x não podem ler o valor de sistema sv\_position.. Os sombreadores de pixel com modelo 4.0 (e superior) podem usar SV\_POSITION para recuperar o local do pixel na tela, em que x fica entre 0 e a largura do destino de renderização, e y fica entre 0 e a altura do destino de renderização (cada um com desvio de 0,5).

 

A maioria dos sombreadores de pixel é muito mais complexa do que uma passagem; não se esqueça que quanto maior o nível de recursos Direct3D, maior é o número de cálculos por programa do sombreador.

Sombreador de pixel HLSL (nível de recursos 9.1)

```cpp
struct PS_INPUT
{
    float4 Position : SV_POSITION;  // interpolated vertex position (system value)
    float4 Color    : COLOR0;       // interpolated diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : SV_TARGET;  // pixel color (your PS computes this system value)
};

PS_OUTPUT main(PS_INPUT In)
{
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}
```

## <a name="compile-and-load-shaders"></a>Compilar e carregar sombreadores


Geralmente, os jogos em Direct3D 9 usavam a biblioteca de efeitos como um modo conveniente de implementar pipelines programáveis. Era possível compilar efeitos em tempo de execução usando o método [**D3DXCreateEffectFromFile function**](https://msdn.microsoft.com/library/windows/desktop/bb172768).

Carregando um efeito no Direct3D 9

```cpp
// Turn off preshader optimization to keep calculations on the GPU
DWORD dwShaderFlags = D3DXSHADER_NO_PRESHADER;

// Only enable debug info when compiling for a debug target
#if defined (DEBUG) || defined (_DEBUG)
dwShaderFlags |= D3DXSHADER_DEBUG;
#endif

D3DXCreateEffectFromFile(
    m_pd3dDevice,
    L"CubeShaders.fx",
    NULL,
    NULL,
    dwShaderFlags,
    NULL,
    &m_pEffect,
    NULL
    );
```

O Direct3D 11 funciona com programas sombreadores como recursos binários. Os sombreadores são compilados quando o projeto é criado e tratados como recursos. Por isso, nosso exemplo carregará o código de bytes do sombreador na memória do sistema, usará a interface do dispositivo Direct3D para criar um recurso Direct3D para cada sombreador e apontará para os recursos do sombreador do Direct3D quando configurarmos cada quadro.

Carregando um recurso do sombreador no Direct3D 11

```cpp
// BasicReaderWriter is a tested file loader used in SDK samples.
BasicReaderWriter^ readerWriter = ref new BasicReaderWriter();


// Load vertex shader:
Platform::Array<byte>^ vertexShaderData =
    readerWriter->ReadData("CubeVertexShader.cso");

// This call allocates a device resource, validates the vertex shader 
// with the device feature level, and stores the vertex shader bits in 
// graphics memory.
m_d3dDevice->CreateVertexShader(
    vertexShaderData->Data,
    vertexShaderData->Length,
    nullptr,
    &m_vertexShader
    );
```

Para incluir o código de bytes do sombreador no pacote do aplicativo compilado, adicione o arquivo HLSL ao projeto do Visual Studio. O Visual Studio usará a [Ferramenta Compilador de Efeitos](https://msdn.microsoft.com/library/windows/desktop/bb232919) (FXC) para compilar arquivos HLSL em arquivos .CSO (objetos compilados de sombreador) e incluí-los no pacote do aplicativo.

> **Observação**  não se esqueça de definir o nível de recurso de destino correto para o compilador HLSL: clique com botão direito o arquivo de origem HLSL no Visual Studio, selecione Propriedades e altere a configuração de **Modelo de sombreador** em **compilador HLSL -&gt; geral**. O Direct3D compara essa propriedade com os recursos de hardware quando o aplicativo cria o recurso do sombreador Direct3D.

 

![Propriedades do sombreador hlsl](images/hlslshaderpropertiesmenu.png)![tipo de sombreador hlsl](images/hlslshadertypeproperties.png)

Este é um bom momento para criar o layout de entrada, que corresponde à declaração do fluxo de vértice no Direct3D 9. A estrutura de dados de vértice precisa corresponder àquela utilizada pelo sombreador de vértice; no Direct3D 11, temos mais controle sobre o layout de entrada; podemos definir o tamanho da matriz e o tamanho dos bits de vetores com ponto flutuante, bem como especificar a semântica do sombreador de vértice. Criamos uma estrutura [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) e a usamos para informar ao Direct3D como serão os dados de vértice. Houve um motivo para aguardarmos o carregamento do sombreador de vértice para definir o layout de entrada: isso ocorreu porque a API valida o layout de entrada com base no recurso desse sombreador. Quando o layout não é compatível, o Direct3D emite uma exceção.

Os dados de vértice devem ser armazenados em tipos compatíveis na memória do sistema. Os tipos de dados DirectXMath podem ajudar; por exemplo, DXGI\_FORMAT\_R32G32B32\_FLOAT corresponde a [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475).

> **Observação**  buffers constantes usam um layout de entrada fixo, alinhado a quatro números de ponto flutuante por vez. [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) (e suas derivativas) são recomendados para dados de buffer de constantes.

 

Definindo o layout de entrada no Direct3D 11

```cpp
// Create input layout:
const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT,
        0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },

    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 
        0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
```

## <a name="create-geometry-resources"></a>Criar recursos de geometria


No Direct3D 9, armazenávamos recursos de geometria criando buffers no dispositivo Direct3D, bloqueando a memória e copiando dados da memória da CPU para a da GPU.

Direct3D 9

```cpp
// Create vertex buffer:
VOID* pVertices;

// In Direct3D 9 we create the buffer, lock it, and copy the data from 
// system memory to graphics memory.
m_pd3dDevice->CreateVertexBuffer(
    sizeof(CubeVertices),
    0,
    D3DFVF_XYZ | D3DFVF_DIFFUSE,
    D3DPOOL_MANAGED,
    &pVertexBuffer,
    NULL);

pVertexBuffer->Lock(
    0,
    sizeof(CubeVertices),
    &pVertices,
    0);

memcpy(pVertices, CubeVertices, sizeof(CubeVertices));
pVertexBuffer->Unlock();
```

O DirectX 11 segue um processo mais simples. A API copia automaticamente os dados da memória do sistema para a GPU. Podemos usar ponteiros inteligentes COM para facilitar a programação.

DirectX 11

```cpp
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = CubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(
    sizeof(CubeVertices),
    D3D11_BIND_VERTEX_BUFFER);
  
// This call allocates a device resource for the vertex buffer and copies
// in the data.
m_d3dDevice->CreateBuffer(
    &vertexBufferDesc,
    &vertexBufferData,
    &m_vertexBuffer
    );
```

## <a name="implement-the-rendering-chain"></a>Implementar a cadeia de renderização


Às vezes os jogos em Direct3D 9 usavam uma cadeia de renderização baseada em efeitos. O tipo de cadeia de renderização configura o objeto de efeito, fornece a ele os recursos necessários e permite a renderização de cada passagem.

Cadeia de renderização do Direct3D 9

```cpp
// Clear the render target and the z-buffer.
m_pd3dDevice->Clear(
    0, NULL,
    D3DCLEAR_TARGET | D3DCLEAR_ZBUFFER,
    D3DCOLOR_ARGB(0, 45, 50, 170),
    1.0f, 0
    );

// Set the effect technique
m_pEffect->SetTechnique("RenderSceneSimple");

// Rotate the cube 1 degree per frame.
D3DXMATRIX world;
D3DXMatrixRotationY(&world, D3DXToRadian(m_frameCount++));


// Set the matrices up using traditional functions.
m_pEffect->SetMatrix("g_mWorld", &world);
m_pEffect->SetMatrix("g_View", &m_view);
m_pEffect->SetMatrix("g_Projection", &m_projection);

// Render the scene using the Effects library.
if(SUCCEEDED(m_pd3dDevice->BeginScene()))
{
    // Begin rendering effect passes.
    UINT passes = 0;
    m_pEffect->Begin(&passes, 0);
    
    for (UINT i = 0; i < passes; i++)
    {
        m_pEffect->BeginPass(i);
        
        // Send vertex data to the pipeline.
        m_pd3dDevice->SetFVF(D3DFVF_XYZ | D3DFVF_DIFFUSE);
        m_pd3dDevice->SetStreamSource(
            0, pVertexBuffer,
            0, sizeof(VertexPositionColor)
            );
        m_pd3dDevice->SetIndices(pIndexBuffer);
        
        // Draw the cube.
        m_pd3dDevice->DrawIndexedPrimitive(
            D3DPT_TRIANGLELIST,
            0, 0, 8, 0, 12
            );
        m_pEffect->EndPass();
    }
    m_pEffect->End();
    
    // End drawing.
    m_pd3dDevice->EndScene();
}

// Present frame:
// Show the frame on the primary surface.
m_pd3dDevice->Present(NULL, NULL, NULL, NULL);
```

A cadeia de renderização do DirectX 11 ainda realiza as mesmas tarefas, mas as passagens de renderização devem ser implementadas de modo diferente. Em vez de inserir os detalhes em arquivos FX e permitir que as técnicas de renderização sejam mais ou menos opacas em relação ao código C++, iremos configurar toda a renderização em C++.

Consulte como fica a cadeia de renderização. Precisamos fornecer o layout de entrada criado após o carregamento do sombreador de vértice, fornecer cada objeto do sombreador e especificar os buffers constantes usados por cada sombreador. Este exemplo não inclui várias passagens de renderização; mas se incluísse, faríamos uma cadeia de renderização semelhante para cada passe, alterando a configuração quando preciso.

Cadeia de renderização do Direct3D 11

```cpp
// Clear the back buffer.
const float midnightBlue[] = { 0.098f, 0.098f, 0.439f, 1.000f };
m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    midnightBlue
    );

// Set the render target. This starts the drawing operation.
m_d3dContext->OMSetRenderTargets(
    1,  // number of render target views for this drawing operation.
    m_renderTargetView.GetAddressOf(),
    nullptr
    );


// Rotate the cube 1 degree per frame.
XMStoreFloat4x4(
    &m_constantBufferData.model, 
    XMMatrixTranspose(XMMatrixRotationY(m_frameCount++ * XM_PI / 180.f))
    );

// Copy the updated constant buffer from system memory to video memory.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,      // update the 0th subresource
    NULL,   // use the whole destination
    &m_constantBufferData,
    0,      // default pitch
    0       // default pitch
    );


// Send vertex data to the Input Assembler stage.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;

m_d3dContext->IASetVertexBuffers(
    0,  // start with the first vertex buffer
    1,  // one vertex buffer
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset
    );

m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0   // no offset
    );

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());


// Set the vertex shader.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0
    );

// Set the vertex shader constant buffer data.
m_d3dContext->VSSetConstantBuffers(
    0,  // register 0
    1,  // one constant buffer
    m_constantBuffer.GetAddressOf()
    );


// Set the pixel shader.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0
    );


// Draw the cube.
m_d3dContext->DrawIndexed(
    m_indexCount,
    0,  // start with index 0
    0   // start with vertex 0
    );
```

A cadeia de permuta é parte da infraestrutura gráfica, então usamos a cadeia de permuta DXGI para apresentar o quadro concluído. A DXGI bloqueia a chamada até a próxima vsync; em seguida, ela retorna, e o loop do jogo pode continuar até a próxima iteração.

Apresentando um quadro na tela usando DirectX 11

```cpp
m_swapChain->Present(1, 0);
```

A cadeia de renderização criada será chamada a partir de um loop do jogo implementado no método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505). Isso é mostrado na [parte 3: visor e loop do jogo](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md).

 

 




