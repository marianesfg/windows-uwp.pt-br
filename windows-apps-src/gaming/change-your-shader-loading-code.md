---
title: Comparar o pipeline do sombreador do OpenGL ES 2.0 com Direct3D
description: Conceitualmente, o pipeline de sombreador do Direct3D 11 é bem parecido com o do OpenGL ES 2.0.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, opengl, direct3d, pipeline do sombreador
ms.localizationpriority: medium
ms.openlocfilehash: 8793ef8b44df1ca1d93133383666434f525f2d07
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368970"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>Comparar o pipeline do sombreador do OpenGL ES 2.0 com Direct3D




**APIs importantes**

-   [Estágio do Assembler de entrada](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [Estágio de sombreador de vértice](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85))
-   [Estágio de sombreador de pixel](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85))

Conceitualmente, o pipeline de sombreador do Direct3D 11 é bem parecido com o do OpenGL ES 2.0. Mas em termos de design da API, os componentes principais para a criação e o gerenciamento dos estágios do sombreador fazem parte de duas interfaces primárias, [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) e [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). O objetivo deste tópico é correlacionar os padrões das APIs do pipeline de sombreador do OpenGL ES 2.0 com os equivalentes em Direct3D 11 nessas interfaces.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Revisando o pipeline de sombreador do Direct3D 11


Os objetos de sombreador são criados com métodos na interface [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), bem como na [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) e [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader).

O pipeline de elementos gráficos do Direct3D 11 é gerenciado por instâncias da interface [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) e tem os seguintes estágios:

-   [Estágio do assembler de entrada](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage). O estágio do assembler de entrada fornece dados (triângulos, linhas e pontos) ao pipeline. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) métodos que oferecem suporte a esse estágio são prefixados com "IA".
-   [Estágio do sombreador de vértice](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85)) – Esse estágio processa vértices, normalmente realizando operações como transformações, aplicação de capas e iluminação. Um sombreador de vértice sempre usa um único vértice de entrada para produzir um vértice de saída. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) métodos que oferecem suporte a esse estágio são prefixados com "VS".
-   [Estágio de saída de fluxo](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage) – Nesse estágio, os dados são transmitidos do pipeline para a memória em seu caminho para o rasterizador. Os dados podem ser transmitidos para fora do rasterizador e/ou passados para ele. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) métodos que oferecem suporte a esse estágio são prefixados com "Outros".
-   [Estágio do rasterizador](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) – O rasterizador recorta primitivas, prepara-as para o sombreador de pixel e determina como invocar os sombreadores de pixel. Você pode desabilitar a rasterização informando que o pipeline não há nenhum sombreador de pixel (definir o estágio de sombreador de pixel como NULL com [ **ID3D11DeviceContext::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)) e desabilitando a profundidade e estêncil de teste ( Definir DepthEnable e StencilEnable como FALSE na [ **D3D11\_PROFUNDIDADE\_ESTÊNCIL\_DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)). Enquanto estão desabilitados, os contadores do pipeline relacionados à rasterização não serão atualizados.
-   [Estágio do sombreador de pixel](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85)) – Esse estágio recebe dados interpolados para uma primitiva e gera dados por pixel, como cor. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) métodos que oferecem suporte a esse estágio são prefixados com "PS".
-   [Estágio de fusão de saída](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) – Esse estágio combina vários tipos de dados de saída (valores de sombreador de pixel, informações de profundidade e estêncil) com o conteúdo do destino de renderização e buffers de profundidade/estêncil para gerar o resultado do pipeline final. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) métodos que oferecem suporte a esse estágio são prefixados com "OM".

Também há estágios para sombreadores de geometria, sombreadores hull, tesselators e sombreadores de domínio, mas como eles não têm elementos semelhantes no OpenGL ES 2.0, não falaremos sobre eles aqui. Consulte uma lista completa de métodos desses estágios nas páginas de referência [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) e [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). O **ID3D11DeviceContext1** estende o **ID3D11DeviceContext** para o Direct3D 11.

## <a name="creating-a-shader"></a>Criando um sombreador


Em Direct3D, os recursos de sombreador não são criados antes da compilação e carregamento deles; o recurso é criado quando o HLSL é carregado. Portanto, não há nenhuma função diretamente análoga a glCreateShader, que cria um recurso do sombreador inicializado de um tipo específico (como GL\_VÉRTICE\_SOMBREADOR ou GL\_FRAGMENTO\_SOMBREADOR). Em vez disso, os sombreadores são criados após o carregamento de HLSL com funções específicas como [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) e [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader), que consideram o tipo e o HLSL compilado como parâmetros.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | Chame [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) e [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) depois de carregar o objeto de sombreador compilado, passando para eles o CSO como um buffer. |

 

