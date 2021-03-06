---
title: Desenhar na tela
description: Por fim, compatibilizamos o código que desenha o cubo giratório na tela.
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, elementos gráficos
ms.localizationpriority: medium
ms.openlocfilehash: 68d2c6ec250286b9820ff218f9b35637f49f3b97
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368757"
---
# <a name="draw-to-the-screen"></a>Desenhar na tela




**APIs importantes**

-   [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)
-   [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)
-   [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)

Por fim, compatibilizamos o código que desenha o cubo giratório na tela.

No OpenGL ES 2.0, o contexto de desenho é definido como um tipo EGLContext, que contém os parâmetros de janela e superfície, além dos recursos necessários para desenhar em destinos de renderização que serão usados para compor a imagem final exibida na janela. Use esse contexto para configurar os recursos gráficos e apresentar corretamente os resultados do pipeline do sombreador na exibição. Um dos recursos principais é o "buffer de fundo" (ou "objeto de buffer de quadros"), que contém os destinos de renderização finais (compostos), prontos para apresentação na exibição.

Com o Direct3D, o processo de configuração dos recursos gráficos para desenhar na tela é mais didático e requer bem menos APIs. (Um modelo do Microsoft Visual Studio Direct3D pode simplificar significativamente esse processo, porém!) Para obter um contexto (chamado de um contexto de dispositivo de Direct3D), você deve primeiro obter um [ **ID3D11Device1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) de objeto e usá-lo para criar e configurar um [ **ID3D11DeviceContext1**  ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) objeto. Esses dois objetos são usados em conjunto para configurar recursos específicos de desenho na exibição.

Resumindo: as APIs da DXGI contêm principalmente recursos de gerenciamento relacionados diretamente ao adaptador gráfico, e o Direct3D contém APIs que permitem a interação entre a GPU e o programa principal executado na CPU.

Para fins de comparação nesta amostra, consulte os tipos relevantes de APIs:

-   [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1): fornece uma representação virtual do dispositivo de gráficos e seus recursos.
-   [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1): fornece a interface para configurar buffers e emitir comandos de renderização.
-   [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1): a cadeia de troca é análoga ao buffer de fundo em OpenGL ES 2.0. No adaptador gráfico, corresponde à região da memória que contém as imagens finais renderizadas para exibição. Ela é chamada de "cadeia de troca" porque tem diversos buffers que podem ser gravados e "trocados" para apresentar o renderizador mais recente na tela.
-   [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview): ele contém o buffer de bitmap 2D que o contexto do dispositivo Direct3D desenha na e qual é apresentado pela cadeia de troca. Assim como no OpenGL ES 2.0, você pode ter vários destinos de renderização, alguns não vinculados à cadeia de troca, mas usados para técnicas de sombreamento com passagem múltipla.

No modelo, o objeto de renderizador contém os seguintes campos:

Direct3D 11: Dispositivo e declarações de contexto de dispositivo

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

Veja a seguir como o buffer de fundo é configurado como destino de renderização e fornecido à cadeia de troca.

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

O tempo de execução do Direct3D cria implicitamente uma [**IDXGISurface1**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1) para a [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d), que representa a textura como um "buffer de fundo" que a cadeia de troca pode usar para exibição.

A inicialização e a configuração do dispositivo Direct3D e de seu respectivo contexto, bem como dos destinos de renderização, podem ser encontradas nos métodos personalizados **CreateDeviceResources** e **CreateWindowSizeDependentResources** no modelo do Direct3D.

Para saber mais sobre o contexto de dispositivo Direct3D relacionado a EGL e ao tipo EGLContext, leia o tópico sobre [portabilidade do código EGL para DXGI e Direct3D](moving-from-egl-to-dxgi.md).

## <a name="instructions"></a>Instruções

### <a name="step-1-rendering-the-scene-and-displaying-it"></a>Etapa 1: Renderização da cena e exibi-la

Depois de atualizar os dados do cubo (neste caso, girando-o um pouco em torno do eixo y), o método Render define o visor de acordo com as dimensões do contexto de desenho (EGLContext). Esse contexto contém o buffer de cor que será exibido na superfície da janela (uma EGLSurface), usando a exibição configurada (EGLDisplay). Neste momento, o exemplo atualiza os atributos de dados de vértice, vincula o buffer de índice novamente, desenha o cubo e alterna para o buffer de cor desenhado pelo pipeline de sombreamento na superfície de exibição.

