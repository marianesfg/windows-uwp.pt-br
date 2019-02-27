---
title: Introdução à renderização
description: Saiba como montar o pipeline de renderização para exibir elementos gráficos. Introdução à renderização.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, renderização
ms.localizationpriority: medium
ms.openlocfilehash: 4c16f1fbb55374b1d04c9fc9f5f7eae72ad19b00
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117776"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Estrutura de renderização I: introdução à renderização

Abordamos como estruturar um jogo da Plataforma Universal do Windows (UWP) e como definir uma máquina de estado para lidar com o fluxo do jogo nos tópicos anteriores. Agora está na hora de aprender como montar a estrutura de renderização. Vamos examinar como o jogo de exemplo renderiza a cena de jogo usando Direct3D11 (conhecido como DirectX 11).

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

O Direct3D 11 contém um conjunto de APIs que fornecem acesso aos recursos avançados de hardware de gráficos de alto desempenho que podem ser usados para criar elementos gráficos 3D para aplicativos ricos em elementos gráficos, como jogos.

A renderização de elementos gráficos do jogo na tela consiste basicamente em renderizar uma sequência de quadros na tela. Em cada quadro, você precisará renderizar objetos visíveis na cena, com base na visualização. 

Para renderizar um quadro, você precisará passar as informações de cena necessárias para o hardware de modo que possa ser exibido na tela. Para exibir algo na tela, você precisará iniciar a renderização assim que o jogo começar a ser executado.

## <a name="objective"></a>Objetivo

Para configurar uma estrutura de renderização básica para exibir a saída gráfica de um jogo UWP em DirectX, é possível agrupá-las facilmente nestas três etapas.

 1. Estabelecer uma conexão com a interface gráfica
 2. Criar os recursos necessários para desenhar os elementos gráficos
 3. Exibir os elementos gráficos renderizando o quadro

Este artigo explica como elementos gráficos são renderizados, abrangendo as etapas 1 e 3.

[Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md) aborda a etapa 2; como configurar a estrutura de renderização e como os dados são preparados antes que a renderização possa ocorrer.

## <a name="get-started"></a>Introdução