## <a name="compiling-a-shader"></a>Compilando um sombreador


Os sombreadores do Direct3D devem ser recompilados como arquivos .cso (objeto de sombreador compilado) em aplicativos UWP (Plataforma Universal do Windows) e carregados usando uma das APIs de arquivos do Windows Runtime. (Aplicativos da área de trabalho podem compilar os sombreadores de arquivos de texto ou de cadeia de caracteres em tempo de execução). Os arquivos CSO são criados de todos os arquivos. HLSL que fazem parte do seu projeto do Microsoft Visual Studio e mantém os mesmos nomes, apenas com uma extensão de arquivo. CSO. Não se esqueça de incluí-los em seu pacote ao enviá-lo!

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | N/D. Compile os sombreadores em arquivos .cso no Visual Studio e inclua-os em seu pacote.                                                                                     |
| Usando glGetShaderiv para o status de compilação | N/D. Se houver erros na compilação, consulte a saída do processo no FXC (FX Compiler) do Visual Studio. Um arquivo CSO correspondente será criado se a compilação for realizada com êxito. |

 

## <a name="loading-a-shader"></a>Carregando um sombreador


Como falamos na seção sobre criação de um sombreador, o Direct3D 11 cria o sombreador quando o arquivo CSO correspondente é carregado em um buffer e passado para um dos métodos na tabela a seguir.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | Chame [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) e [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) depois de carregar o objeto de sombreador compilado com êxito. |

 

## <a name="setting-up-the-pipeline"></a>Configurando o pipeline


O OpenGL ES 2.0 possui o objeto "programa de sombreador", que contém vários sombreadores para execução. Os sombreadores individuais são anexados ao objeto do programa de sombreador. Porém, no Direct3D 11, você deve trabalhar diretamente com o contexto de renderização ([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) e criar sombreadores nele.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | N/D. O Direct3D 11 não usa a abstração do objeto do programa de sombreador.                          |
| glLinkProgram   | N/D. O Direct3D 11 não usa a abstração do objeto do programa de sombreador.                          |
| glUseProgram    | N/D. O Direct3D 11 não usa a abstração do objeto do programa de sombreador.                          |
| glGetProgramiv  | Use a referência criada para [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). |

 

Crie uma instância do [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) e [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) com o método estático [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>Configurando os visores


O processo de configuração de um visor no Direct3D 11 é muito parecido com o processo no OpenGL ES 2.0. No Direct3D 11, chame [ **ID3D11DeviceContext::RSSetViewports** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) com um configurado [ **CD3D11\_visor**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)).

Direct3D 11: Definindo um visor.

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11\_VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)), [**ID3D11DeviceContext::RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>Configurando os sombreadores de vértice


A configuração de um sombreador de vértice no Direct3D 11 é feita quando o sombreador é carregado. Os uniformes são passados como buffers constantes usando [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1).

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::VSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1). |

 

## <a name="configuring-the-pixel-shaders"></a>Configurando os sombreadores de pixel


A configuração de um sombreador de pixel no Direct3D 11 é feita quando o sombreador é carregado. Os uniformes são passados como buffers constantes usando [**ID3D11DeviceContext1::PSSetConstantBuffers1.** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1)

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::PSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1). |

 

## <a name="generating-the-final-results"></a>Gerando os resultados finais


