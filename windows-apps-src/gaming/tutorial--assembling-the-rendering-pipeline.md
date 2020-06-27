---
title: Introdução à renderização
description: Saiba como desenvolver o pipeline de renderização para exibir gráficos. Introdução à renderização.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, renderização
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409635"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Estrutura de renderização I: introdução à renderização

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

Até agora, abordamos como estruturar um jogo Plataforma Universal do Windows (UWP) e como definir um computador de estado para lidar com o fluxo do jogo. Agora é hora de aprender a desenvolver a estrutura de renderização. Vejamos como o jogo de exemplo renderiza a cena do jogo usando o Direct3D 11.

O Direct3D 11 contém um conjunto de APIs que fornece acesso aos recursos avançados de hardware gráfico de alto desempenho que pode ser usado para criar gráficos 3D para aplicativos com uso intensivo de gráficos, como jogos.

Renderizar gráficos de jogos na tela significa basicamente renderizar uma sequência de quadros na tela. Em cada quadro, você precisará renderizar objetos visíveis na cena, com base na visualização.

Para renderizar um quadro, você precisará passar as informações de cena necessárias para o hardware de modo que possa ser exibido na tela. Para exibir algo na tela, você precisará iniciar a renderização assim que o jogo começar a ser executado.

## <a name="objectives"></a>Objetivos

Para configurar uma estrutura de renderização básica para exibir a saída de gráficos para um jogo do UWP DirectX. Você pode dividi-lo livremente nestas três etapas.

1. Estabeleça uma conexão com a interface gráfica.
2. Crie os recursos necessários para desenhar os gráficos.
3. Exiba os gráficos renderizando o quadro.

Este tópico explica como os gráficos são renderizados, cobrindo as etapas 1 e 3.

[Estrutura de RENDERIZAÇÃO II: a renderização de jogos](tutorial-game-rendering.md) aborda a etapa 2 &mdash; como configurar a estrutura de renderização e como os dados são preparados antes que a renderização possa ocorrer.

## <a name="get-started"></a>Introdução

