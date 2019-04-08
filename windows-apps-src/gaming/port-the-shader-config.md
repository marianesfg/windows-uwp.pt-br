---
title: Fazer a portabilidade de objetos de sombreador
description: Ao fazer a portabilidade do renderizador simples do OpenGL ES 2.0, a primeira etapa é definir o vértice e os objetos de sombreadores equivalentes no Direct3D 11 e certificar que o programa principal consegue se comunicar com os objetos de sombreador depois de eles serem compilados.
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, porta, sombreador, direct3d, opengl
ms.localizationpriority: medium
ms.openlocfilehash: f061d31ca779cb4c6cbe76f163e190996a6985cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618741"
---
# <a name="port-the-shader-objects"></a>Fazer a portabilidade de objetos de sombreador




**APIs importantes**

-   [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)
-   [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)

Ao fazer a portabilidade do renderizador simples do OpenGL ES 2.0, a primeira etapa é definir o vértice e os objetos de sombreadores equivalentes no Direct3D 11 e certificar que o programa principal consegue se comunicar com os objetos de sombreador depois de eles serem compilados.

> **Observação**    você criou um novo projeto de Direct3D? Caso não tenha criado, siga as instruções em [Criar um novo projeto DirectX 11 para a Plataforma Universal do Windows (UWP)](user-interface.md). Este guia passo a passo parte do pressuposto de que você criou os recursos DXGI e Direct3D para desenhar na tela, os quais são fornecidos no modelo.

 

Os sombreadores compilados no Direct3D devem ser associados a um contexto de desenho, de forma muito parecida com o OpenGL ES 2.0. Entretanto, o Direct3D não tem o conceito de um programa sombreador por si. Em vez disso, você deve atribuir os sombreadores diretamente a um [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385). Essa etapa segue o processo de criação e associação de objetos de sombreador do OpenGL ES 2.0 e fornece os comportamentos de API correspondentes no Direct3D.

<a name="instructions"></a>Instruções
------------

### <a name="step-1-compile-the-shaders"></a>Etapa 1: Compilar os sombreadores

Nesta amostra simples do OpenGL ES 2.0, os sombreadores são armazenados como arquivos de texto e carregados como dados de cadeia de caracteres para compilação de tempo de execução.

OpenGL ES 2.0: Compilar um sombreador

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

No Direct3D, os sombreadores não são compilados durante o tempo de execução; eles sempre são compilados em arquivos CSO quando o resto do programa é compilado. Quando você compila o seu aplicativo com Microsoft Visual Studio, os arquivos HLSL são compilados a arquivos CSO (.cso) que o seu aplicativo deve carregar. Não se esqueça de incluir esses arquivos CSO ao seu aplicativo em seu pacote!

> **Observação**    o exemplo a seguir executa o carregamento de sombreador e a compilação de forma assíncrona usando o **automático** sintaxe de palavra-chave e lambda. ReadDataAsync() é um método implementado para o modelo que lê em um arquivo CSO como uma matriz de dados em byte (fileData).

 

Direct3D 11: Compilar um sombreador

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>Etapa 2: Criar e carregar o vértice e fragmentar sombreadores (pixel)

OpenGL ES 2.0 tem a noção de um "programa" sombreador, que serve como interface entre o programa principal sendo executado na CPU e os sombreadores, que são executados na GPU. Os sombreadores são compilados :(ou carregados de origens compiladas) e associados a um programa, que habilita a execução na GPU.

OpenGL ES 2.0: Carregando os sombreadores de vértices e de fragmento em um programa de sombreamento

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

