---
title: Configuração
description: Saiba como montar o pipeline de renderização para exibir elementos gráficos. Renderização de jogos, configurar e preparar dados.
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, renderização
ms.localizationpriority: medium
ms.openlocfilehash: 1a3f689f86e629ce81946927fa732a3ab692b219
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409505"
---
# <a name="rendering-framework-ii-game-rendering"></a>Estrutura de renderização II: introdução ao jogo

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

Em [Estrutura de renderização I](tutorial--assembling-the-rendering-pipeline.md), falamos sobre como podemos capturar as informações da cena e apresentá-las na tela. Agora, vamos voltar um pouco e aprender a preparar os dados para renderização.

>[!Note]
>Se você não tiver baixado o código de jogo mais recente para este exemplo, vá para [jogo de exemplo do Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Recapitulação rápida do objetivo. Entender como configurar uma estrutura de renderização básica para exibir a saída gráfica de um jogo UWP em DirectX. Podemos agrupá-las facilmente nestas três etapas.

 1. Estabelecer uma conexão com a interface gráfica
 2. Preparação: criar os recursos necessários para desenhar os elementos gráficos
 3. Exibir os elementos gráficos: renderizar o quadro

A [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md) descreve como os elementos gráficos são renderizados, abordagem nas etapas 1 e 3. 

Este artigo explica como configurar as outras partes dessa estrutura e preparar os dados necessários para que a renderização possa ser feita, que é a etapa 2 do processo.

## <a name="design-the-renderer"></a>Projetar o renderizador

O renderizador é responsável por criar e manter todos os objetos D3D11 e D2D usados para gerar os elementos visuais do jogo. A classe __GameRenderer__ é o renderizador deste jogo de exemplo e é projetada para atender às necessidades de renderização do jogo.

Estes são alguns conceitos que você pode usar projetar o renderizador do seu jogo:
* Como as APIs do Direct3D 11 são definidas como APIS [COM](/windows/desktop/com/the-component-object-model), você deve fornecer referências [ComPtr](/cpp/windows/comptr-class) aos objetos definidos por essas APIs. Esses objetos são liberados automaticamente quando sua última referência fica fora do escopo após o encerramento do app. Para obter mais informações, consulte [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr). Exemplo desses objetos: buffers de constantes, objetos sombreadores - [sombreador de vértice](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [sombreador de pixel](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders) e objetos de recurso de sombreador.
* Os buffers de constantes são definidos nessa classe para armazenar vários dados necessários à renderização.
    * Use vários buffers de constantes com diferentes frequências para reduzir a quantidade de dados que deve ser enviada à GPU por quadro. Este exemplo separa as constantes em diferentes buffers com base na frequência em que eles devem ser atualizados. Esta é a prática recomendada para programação do Direct3D. 
    * Neste jogo de exemplo, 4 buffers de constantes são definidos.
        1. __m \_ constantBufferNeverChanges__ contém os parâmetros de iluminação. Ele é definido uma vez no método __FinalizeCreateGameDeviceResources__ e nunca mais é alterado.
        2. __m \_ constantBufferChangeOnResize__ contém a matriz de projeção. A matriz de projeção depende do tamanho e da taxa de proporção da janela. Ele é definido em [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) e, em seguida, atualizado depois que os recursos são carregados no método [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method). Se a renderização for em 3D, ele também será alterado duas vezes por quadro.
        3. __m \_ constantBufferChangesEveryFrame__ contém a matriz de exibição. Essa matriz depende da posição da câmera e da direção do olhar (o normal para a projeção) e muda somente uma vez por quadro no método __Render__. Isso foi abordado anteriormente em __Estrutura de renderização I: introdução à renderização__, no método [__GameRenderer::Render__ method](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).
        4. __m \_ constantBufferChangesEveryPrim__ contém a matriz do modelo e as propriedades de material de cada primitivo. A matriz do modelo transforma os vértices das coordenadas locais em coordenadas mundiais. Essas constantes são específicas de cada primitiva e atualizadas para cada chamada de desenho. Isso foi abordado anteriormente em __Estrutura de renderização I: introdução à renderização__, em [Primitive rendering](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering).
* Os objetos de recurso de sombreador que contêm as texturas das primitivas também são definidos nesta classe.
    * Alguns texturas são predefinidas. (O [DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) é um formato de arquivo que pode ser usado para armazenar texturas compactadas e descompactadas. As texturas DDS são usadas para as paredes e o piso do mundo, bem como para as esferas de munição.)
    * Neste jogo de exemplo, os objetos de recurso do sombreador são: __m \_ sphereTexture__, __m \_ cylinderTexture__, __m \_ ceilingTexture__, __m \_ floorTexture__, __m \_ wallsTexture__.
* Os objetos de sombreador são definidos nesta classe para calcular nossas primitivas e texturas. 
    * Neste jogo de exemplo, os objetos do sombreador são __m \_ vertexShader__, __m \_ vertexShaderFlat__e __m \_ pixelShader__, __m \_ pixelShaderFlat__.
    * O sombreador de vértice processa as primitivas e a iluminação básica, e o sombreador de pixel (às vezes, chamou um sombreador de fragmento) processa as texturas e quaisquer efeitos por pixel.
    * Existem duas versões desses sombreadores (regulares e planos) para a renderização de diferentes primitivas. O motivo pelo qual temos diferentes versões é que as versões planas são muito mais simples e não fazem realces especulares por efeitos de iluminação de pixel. Elas são usadas nas paredes e tornam a renderização mais rápida em dispositivos com pouco consumo de energia.

## <a name="gamerendererh"></a>GameRenderer.h

Agora, vamos examinar o código no objeto de classe do renderizador do jogo de exemplo.

```cppwinrt
// Class handling the rendering of the game
class GameRenderer : public std::enable_shared_from_this<GameRenderer>
{
public:
    GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources);

    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();
    // --- end of async related methods section

    winrt::Windows::Foundation::IAsyncAction CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game);
    void FinalizeCreateGameDeviceResources();
    winrt::Windows::Foundation::IAsyncAction LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    Simple3DGameDX::IGameUIControl* GameUIControl() { return &m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay.Visible(); }
    // --- end of rendering overlay section
...
private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>        m_deviceResources;

    ...

    // Shader resource objects
    winrt::com_ptr<ID3D11ShaderResourceView>    m_sphereTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_cylinderTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_ceilingTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_floorTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant buffers
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferNeverChanges;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    winrt::com_ptr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShader;
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShaderFlat;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShader;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShaderFlat;
    winrt::com_ptr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>Construtor

Em seguida, vamos examinar o construtor __GameRenderer__ do jogo de exemplo e compará-lo com o construtor __Sample3DSceneRenderer__ fornecido no modelo de aplicativo DirectX 11.

```cppwinrt
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
    m_gameInfoOverlay(deviceResources),
    m_gameHud(deviceResources, L"Windows platform samples", L"DirectX first-person game sample")
{
    // m_gameInfoOverlay is a GameHud object to render text in the top left corner of the screen.
    // m_gameHud is Game info rendered as an overlay on the top-right corner of the screen,
    // for example hits, shots, and time.

    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>Criar e carregar recursos gráficos DirectX

No jogo de exemplo (e no modelo de __aplicativo DirectX 11 do Visual Studio (universal do Windows)__ ), a criação e o carregamento de recursos do jogo são implementados usando esses dois métodos que são chamados do construtor __GameRenderer__ :

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)

## <a name="createdevicedependentresources-method"></a>Método CreateDeviceDependentResources

No modelo do app DirectX 11, este método é usado para carregar o sombreador de vértice e de pixel de forma assíncrona, criar o buffer de constantes e de sombreador, criar uma malha com vértices que contêm informações de posição e de cor. 

No jogo de exemplo, essas operações dos objetos de cena são divididas entre os métodos [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) e [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method). 

Para este jogo de exemplo, o que entra nesse método?

* Variáveis instanciadas (__m \_ gameResourcesLoaded__ = false e __m \_ levelResourcesLoaded__ = false) que indicam se os recursos foram carregados antes de avançar para render, já que estamos carregando-os de forma assíncrona. 
* Como a HUD e a renderização de sobreposição estão em objetos de classe separados, chame os métodos __GameHud::CreateDeviceDependentResources__ and __GameInfoOverlay::CreateDeviceDependentResources__ aqui.

Este é o código de __GameRenderer::CreateDeviceDependentResources__.

```cppwinrt
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate whether resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud.CreateDeviceDependentResources();
    m_gameInfoOverlay.CreateDeviceDependentResources();
}
```

Abaixo está uma lista dos métodos que são usados para criar e carregar recursos.

- CreateDeviceDependentResources
  - CreateGameDeviceResourcesAsync (adicionado)
  - FinalizeCreateGameDeviceResources (adicionado)
- CreateWindowSizeDependentResources

Antes de aprofundarmos nos outros métodos usados para criar e carregar recursos, vamos primeiro criar o renderizador e observar como ele se encaixa no loop do jogo.

## <a name="create-the-renderer"></a>Criar o renderizador

O __GameRenderer__ é criado no construtor do __GameMain__. Ele também chama os dois outros métodos, [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) e [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method), que são adicionados para criar e carregar os recursos.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // Creation of GameRenderer
    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);

    ...

    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    ...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>Método CreateGameDeviceResourcesAsync

__CreateGameDeviceResourcesAsync__ é chamado a partir do método de construtor __GameMain__ no loop de __criação de \_ tarefa__ , já que estamos carregando os recursos do jogo de forma assíncrona.
        
__CreateDeviceResourcesAsync__ é um método executado como um conjunto separado de tarefas assíncronas para carregar os recursos do jogo. Como é esperado que ele seja executado em um thread separado, ele só tem acesso aos métodos de dispositivo Direct3D 11 (os definidos no __ID3D11Device__), e não aos métodos de contexto de dispositivo (os métodos definidos no __ID3D11DeviceContext__); sendo assim, ele não faz nenhuma renderização.

O método __FinalizeCreateGameDeviceResources__ é executado no thread principal e não tem acesso aos métodos de contexto de dispositivo Direct3D 11.

Em princípio:
* Use apenas os métodos __ID3D11Device__ de __CreateGameDeviceResourcesAsync__ porque eles não têm threads, o que significa que eles podem ser executados em qualquer thread. Também é esperado que eles não sejam executados no mesmo thread em que o __GameRenderer__ foi criado. 
* Não use métodos do __ID3D11DeviceContext__ aqui porque eles precisam ser executados em um único thread e no mesmo thread de __GameRenderer__.
* Use este método para criar buffers de constantes.
* Use este método para carregar texturas (como os arquivos .dds) e informações de sombreador (como os arquivos .cso) nos [sombreadores](tutorial--assembling-the-rendering-pipeline.md#shaders).

Este método é usado para:
* Crie os 4 [buffers constantes](tutorial--assembling-the-rendering-pipeline.md#buffer): __m \_ constantBufferNeverChanges__, __m \_ constantBufferChangeOnResize__, __m \_ constantBufferChangesEveryFrame__, __m \_ constantBufferChangesEveryPrim__
* Criar um objeto de [estado de amostra](tutorial--assembling-the-rendering-pipeline.md#sampler-state) que encapsula informações de amostragem de uma textura
* Crie um grupo de tarefas que contenha todas as tarefas assíncronas criadas pelo método. Ele aguarda a conclusão de todas essas tarefas assíncronas e chama __FinalizeCreateGameDeviceResources__.
* Crie um carregador usando [Carregador básico](tutorial--assembling-the-rendering-pipeline.md#the-basicloader-class). Adicione as operações de carregamento assíncrono do carregador como tarefas ao grupo de tarefas criado anteriormente.
* Métodos como __BasicLoader::LoadShaderAsync__ e __BasicLoader::LoadTextureAsync__ são usados para carregar:
    * objetos de sombreador compilados (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso e PixelShaderFlat.cso). Para obter mais informações, acesse [Vários formatos de arquivo de sombreador](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats).
    * texturas específicas do jogo (metal_texture assets cellceiling. DDS, \\ cellfloor. DDS, cellwall. DDS).

```cppwinrt
IAsyncAction GameRenderer::CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game)
{
    auto lifetime = shared_from_this();

    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method. It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC. See
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_buffer_desc
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    ...

    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11device-createbuffer.
    winrt::check_hresult(
        d3dDevice->CreateBuffer(&bd, nullptr, m_constantBufferNeverChanges.put())
        );

    ...

    // Define D3D11_SAMPLER_DESC. For API ref, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_sampler_desc.
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. For API ref, see
    // https://docs.microsoft.com/previous-versions/windows/desktop/legacy/aa366920(v=vs.85).
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    ...

    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    winrt::check_hresult(
        d3dDevice->CreateSamplerState(&sampDesc, m_samplerLinear.put())
        );

    // Start the async tasks to load the shaders and textures.

    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. For more info, see
    // https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader.
    BasicLoader loader{ d3dDevice };

    std::vector<IAsyncAction> tasks;

    uint32_t numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the
    // BasicLoader::LoadShaderAsync method. Push these method calls into a list of tasks.
    tasks.push_back(loader.LoadShaderAsync(L"VertexShader.cso", PNTVertexLayout, numElements, m_vertexShader.put(), m_vertexLayout.put()));
    tasks.push_back(loader.LoadShaderAsync(L"VertexShaderFlat.cso", nullptr, numElements, m_vertexShaderFlat.put(), nullptr));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShader.cso", m_pixelShader.put()));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShaderFlat.cso", m_pixelShaderFlat.put()));

    // Make sure the previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds,
    // cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks.
    tasks.push_back(loader.LoadTextureAsync(L"Assets\\seafloor.dds", nullptr, m_sphereTexture.put()));
    ...

    // Simulate loading additional resources by introducing a delay.
    tasks.push_back([]() -> IAsyncAction { co_await winrt::resume_after(GameConstants::InitialLoadingDelay); }());

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    for (auto&& task : tasks)
    {
        co_await task;
    }
}
```

## <a name="finalizecreategamedeviceresources-method"></a>Método FinalizeCreateGameDeviceResources

O método __FinalizeCreateGameDeviceResources__ é chamado depois que todas as tarefas de carregamento de recursos do método __CreateGameDeviceResourcesAsync__ são concluídas. 

* Inicialize constantBufferNeverChanges com as posições de luz e cor. Carrega os dados iniciais nos buffers de constantes com uma chamada de método de contexto de dispositivo para __ID3D11DeviceContext::UpdateSubresource__.
* Assim que os recursos carregados de forma assíncrona terminam o carregamento, é possível associá-los aos objetos de jogo apropriados.
* Para cada objeto de jogo, crie a malha e o material usando as texturas que foram carregadas. Em seguida, associe a malha e o material ao objeto de jogo.
* Para o objeto de jogo de destino, a textura composta por anéis concêntricos coloridos, com um valor numérico na parte superior, não é carregada a partir de um arquivo de textura. Em vez disso, ele é gerado por procedimento através do código em __TargetTexture.cpp__. A classe __TargetTexture__ cria os recursos necessários para desenhar a textura em um recurso de tela desativada no momento da inicialização. A textura resultante é, então, associada aos objetos de jogo de destino apropriados.

__FinalizeCreateGameDeviceResources__ e [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) compartilham partes similares do código para isso:
* Use __SetProjParams__ para garantir que a câmera esteja com a matriz de projeção correta. Para obter mais informações, acesse [Câmera e espaço de coordenadas](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space).
* Manipule a rotação da tela pós-multiplicando a matriz de rotação 3D na matriz de projeção da câmera. Em seguida, atualize o buffer de constantes __ConstantBufferChangeOnResize__ com a matriz de projeção resultante.
* Defina a variável global __booleana__ do __m \_ gameResourcesLoaded__ para indicar que os recursos agora estão carregados nos buffers, prontos para a próxima etapa. Lembre-se de que inicializamos primeiro essa variável como __FALSE__ no método de construtor do __GameRenderer__, por meio do método __GameRenderer::CreateDeviceDependentResources__. 
* Quando esse __m \_ GameResourcesLoaded__ é __verdadeiro__, a renderização dos objetos de cena pode ocorrer. Isso foi abordado no artigo __Estrutura de renderização I: introdução à renderização__, no [__método GameRenderer::Render__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).

```cppwinrt
// This method is called from the GameMain constructor.
// Make sure that 2D rendering is occurring on the same thread as the main rendering.
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4(3.5f, 2.5f, 5.5f, 1.0f);
    ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture textureGenerator(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    auto cylinderMesh = std::make_shared<CylinderMesh>(d3dDevice, (uint16_t)26);
    ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    auto cylinderMaterial = std::make_shared<Material>(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.get(),
        m_vertexShader.get(),
        m_pixelShader.get()
        );

    ...

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto&& object : m_game->RenderObjects())
    {
        if (object->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            object->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.get(),
                    m_vertexShaderFlat.get(),
                    m_pixelShaderFlat.get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            object->Mesh(std::make_shared<WorldFloorMesh>(d3dDevice));
        }
        ...
        else if (auto cylinder = dynamic_cast<Cylinder*>(object.get()))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (auto target = dynamic_cast<Face*>(object.get()))
        {
            const int bufferLength = 16;
            wchar_t str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            auto string{ winrt::hstring(str, len) };

            winrt::com_ptr<ID3D11ShaderResourceView> texture;
            textureGenerator.CreateTextureResourceView(string, texture.put());
            target->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            texture = nullptr;
            textureGenerator.CreateHitTextureResourceView(string, texture.put());
            target->HitMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            target->Mesh(targetMesh);
        }
        ...
    }

    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera().SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera().Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>Método CreateWindowSizeDependentResource

Os métodos CreateWindowSizeDependentResources são chamados sempre que o tamanho da janela, a orientação, a renderização habilitada para estéreo ou a resolução é alterada. No jogo de exemplo, ele atualiza a matriz de projeção em __ConstantBufferChangeOnResize__.

Os recursos de tamanho de janela são atualizados desta maneira: 
* A estrutura do app recebe um dos vários eventos possíveis, indicando uma alteração no estado da janela. 
* O loop principal do jogo é, então, informado sobre o evento e chama __CreateWindowSizeDependentResources__ na instância da classe principal (__GameMain__), que, por sua vez, chama a implementação __CreateWindowSizeDependentResources__ na classe do renderizador do jogo (__GameRenderer__).
* O principal trabalho desse método é verificar se os objetos visuais não ficaram confusos ou se tornaram inválidos devido a uma alteração nas propriedades da janela.

Para este jogo de exemplo, várias chamadas de método são as mesmas que o método [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) . Para obter instruções passo a passo sobre o código, vá para a seção anterior.

Os ajustes de renderização de tamanho da HUD e da janela de sobreposição do jogo são abordados em [Adicionar uma interface do usuário](tutorial--adding-a-user-interface.md).

```cppwinrt
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{
    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud.CreateWindowSizeDependentResources();

    ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    ...

    m_gameInfoOverlay.CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera().SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera().Projection()),
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
}
```

## <a name="next-steps"></a>Próximas etapas

Esse é o processo básico de implementação da estrutura de renderização de elementos gráficos de um jogo. Quanto maior o jogo, mais abstrações precisarão ser empregadas para lidar com as hierarquias de tipos de objeto e comportamentos de animação. Você precisa implementar métodos mais complexos para carregar e gerenciar ativos, como malhas e texturas. Em seguida, vamos aprender a [adicionar uma interface do usuário](tutorial--adding-a-user-interface.md).