É uma boa ideia se familiarizar com os conceitos básicos de renderização e elementos gráficos. Se você for novo no Direct3D e na renderização, consulte [termos e conceitos](#terms-and-concepts) para obter uma breve descrição dos elementos gráficos e de renderização usados neste tópico.

Para este jogo, a classe **GameRenderer** representa o renderizador para este jogo de exemplo. É responsável por criar e manter todos os objetos Direct3D 11 e Direct2D usados para gerar os elementos visuais do jogo. Ele também mantém uma referência ao objeto **Simple3DGame** usado para recuperar a lista de objetos a serem renderizados, bem como o status do jogo para a exibição de cabeçotes (HUD).

Nesta parte do tutorial, vamos nos concentrar na renderização de objetos 3D no jogo.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Estabelecer uma conexão com a interface gráfica

Para obter informações sobre como acessar o hardware para renderização, consulte o tópico [definir a estrutura de aplicativo UWP do jogo](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method) .

### <a name="the-appinitialize-method"></a>O método App:: Initialize

A função **std:: make_shared** , conforme mostrado abaixo, é usada para criar um **shared_ptr** para **DX::D eviceresources**, que também fornece acesso ao dispositivo.

No Direct3D 11, um [dispositivo](#device) é usado para alocar e destruir objetos, renderizar primitivas e comunicar-se com a placa gráfica por meio do driver gráfico.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Exibir os elementos gráficos renderizando o quadro

A cena de jogo precisará renderizar quando o jogo for iniciado. As instruções para renderização começam no método [**GameMain:: Run**](#gamemainrun-method) , conforme mostrado abaixo.

O fluxo simples é isso.

1. **Atualização**
2. **Render**
3. **Presente**

### <a name="gamemainrun-method"></a>Método GameMain::Run

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

### <a name="update"></a>Atualizar

Consulte o tópico [Gerenciamento de fluxo de jogos](tutorial-game-flow-management.md) para obter mais informações sobre como os Estados de jogos são atualizados no método [**GameMain:: Update**](tutorial-game-flow-management.md#the-gamemainupdate-method) .

### <a name="render"></a>Renderizar

A renderização é implementada chamando o método [**GameRenderer:: render**](#gamerendererrender-method) de **GameMain:: Run**.

Se a [renderização de estéreo](#stereo-rendering) estiver habilitada, haverá duas passagens &mdash; de renderização uma para o olho à esquerda e outra para a direita. Em cada passagem de renderização, associaremos o destino de renderização e a exibição de estêncil de profundidade ao dispositivo. Também limparemos a exibição de estêncil de profundidade posteriormente.

> [!NOTE]
> A renderização estéreo pode ser alcançada por meio de outros métodos, como estéreo de passagem única usando instanciação de vértice ou sombreadores de geometria. O método de duas renderizações-passagens é uma maneira mais lenta, mas mais conveniente para realizar a renderização de estéreo.

Depois que o jogo estiver em execução e os recursos forem carregados, atualizaremos a [matriz de projeção](#projection-transform-matrix), uma vez por passagem de renderização. Os objetos são um pouco diferentes em cada visualização. Em seguida, configuramos o [pipeline de renderização de gráficos](#rendering-pipeline). 

> [!NOTE]
> Consulte [Criar e carregar recursos gráficos DirectX](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) para obter mais informações sobre como os recursos são carregados.

Neste jogo de exemplo, o renderizador foi projetado para usar um layout de vértice padrão em todos os objetos. Isso simplifica o design do sombreador e permite alterações fáceis entre sombreadores, independentemente da geometria dos objetos.

#### <a name="gamerendererrender-method"></a>Método GameRenderer::Render

Definimos o contexto do Direct3D para usar um layout de vértice de entrada. Os objetos de layout de entrada descrevem como os dados do buffer de vértices são transmitidos para o [pipeline de renderização](#rendering-pipeline). 

Em seguida, definimos o contexto do Direct3D para usar os buffers de constantes definidos anteriormente, que são usados pelo estágio de pipeline do [sombreador de vértice](#vertex-shaders-and-pixel-shaders) e pelo estágio de pipeline do [sombreador de pixels](#vertex-shaders-and-pixel-shaders) . 

> [!NOTE]
> Consulte [Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md) para obter mais informações sobre a definição dos buffers constantes.

Como os mesmos layout de entrada e conjunto de buffers constantes são usados em todos os sombreadores no pipeline, é configurado uma vez por quadro.

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

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

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
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
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>Renderização de primitivas

Ao renderizar a cena, você executará um loop por todos os objetos que precisam ser renderizados. As etapas a seguir são repetidas para cada objeto (primitiva).

- Atualize o buffer de constantes (**m_constantBufferChangesEveryPrim**) com a [matriz de transformação mundial](#world-transform-matrix) do modelo e as informações de material.
- O **m_constantBufferChangesEveryPrim** contém parâmetros para cada objeto. Ele inclui a matriz de transformação objeto a mundo, bem como propriedades de material, como expoente de cor e especulação para cálculos de iluminação.
- Defina o contexto do Direct3D para usar o layout de vértice de entrada para os dados do objeto de malha a serem transmitidos para o estágio IA (assembler de entrada) do [pipeline de renderização](#rendering-pipeline).
- Defina o contexto do Direct3D para usar um [buffer de índice](#index-buffer) no estágio ia. Forneça as informações de primitiva: tipo, ordem de dados.
- Envie uma chamada de desenho para desenhar a primitiva indexada não instanciada. O método **GameObject::Render** atualiza o [buffer constante](#constant-buffer-or-shader-constant-buffer) de primitiva com os dados específicos de uma determinada primitiva. Isso resulta em uma chamada de **DrawIndexed** no contexto para desenhar a geometria dessa primitiva. Especificamente, essa chamada de desenho enfileira comandos e dados para a GPU (Unidade de Processamento de Gráficos), conforme parametrizados pelos dados do buffer constante. Cada chamada de desenho executa o sombreador de vértice uma vez por vértice e, em seguida, o [sombreador de pixel](#vertex-shaders-and-pixel-shaders) uma vez para cada pixel de cada triângulo da primitiva. As texturas fazem parte do estado que o sombreador de pixel usa para executar a renderização.

Aqui estão os motivos para usar vários buffers de constantes.

- O jogo usa vários buffers de constantes, mas só precisa atualizar esses buffers uma vez por primitivo. Conforme mencionado anteriormente, os buffers constantes são como uma entrada de dados para os sombreadores executados para cada primitiva. Alguns dados são estáticos (**m_constantBufferNeverChanges**); alguns dados são constantes sobre o quadro (**m_constantBufferChangesEveryFrame**), como a posição da câmera; e alguns dados são específicos para o primitivo, como suas cores e texturas (**m_constantBufferChangesEveryPrim**).
- O renderizador do jogo separa essas entradas em diferentes buffers constantes para otimizar a largura de banda de memória usada pela CPU e pela GPU. Essa abordagem também ajuda a minimizar a quantidade de dados que a GPU precisa controlar. A GPU possui uma grande fila de comandos, e sempre que o jogo chama **Draw**, esse comando é inserido na fila com os respectivos dados. Quando o jogo atualiza o buffer de constantes de primitiva e emite o próximo comando **Draw**, o driver gráfico adiciona esse comando e os dados associados à fila. Se o jogo desenha 100 primitivas, pode haver 100 cópias dos dados do buffer constante na fila. Para minimizar a quantidade de dados enviados à GPU, o jogo usa um buffer de constantes de primitiva em separado contendo apenas as atualizações para cada primitiva.

#### <a name="gameobjectrender-method"></a>Método GameObject::Render

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
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
```

#### <a name="meshobjectrender-method"></a>Método meshcollection:: render

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>DeviceResources: método reenviado:P

Chamamos o método **DeviceResources::P reenviado** para exibir o conteúdo que colocamos nos buffers.

Usamos o termo cadeia de troca para uma coleção de buffers que são utilizados para exibir quadros para o usuário. Cada vez que um aplicativo apresenta um novo quadro para exibição, o primeiro buffer da cadeia de troca assume o lugar do buffer exibido. Esse processo é chamado troca ou inversão. Para obter mais informações, veja [Cadeias de troca](../graphics-concepts/swap-chains.md).

- O método **Present** da interface **IDXGISwapChain1** instrui o [dxgi](#dxgi) a bloquear até que a sincronização vertical (Vsync) ocorra, colocando o aplicativo em suspensão até o próximo vsync. Isso garante que você não perca nenhum ciclo renderizando quadros que nunca serão exibidos na tela.
- O método **DiscardView** da interface **ID3D11DeviceContext3** descarta o conteúdo do [destino render](#render-target). Trata-se de uma operação válida somente quando o conteúdo existente for inteiramente substituído. Se os retângulos sujos ou de rolagem forem usados, essa chamada deverá ser removida.
* Usando o mesmo método **DiscardView**, descarte o conteúdo de [estêncil de profundidade](#depth-stencil).
* O método **HandleDeviceLost** é usado para gerenciar o cenário do [dispositivo](#device) que está sendo removido. Se o dispositivo tiver sido removido por uma desconexão ou uma atualização de driver, você deverá recriar todos os recursos do dispositivo. Para obter mais informações, consulte [Lidar com cenários removidos de dispositivos no Direct3D 11](handling-device-lost-scenarios.md).

> [!TIP]
> Para obter uma taxa de quadros suave, você deve garantir que a quantidade de trabalho para renderizar um quadro caiba no tempo entre os VSyncs.

```cppwinrt
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
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>Próximas etapas

Este tópico explicou como os gráficos são renderizados na exibição e fornece uma breve descrição para alguns dos termos de renderização usados (abaixo). Saiba mais sobre renderização na [estrutura de RENDERIZAÇÃO II:](tutorial-game-rendering.md) tópico de renderização de jogos e saiba como preparar os dados necessários antes de renderizar.

## <a name="terms-and-concepts"></a>Termos e conceitos

### <a name="simple-game-scene"></a>Cena de jogo simples

Uma cena de jogo simples é composta de alguns objetos com várias fontes de iluminação.

A forma de um objeto é definida por um conjunto de coordenadas X, Y, Z no espaço. O local de renderização real no mundo do jogo pode ser determinado pela aplicação de uma matriz de transformação às coordenadas X, Y, Z de posição. Ele também pode ter um conjunto de texturas &mdash; que você e V &mdash; especificam como um material é aplicado ao objeto. Isso define as propriedades da superfície do objeto e oferece a capacidade de ver se um objeto tem uma superfície aproximada (como uma bola de tênis) ou uma superfície brilhante suave (como uma bola de boliche).

As informações de cena e objeto são usadas pela estrutura de renderização para recriar o quadro de cena por quadro, tornando-o ativo no monitor de exibição.

### <a name="rendering-pipeline"></a>Pipeline de renderização

O pipeline de renderização é o processo pelo qual as informações de cena 3D são convertidas em uma imagem exibida na tela. No Direct3D 11, esse pipeline é programável. É possível adaptar os estágios para atender às suas necessidades de renderização. Estágios que apresentam núcleos comuns de sombreador são programáveis por meio da linguagem de programação HLSL. Ele também é conhecido como *pipeline de renderização de gráficos*ou simplesmente *pipeline*.

Para ajudá-lo a criar esse pipeline, você precisa estar familiarizado com esses detalhes.

- [HLSL](#hlsl). Recomendamos o uso do modelo de sombreador 5.1 do HLSL e acima para jogos UWP do DirectX.
- [Sombreadores](#shaders).
- Sombreadores [de vértice e sombreadores de pixel](#vertex-shaders-and-pixel-shaders).
- [Estágios do sombreador](#shader-stages).
- [Vários formatos de arquivo de sombreador](#various-shader-file-formats).

Para obter mais informações, consulte [Noções básicas sobre o pipeline de renderização do Direct3D 11](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline) e [Pipeline de elementos gráficos](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="hlsl"></a>HLSL

HLSL é a linguagem de sombreador de alto nível para DirectX. Usando o HLSL, você pode criar sombreadores programáveis C para o pipeline do Direct3D. Para obter mais informações, consulte [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl).

#### <a name="shaders"></a>Sombreadores

Um sombreador pode ser considerado como um conjunto de instruções que determinam como a superfície de um objeto aparece quando renderizado. Aqueles que são programados usando HLSL são conhecidos como sombreadores HLSL. Os arquivos de código-fonte para os sombreadores de [HLSL]) (#hlsl) têm a `.hlsl` extensão de arquivo. Esses sombreadores podem ser compilados no tempo de compilação ou no tempo de execução e definidos no tempo de execução no estágio de pipeline apropriado. Um objeto de sombreador compilado tem uma `.cso` extensão de arquivo.

Os sombreadores do Direct3D 9 podem ser criados usando o modelo do sombreador 1, o modelo do sombreador 2 e o modelo do sombreador 3; Os sombreadores do Direct3D 10 podem ser criados apenas no modelo do sombreador 4. Os sombreadores do Direct3D 11 podem ser criados no modelo de sombreador 5. O Direct3D 11.3 e o Direct3D 12 podem ser criados no modelo de sombreador 5.1, bem como o Direct3D 12 pode ser criado no modelo de sombreador 6.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Sombreadores de vértice e sombreadores de pixel

Os dados entram no pipeline de gráficos como um fluxo de primitivos e são processados por vários sombreadores, como os sombreadores de vértices e sombreadores de pixel. 

Os sombreadores de vértice processam vértices, geralmente realizando operações como transformações, aplicação de capas e iluminação. Os sombreadores de pixel permitem técnicas de sombreamento avançadas, como iluminação por pixel e pós-processamento. Combina variáveis constantes, dados de textura, valores interpolados por vértice e outros dados para produzir saídas por pixel. 

#### <a name="shader-stages"></a>Estágios de sombreador

Uma sequência desses vários sombreadores definidos para processar esse fluxo de primitivas é conhecida como estágios de sombreador em um pipeline de renderização. Os estágios reais dependem da versão do Direct3D, mas geralmente incluem o vértice, a geometria e os estágios de pixel. Também há outros estágios, como os sombreadores hull e de domínio para mosaico e o sombreador de computação. Todos esses estágios são totalmente programáveis usando [HLSL](#hlsl). Para obter mais informações, consulte [Pipeline de elementos gráficos](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="various-shader-file-formats"></a>Vários formatos de arquivo de sombreador

Aqui estão as extensões de arquivo de código do sombreador.

- Um arquivo com a `.hlsl` extensão contém [HLSL]) (#hlsl) código-fonte.
- Um arquivo com a `.cso` extensão contém um objeto de sombreador compilado.
- Um arquivo com a `.h` extensão é um arquivo de cabeçalho, mas em um contexto de código de sombreador, esse arquivo de cabeçalho define uma matriz de bytes que contém dados de sombreador.
- Um arquivo com a `.hlsli` extensão contém o formato dos buffers de constantes. No jogo de exemplo, o arquivo é **shaders**  >  **ConstantBuffers. hlsli**.

> [!NOTE]
> Você pode inserir um sombreador carregando um `.cso` arquivo em tempo de execução ou adicionando um `.h` arquivo em seu código executável. Mas você não usaria ambos para o mesmo sombreador.

### <a name="deeper-understanding-of-directx"></a>Compreensão mais profunda do DirectX

O Direct3D 11 é um conjunto de APIs que pode nos ajudar a criar gráficos para aplicativos com uso intensivo de gráficos, como jogos, em que desejamos ter uma boa placa gráfica para processar a computação intensiva. Esta seção explica resumidamente os conceitos de programação de elementos gráficos do Direct3D 11: recurso, sub-recurso, dispositivo e contexto de dispositivo.

#### <a name="resource"></a>Recurso

Você pode pensar em recursos (também conhecidos como recursos do dispositivo) como informações sobre como renderizar um objeto, como textura, posição ou cor. Os recursos fornecem dados para o pipeline e definem o que é renderizado durante sua cena. Os recursos podem ser carregados de sua mídia de jogos ou criados dinamicamente em tempo de execução.

Na verdade, um recurso é uma área na memória que pode ser acessada pelo [pipeline](#rendering-pipeline) de Direct3D. Para que o pipeline acesse a memória com eficiência, os dados fornecidos ao pipeline (como geometria de entrada, recursos de sombreador e texturas) devem ser armazenados em um recurso. Existem dois tipos de recursos dos quais todos os recursos de Direct3D são derivados: um buffer ou uma textura. Até 128 recursos podem estar ativos para cada estágio do pipeline. Para saber mais, confira [Recursos](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Sub-recurso

O termo sub-recurso refere-se ao subconjunto de um recurso. O Direct3D pode fazer referência a um recurso inteiro ou pode fazer referência a subconjuntos de um recurso. Para obter mais informações, consulte [Sub-recurso](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Estêncil de profundidade

Um recurso de estêncil de profundidade contém o formato e o buffer para manter informações de profundidade e estêncil. É criado por meio de um recurso de textura. Para obter mais informações sobre como criar um recurso de estêncil de profundidade, consulte [Configuração da funcionalidade de estêncil de profundidade](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil). Acessamos o recurso de estêncil de profundidade por meio da exibição de estêncil de profundidade implementada usando-se a interface [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview).

As informações de profundidade nos dizem quais áreas de polígonos estão atrás de outras pessoas, para que possamos determinar quais estão ocultas. As informações de estêncil mostram quais pixels estão mascarados. Isso pode ser usado para produzir efeitos especiais, pois determina se um pixel é desenhado ou não; define o bit como 1 ou 0. 

Para obter mais informações, consulte [exibição de estêncil de profundidade](../graphics-concepts/depth-stencil-view--dsv-.md), [buffer de profundidade](../graphics-concepts/depth-buffers.md)e buffer de [estêncil](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Destino de renderização

Um destino de renderização é um recurso que podemos gravar ao término de uma passagem de renderização. Em geral, é criado usando-se o método [ID3D11Device::CreateRenderTargetView](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) por meio do buffer de fundo da cadeia de troca (que também é um recurso) como o parâmetro de entrada. 

Cada destino de renderização também deve ter um modo de exibição de estêncil de profundidade correspondente porque, quando usamos [OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) para definir o destino de renderização antes de usá-lo, também é necessário um modo de exibição de estêncil de profundidade. Acessamos o recurso de destino de renderização por meio de exibição do destino de renderização implementada usando a interface [ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview). 

#### <a name="device"></a>Dispositivo

Você pode imaginar um dispositivo como uma maneira de alocar e destruir objetos, renderizar primitivos e se comunicar com a placa gráfica por meio do driver de gráficos. 

Para obter uma explicação mais precisa, um dispositivo Direct3D é o componente de renderização do Direct3D. Um dispositivo encapsula e armazena o estado de renderização, executa transformações e operações de iluminação e rasteriza uma imagem para uma superfície. Para obter mais informações, consulte [Dispositivos](../graphics-concepts/devices.md)

Um dispositivo é representado pela interface [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) . Em outras palavras, a interface **ID3D11Device** representa um adaptador de vídeo virtual e é usada para criar recursos que pertencem a um dispositivo. 

Há diferentes versões do ID3D11Device. O [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5) é a versão mais recente e adiciona novos métodos a eles no **ID3D11Device4**. Para obter mais informações sobre como o Direct3D se comunica com o hardware subjacente, consulte [Arquitetura do Windows Device Driver Model (WDDM)](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Cada aplicativo deve ter pelo menos um dispositivo; a maioria dos aplicativos cria apenas um. Crie um dispositivo para um dos drivers de hardware instalados em seu computador chamando **D3D11CreateDevice** ou **D3D11CreateDeviceAndSwapChain** e especificando o tipo de driver com o sinalizador **D3D_DRIVER_TYPE** . Cada dispositivo pode usar um ou mais contextos de dispositivo, dependendo da funcionalidade desejada. Para obter mais informações, consulte a [função D3D11CreateDevice](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

#### <a name="device-context"></a>Contexto de dispositivo

Um contexto de dispositivo é usado para definir o estado do [pipeline](#rendering-pipeline) e gerar comandos de renderização usando os [recursos](#resource) de propriedade de um [dispositivo](#device). 

O Direct3D 11 implementa dois tipos de contextos de dispositivo, um para renderização imediata e outro para renderização adiada; ambos os contextos são representados com uma interface [ID3D11DeviceContext](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext). 

As interfaces **ID3D11DeviceContext** têm diferentes versões; **ID3D11DeviceContext4** acrescenta novos métodos àquela em **ID3D11DeviceContext3**.

O **ID3D11DeviceContext4** é introduzido na atualização do Windows 10 para criadores e é a versão mais recente da interface **ID3D11DeviceContext** . Os aplicativos destinados à atualização do Windows 10 para criadores e posterior devem usar essa interface em vez de versões anteriores. Para obter mais informações, consulte [ID3D11DeviceContext4](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4).

#### <a name="dxdeviceresources"></a>DX::DeviceResources

A classe **DX::D eviceresources** está nos arquivos **DeviceResources. cpp** / **. h** e controla todos os recursos do dispositivo DirectX.

### <a name="buffer"></a>Buffer

Um recurso de buffer é uma coleção de dados completamente tipados, agrupados em elementos. É possível usar buffers para armazenar uma ampla variedade de dados, incluindo vetores de posição, vetores normais, coordenadas de textura em um buffer de vértice, índices em um buffer de índice ou estado do dispositivo. Os elementos de buffer podem incluir valores de dados compactados (como valores de superfície **R8G8B8A8** ), inteiros de 8 bits únicos ou valores de ponto flutuante de 4 32 bits.

Há três tipos de buffers disponíveis: buffer de vértice, buffer de índice e buffer de constante.

#### <a name="vertex-buffer"></a>Buffer de vértice

Contém os dados de vértice usados para definir sua geometria. Dados de vértice incluem coordenadas de posição, dados de cor, dados de coordenadas de textura, dados normais e assim por diante. 

#### <a name="index-buffer"></a>Buffer de índice

Contém deslocamentos de inteiros em buffers de vértice e são usados para renderizar primitivas de forma mais eficiente. Um buffer de índice contém um conjunto sequencial de índices de 16 ou 32 bits; cada índice é usado para identificar um vértice em um buffer de vértices.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Buffer de constantes ou buffer de constante de sombreador

Permite que você forneça com eficiência dados do sombreador no pipeline. É possível usar buffers constantes como entradas para os sombreadores executados para cada primitiva e armazenar os resultados do estágio de saída de fluxo do pipeline de renderização. Conceitualmente, um buffer constante parece muito com um buffer de vértice de elemento único.

#### <a name="design-and-implementation-of-buffers"></a>Design e implementação de buffers

Você pode criar buffers com base no tipo de dados, por exemplo, como em nosso jogo de exemplo, um buffer é criado para dados estáticos, outro para os dados que são constantes sobre o quadro e outro para dados específicos de um primitivo.

Todos os tipos de buffer são encapsulados pela interface **ID3D11Buffer**, e você pode criar um recurso de buffer chamando **ID3D11Device::CreateBuffer**. Contudo, um buffer deve ser vinculado ao pipeline antes que possa ser acessado. Os buffers podem ser vinculados a vários estágios de pipeline simultaneamente para leitura. Um buffer também pode ser associado a um único estágio de pipeline para gravação; no entanto, o mesmo buffer não pode ser associado a leitura e gravação simultaneamente.

Você pode associar buffers das maneiras a seguir.

- Para o estágio de Assembler de entrada chamando métodos **ID3D11DeviceContext** , como **ID3D11DeviceContext:: IASetVertexBuffers** e **ID3D11DeviceContext:: IASetIndexBuffer**.
- Para o estágio Stream-output chamando **ID3D11DeviceContext:: SOSetTargets**.
- Para o estágio do sombreador chamando métodos de sombreador, como **ID3D11DeviceContext:: VSSetConstantBuffers**.

Para obter mais informações, consulte [Introdução a buffers no Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro).

### <a name="dxgi"></a>DXGI

O Microsoft DirectX Graphics Infrastructure (DXGI) é um subsistema que encapsula algumas das tarefas de nível inferior que são necessárias para o Direct3D. Deve-se tomar cuidado especial ao usar DXGI em um aplicativo multithread para garantir que os deadlocks não ocorram. Para obter mais informações, consulte [multithreading e dxgi](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi)

### <a name="feature-level"></a>Nível de recurso

Nível de recursos é um conceito apresentado no Direct3D 11 para manipular a diversidade de placas de vídeo em computadores novos e existentes. Um nível de recurso é um conjunto bem definido de funcionalidades de GPU (unidade de processamento gráfico). 

Cada placa de vídeo implementa um certo nível de funcionalidade de DirectX dependendo das GPUs instaladas. Em versões anteriores do Microsoft Direct3D, você poderia descobrir a versão do Direct3D que a placa de vídeo implementou e programar o seu aplicativo adequadamente. 

Com o nível de recursos, na criação de um dispositivo, pode-se tentar criá-lo para o nível de recursos que se deseja solicitar. Se for possível criar um dispositivo, isso significa que o nível de recursos existe; caso contrário, o hardware não permite tal nível de recursos. Você pode tentar recriar um dispositivo em um nível de recurso inferior ou pode optar por sair do aplicativo. Por exemplo, o nível de recurso 12_0 requer Direct3D 11,3 ou Direct3D 12 e o modelo de sombreador 5,1. Para obter mais informações, consulte [Níveis de recursos do Direct3D: visão geral de cada nível de recursos](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

Usando os níveis de recursos, é possível desenvolver um aplicativo para Direct3D 9, o Microsoft Direct3D 10 ou o Direct3D 11 e, em seguida, executá-lo no hardware 9, 10 ou 11 (com algumas exceções). Para obter mais informações, consulte [Níveis de recursos do Direct3D](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

### <a name="stereo-rendering"></a>Renderização estéreo

A renderização estéreo é usada para aumentar a ilusão de profundidade. Usa duas imagens, uma do olho esquerdo e a outra do olho direito para exibir uma cena na tela de exibição. 

Matematicamente, aplicamos uma matriz de projeção estéreo, que é um pequeno deslocamento horizontal à direita e à esquerda, da matriz de projeção mono regular para obter esse efeito.

Fizemos dois passos de renderização para obter a renderização de estéreo neste jogo de exemplo.

- Vincule ao destino de renderização direito, aplique a projeção direita e depois desenhe o objeto de primitiva.
- Vincule ao destino de renderização esquerdo, aplique a projeção esquerda e depois desenhe o objeto de primitiva.

### <a name="camera-and-coordinate-space"></a>Câmera e espaço de coordenadas

O jogo já dispõe do código necessário para atualizar o mundo em seu próprio sistema de coordenadas (também conhecido como espaço do mundo ou espaço da cena). Todos os objetos, inclusive a câmera, são posicionados e orientados nesse espaço. Para obter mais informações, consulte [Sistemas de coordenadas](../graphics-concepts/coordinate-systems.md).

Um shader de vértice executa o trabalho pesado de conversão das coordenadas do modelo para os coordenadas do dispositivo por meio do seguinte algoritmo (onde V é um vetor e M uma matriz).

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`é uma matriz de transformação para coordenadas de modelo para coordenadas mundiais, também conhecida como [matriz de transformação mundial](#world-transform-matrix). Isso é fornecido pela primitiva.
- `M(world-to-view)`é uma matriz de transformação para coordenadas mundiais para exibir coordenadas, também conhecida como [matriz de transformação de exibição](#view-transform-matrix).
  - Isso é fornecido pela matriz de visualização da câmera. Ele é definido pela posição da câmera junto com os vetores de aparência (a *aparência* do vetor que aponta diretamente para a cena a partir da câmera e o vetor de *pesquisa* que é o mais perpendicular a ele).
  - No jogo de exemplo, **m_viewMatrix** é a matriz de transformação View e é calculada usando **Camera:: SetViewParams**.
- `M(view-to-device)`é uma matriz de transformação para coordenadas de exibição para coordenadas de dispositivo, também conhecida como [matriz de transformação de projeção](#projection-transform-matrix).
  - Isso é fornecido pela projeção da câmera. Ele fornece informações sobre quanto desse espaço está realmente visível na cena final. O campo de exibição (FoV), a taxa de proporção e os planos de recorte definem a matriz de transformação de projeção.
  - No jogo de exemplo, **m_projectionMatrix** define a transformação para as coordenadas de projeção, calculada usando **Camera:: SetProjParams** (para projeção de estéreo, você usa duas matrizes de projeção &mdash; uma para cada exibição de olho).

O código do sombreador no `VertexShader.hlsl` é carregado com esses vetores e matrizes dos buffers de constantes e executa essa transformação para cada vértice.

### <a name="coordinate-transformation"></a>Transformação de coordenadas

O Direct3D usa três transformações para alterar suas coordenadas de modelo 3D em coordenadas de pixel (espaço da tela). Essas transformações são transformação de mundo, transformação de exibição e transformação de projeção. Para obter mais informações, consulte [visão geral da transformação](../graphics-concepts/transform-overview.md).

#### <a name="world-transform-matrix"></a>Matriz de transformação de mundo

Uma transformação de mundo altera as coordenadas do espaço do modelo, no qual são definidos os vértices relativos à origem local de um modelo, para o espaço do mundo, no qual são definidos os vértices relativos a uma origem comum para todos os objetos em uma cena. Em essência, a transformação do mundo coloca um modelo no mundo; daí o seu nome. Para obter mais informações, consulte [Transformação de mundo](../graphics-concepts/world-transform.md).

#### <a name="view-transform-matrix"></a>Matriz de transformação de exibição

A transformação de exibição localiza o visualizador no espaço do mundo, transformando vértices em espaço da câmera. No espaço de câmera, a câmera ou visualizador está na origem, olhando na direção z positiva. Para obter mais informações, acesse [Transformação da exibição](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Matriz de transformação da projeção

A transformação da projeção converte o tronco de exibição em uma forma cuboide. Um tronco de exibição é um volume 3D em uma cena posicionado em relação à câmera do visor. O visor é um retângulo 2D no qual uma cena 3D é projetada. Para obter mais informações, consulte [Visores e recorte](../graphics-concepts/viewports-and-clipping.md).

Como a parte perto do final do tronco de exibição é menor do que a extremidade oposta, isso tem o efeito de expandir objetos próximos da câmera; é assim que a perspectiva é aplicada à cena. Assim, os objetos mais próximos do jogador parecem maiores; os objetos que estão mais distantes parecem menores.

Matematicamente, a transformação projeção é uma matriz que normalmente é uma escala e uma projeção em perspectiva. Isso funciona como a lente de uma câmera. Para obter mais informações, consulte [Transformação da projeção](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>Estado de amostra

O estado de amostra determina como os dados de textura são amostrados por meio de modos de endereçamento de textura, filtragem e nível de detalhe. A amostragem é feita toda vez que um pixel de textura (ou Texel) é lido de uma textura.

Uma textura contém uma matriz de texels. A posição de cada Texel é denotada por `(u,v)` , em que `u` é a largura e `v` é a altura, e é mapeada entre 0 e 1 com base na largura e na altura da textura. As coordenadas de textura resultantes são usadas para tratar um texel na amostragem de uma textura.

Quando as coordenadas de textura estão abaixo de 0 ou acima de 1, o modo de endereçamento de textura define como a coordenada de textura trata uma localização de texel. Por exemplo, ao usar **TextureAddressMode.Clamp**, qualquer coordenada fora do intervalo 0-1 é vinculada a um valor máximo de 1 e um valor mínimo de 0 antes da amostragem.

Se a textura for muito grande ou muito pequena para o polígono, a textura será filtrada para se ajustar ao espaço. Um filtro de ampliação aumenta uma textura; um filtro de redução diminui a textura para que se ajuste a uma área menor. A ampliação da textura repete o texel de amostra para um ou mais endereços, produzindo uma imagem mais desfocada. A minificação de textura é mais complicada porque requer a combinação de mais de um valor de Texel em um único. Isso pode causar bordas serrilhadas ou irregulares dependendo dos dados de textura. A abordagem mais popular para redução é usar um minimapa. Um minimapa é uma textura com vários níveis. O tamanho de cada nível é uma potência de 2 menor do que o nível anterior até uma textura 1x1. Quando a redução é utilizada, o jogo escolhe o nível de minimapa mais próximo ao tamanho necessário no momento de renderização. 

### <a name="the-basicloader-class"></a>A classe BasicLoader

**BasicLoader** é uma classe de carregador simples que fornece suporte para carregar sombreadores, texturas e malhas de arquivos em disco. Fornece métodos síncronos e assíncronos. Neste jogo de exemplo, os `BasicLoader.h/.cpp` arquivos são encontrados na pasta **Utilities** .

Para obter mais informações, consulte [Carregador básico](complete-code-for-basicloader.md).