O Direct3D não tem o conceito de objeto de programa de sombreador. Em vez disso, os sombreadores são criados quando um dos métodos de criação de sombreador na interface [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (como [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) ou [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)) é chamado. Para definir os sombreadores para o contexto de desenho atual, nós os fornecemos ao [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) correspondente com um método de sombreamento definido, como[**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) para o sombreador de vértice ou [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) para o sombreador de fragmento.

Direct3D 11: Defina os sombreadores para o contexto de desenho de dispositivo de gráficos.

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>Etapa 3: Definir os dados para fornecer aos sombreadores

No nosso exemplo de OpenGL ES 2.0, temos um **uniforme** a declarar para o pipeline do sombreador.

-   **u\_mvpMatrix**: uma matriz de 4 x 4 de floats que representa a matriz de transformação de projeção de exibição do modelo final que usa o modelo de coordenadas para o cubo e transforma-os em coordenadas de projeção 2D para conversão de verificação.

E dois valores de **atributo** para os dados do vértice:

-   **uma\_posição**: um vetor de float de 4 para as coordenadas do modelo de um vértice.
-   **uma\_cor**: Um vetor de float de 4 para o valor de cor RGBA associado ao vértice.

Open GL ES 2.0: Definições do GLSL uniformes e atributos

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

Neste caso, as variáveis correspondentes do programa principal são definidas como campos no objeto de renderizador (Consulte o cabeçalho na [como: um renderizador simple do OpenGL ES 2.0 ao Direct3D 11 da porta](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md).) Depois que nós já fizemos isso, precisamos especificar os locais na memória em que o programa principal fornecerá esses valores para o pipeline de sombreador, que normalmente fazemos logo antes de uma chamada de desenho:

OpenGL ES 2.0: O local dos dados uniforme e atributo de marcação

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

O Direct3D não tem um conceito de "atributo" ou "uniforme" com o mesmo sentido (ou, pelo menos, não compartilha essa sintaxe). Em vez disso, ele tem buffers constantes, representados como sub-recursos Direct3D (recursos esses compartilhados entre o programa principal e os programas de sombreador). Alguns desses sub-recursos, como posições e cores de vértice, são descritos como semântica HLSL. Para saber mais sobre buffers constantes e semântica HLSL relacionados a conceitos do OpenGL ES 2.0, leia o tópico sobre [portabilidade de uniformes, atributos e objetos de buffer de quadros](porting-uniforms-and-attributes.md).

Ao mover esse processo para o Direct3D, convertemos o uniforme para um buffer constante (cbuffer) do Direct3D e atribuímos a ele um registro de pesquisa com a semântica HLSL **registro**. Os dois atributos de vértice são manipulados como elementos de entrada para os estágios de pipeline de sombreador, e também são atribuídos [HLSL semantics](https://msdn.microsoft.com/library/windows/desktop/bb205574) (POSITION and COLOR0) que informam aos sombreadores. O sombreador de pixel leva um SV\_posição, com a VA\_ prefixo indicando que é um valor do sistema gerado pela GPU. (Nesse caso, é uma posição de pixel gerada durante a verificação de conversão.) VertexShaderInput e PixelShaderInput não são declaradas como constante armazena em buffer porque o primeiro será usado para definir o buffer de vértice (consulte [os buffers de vértices e os dados da porta](port-the-vertex-buffers-and-data-config.md)), e os dados para o último são gerados como resultado de uma estágio anterior no pipeline, que nesse caso é o sombreador de vértices.

Direct3D: Definições de HLSL para os buffers de constantes e os dados de vértice

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Para saber mais sobre a portabilidade para buffers de constantes e aplicação de semântica HLSL, leia o tópico sobre [portabilidade de uniformes, atributos e objetos de buffer de quadros](porting-uniforms-and-attributes.md).

Consulte as estruturas para o layout dos dados passados ao pipeline do sombreador com um buffer de vértice ou constante.

Direct3D 11: Declarando o layout de buffers de vértice e de constante

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

Use o XM DirectXMath\* tipos para a constante do buffer elementos, pois eles fornecem o empacotamento adequado e alinhamento do conteúdo quando eles são enviados para o pipeline de sombreador. Se você usa tipos de float e matrizes padrão da plataforma Windows, deve executar a compactação e o alinhamento sozinho.

Para associar um buffer de constantes, crie uma descrição de layout como um [ **CD3D11\_BUFFER\_DESC** ](https://msdn.microsoft.com/library/windows/desktop/jj151620) estruturar e passá-lo para [ **ID3DDevice:: CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). Em seguida, no seu método de renderização, passe o buffer constante para [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) antes de desenhar.

Direct3D 11: Associar o buffer de constantes

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

O buffer de vértices é criado e atualizado de forma semelhante, e é discutido na próxima etapa, [Fazer a portabilidade de dados e buffers de vértices](port-the-vertex-buffers-and-data-config.md).

<a name="next-step"></a>Próximas etapas
---------

[Realizar a portabilidade de dados e buffers de vértice](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>Tópicos relacionados


[Como: um renderizador simple do OpenGL ES 2.0 ao Direct3D 11 da porta](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Realizar a portabilidade de dados e buffers de vértice](port-the-vertex-buffers-and-data-config.md)

[Fazer a portabilidade do GLSL](port-the-glsl.md)

[Desenhar na tela](draw-to-the-screen.md)

 

 