Antes de começar, você deve se familiarizar com os elementos gráficos básicos e os conceitos de renderização. Se você for iniciante em Direct3D e renderização, consulte [Termos e conceitos](#terms-and-concepts) para obter uma breve descrição dos elementos gráficos e termos de renderização usados neste artigo.

Para esse jogo, o objeto de classe __GameRenderer__ representará o renderizador do jogo de exemplo.  É responsável por criar e manter todos os objetos Direct3D 11 e Direct2D usados para gerar os elementos visuais do jogo.  Também mantém uma referência ao objeto __Simple3DGame__ usado para recuperar a lista de objetos que serão renderizados, bem como o status do jogo em relação ao Painel Transparente (HUD). 

Nesta parte do tutorial, vamos nos concentrar na renderização de objetos 3D no jogo.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Estabelecer uma conexão com a interface gráfica

Para acessar o hardware para renderização, consulte o artigo Estrutura UWP em [__App::Initialize__](tutorial--building-the-games-uwp-app-framework.md#appinitialize-method).

O __make\_shared function__, conforme mostrado [abaixo](#appinitialize-method), é usado para criar um __shared\_ptr__ para [__DX::DeviceResources__](#dxdeviceresources), que também fornece acesso ao dispositivo. 

No Direct3D 11, um [dispositivo](#device) é usado para alocar e destruir objetos, renderizar primitivas e comunicar-se com a placa gráfica por meio do driver gráfico.

### <a name="appinitialize-method"></a>Método App::Initialize

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    //...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Exibir os elementos gráficos renderizando o quadro

A cena de jogo precisará renderizar quando o jogo for iniciado. As instruções para renderização começam no método  [__GameMain::Run__](#gamemainrun-method), conforme mostrado abaixo.

O fluxo simples é:
1. __Update__
2. __Renderizar__
3. __Apresentar__

### <a name="gamemainrun-method"></a>Método GameMain::Run

```cpp
// Comparing this to App::Run() in the DirectX 11 App template
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            // Added: Switching different game states since this game sample is more complex
            switch (m_updateState) 
            {

                // ...
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                // 1. Update
                // Also exist in DirectX 11 App template: Update loop to get the latest game changes
                Update();
                
                // 2. Render
                // Present also in DirectX 11 App template: Prepare to render
                m_renderer->Render();
                
                // 3. Present
                // Present also in DirectX 11 App template: Present the 
                // contents of the swap chain to the screen.
                m_deviceResources->Present(); 
                
                // Added: resetting a variable we've added to indicate that 
                // render has been done and no longer needed to render
                m_renderNeeded = false; 
            }
        }
        else
        {   
            // Present also in DirectX 11 App template
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending); 
        }
    }
    m_game->OnSuspending();  // exiting due to window close. Make sure to save state.
}
```

### <a name="update"></a>Atualizar

Consulte o artigo [Gerenciamento de fluxo de jogo](tutorial-game-flow-management.md) para obter mais informações sobre como os estados do jogo são atualizados no métodos [__App::Update__ e __GameMain::Update__](tutorial-game-flow-management.md#appupdate-method).

### <a name="render"></a>Renderizar

A renderização é implementada chamando o método [__GameRenderer::Render__](#gamerendererrender-method) em __GameMain::Run__.

Se a [renderização estéreo](#stereo-rendering) for habilitada, haverá duas passagens de renderização: uma para o olho direito e a outra para o olho esquerdo. Em cada passagem de renderização, associaremos o destino de renderização e a exibição de estêncil de profundidade ao dispositivo. Também limparemos a exibição de estêncil de profundidade posteriormente.

> [!Note]
> A renderização estéreo pode ser alcançada por meio de outros métodos, como estéreo de passagem única usando instanciação de vértice ou sombreadores de geometria. Embora o método com duas passagens de renderização seja mais lento, é mais conveniente para se obter a renderização estéreo.

Após a criação do jogo e o carregamento dos recursos, atualize a [matriz de projeção](#projection-transform-matrix), uma vez durante a passagem de renderização. Os objetos são um pouco diferentes em cada visualização. Em seguida, configure o [pipeline de renderização de gráficos](#rendering-pipeline). 

> [!Note]
> Consulte [Criar e carregar recursos gráficos DirectX](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) para obter mais informações sobre como os recursos são carregados.

Neste jogo de exemplo, o renderizador foi criado para usar um layout de vértice padrão em todos os objetos. Isso simplifica o design de sombreador e permite alterações fáceis entre os sombreadores, independentemente da geometria dos objetos.

#### <a name="gamerendererrender-method"></a>Método GameRenderer::Render

Defina o contexto Direct3D para usar um layout de vértice de entrada. Os objetos de layout de entrada descrevem como os dados do buffer de vértices são transmitidos para o [pipeline de renderização](#rendering-pipeline). 

Em seguida, definimos o contexto Direct3D para usar os buffers constantes definidos anteriormente, que são usados pelo estágio de pipeline de [sombreador de vértice](#vertex-shaders-and-pixel-shaders) e pelo estágio de pipeline de [sombreador de pixel](#vertex-shaders-and-pixel-shaders). 

> [!Note]
> Consulte [Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md) para obter mais informações sobre a definição dos buffers constantes.

Como os mesmos layout de entrada e conjunto de buffers constantes são usados em todos os sombreadores no pipeline, é configurado uma vez por quadro.

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            
            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() }; 
            
            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap()); 
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;

                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, 
            // go to https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476454.aspx
            d3dContext->IASetInputLayout(m_vertexLayout.Get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476491.aspx

            d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476470.aspx

            d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); /
            }
        }
        else
        {
            const float ClearColor[4] = {0.5f, 0.5f, 0.8f, 1.0f};

            // Only need to clear the background when not rendering the full 3D scene since
            // the 3D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (i > 0)
            {
                // Doing the Right Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), ClearColor);
            }
        }

        // Start of 2D rendering
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // ...
    }
}
```

### <a name="primitive-rendering"></a>Renderização de primitivas

Ao renderizar a cena, você executará um loop por todos os objetos que precisam ser renderizados. As etapas a seguir são repetidas para cada objeto (primitiva).

* Atualize o buffer constante (__m\_constantBufferChangesEveryPrim__) com a [matriz de transformação de mundo](#world-transform-matrix) do modelo e informações materiais.
* O __m\_constantBufferChangesEveryPrim__ contém os parâmetros para cada objeto.  Inclui o objeto para matriz de transformação de mundo e propriedades materiais como cor e expoente especular para cálculos de iluminação.
* Defina o contexto Direct3D para usar o layout de vértice de entrada para os dados de objeto de malha que serão transmitidos ao estágio do assembler de entrada (IA) do [pipeline de renderização](#rendering-pipeline)
* Defina o contexto Direct3D para usar um [buffer de índice](#index-buffer) no estágio do IA. Forneça as informações de primitiva: tipo, ordem de dados.
* Envie uma chamada de desenho para desenhar a primitiva indexada não instanciada. O método __GameObject::Render__ atualiza o [buffer constante](#constant-buffer-or-shader-constant-buffer) de primitiva com os dados específicos de uma determinada primitiva. Isso resulta em uma chamada de __DrawIndexed__ no contexto para desenhar a geometria dessa primitiva. Especificamente, essa chamada de desenho enfileira comandos e dados para a GPU (Unidade de Processamento de Gráficos), conforme parametrizados pelos dados do buffer constante. Cada chamada de desenho executa o sombreador de vértice uma vez por vértice e, em seguida, o [sombreador de pixel](#vertex-shaders-and-pixel-shaders) uma vez para cada pixel de cada triângulo da primitiva. As texturas fazem parte do estado que o sombreador de pixel usa para executar a renderização.

Motivos para vários buffers constantes:
    * O jogo usa vários buffers constantes, mas só precisa atualizar esses buffers uma vez para cada primitiva. Conforme mencionado anteriormente, os buffers constantes são como uma entrada de dados para os sombreadores executados para cada primitiva. Alguns dados são estáticos (__m\_constantBufferNeverChanges__); alguns são constantes no quadro (__m\_constantBufferChangesEveryFrame__), como a posição da câmera; e alguns são específicos da primitiva, como sua cor e suas texturas (__m\_constantBufferChangesEveryPrim__)
    * O renderizador do jogo separa essas entradas em diferentes buffers constantes para otimizar a largura de banda de memória usada pela CPU e pela GPU. Essa abordagem também ajuda a minimizar a quantidade de dados que a GPU precisa controlar. A GPU possui uma grande fila de comandos, e sempre que o jogo chama __Draw__, esse comando é inserido na fila com os respectivos dados. Quando o jogo atualiza o buffer de constantes de primitiva e emite o próximo comando __Draw__, o driver gráfico adiciona esse comando e os dados associados à fila. Se o jogo desenha 100 primitivas, pode haver 100 cópias dos dados do buffer constante na fila. Para minimizar a quantidade de dados enviados à GPU, o jogo usa um buffer de constantes de primitiva em separado contendo apenas as atualizações para cada primitiva.

#### <a name="gameobjectrender-method"></a>Método GameObject::Render

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m\_active || (m\_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}

#### MeshObject::Render method

void MeshObject::Render(\_In\_ ID3D11DeviceContext *context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476453.aspx
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476455.aspx
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476409.aspx
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="present"></a>Apresentar

Chamamos o método __DX::DeviceResources::Present__ para pôr o conteúdo que colocamos nos buffers e exibi-lo.

Usamos o termo cadeia de troca para uma coleção de buffers que são utilizados para exibir quadros para o usuário. Cada vez que um aplicativo apresenta um novo quadro para exibição, o primeiro buffer da cadeia de troca assume o lugar do buffer exibido. Esse processo é chamado troca ou inversão. Para obter mais informações, veja [Cadeias de troca](../graphics-concepts/swap-chains.md).

* O método __Present__ da interface __IDXGISwapChain1__ instrui a [DXGI](#dxgi) para bloqueio até a sincronização vertical (VSync), colocando o aplicativo no modo de suspensão até a próxima VSync. Com isso, você não perderá ciclos na renderização de quadros que jamais serão exibidos na tela.
* O método __DiscardView__ da interface __ID3D11DeviceContext3__ descarta o conteúdo do [destino de renderização](#render-target). Trata-se de uma operação válida somente quando o conteúdo existente for inteiramente substituído. Se os retângulos sujos ou de rolagem forem usados, isso deverá ser removido.
* Usando o mesmo método __DiscardView__, descarte o conteúdo de [estêncil de profundidade](#depth-stencil).
* O método __HandleDeviceLost__ é usado para gerenciar o cenário se o [dispositivo](#device) for removido. Se o dispositivo foi removido por uma desconexão ou uma atualização de driver, você deverá recriar todos os recursos do dispositivo. Para obter mais informações, consulte [Lidar com cenários removidos de dispositivos no Direct3D 11](handling-device-lost-scenarios.md).

> [!Tip]
> Para obter uma taxa de quadros fluida, você deverá garantir que a quantidade de trabalho para renderizar um quadro se adeque ao tempo entre a VSyncs.

```cpp
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());

    // Discard the contents of the depth-stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    // For more info about how to handle a device lost scenario, go to:
    // https://docs.microsoft.com/windows/uwp/gaming/handling-device-lost-scenarios
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        DX::ThrowIfFailed(hr);
    }
}
```

## <a name="next-steps"></a>Próximas etapas

Este artigo explicou como elementos gráficos são renderizados na tela e forneceu uma breve descrição para alguns dos termos de renderização usados. Saiba mais sobre renderização no artigo [Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md) e como preparar os dados necessários antes da renderização.

## <a name="terms-and-concepts"></a>Termos e conceitos

### <a name="simple-game-scene"></a>Cena de jogo simples

Uma cena de jogo simples é composta de alguns objetos com várias fontes de iluminação.

A forma de um objeto é definida por um conjunto de coordenadas X, Y, Z no espaço. O local de renderização real no mundo do jogo pode ser determinado pela aplicação de uma matriz de transformação às coordenadas X, Y, Z de posição. Também pode haver um conjunto de coordenadas de textura: U e V que especificam como um material é aplicado ao objeto. Isso define as propriedades de superfície do objeto e permite verificar se um objeto tem uma superfície áspera, como uma bola de tênis, ou uma superfície suave brilhante, como uma bola de boliche.

As informações de cena e objeto são usadas pela estrutura de renderização para recriar a cena quadro a quadro, fazendo com que ganhe vida na tela.

### <a name="rendering-pipeline"></a>Pipeline de renderização

O pipeline de renderização é o processo em que as informações da cena 3D são convertidas em uma imagem exibida na tela. No Direct3D 11, esse pipeline é programável. É possível adaptar os estágios para atender às suas necessidades de renderização. Estágios que apresentam núcleos comuns de sombreador são programáveis por meio da linguagem de programação HLSL. Também é conhecido como o pipeline de renderização de elementos gráficos ou simplesmente o pipeline.

Para criar esse pipeline, você precisa estar familiarizado com:
* [HLSL](#HLSL). Recomendamos o uso do modelo de sombreador 5.1 do HLSL e acima para jogos UWP do DirectX.
* [Sombreadores](#Shaders)
* [Sombreadores de vértice e sombreadores de pixel](#vertext-shaders-pixel-shaders)
* [Estágios de sombreador](#shader-stages)
* [Vários formatos de arquivo de sombreador](#various-shader-file-formats)

Para obter mais informações, consulte [Noções básicas sobre o pipeline de renderização do Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/dn643746.aspx) e [Pipeline de elementos gráficos](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="hlsl"></a>HLSL

HLSL é a linguagem de sombreamento de alto nível para o DirectX. Usando o HLSL, você pode criar sombreadores programáveis semelhantes ao C para o pipeline de Direct3D. Para obter mais informações, consulte [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561.aspx).

#### <a name="shaders"></a>Sombreadores

Pode-se considerar os sombreadores como um conjunto de instruções que determinam como a superfície de um objeto aparecerá quando renderizada. Aqueles que são programados usando HLSL são conhecidos como sombreadores HLSL. Arquivos de código-fonte para sombreadores [HLSL])(#hlsl) têm extensão de arquivo .hlsl. Esses sombreadores podem ser compilados no momento da criação ou no momento da execução, no estágio de pipeline apropriado; um objeto de sombreador compilado tem uma extensão de arquivo .cso.

Os sombreadores do Direct3D 9 podem ser desenvolvidos por meio de um modelo de sombreador 1, um modelo de sombreador 2 e um modelo de sombreador 3; os sombreadores do Direct3D 10 só podem ser criados em um modelo de sombreador 4. Os sombreadores do Direct3D 11 podem ser criados no modelo de sombreador 5. O Direct3D 11.3 e o Direct3D 12 podem ser criados no modelo de sombreador 5.1, bem como o Direct3D 12 pode ser criado no modelo de sombreador 6.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Sombreadores de vértice e sombreadores de pixel

Os dados inserem o pipeline gráfico como um fluxo de primitivas e são processados pelos vários sombreadores, como os sombreadores de vértice e os sombreadores de pixel. 

Os sombreadores de vértice processam vértices, geralmente realizando operações como transformações, aplicação de capas e iluminação.  Os sombreadores de pixel permitem técnicas de sombreamento avançadas, como iluminação por pixel e pós-processamento. Combina variáveis constantes, dados de textura, valores interpolados por vértice e outros dados para produzir saídas por pixel. 

#### <a name="shader-stages"></a>Estágios de sombreador

Uma sequência desses vários sombreadores definidos para processar esse fluxo de primitivas é conhecida como estágios de sombreador em um pipeline de renderização. Os estágios reais dependem da versão do Direct3D, mas geralmente incluem o vértice, a geometria e os estágios de pixel. Também há outros estágios, como os sombreadores hull e de domínio para mosaico e o sombreador de computação. Todos esses estágios são completamente programáveis usando [HLSL])(#hlsl). Para obter mais informações, consulte [Pipeline de elementos gráficos](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="various-shader-file-formats"></a>Vários formatos de arquivo de sombreador

Extensões de arquivo de código de sombreador:
    * Um arquivo com a extensão .hlsl contém o código-fonte [HLSL])(#hlsl).
    * Um arquivo com a extensão .cso contém um objeto de sombreador compilado.
    * Um arquivo com a extensão .h é um arquivo de cabeçalho, mas, em um contexto de código de sombreador, esse arquivo de cabeçalho define uma matriz de bytes que contém dados de sombreador.
    * Um arquivo com a extensão .hlsli contém o formato dos buffers constantes. No jogo de exemplo, o arquivo é __Shaders__>__ConstantBuffers.hlsli__.

> [!Note]
> Você incorporaria o sombreador carregando um arquivo .cso no momento da execução ou adicionando o arquivo .h em seu código executável. No entanto, não usaria para o mesmo sombreador.

### <a name="deeper-understanding-of-directx"></a>Compreensão mais profunda do DirectX

O Direct3D 11 é um conjunto de APIs que nos ajudam a criar elementos gráficos de aplicativos ricos em elementos gráficos, como jogos, nos quais é preciso ter uma boa placa gráfica para processar cálculos intensos. Esta seção explica resumidamente os conceitos de programação de elementos gráficos do Direct3D 11: recurso, sub-recurso, dispositivo e contexto de dispositivo.

#### <a name="resource"></a>Recurso

Para usuários novatos, pode-se considerar os recursos (também conhecidos como recursos de dispositivo) como informações sobre como renderizar um objeto, como textura, posição, cor. Os recursos fornecem dados para o pipeline e definem o que é renderizado durante a cena. Os recursos podem ser carregados a partir da mídia de jogo ou criados dinamicamente no momento de execução.

Na verdade, um recurso é uma área na memória que pode ser acessada pelo [pipeline](#rendering-pipeline) de Direct3D. Para que o pipeline acesse a memória com eficiência, os dados fornecidos ao pipeline (como geometria de entrada, recursos de sombreador e texturas) devem ser armazenados em um recurso. Existem dois tipos de recursos a partir dos quais todos os recursos de Direct3D são derivados: um buffer ou uma textura. Até 128 recursos podem estar ativos para cada estágio do pipeline. Para obter mais informações, consulte [Recursos](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Sub-recurso

O termo sub-recurso refere-se ao subconjunto de um recurso. O Direct3D pode fazer referência a um recurso inteiro ou pode fazer referência aos subconjuntos de um recurso. Para obter mais informações, consulte [Sub-recurso](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Estêncil de profundidade

Um recurso de estêncil de profundidade contém o formato e o buffer para manter informações de profundidade e estêncil. É criado por meio de um recurso de textura. Para obter mais informações sobre como criar um recurso de estêncil de profundidade, consulte [Configuração da funcionalidade de estêncil de profundidade](https://msdn.microsoft.com/library/windows/desktop/bb205074.aspx). Acessamos o recurso de estêncil de profundidade por meio da exibição de estêncil de profundidade implementada usando-se a interface [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377.aspx).

As informações de profundidade mostram quais áreas de polígonos são renderizadas em vez de serem ocultas da exibição. As informações de estêncil mostram quais pixels estão mascarados. Isso pode ser usado para produzir efeitos especiais, pois determina se um pixel é desenhado ou não; define o bit como 1 ou 0. 

Para obter mais informações, consulte: [Modo de exibição de estêncil de profundidade](../graphics-concepts/depth-stencil-view--dsv-.md), [buffer de profundidade](../graphics-concepts/depth-buffers.md) e [buffer de estêncil](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Destino de renderização

Um destino de renderização é um recurso que podemos gravar ao término de uma passagem de renderização. Em geral, é criado usando-se o método [ID3D11Device::CreateRenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476517.aspx) por meio do buffer de fundo da cadeia de troca (que também é um recurso) como o parâmetro de entrada. 

Cada destino de renderização também deve ter um modo de exibição de estêncil de profundidade correspondente porque, quando usamos [OMSetRenderTargets](https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx) para definir o destino de renderização antes de usá-lo, também é necessário um modo de exibição de estêncil de profundidade. Acessamos o recurso de destino de renderização por meio de exibição do destino de renderização implementada usando a interface [ID3D11RenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476582.aspx). 

#### <a name="device"></a>Dispositivo

Para usuários novatos no Direct3D 11, é possível imaginar um dispositivo como uma forma de se alocar e destruir objetos, renderizar primitivas e comunicar-se com a placa gráfica por meio do driver gráfico. 

Para obter uma explicação mais precisa, um dispositivo Direct3D é o componente de renderização do Direct3D. Um dispositivo encapsula e armazena o estado de renderização, executa transformações e operações de iluminação e rasteriza uma imagem para uma superfície. Para obter mais informações, consulte [Dispositivos](../graphics-concepts/devices.md)

Um dispositivo é representado pela interface [ID3D11Device](https://msdn.microsoft.com/library/windows/desktop/ff476379.aspx). Ou seja, a interface ID3D11Device representa um adaptador de vídeo virtual e é usada para criar recursos que pertencem a um dispositivo. 

Observe que existem versões diferentes da ID3D11Device, [ID3D11Device5](https://msdn.microsoft.com/library/windows/desktop/mt492478.aspx) é a versão mais recente e acrescenta novos métodos àquela em ID3D11Device4. Para obter mais informações sobre como o Direct3D se comunica com o hardware subjacente, consulte [Arquitetura do Windows Device Driver Model (WDDM)](https://docs.microsoft.com/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Cada aplicativo deve ter pelo menos um dispositivo; a maioria dos aplicativos só cria um dispositivo. Crie um dispositivo para um dos drivers de hardware instalados no seu computador chamando __D3D11CreateDevice__ ou __D3D11CreateDeviceAndSwapChain__ e especificando o tipo de driver com o sinalizador D3D\_DRIVER\_TYPE. Cada dispositivo pode usar um ou mais contextos de dispositivo, dependendo da funcionalidade desejada. Para obter mais informações, consulte a [função D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082.aspx).

#### <a name="device-context"></a>Contexto de dispositivo

Um contexto de dispositivo é usado para definir o estado do [pipeline](#rendering-pipeline) e gerar comandos de renderização por meio dos [recursos](#resource) pertencentes a um [dispositivo](#device). 

O Direct3D 11 implementa dois tipos de contextos de dispositivo, um para renderização imediata e outro para renderização adiada; ambos os contextos são representados com uma interface [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476385.aspx).  

As interfaces __ID3D11DeviceContext__ têm diferentes versões; __ID3D11DeviceContext4__ acrescenta novos métodos àquela em __ID3D11DeviceContext3__.

Observação: __ID3D11DeviceContext4__ é apresentada na Atualização do Windows 10 para Criadores e é a versão mais recente da interface __ID3D11DeviceContext__. Aplicativos que se destinam à Atualização do Windows 10 para Criadores devem usar essa interface em vez de versões anteriores. Para obter mais informações, consulte [ID3D11DeviceContext4](https://msdn.microsoft.com/library/windows/desktop/mt492481.aspx).

#### <a name="dxdeviceresources"></a>DX::DeviceResources

A classe __DX::DeviceResources__ está nos arquivos __DeviceResources.cpp__/__.h__ e controla todos os recursos de dispositivo do DirectX. No projeto de jogo de exemplo e no projeto de modelo de aplicativo do DirectX 11, esses arquivos estão no arquivo __Commons__. Você pode obter a versão mais recente desses arquivos ao criar um projeto de modelo de aplicativo do DirectX 11 no Visual Studio 2015 ou posterior.

### <a name="buffer"></a>Buffer

Um recurso de buffer é uma coleção de dados completamente tipados, agrupados em elementos. É possível usar buffers para armazenar uma ampla variedade de dados, incluindo vetores de posição, vetores normais, coordenadas de textura em um buffer de vértice, índices em um buffer de índice ou estado do dispositivo. Elementos de buffer podem incluir valores de dados de pacote (como valores de superfície R8G8B8A8), inteiros de 8 bits únicos ou quatro valores de ponto flutuante de 32 bits.

Há três tipos de buffers disponíveis: buffer de vértice, buffer de índice e buffer constante.

#### <a name="vertex-buffer"></a>Buffer de vértice

Contém os dados de vértice usados para definir sua geometria. Dados de vértice incluem coordenadas de posição, dados de cor, dados de coordenadas de textura, dados normais e assim por diante. 

#### <a name="index-buffer"></a>Buffer de índice

Contém deslocamentos de inteiros em buffers de vértice e são usados para renderizar primitivas de forma mais eficiente. Um buffer de índice contém um conjunto sequencial de índices de 16 ou 32 bits; cada índice é usado para identificar um vértice em um buffer de vértices.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Buffer constante ou buffer constante de sombreador

Permite que você forneça com eficiência dados do sombreador no pipeline. É possível usar buffers constantes como entradas para os sombreadores executados para cada primitiva e armazenar os resultados do estágio de saída de fluxo do pipeline de renderização. Conceitualmente, um buffer constante parece muito com um buffer de vértice de elemento único.

#### <a name="design-and-implementation-of-buffers"></a>Design e implementação de buffers

Você pode criar buffers com base no tipo de dados; por exemplo, como em nosso jogo de amostra, um buffer é criado para os dados estáticos, outro para os dados que permanecem constantes ao longo do quadro e outro para os dados que são específicos para uma primitiva.

Todos os tipos de buffer são encapsulados pela interface __ID3D11Buffer__, e você pode criar um recurso de buffer chamando __ID3D11Device::CreateBuffer__. Contudo, um buffer deve ser vinculado ao pipeline antes que possa ser acessado. Os buffers podem ser vinculados a vários estágios de pipeline simultaneamente para leitura. Um buffer também pode ser vinculado a um estágio de pipeline único para gravação; porém, o mesmo buffer não pode ser vinculado para leitura e gravação ao mesmo tempo.

Vincule os buffers:
    * Ao estágio do assembler de entrada chamando os métodos __ID3D11DeviceContext__ como __ID3D11DeviceContext::IASetVertexBuffers__ e __ID3D11DeviceContext::IASetIndexBuffer__
    * Ao estágio de saída de fluxo chamando __ID3D11DeviceContext::SOSetTargets__
    * Ao estágio de sombreador chamando métodos de sombreador, como __ID3D11DeviceContext::VSSetConstantBuffers__

Para obter mais informações, consulte [Introdução a buffers no Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898.aspx).

### <a name="dxgi"></a>DXGI

Microsoft DirectX Graphics Infrastructure (DXGI) é um novo subsistema que foi introduzido com o Windows Vista que encapsula algumas das tarefas de baixo nível que são necessárias pelo Direct3D 10, 10.1, 11 e 11.1. Deve-se ter um cuidado especial ao usar a DXGI em um aplicativo multithread para garantir que não ocorram deadlocks. Para obter mais informações, consulte [DirectX Graphics Infrastructure (DXGI): Práticas recomendadas - Multithreading](https://msdn.microsoft.com/library/windows/desktop/ee417025.aspx#multithreading_and_dxgi)

### <a name="feature-level"></a>Nível de recursos

Nível de recursos é um conceito apresentado no Direct3D 11 para manipular a diversidade de placas de vídeo em computadores novos e existentes. Um nível de recursos é um conjunto bem definido de funcionalidades de GPU (Unidade de Processamento de Gráficos). 

Cada placa de vídeo implementa um certo nível de funcionalidade de DirectX dependendo das GPUs instaladas. Em versões anteriores do Microsoft Direct3D, você poderia descobrir a versão do Direct3D que a placa de vídeo implementou e programar o seu aplicativo adequadamente. 

Com o nível de recursos, na criação de um dispositivo, pode-se tentar criá-lo para o nível de recursos que se deseja solicitar. Se for possível criar um dispositivo, isso significa que o nível de recursos existe; caso contrário, o hardware não permite tal nível de recursos. Você pode tentar recriar um dispositivo em um nível de recursos inferior ou optar por sair do aplicativo. Por exemplo, o nível de recursos 12\_0 requer Direct3D 11.3 ou Direct3D 12 e modelo de sombreador 5.1. Para obter mais informações, consulte [Níveis de recursos do Direct3D: visão geral de cada nível de recursos](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx#Overview).

Usando os níveis de recursos, você pode desenvolver um aplicativo para Direct3D9, Microsoft Direct3D10 ou Direct3D11 e, em seguida, executá-lo em 9, 10 ou 11 hardware (com algumas exceções). Para obter mais informações, consulte [Níveis de recursos do Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx).

### <a name="stereo-rendering"></a>Renderização estéreo

A renderização estéreo é usada para aumentar a ilusão de profundidade. Usa duas imagens, uma do olho esquerdo e a outra do olho direito para exibir uma cena na tela de exibição. 

Matematicamente, aplicamos uma matriz de projeção estéreo, que é um pequeno deslocamento horizontal à direita e à esquerda, da matriz de projeção mono regular para obter esse efeito.

Fizemos duas passagens de renderização para obter uma renderização estéreo neste jogo de exemplo:
* Vincule ao destino de renderização direito, aplique a projeção direita e depois desenhe o objeto de primitiva.
* Vincule ao destino de renderização esquerdo, aplique a projeção esquerda e depois desenhe o objeto de primitiva.

### <a name="camera-and-coordinate-space"></a>Câmera e espaço de coordenadas

O jogo já dispõe do código necessário para atualizar o mundo em seu próprio sistema de coordenadas (também conhecido como espaço do mundo ou espaço da cena). Todos os objetos, inclusive a câmera, são posicionados e orientados nesse espaço. Para obter mais informações, consulte [Sistemas de coordenadas](../graphics-concepts/coordinate-systems.md).

Um shader de vértice executa o trabalho pesado de conversão das coordenadas do modelo para os coordenadas do dispositivo por meio do seguinte algoritmo (onde V é um vetor e M uma matriz).

V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device).

onde: 
* M(model-to-world) é uma matriz de transformação de coordenadas de modelo para coordenadas de mundo, também conhecida como [Matriz de transformação de mundo](#world-transform-matrix). Isso é fornecido pela primitiva.
* M(world-to-view) é uma matriz de transformação de coordenadas de mundo para coordenadas de exibição, também conhecida como [Matriz de transformação de exibição](#view-transform-matrix).
    * Isso é fornecido pela matriz de visualização da câmera. A posição da câmera, juntamente com os vetores de visão (o vetor "olhar para" que aponta diretamente para a cena a partir da câmera e o vetor "olhar para cima" que sobe perpendicularmente dele).
    * No jogo de exemplo, __m\_viewMatrix__ é a matriz de transformação de exibição e é calculada usando __Camera::SetViewParams__ 
* M(view-to-device) é uma matriz de transformação de coordenadas de exibição para coordenadas de dispositivo, também conhecida como [Matriz de transformação de projeção](#projection-transform-matrix).
    * Isso é fornecido pela projeção da câmera. Fornece informações sobre como muito desse espaço é realmente visível na cena final. O campo de visão (FoV), a taxa de proporção e os planos de recorte definem a matriz de transformação de projeção.
    * No jogo de exemplo, __m\_projectionMatrix__ define a transformação para as coordenadas de projeção, calculadas por meio de __Camera::SetProjParams__ (Para projeção estéreo, você usa duas matrizes de projeção: um para cada olho.) 

O código de shader em VertexShader.hlsl é carregado com esses vetores e matrizes dos buffers constantes e executa essa transformação para cada vértice.

### <a name="coordinate-transformation"></a>Transformação de coordenadas

O Direct3D usa três transformações para alterar suas coordenadas de modelo 3D em coordenadas de pixel (espaço da tela). Essas transformações são transformação de mundo, transformação de exibição e transformação de projeção. Para obter mais informações, acesse [Visão geral da transformação](../graphics-concepts/transform-overview.md).

#### <a name="world-transform-matrix"></a>Matriz de transformação de mundo

Uma transformação de mundo altera as coordenadas do espaço do modelo, no qual são definidos os vértices relativos à origem local de um modelo, para o espaço do mundo, no qual são definidos os vértices relativos a uma origem comum para todos os objetos em uma cena. Em essência, a transformação do mundo coloca um modelo no mundo; daí o seu nome. Para obter mais informações, consulte [Transformação de mundo](../graphics-concepts/world-transform.md).

####  <a name="view-transform-matrix"></a>Matriz de transformação de exibição

A transformação de exibição localiza o visualizador no espaço do mundo, transformando vértices em espaço da câmera. No espaço de câmera, a câmera ou visualizador está na origem, olhando na direção z positiva. Para obter mais informações, acesse [Transformação da exibição](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Matriz de transformação da projeção

A transformação da projeção converte o tronco de exibição em uma forma cuboide. Um tronco de exibição é um volume 3D em uma cena posicionado em relação à câmera do visor. O visor é um retângulo 2D no qual uma cena 3D é projetada. Para obter mais informações, consulte [Visores e recorte](../graphics-concepts/viewports-and-clipping.md).

Como a parte perto do final do tronco de exibição é menor do que a extremidade oposta, isso tem o efeito de expandir objetos próximos da câmera; é assim que a perspectiva é aplicada à cena. Assim, os objetos que estão mais próximos ao player parecem maiores; os objetos que estão mais distantes parecem menores.

Matematicamente, a transformação da projeção é uma matriz que em geral é uma projeção de escala e perspectiva. Isso funciona como a lente de uma câmera. Para obter mais informações, consulte [Transformação da projeção](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>Estado de amostra

O estado de amostra determina como os dados de textura são amostrados por meio de modos de endereçamento de textura, filtragem e nível de detalhe. A amostragem é feita sempre que um pixel de textura ou texel é lido a partir de uma textura.

Uma textura contém uma matriz de texels ou pixels de textura. A posição de cada texel é indicada por (u, v), onde u é a largura e v é a altura, e é mapeada entre 0 e 1 com base na altura e largura da textura. As coordenadas de textura resultantes são usadas para tratar um texel na amostragem de uma textura.

Quando as coordenadas de textura estão abaixo de 0 ou acima de 1, o modo de endereçamento de textura define como a coordenada de textura trata uma localização de texel. Por exemplo, ao usar __TextureAddressMode.Clamp__, qualquer coordenada fora do intervalo 0-1 é vinculada a um valor máximo de 1 e um valor mínimo de 0 antes da amostragem.

Se a textura for muito grande ou muito pequena em relação ao polígono, será filtrada para se ajustar ao espaço. Um filtro de ampliação aumenta uma textura; um filtro de redução diminui a textura para que se ajuste a uma área menor. A ampliação da textura repete o texel de amostra para um ou mais endereços, produzindo uma imagem mais desfocada. A redução da textura é mais complicada porque requer uma combinação de mais de um valor de texel em um único valor. Isso pode causar bordas serrilhadas ou irregulares dependendo dos dados de textura. A abordagem mais popular para redução é usar um minimapa. Um minimapa é uma textura com vários níveis. O tamanho de cada nível é uma potência de dois menor do que o nível anterior até uma textura 1x1. Quando a redução é utilizada, o jogo escolhe o nível de minimapa mais próximo ao tamanho necessário no momento de renderização. 

### <a name="basicloader"></a>BasicLoader

__BasicLoader__ é uma classe de carregador simples que fornece suporte para carregar sombreadores, texturas e malhas de arquivos em disco. Fornece métodos síncronos e assíncronos. Neste jogo de exemplo, os arquivos BasicLoader.h/.cpp são encontrados na pasta __Commons__.

Para obter mais informações, consulte [Carregador básico](complete-code-for-basicloader.md).