OpenGL ES 2.0: Renderização de um quadro para exibição

``` syntax
void Render(GraphicsContext *drawContext)
{
  Renderer *renderer = drawContext->renderer;

  int loc;
   
  // Set the viewport
  glViewport ( 0, 0, drawContext->width, drawContext->height );
   
   
  // Clear the color buffer
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glEnable(GL_DEPTH_TEST);


  // Use the program object
  glUseProgram (renderer->programObject);

  // Load the a_position attribute with the vertex position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_position");
  glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), 0);
  glEnableVertexAttribArray(loc);

  // Load the a_color attribute with the color position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_color");
  glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
  glEnableVertexAttribArray(loc);

  // Bind the index buffer
  glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);

  // Load the MVP matrix
  glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);

  // Draw the cube
  glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

  eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
}
```

No Direct3D 11, o processo é muito parecido (supomos que você esteja usando a configuração de destino de renderização e visor do modelo Direct3D).

-   Atualize os buffers constantes (a matriz modelo-exibição-projeção, neste caso) com chamadas para [**ID3D11DeviceContext1::UpdateSubresource**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-updatesubresource1).
-   Defina o buffer de vértice com [**ID3D11DeviceContext1::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers).
-   Defina o buffer de índice com [**ID3D11DeviceContext1::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer).
-   Defina a topologia de triângulos específica (uma lista de triângulos) com [**ID3D11DeviceContext1::IASetPrimitiveTopology**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology).
-   Defina o layout de entrada do buffer de vértice com [**ID3D11DeviceContext1::IASetInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout).
-   Vincule o sombreador de vértice com [**ID3D11DeviceContext1::VSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader).
-   Vincule o sombreador de fragmento com [**ID3D11DeviceContext1::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader).
-   Envie os vértices indexados pelos sombreadores e gere os resultados de cores no buffer de destino de renderização com [**ID3D11DeviceContext1::DrawIndexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed).
-   Exiba o buffer de destino de renderização com [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

Direct3D 11: Renderização de um quadro para exibição

``` syntax
void RenderObject::Render()
{
  // ...

  // Only update shader resources that have changed since the last frame.
  m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    NULL,
    &m_constantBufferData,
    0,
    0);

  // Set up the IA stage corresponding to the current draw operation.
  UINT stride = sizeof(VertexPositionColor);
  UINT offset = 0;
  m_d3dContext->IASetVertexBuffers(
    0,
    1,
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset);

  m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0);

  m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
  m_d3dContext->IASetInputLayout(m_inputLayout.Get());

  // Set up the vertex shader corresponding to the current draw operation.
  m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0);

  m_d3dContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf());

  // Set up the pixel shader corresponding to the current draw operation.
  m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0);

  m_d3dContext->DrawIndexed(
    m_indexCount,
    0,
    0);

    // ...

  m_swapChainCoreWindow->Present1(1, 0, &parameters);
}

```

Depois que você chama [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1), seu quadro é gerado na exibição configurada.

## <a name="previous-step"></a>Etapa anterior


[Fazer a portabilidade do GLSL](port-the-glsl.md)

## <a name="remarks"></a>Comentários

Este exemplo fala muito da complexidade de configurar recursos de dispositivo, principalmente para aplicativos UWP (Plataforma Universal do Windows) em DirectX. Sugerimos que você revise o código completo do modelo, especialmente as partes que executam a configuração e o gerenciamento de recursos de janela e dispositivo. Os aplicativos UWP devem oferecer suporte a eventos de rotação e suspensão/retomada. Além disso, o modelo demonstra as práticas recomendadas para lidar com a perda e uma interface ou alterações nos parâmetros de exibição.

## <a name="related-topics"></a>Tópicos relacionados


* [Como: um renderizador simple do OpenGL ES 2.0 ao Direct3D 11 da porta](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Fazer a portabilidade de objetos de sombreador](port-the-shader-config.md)
* [Fazer a portabilidade do GLSL](port-the-glsl.md)
* [Desenhar na tela](draw-to-the-screen.md)

 

 




