---
title: Compatibilizar os objetos de sombreadores
description: Ao compatibilizar o renderizador simples do OpenGL ES 2.0, a primeira etapa é definir o vértice e os objetos de sombreadores equivalentes no Direct3D 11 e certificar que o programa principal consegue se comunicar com os objetos de sombreador depois de eles serem compilados.
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
---

# Compatibilizar os objetos de sombreadores


\[ Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs Importantes**

-   [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)
-   [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)

Ao compatibilizar o renderizador simples do OpenGL ES 2.0, a primeira etapa é definir o vértice e os objetos de sombreadores equivalentes no Direct3D 11 e certificar que o programa principal consegue se comunicar com os objetos de sombreador depois de eles serem compilados.

> **Observação**   Você criou um projeto Direct3D novo? Caso não tenha criado, siga as instruções em [Criar um novo projeto DirectX 11 para a Plataforma Universal do Windows (UWP)](user-interface.md). Este guia passo a passo parte do pressuposto de que você criou os recursos DXGI e Direct3D para desenhar na tela, os quais são fornecidos no modelo.

 

Os sombreadores compilados no Direct3D devem ser associados a um contexto de desenho, de forma muito parecida com o OpenGL ES 2.0. Entretanto, o Direct3D não tem o conceito de um programa sombreador por si. Em vez disso, você deve atribuir os sombreadores diretamente a um [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385). Essa etapa segue o processo de criação e associação de objetos de sombreador do OpenGL ES 2.0 e fornece os comportamentos de API correspondentes no Direct3D.

Instruções
------------

### Etapa 1: compile os sombreadores

Nesta amostra simples do OpenGL ES 2.0, os sombreadores são armazenados como arquivos de texto e carregados como dados de cadeia de caracteres para compilação de tempo de execução.

OpenGL ES 2.0: compilar um sombreador

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

> **Observação**   O exemplo a seguir executa o carregamento e a compilação do sombreador de forma assíncrona usando a palavra-chave **auto** e a sintaxe lambda. ReadDataAsync() é um método implementado para o modelo que lê em um arquivo CSO como uma matriz de dados em byte (fileData).

 

Direct3D 11: compile um sombreador

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

### Etapa 2: crie e carregue o vértice e os sombreadores de fragmento (pixel)

OpenGL ES 2.0 tem a noção de um "programa" sombreador, que serve como interface entre o programa principal sendo executado na CPU e os sombreadores, que são executados na GPU. Os sombreadores são compilados :(ou carregados de origens compiladas) e associados a um programa, que habilita a execução na GPU.

OpenGL ES 2.0: carregando sombreadores de vértice e fragmento em um programa de sombreamento

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

Direct3D 11: defina os sombreadores para o contexto de desenho do dispositivo de elementos gráficos.

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

### Etapa 3: defina os dados a serem fornecidos aos sombreadores

No nosso exemplo de OpenGL ES 2.0, temos um **uniforme** a declarar para o pipeline do sombreador.

-   **u\_mvpMatrix**: uma matriz 4 x 4 de flutuações que representa a matriz de transformação de projeção de exibição do modelo final que utiliza as coordenadas de modelo para o cubo e as transforma em coordenadas de projeção 2D para conversão de varredura.

E dois valores de **atributo** para os dados do vértice:

-   **a\_position**: um vetor de 4 floats para as coordenadas de modelo de um vértice.
-   **a\_color**: um vetor de 4 floats para o valor de cor RGBA associado ao vértice.

Open GL ES 2.0: definições GLSL para uniformes e atributos

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

Neste caso, as variáveis correspondentes do programa principal são definidas como campos no objeto de renderizador (Consulte o cabeçalho em [Como: compatibilizar um renderizador OpenGL ES 2.0 simples ao Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md).) Depois disso, precisamos especificar as localizações na memória, onde o programa principal fornecerá esses valores para o pipeline de sombreador, que geralmente fazemos pouco antes de uma chamada de desenho:

OpenGL ES 2.0: marcando o local de dados de atributos e uniformes

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

Ao mover esse processo para o Direct3D, convertemos o uniforme para um buffer constante (cbuffer) do Direct3D e atribuímos a ele um registro de pesquisa com a semântica HLSL **registro**. Os dois atributos de vértice são manipulados como elementos de entrada para os estágios de pipeline de sombreador, e também são atribuídos [HLSL semantics](https://msdn.microsoft.com/library/windows/desktop/bb205574) (POSITION and COLOR0) que informam aos sombreadores. O sombreador de pixel usa um SV\_POSITION, com o prefixo SV\_ indicando que é um valor de sistema gerado pela GPU. (Neste caso, é uma posição de pixel gerada durante conversão de varredura) VertexShaderInput e PixelShaderInput não são declarados como buffers constantes porque o primeiro será usado para definir o buffer de vértices (consulte [Compatibilizar buffers de vértices e dados](port-the-vertex-buffers-and-data-config.md)), e os dados para o segundo são gerados como resultado de um estágio anterior no pipeline, que, neste caso, é o sombreador de vértice.

Direct3D: definições HLSL para buffers constantes e dados de vértice

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

Direct3D 11: declarando o layout de constantes e buffers de vértices

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

Use os tipos DirectXMath XM\* para os seus elementos de buffer constantes, já que eles fornecem compactação e alinhamento adequados para os conteúdos quando são enviados para o pipeline de sombreador. Se você usa tipos de float e matrizes padrão da plataforma Windows, deve executar a compactação e o alinhamento sozinho.

Para associar um buffer constante, crie uma descrição de layout como uma estrutura [**CD3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/jj151620) e a passe para [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). Em seguida, no seu método de renderização, passe o buffer constante para [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) antes de desenhar.

Direct3D 11: associe o buffer constante

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

O buffer de vértices é criado e atualizado de forma semelhante, e é discutido na próxima etapa, [Compatibilizar buffers de vértices e dados](port-the-vertex-buffers-and-data-config.md).

Próxima etapa
---------

[Compatibilizar buffers de vértices e dados](port-the-vertex-buffers-and-data-config.md)
## Tópicos relacionados


[Como: compatibilizar um renderizador simples do OpenGL ES 2.0 ao Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Compatibilizar buffers de vértices e dados](port-the-vertex-buffers-and-data-config.md)

[Fazer a portabilidade do GLSL](port-the-glsl.md)

[Desenhar na tela](draw-to-the-screen.md)

 

 






<!--HONumber=Mar16_HO1-->