Quando o pipeline é concluído, desenhe os resultados dos estágios do sombreador no buffer de fundo. No Direct3D 11, assim como com o Open GL ES 2.0, isso envolve a chamada a um comando de desenho para gerar os resultados como um mapa de cores no buffer de fundo e, depois, enviar o buffer de fundo para a exibição.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw), [ **ID3D11DeviceContext1::DrawIndexed** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (ou outro Draw\* métodos no [  **ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)). |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>Fazendo a portabilidade do GLSL para HLSL


Com exceção do suporte a tipos complexos e de alguns aspectos da sintaxe geral, o GLSL e o HLSL não são muito diferentes. Muitos desenvolvedores acham mais fácil fazer a portabilidade entre os dois por meio da atribuição de alias de instruções e definições comuns do OpenGL ES 2.0 aos seus equivalentes em HLSL. Lembre-se de que o Direct3D usa a versão do modelo de sombreador para expressar o conjunto de recursos do HLSL compatível com uma interface gráfica; o OpenGL tem uma especificação de versão diferente para o HLSL. O intuito da tabela a seguir é dar uma ideia aproximada dos conjuntos de recursos de linguagem de sombreador definidos para Direct3D 11 e OpenGL ES 2.0 nos termos da versão da outra linguagem.

| Linguagem do sombreador           | Versão de recursos do GLSL                                                                                                                                                                                                      | Modelo de sombreador do Direct3D |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| HLSL do Direct3D 11          | Até 4.30.                                                                                                                                                                                                                    | SM 5.0                |
| GLSL ES para OpenGL ES 2.0 | 1.40. As implementações mais antigas de GLSL ES para OpenGL ES 2.0 podem usar as versões de 1.10 a 1.30. Verifique seu código original com glGetString (GL\_SOMBREAMENTO\_LINGUAGEM\_versão) ou glGetString (SOMBREAMENTO\_idioma\_versão) para determinar a ele. | Até SM 2.0               |

 

Leia a [referência de GLSL para HLSL](glsl-to-hlsl-reference.md) e conheça melhor as diferenças entre as duas linguagens de sombreador, além de correlações comuns entre as sintaxes.

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>Fazendo a portabilidade de intrínsecos do OpenGL para semântica do HLSL


A semântica HLSL do Direct3D 11 é composta de cadeias que, como um nome de atributo ou uniforme, são usadas para identificar um valor passado de um aplicativo para um programa de sombreador (e vice-versa). Embora haja uma grande variedade de cadeias possíveis, o melhor a fazer é usar uma cadeia, como POSITION ou COLOR, que indique o uso. Você deve atribuir essa semântica ao construir um buffer constante ou um layout de entrada de buffer. Você também pode adicionar um número de 0 a 7 à semântica, o que permite usar Registros separados para valores semelhantes. Por exemplo: COLOR0, COLOR1, COLOR2...

Semânticas que são prefixadas com "SV\_" são a semântica de valor do sistema que são gravados pelo seu programa sombreador; seu aplicativo em si (em execução na CPU) não é possível modificá-los. Normalmente, esses valores são entradas ou saídas de outro estágio do sombreador no pipeline gráfico ou são completamente gerados pela GPU.

Além disso, a VA\_ semântica têm comportamentos diferentes quando eles são usados para especificar a entrada ou saída de um estágio de sombreador. Por exemplo, SV\_posição (saída) contém os dados de vértice convertidos durante o estágio de sombreador de vértice e VA\_posição (entrada) contém os valores de posição do pixel interpolados durante a rasterização.

Veja algumas correlações com intrínsecos de sombreador comuns do OpenGL ES 2.0:

| Valor de sistema no OpenGL | Use esta semântica HLSL                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GL\_posição        | POSITION(n) dos dados do buffer de vértice. VA\_posição fornece uma posição de pixel para o sombreador de pixel e não pode ser gravada pelo seu aplicativo.                                        |
| gl\_Normal          | NORMAL(n) de dados normais fornecidos pelo buffer de vértice.                                                                                                                 |
| gl\_TexCoord\[n\]   | TEXCOORD(n) de dados de coordenadas UV (ST em alguns documentos do OpenGL) de textura fornecidos a um sombreador.                                                                       |
| gl\_FragColor       | COLOR(n) de dados de cor RGBA fornecidos a um sombreador. Não se esqueça de que eles são tratados de forma idêntica aos dados de coordenadas; a semântica simplesmente o ajuda a identificar que são dados de cor. |
| gl\_FragData\[n\]   | SV\_alvo\[n\] para gravação de um sombreador de pixel para uma textura de destino ou outro buffer de pixel.                                                                               |

 

O método por meio do qual a semântica é codificada não é igual ao uso de intrínsecos no OpenGL ES 2.0. No OpenGL, você pode acessar muitos intrínsecos diretamente, sem qualquer configuração ou declaração; no Direct3D, você deve declarar um campo em um buffer de constantes específico para usar uma semântica particular ou declará-lo como o valor de retorno de um método **main()** do sombreador.

Consulte um exemplo de semântica usada na definição de um buffer constante:

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Este código define um par de buffers constantes simples

Aqui, um exemplo de semântica usada para definir o valor retornado por um sombreador de fragmento:

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

Nesse caso, a VA\_destino é o local do destino de renderização que a cor do pixel (definida como um vetor com quatro valores de float) é gravada quando o sombreador conclui a execução.

Para saber mais sobre o uso de semântica com Direct3D, leia [Semântica HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics).

 

 




