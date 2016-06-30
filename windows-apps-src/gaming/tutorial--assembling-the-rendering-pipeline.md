---
author: mtoepke
title: "Montar a estrutura de renderização"
description: "Agora chegou o momento de examinar como o jogo de exemplo usa essa estrutura e esse estado para exibir seus elementos gráficos."
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5eb6ea7ad1a30f020c155007396383b88d10c0a8

---

# Montar a estrutura de renderização


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Até aqui, você aprendeu a estruturar um jogo da Plataforma Universal do Windows (UWP) para trabalhar com o Windows Runtime e viu como definir uma máquina de estado para lidar com o fluxo do jogo. Agora chegou o momento de examinar como o jogo de exemplo usa essa estrutura e esse estado para exibir seus elementos gráficos. Aqui vemos a implementação de uma estrutura de renderização, desde a inicialização do dispositivo gráfico até a apresentação dos objetos gráficos para exibição.

## Objetivo


-   Entender como configurar uma estrutura de renderização básica para exibir a saída gráfica de um jogo DirectX UWP.

> **Observação**   Os seguintes arquivos de código não são abordados aqui, mas fornecem classes e métodos mencionados neste tópico e são [fornecidos como código no final deste tópico](#code_sample):
-   **Animate.h/.cpp**.
-   **BasicLoader.h/.cpp**. Fornece métodos para carregar malhas, sombreadores e texturas, tanto sincronicamente como assincronicamente. Isso é muito útil!
-   **MeshObject.h/.cpp**, **SphereMesh.h/.cpp**, **CylinderMesh.h/.cpp**, **FaceMesh.h/.cpp** e **WorldMesh.h/.cpp**. Contém as definições das primitivas do objeto usadas no jogo, como as esferas de munição, o cilindro e os obstáculos de cone, e as paredes da galeria de tiros. (**GameObject.cpp**, brevemente descrito neste tópico, contém o método para renderização dessas primitivas).
-   **Level.h/.cpp** e **Level\[1-6\].h/.cpp**. Contém a configuração de cada um dos seis níveis do jogo, incluindo os critérios de sucesso e o número e a posição das metas e obstáculos.
-   **TargetTexture.h/.cpp**. Contém um conjunto de métodos para desenhar os bitmaps usados como texturas nas metas.

Esses arquivos contêm um código que não é específico para os jogos UWP em DirectX. Mas você poderá revisá-los separadamente, se quiser mais detalhes sobre a implementação.

 

Esta seção abrange três arquivos principais do exemplo de jogo ([fornecido como código no final deste tópico](#code_sample)):

-   **Camera.h/.cpp**
-   **GameRenderer.h/.cpp**
-   **PrimObject.h/.cpp**

Novamente, presumimos que você compreende os conceitos básicos de programação 3D como malhas, vértices e texturas. Para saber mais sobre programação Direct3D 11 em geral, veja [Guia de Programação para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).
Tendo dito isso, examinaremos agora o trabalho que deve ser feito para colocar nosso jogo na tela.

## Visão geral do Windows Runtime e do DirectX


DirectX é uma parte fundamental do Windows Runtime e da experiência do Windows 10. Todos os elementos visuais do Windows 10 se baseiam no DirectX, e você dispõe da mesma linha direta para a mesma interface gráfica de baixo nível, [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534), que fornece uma camada de abstração para o hardware gráfico e seus drivers. Todas as APIs do Direct3D 11 estão disponíveis para você se comunicar diretamente com a DXGI. O resultado são gráficos rápidos e de alto desempenho em seus jogos, permitindo que você acesse todos os recursos do hardware gráfico mais avançado.

Para adicionar o suporte ao DirectX a um aplicativo UWP, crie um provedor de exibição para os recursos DirectX implementando as interfaces [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) e [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478). Essas interfaces fornecem um padrão de fábrica para o tipo do seu provedor de exibição e a implementação do provedor de exibição do DirectX, respectivamente. O singleton UWP, representado pelo objeto [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), executa essa implementação.

Em [Definindo a estrutura UWP do jogo](tutorial--building-the-games-metro-style-app-framework.md), analisamos como o renderizador se encaixa na estrutura do aplicativo do jogo de exemplo. Agora veremos como o renderizador do jogo se conecta à visualização e constrói os elementos gráficos que definem a aparência do jogo.

## Definindo o renderizador


O tipo abstrato **GameRenderer** herda do tipo de renderizador **DirectXBase**, adiciona suporte a 3D estéreo e declara buffers e recursos constantes para os sombreadores que criam e definem nossas primitivas gráficas.

Aqui está a definição de **GameRenderer**.

```cpp
ref class GameRenderer : public DirectXBase
{
internal:
    GameRenderer();

    virtual void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window,
        float dpi
        ) override;

    virtual void CreateDeviceIndependentResources() override;
    virtual void CreateDeviceResources() override;
    virtual void UpdateForWindowSizeChange() override;
    virtual void Render() override;
    virtual void SetDpi(float dpi) override;

    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    GameInfoOverlay^ InfoOverlay()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f
            );
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f + GameInfoOverlayConstant::Width,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f + GameInfoOverlayConstant::Height
            );
    };

protected private:
    bool                                                m_initialized;
    bool                                                m_gameResourcesLoaded;
    bool                                                m_levelResourcesLoaded;
    GameInfoOverlay^                                    m_gameInfoOverlay;
    GameHud^                                            m_gameHud;
    Simple3DGame^                                       m_game;

    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

Como as APIs do Direct3D 11 são definidas como APIS COM, você deve fornecer referências [**ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983) para os objetos definidos por essas APIs. Esses objetos são liberados automaticamente quando sua última referência fica fora do escopo quando o aplicativo termina.

O exemplo de jogo declara 4 buffers constantes específicos:

-   **m\_constantBufferNeverChanges**. Esse buffer constante contém os parâmetros de iluminação. Ele é definido uma vez e nunca muda novamente.
-   **m\_constantBufferChangeOnResize**. Esse buffer constante contém a matriz de projeção. A matriz de projeção depende do tamanho e da taxa de proporção da janela. Ela será atualizada somente quando o tamanho da janela for modificado.
-   **m\_constantBufferChangesEveryFrame**. Esse buffer constante contém a matriz de visualização. Essa matriz depende da posição da câmera e da direção do olhar (o normal para a projeção) e muda somente uma vez por quadro.
-   **m\_constantBufferChangesEveryPrim**. Esse buffer constante contém a matriz do modelo e as propriedades de material de cada primitiva. A matriz do modelo transforma os vértices das coordenadas locais em coordenadas mundiais. Essas constantes são específicas para cada primitiva e são atualizadas para cada chamada de desenho.

A ideia de vários buffers constantes com diferentes frequências é reduzir a quantidade de dados que deve ser enviada para a GPU por quadro. Portanto, o exemplo separa as constantes em diferentes buffers com base na frequência em que eles devem ser atualizados. Esta é a prática recomendada para programação do Direct3D.

O renderizador contém objetos de sombreador que preenchem nossas primitivas e texturas: **m\_vertexShader** e **m\_pixelShader**. O sombreador de vértice processa as primitivas e a iluminação básica, e o sombreador de pixel (às vezes, chamou um sombreador de fragmento) processa as texturas e quaisquer efeitos por pixel. Existem duas versões desses sombreadores (regulares e planos) para a renderização de diferentes primitivas. As versões simples são muito mais simples e não fazem realces especulares ou qualquer por efeitos de iluminação de pixel. Estes são usados nas paredes e tornam a renderização mais rápida em dispositivos com pouco consumo de energia.

A classe do renderizador contém os recursos de [DirectWrite e Direct2D](https://msdn.microsoft.com/library/windows/desktop/ff729481) usados para a sobreposição e o Painel Transparente (o objeto **GameHud**). A sobreposição e o HUD são adicionados sobre o destino de renderização quando a projeção é concluída no pipeline de gráficos.

O renderizador também define os objetos de recurso do sombreador que contêm as texturas para as primitivas. Algumas dessas texturas são predefinidas (texturas DDS para as paredes e piso do mundo bem como as esferas de munição).

Agora, chegou o momento de ver como esse objeto é criado!

## Inicializando o renderizador


O jogo de exemplo chama este método **Initialize** como parte da sequência de inicialização do CoreApplication no **App::SetWindow**.

```cpp
void GameRenderer::Initialize(
    _In_ CoreWindow^ window,
    float dpi
    )
{
    if (!m_initialized)
    {
        m_gameHud = ref new GameHud(
            "Windows 8 Samples",
            "DirectX first-person game sample"
            );
        m_gameInfoOverlay = ref new GameInfoOverlay();
        m_initialized = true;
    }

    DirectXBase::Initialize(window, dpi);

    // Initialize could be called multiple times as a result of an error with the hardware device
    // that requires it to be reinitialized.  Because the m_gameInfoOverlay variable has resources that are
    // dependent on the device, it will need to be reinitialized each time with the new device information.
    m_gameInfoOverlay->Initialize(m_d2dDevice.Get(), m_d2dContext.Get(), m_dwriteFactory.Get(), dpi);
}
```

É um método bastante simples. Ele verifica se o renderizador foi inicializado anteriormente e, se não tiver sido, inicia os objetos **GameHud** e **GameInfoOverlay**.

Depois disso, o processo de inicialização do renderizador executa a implementação base de **Initialize** fornecida na classe **DirectXBase** de que ele herdou.

Quando a inicialização do DirectXBase é concluída, o objeto **GameInfoOverlay** é inicializado. Tendo concluído a inicialização, chegou a hora de examinarmos os métodos para a criação e carregamento dos recursos gráficos do jogo.

## Criando e carregando recursos gráficos DirectX


A primeira providência em qualquer jogo é estabelecer uma conexão com a interface gráfica, criar os recursos necessários para desenhar os elementos gráficos e configurar um destino de renderização no qual esses elementos gráficos possam ser desenhados. No jogo de exemplo (e no modelo **DirectX 11 App (Universal Windows)** do Microsoft Visual Studio), esse processo é implementado com três métodos:

-   **CreateDeviceIndependentResources**
-   **CreateDeviceResources**
-   **CreateWindowSizeDependentResources**

Agora, no exemplo de jogo, substituímos dois desses métodos (**CreateDeviceIndependentResources** e **CreateDeviceResources**) fornecidos na classe **DirectXBase** implementada no modelo **DirectX 11 (Aplicativo Universal do Windows)**. Para cada um desses métodos de substituição, chamamos primeiro as implementações de **DirectXBase** que eles substituem e, em seguida, adicionamos mais detalhes de implementação específicos do exemplo de jogo. Esteja ciente que a implementação de classe **DirectXBase** incluída no jogo de exemplo foi modificada da versão fornecida no modelo do Visual Studio para incluir o suporte ao modo de exibição estereoscópico e incluir a pré-rotação do objeto **SwapBuffer**.

**CreateWindowSizeDependentResources** não é substituído pelo objeto **GameRenderer**. Usamos a implementação dele fornecida na classe **DirectXBase**.

Para saber mais sobre as implementações base do **DirectXBase** desses métodos, veja [Como configurar o seu aplicativo DirectX UWP para mostrar uma visualização](https://msdn.microsoft.com/library/windows/apps/hh465077).

O primeiro desses métodos substituídos, **CreateDeviceIndependentResources**, chama o método **GameHud::CreateDeviceIndependentResources** para criar os recursos de texto do [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) que usam a fonte Segoe UI, que é a fonte usada na maioria dos aplicativos UWP.

CreateDeviceIndependentResources

```cpp
void GameRenderer::CreateDeviceIndependentResources()
{
    DirectXBase::CreateDeviceIndependentResources();
    m_gameHud->CreateDeviceIndependentResources(m_dwriteFactory.Get(), m_wicFactory.Get());
}
```

```cpp
void GameHud::CreateDeviceIndependentResources(
    _In_ IDWriteFactory* dwriteFactory,
    _In_ IWICImagingFactory* wicFactory
    )
{
    m_dwriteFactory = dwriteFactory;
    m_wicFactory = wicFactory;

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBody
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Symbol",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBodySymbol
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleHeaderPointSize,
            L"en-us",
            &m_textFormatTitleHeader
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleBodyPointSize,
            L"en-us",
            &m_textFormatTitleBody
            )
        );

    DX::ThrowIfFailed(m_textFormatBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
}
```

O exemplo usa quatro formatadores de texto: dois para o cabeçalho do título e o corpo de texto do título, e dois para o corpo de texto. Isso é usado em boa parte do texto da sobreposição.

O segundo método, **CreateDeviceResources**, carrega os recursos específicos para o jogo que serão calculados no dispositivo gráfico. Examinaremos agora o código para esse método.

CreateDeviceResources

```cpp
oid GameRenderer::CreateDeviceResources()
{
    DirectXBase::CreateDeviceResources();

    m_gameHud->CreateDeviceResources(m_d2dContext.Get());

    if (m_game != nullptr)
    {
        // The initial invocation of CreateDeviceResources occurs
        // before the Game State is initialized when the device is first
        // being created, so that the inital loading screen can be displayed.
        // Subsequent invocations of CreateDeviceResources will be a result
        // of an error with the device that requires the resources to be
        // recreated.  In this case, the game state is already initialized
        // so the game device resources need to be recreated.

        // This sample doesn't gracefully handle all the async recreation
        // of resources so an exception is thrown.
        throw Platform::Exception::CreateException(
            DXGI_ERROR_DEVICE_REMOVED,
            "GameRenderer::CreateDeviceResources - Recreation of resources after TDR not available\n"
            );
    }
}
```

```cpp
void GameHud::CreateDeviceResources(_In_ ID2D1DeviceContext* d2dContext)
{
    auto location = Package::Current->InstalledLocation;
    Platform::String^ path = Platform::String::Concat(location->Path, "\\");
    path = Platform::String::Concat(path, "windows-sdk.png");

    ComPtr<IWICBitmapDecoder> wicBitmapDecoder;
    DX::ThrowIfFailed(
        m_wicFactory->CreateDecoderFromFilename(
            path->Data(),
            nullptr,
            GENERIC_READ,
            WICDecodeMetadataCacheOnDemand,
            &wicBitmapDecoder
            )
        );

    ComPtr<IWICBitmapFrameDecode> wicBitmapFrame;
    DX::ThrowIfFailed(
        wicBitmapDecoder->GetFrame(0, &wicBitmapFrame)
        );

    ComPtr<IWICFormatConverter> wicFormatConverter;
    DX::ThrowIfFailed(
        m_wicFactory->CreateFormatConverter(&wicFormatConverter)
        );

    DX::ThrowIfFailed(
        wicFormatConverter->Initialize(
            wicBitmapFrame.Get(),
            GUID_WICPixelFormat32bppPBGRA,
            WICBitmapDitherTypeNone,
            nullptr,
            0.0,
            WICBitmapPaletteTypeCustom  // The BGRA format has no palette so this value is ignored.
            )
        );

    double dpiX = 96.0f;
    double dpiY = 96.0f;
    DX::ThrowIfFailed(
        wicFormatConverter->GetResolution(&dpiX, &dpiY)
        );

    // Create D2D Resources.
    DX::ThrowIfFailed(
        d2dContext->CreateBitmapFromWicBitmap(
            wicFormatConverter.Get(),
            BitmapProperties(
                PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
                static_cast<float>(dpiX),
                static_cast<float>(dpiY)
                ),
            &m_logoBitmap
            )
        );

    m_logoSize = m_logoBitmap->GetSize();

    DX::ThrowIfFailed(
        d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White),
            &m_textBrush
            )
        );
}
```

Neste exemplo, na execução normal, o método **CreateDeviceResources** chama o método de classe base e chama o método **GameHud::CreateDeviceResources** (também listado anteriormente). Se há um problema mais tarde com o dispositivo de gráficos subjacente, ele terá que ser reinicializado. Nesse caso, o método **CreateDeviceResources** inicia um conjunto de tarefas assíncronas para criar os recursos de dispositivo do jogo. Isso é feito por meio de uma sequência de dois métodos: uma chamada para o **CreateDeviceResourcesAsync** e, quando for concluída, **FinalizeCreateGameDeviceResources**.

CreateGameDeviceResourcesAsync and FinalizeCreateGameDeviceResources

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Set the Loading state to wait until any async resources have
    // been loaded before proceeding.
    m_game = game;

    // NOTE: Only the m_d3dDevice is used in this method.  It's expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the m_d3dDevice are free-threaded and are safe while any methods
    // in the m_d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.

    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    bd.CPUAccessFlags = 0;
    bd.ByteWidth = (sizeof(ConstantBufferNeverChanges) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangeOnResize) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangeOnResize)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryFrame) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryFrame)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryPrim) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryPrim)
        );

    D3D11_SAMPLER_DESC sampDesc;
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;
    sampDesc.MinLOD = 0;
    sampDesc.MaxLOD = FLT_MAX;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    m_cylinderTexture = nullptr;
    m_ceilingTexture = nullptr;
    m_floorTexture = nullptr;
    m_wallsTexture = nullptr;

    // Load Game specific Textures.
    tasks.push_back(loader->LoadTextureAsync("seafloor.dds", nullptr, &m_sphereTexture));
    tasks.push_back(loader->LoadTextureAsync("metal_texture.dds", nullptr, &m_cylinderTexture));
    tasks.push_back(loader->LoadTextureAsync("cellceiling.dds", nullptr, &m_ceilingTexture));
    tasks.push_back(loader->LoadTextureAsync("cellfloor.dds", nullptr, &m_floorTexture));
    tasks.push_back(loader->LoadTextureAsync("cellwall.dds", nullptr, &m_wallsTexture));
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Return the task group of all the async tasks for loading the shader and texture assets.
    return when_all(tasks.begin(), tasks.end());
}
```

```cpp
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now, associate all the resources with the appropriate
    // Game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[1] = XMFLOAT4( 3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[2] = XMFLOAT4(-3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[3] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);
    m_d3dContext->UpdateSubresource(m_constantBufferNeverChanges.Get(), 0, nullptr, &constantBufferNeverChanges, 0, 0);

    // For the targets, there are two unique generated textures.
    // Each texture image includes the number of the texture.
    // Make sure the 2-D rendering is occurring on the same thread
    // as the main rendering.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        m_d3dDevice.Get(),
        m_d2dFactory.Get(),
        m_dwriteFactory.Get(),
        m_d2dContext.Get()
        );

    MeshObject^ cylinderMesh = ref new CylinderMesh(m_d3dDevice.Get(), 26);
    MeshObject^ targetMesh = ref new FaceMesh(m_d3dDevice.Get());
    MeshObject^ sphereMesh = ref new SphereMesh(m_d3dDevice.Get(), 26);

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    Material^ sphereMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        50.0f,
        m_sphereTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldFloorMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldCeilingId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_ceilingTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldCeilingMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldWallsId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_wallsTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldWallsMesh(m_d3dDevice.Get()));
        }
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        else if (Sphere^ sphere = dynamic_cast<Sphere^>(*object))
        {
            sphere->Mesh(sphereMesh);
            sphere->NormalMaterial(sphereMaterial);
        }
    }

    // Ensure that the camera has been initialized with the right Projection
    // matrix.  The camera is not created at the time the first window resize event
    // occurs.
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that Projection matrix has been set in the constant buffer
    // now that all the resources are loaded.
    // DirectXBase is handling screen rotations directly to eliminate an unaligned
    // fullscreen copy.  As a result, it's necessary to post multiply the rotationTransform3D
    // matrix to the camera projection matrix.
    ConstantBufferChangeOnResize changesOnResize;
    XMStoreFloat4x4(
        &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
        );

    m_d3dContext->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    m_gameResourcesLoaded = true;
}
```

**CreateDeviceResourcesAsync** é um método executado como um conjunto separado de tarefas assíncronas para carregar os recursos do jogo. Como se espera que ele seja executado em um thread separado, ele só tem acesso aos métodos de dispositivo do Direct3D 11 (aqueles definidos em [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)) e não os métodos de contexto de dispositivo (os métodos definidos em [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)), assim, ele tem a opção de não fazer renderização. O método **FinalizeCreateGameDeviceResources** é executado no thread principal e não tem acesso aos métodos de contexto de dispositivo do Direct3D 11.

A sequência de eventos para carregar os recursos de dispositivos de jogo funciona da seguinte forma:

**CreateDeviceResourcesAsync** primeiro inicializa buffers constantes para as primitivas. Buffers constantes são buffers de comprimento fixo com baixa latência que contêm os dados usados pelo sombreador durante sua execução. (Pense nesses buffers como passadores de dados para o sombreador que permanecem constantes durante a execução de uma determinada chamada de desenho). Neste exemplo, os buffers contêm os dados que os sombreadores usarão para:

-   Colocar as fontes de iluminação e definir sua cor quando o renderizador inicializar
-   Calcular a matriz de visualização sempre que a janela for redimensionada
-   Calcular a matriz de projeção para cada atualização de quadro
-   Calcular as transformações das primitivas em cada atualização de renderização

As constantes recebem as informações de origem (vértices) e transformam as coordenadas de vértices e dados do espaço do modelo para o espaço do dispositivo. No final do processo, esses dados resultam em coordenadas de texel e em pixels no destino de renderização.

Em seguida, o objeto renderizador do jogo cria um carregador para os sombreadores que executarão o cálculo. (Consulte a implementação específica em **BasicLoader.cpp** no exemplo.)

Em seguida, **CreateDeviceResourcesAsync** inicia tarefas assíncronas para carregar todos os recursos de textura em **ShaderResourceViews**. Esses recursos de textura são armazenados em texturas de superfícies DDS (DirectDraw Surface) fornecidas com o exemplo. As texturas DDS estão em um formato de textura com perdas que opera com DXTC (DirectX Texture Compression). Usamos essas texturas nas paredes, no teto e no piso do mundo, nas esferas de munição e nos obstáculos em forma de pilar.

Finalmente, ele retorna um grupo de tarefas que contém todas as tarefas assíncronas criadas pelo método. A função chamada aguarda a conclusão de todas essas tarefas assíncronas e chama o **FinalizeCreateGameDeviceResources**.

**FinalizeCreateGameDeviceResources** carrega os dados iniciais em buffers de constante com uma chamada de método de contexto de dispositivo para o [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486): `m_deviceContext->UpdateSubresource`. Esse método cria os objetos de malha para a esfera, o cilindro, a face e os objetos de jogo do mundo e os materiais associados. Ele percorre a lista de objetos do jogo que associa os recursos do dispositivo apropriados a cada objeto.

As texturas para os objetos de alvo anelados e numerados são geradas por procedimento usando o código em **TargetTexture.cpp**. O renderizador cria uma instância do tipo **TargetTexture**, que cria a textura de bitmap para os objetos de alvo no jogo quando chamamos o método **TargetTexture::CreateTextureResourceView**. A textura resultante consiste em anéis concêntricos coloridos com um valor numérico na parte superior. Esses recursos gerados são associados aos objetos de jogo de destino apropriados.

Por último, o **FinalizeCreateGameDeviceResources** define a variável global Booliana do `m_gameResourcesLoaded` para indicar que todos os recursos foram carregados.

O jogo tem os recursos para exibir os elementos gráficos na janela atual e pode recriar esses recursos conforme a janela mude. Agora examinaremos a câmera usada para definir a visão da cena pelo jogador nessa janela.

## Implementando o objeto de câmera


O jogo já dispõe do código necessário para atualizar o mundo em seu próprio sistema de coordenadas (também conhecido como espaço do mundo ou espaço da cena). Todos os objetos, inclusive a câmera, são posicionados e orientados nesse espaço. No exemplo de jogo, a posição da câmera, juntamente com os vetores de visão (o vetor "olhar para" que aponta diretamente para a cena a partir da câmera e o vetor "olhar para cima" que sobe perpendicularmente dele), definem o espaço da câmera. Os parâmetros de projeção determinam quanto do espaço será efetivamente visível na cena final; o campo de visão (FoV), a taxa de proporção e os planos de recorte definem a transformação da projeção. Um shader de vértice executa o trabalho pesado de conversão das coordenadas do modelo para os coordenadas do dispositivo por meio do seguinte algoritmo (onde V é um vetor e M uma matriz):

`              V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)           `.

-   `M(model-to-world)` é uma matriz de transformação de coordenadas do modelo para coordenadas do mundo. Isso é fornecido pela primitiva. (Examinaremos isso aqui, na seção sobre primitivas.)
-   `M(world-to-view)` é uma matriz de transformação de coordenadas do mundo para coordenadas de visualização. Isso é fornecido pela matriz de visualização da câmera.
-   `M(view-to-device)` é uma matriz de transformação de coordenadas de visualização para coordenadas do dispositivo. Isso é fornecido pela projeção da câmera.

O código de shader em **VertexShader.hlsl** é carregado com esses vetores e matrizes dos buffers constantes e executa essa transformação para cada vértice.

O objeto **Camera** define as matrizes de visualização e projeção. Agora veremos como isso é declarado pelo exemplo de jogo.

```cpp
ref class Camera
{
internal:
    Camera();

    // Call these from client and use Get*Matrix() to read new matrices.
    // Functions to change camera matrices
    void SetViewParams(_In_ DirectX::XMFLOAT3 eye, _In_ DirectX::XMFLOAT3 lookAt, _In_ DirectX::XMFLOAT3 up);
    void SetProjParams(_In_ float fieldOfView, _In_ float aspectRatio, _In_ float nearPlane, _In_ float farPlane);

    void LookDirection (_In_ DirectX::XMFLOAT3 lookDirection);
    void Eye (_In_ DirectX::XMFLOAT3 position);

    DirectX::XMMATRIX View();
    DirectX::XMMATRIX Projection();
    DirectX::XMMATRIX LeftEyeProjection();
    DirectX::XMMATRIX RightEyeProjection();
    DirectX::XMMATRIX World();
    DirectX::XMFLOAT3 Eye();
    DirectX::XMFLOAT3 LookAt();
    DirectX::XMFLOAT3 Up();
    float NearClipPlane();
    float FarClipPlane();
    float Pitch();
    float Yaw();

protected private:
    DirectX::XMFLOAT4X4 m_viewMatrix;                 // View matrix
    DirectX::XMFLOAT4X4 m_projectionMatrix;           // Projection matrix
    DirectX::XMFLOAT4X4 m_projectionMatrixLeft;       // Projection Matrix for Left Eye Stereo
    DirectX::XMFLOAT4X4 m_projectionMatrixRight;      // Projection Matrix for Right Eye Stereo

    DirectX::XMFLOAT4X4 m_inverseView;

    DirectX::XMFLOAT3 m_eye;                        // Camera eye position
    DirectX::XMFLOAT3 m_lookAt;                     // LookAt position
    DirectX::XMFLOAT3 m_up;                         // Up vector
    float             m_cameraYawAngle;             // Yaw angle of camera
    float             m_cameraPitchAngle;           // Pitch angle of camera

    float             m_fieldOfView;                // Field of view
    float             m_aspectRatio;                // Aspect ratio
    float             m_nearPlane;                  // Near plane
    float             m_farPlane;                   // Far plane
};
```

Há duas matrizes 4x4 que definem as transformações das coordenadas de visualização e projeção, **m\_viewMatrix** e **m\_projectionMatrix**. (Para projeção estéreo, você usa duas matrizes de projeção: um para cada olho.) Elas são calculadas com estes dois métodos, respectivamente:

-   **SetViewParams**
-   **SetProjParams**

O código para esses dois métodos tem a seguinte aparência:

```cpp
void Camera::SetViewParams(
    _In_ XMFLOAT3 eye,
    _In_ XMFLOAT3 lookAt,
    _In_ XMFLOAT3 up
    )
{
    m_eye = eye;
    m_lookAt = lookAt;
    m_up = up;

    // Calc the view matrix.
    XMMATRIX view = XMMatrixLookAtLH(
        XMLoadFloat3(&m_eye),
        XMLoadFloat3(&m_lookAt),
        XMLoadFloat3(&m_up)
        );

    XMVECTOR det;
    XMMATRIX inverseView = XMMatrixInverse(&det, view);
    XMStoreFloat4x4(&m_viewMatrix, view);
    XMStoreFloat4x4(&m_inverseView, inverseView);

    // The axis basis vectors and camera position are stored inside the
    // position matrix in the 4 rows of the camera's world matrix.
    // To figure out the yaw/pitch of the camera, we just need the Z basis vector
    XMFLOAT3* zBasis = (XMFLOAT3*)&inverseView._31;

    m_cameraYawAngle = atan2f(zBasis->x, zBasis->z);

    float len = sqrtf(zBasis->z * zBasis->z + zBasis->x * zBasis->x);
    m_cameraPitchAngle = atan2f(zBasis->y, len);
}
//--------------------------------------------------------------------------------------
// Calculates the projection matrix based on input params
//--------------------------------------------------------------------------------------
void Camera::SetProjParams(
    _In_ float fieldOfView,
    _In_ float aspectRatio,
    _In_ float nearPlane,
    _In_ float farPlane
    )
{
    // Set attributes for the projection matrix
    m_fieldOfView = fieldOfView;
    m_aspectRatio = aspectRatio;
    m_nearPlane = nearPlane;
    m_farPlane = farPlane;
    XMStoreFloat4x4(
        &m_projectionMatrix,
        XMMatrixPerspectiveFovLH(
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane
            )
        );

    STEREO_PARAMETERS* stereoParams = nullptr;
    // change the projection matrix
    XMStoreFloat4x4(
        &m_projectionMatrixLeft,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::LEFT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );

    XMStoreFloat4x4(
        &m_projectionMatrixRight,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::RIGHT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );
}
```

Obtemos a visualização e os dados de projeção resultantes chamando os métodos **View** e **Projection**, respectivamente, no objeto **Camera**. Essas chamadas ocorrem na próxima etapa que será examinada, o método **GameRenderer::Render** chamado no loop do jogo.

Agora veremos como o jogo cria a estrutura para desenhar os elementos gráficos do jogo usando a câmera. Isso inclui a definição das primitivas que formam o mundo do jogo e seus elementos.

## Definindo as primitivas


No código de exemplo do jogo, definimos e implementamos as primitivas em duas classes base e as especializações correspondentes para cada tipo de primitiva.

**MeshObject.h/.cpp** define a classe base para todos os objetos de malha. Os arquivos **SphereMesh.h/.cpp**, **CylinderMesh.h/.cpp**, **FaceMesh.h/.cpp** e **WorldMesh.h/.cpp** contêm o código que preenche os buffers constantes para cada primitiva com o vértice e os dados normais de vértice que define a geometria da primitiva. Esses arquivos de código são um bom lugar para começar se você estiver tentando entender como criar primitivas Direct3D em seu próprio aplicativo de jogo, mas não será discutido aqui porque eles são específicos demais para a implementação deste jogo. Por ora, presumiremos que os buffers de vértice para cada primitiva já foram preenchidos e examinaremos como o exemplo de jogo manipula esses buffers para atualizar o próprio jogo.

A classe base para objetos que representam as primitivas da perspectiva do jogo é definida em **GameObject.h./.cpp.** Essa classe, **GameObject**, define os campos e métodos para os comportamentos comuns em todas as primitivas. Cada tipo de objeto de primitiva é derivado dele. Agora veremos como isso é definido:

```cpp
ref class GameObject
{
internal:
    GameObject();

    // Expect these two functions to be overloaded by subclasses.
    virtual bool IsTouching(
        DirectX::XMFLOAT3 /* point */,
        float /* radius */,
        _Out_ DirectX::XMFLOAT3 *contact,
        _Out_ DirectX::XMFLOAT3 *normal
        )
    {
        *contact = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);
        *normal = DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f);
        return false;
    };

    void Render(
        _In_ ID3D11DeviceContext *context,
        _In_ ID3D11Buffer *primitiveConstantBuffer
        );

    void Active(bool active);
    bool Active();
    void Target(bool target);
    bool Target();
    void Hit(bool hit);
    bool Hit();
    void OnGround(bool ground);
    bool OnGround();
    void TargetId(int targetId);
    int  TargetId();
    void HitTime(float t);
    float HitTime();

    void     AnimatePosition(_In_opt_ Animate^ animate);
    Animate^ AnimatePosition();

    void         HitSound(_In_ SoundEffect^ hitSound);
    SoundEffect^ HitSound();

    void PlaySound(float impactSpeed, DirectX::XMFLOAT3 eyePoint);

    void Mesh(_In_ MeshObject^ mesh);

    void NormalMaterial(_In_ Material^ material);
    Material^ NormalMaterial();
    void HitMaterial(_In_ Material^ material);
    Material^ HitMaterial();

    void Position(DirectX::XMFLOAT3 position);
    void Position(DirectX::XMVECTOR position);
    void Velocity(DirectX::XMFLOAT3 velocity);
    void Velocity(DirectX::XMVECTOR velocity);
    DirectX::XMMATRIX ModelMatrix();
    DirectX::XMFLOAT3 Position();
    DirectX::XMVECTOR VectorPosition();
    DirectX::XMVECTOR VectorVelocity();
    DirectX::XMFLOAT3 Velocity();

protected private:
    virtual void UpdatePosition() {};
    // Object Data
    bool                m_active;
    bool                m_target;
    int                 m_targetId;
    bool                m_hit;
    bool                m_ground;

    DirectX::XMFLOAT3   m_position;
    DirectX::XMFLOAT3   m_velocity;
    DirectX::XMFLOAT4X4 m_modelMatrix;

    Material^           m_normalMaterial;
    Material^           m_hitMaterial;

    DirectX::XMFLOAT3   m_defaultXAxis;
    DirectX::XMFLOAT3   m_defaultYAxis;
    DirectX::XMFLOAT3   m_defaultZAxis;

    float               m_hitTime;

    Animate^            m_animatePosition;
    MeshObject^         m_mesh;

    SoundEffect^        m_hitSound;
};
```

A maioria dos campos contém dados sobre o estado, as propriedades visuais ou a posição da primitiva no mundo do jogo. Alguns métodos em particular são necessários na maioria dos jogos:

-   **Mesh**. Obtém a geometria de malha da primitiva, que é armazenada em **m\_mesh**. Essa geometria é definida em **MeshObject.h/.cpp**.
-   **IsTouching**. Esse método determina se a primitiva está dentro de uma distância específica de um ponto, retornando o ponto em sua superfície mais próximo desse ponto e o normal da superfície do objeto nesse ponto. Como o exemplo só se ocupa de colisões de primitivas de munição, isso é suficiente para a dinâmica do jogo. Não é uma função de interseção primitiva-primitiva de uso geral, embora possa ser usada como base para uma.
-   **AnimatePosition**. Atualiza o movimento e a animação da primitiva.
-   **UpdatePosition**. Atualiza a posição do objeto no espaço de coordenadas do mundo.
-   **Render**. Coloca as propriedades de material da primitiva no buffer constante de primitiva e, em seguida, renderiza (desenha) a geometria da primitiva usando o contexto de dispositivo.

É uma boa prática criar um tipo de objeto base que defina o conjunto mínimo de métodos para uma primitiva, porque a maioria dos jogos tem um grande número de primitivas e o código pode se tornar rapidamente difícil de gerenciar. Isso também simplifica o código do jogo ao permitir que o loop de atualização trate as primitivas de maneira polimórfica, deixando que os próprios objetos definam seus comportamentos de atualização e renderização.

Agora examinaremos a renderização básica de uma primitiva no exemplo de jogo.

## Renderizando as primitivas


As primitivas no exemplo de jogo usam o método base **Render** implementado na classe pai **GameObject**, como aqui:

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    m_mesh->Render(context);
}
```

O método **GameObject::Render** atualiza o buffer constante de primitiva com os dados específicos de uma determinada primitiva. O jogo usa vários buffers constantes, mas só precisa atualizar esses buffers uma vez para cada primitiva.

Pense nos buffers constantes como uma entrada de dados para os shaders que são executados para cada primitiva. Alguns dados são estáticos (**m\_constantBufferNeverChanges**); alguns são constantes no quadro (**m\_constantBufferChangesEveryFrame)**, como a posição da câmera; e alguns são específicos da primitiva, como sua cor e suas texturas (**m\_constantBufferChangesEveryPrim**). O renderizador do jogo separa essas entradas em diferentes buffers constantes para otimizar a largura de banda de memória usada pela CPU e pela GPU. Essa abordagem também ajuda a minimizar a quantidade de dados que a GPU precisa controlar. Lembre-se que a GPU possui uma grande fila de comandos, e sempre que o jogo chama **Draw**, esse comando é inserido na fila com os respectivos dados. Quando o jogo atualiza o buffer de constantes de primitiva e emite o próximo comando **Draw**, o driver gráfico adiciona esse comando e os dados associados à fila. Se o jogo desenha 100 primitivas, pode haver 100 cópias dos dados do buffer constante na fila. Para minimizar a quantidade de dados enviados à GPU, o jogo usa um buffer de constantes de primitiva em separado contendo apenas as atualizações para cada primitiva.

Quando uma colisão (um acerto) é detectada, **GameObject::Render** verifica o contexto atual, que indica se o alvo foi atingido por uma esfera de munição. Se o alvo foi acertado, esse método aplica um material de acerto que inverte as cores dos anéis do alvo para indicar um tiro certeiro ao jogador. Caso contrário, ele aplica o material padrão com o mesmo método. Nos dois casos, ele define o material chamando **Material::RenderSetup**, que define as constantes apropriadas no buffer de constante. Em seguida, ele chama [**ID3D11DeviceContext::PSSetShaderResources**](https://msdn.microsoft.com/library/windows/desktop/ff476473) para definir o recurso de textura correspondente do sombreador de pixel, e [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) e [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) para definir os objetos de sombreador de vértice e de sombreador de pixel, respectivamente.

É assim que o **Material::RenderSetup** configura os buffers de constante e atribui os recursos do sombreador. Novamente, observe que o buffer de constante é o usado especificamente para atualizar as alterações em primitivas.

> **Observação**   A classe **Material** está definida em **Material.h/.cpp**.

 

```cpp
void Material::RenderSetup(
    _In_ ID3D11DeviceContext* context,
    _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
    )
{
    constantBuffer->meshColor = m_meshColor;
    constantBuffer->specularColor = m_specularColor;
    constantBuffer->specularPower = m_specularExponent;
    constantBuffer->diffuseColor = m_diffuseColor;

    context->PSSetShaderResources(0, 1, m_textureRV.GetAddressOf());
    context->VSSetShader(m_vertexShader.Get(), nullptr, 0);
    context->PSSetShader(m_pixelShader.Get(), nullptr, 0);
}
```

Por fim, o **PrimObject::Render** chama o método **Render** do método **MeshObject** subjacente.

```cpp
void MeshObject::Render(_In_ ID3D11DeviceContext *context)
{
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

Agora, o método **MeshObject::Render** do exemplo de jogo adiciona o comando de desenho à fila para executar os sombreadores na GPU usando o estado atual da cena. O sombreador de vértice converte a geometria (os vértices) das coordenadas do modelo para as coordenadas do dispositivo (mundo), levando em conta a posição da câmera e a transformação da perspectiva. Finalmente, os sombreadores de pixel renderizam os triângulos transformados no buffer de fundo usando a textura definida acima.

Isso ocorre no processo de renderização real!

## Criando os shaders de vértice e de pixel


Neste ponto, o exemplo de jogo definiu as primitivas para desenhar e os buffers constantes que definem sua renderização. Esses buffers constantes servem como os conjuntos de parâmetros para os sombreadores que são executados no dispositivo gráfico. Esses programas de sombreador são de dois tipos:

-   Os sombreadores de vértice executam operações por vértice, como transformações de vértice e iluminação.
-   Os sombreadores de pixel (ou fragmento) executam as operações por pixel, como texturização e iluminação por pixel. Eles também podem ser usados para executar efeitos de pós-processamento em bitmaps, como o destino de renderização final.

O código do sombreador é definido usando a linguagem HLSL (High-Level Shader Language), que, no Direct3D 11, é compilada a partir de um programa criado com uma sintaxe semelhante à da linguagem C. (A sintaxe completa pode ser encontrada [aqui](https://msdn.microsoft.com/library/windows/desktop/bb509635).) Os dois sombreadores principais do jogo de exemplo são definidos em **PixelShader.hlsl** e **VertexShader.hlsl**. (Há também dois sombreadores de "baixa potência" definidos para os dispositivos de baixa potência: **PixelShaderFlat.hlsl** e **VertexShaderFlat.hlsl**. Esses dois sombreadores fornecem efeitos reduzidos, como a falta de realces especulares em superfícies de texturas). Finalmente, há um arquivo .hlsli que contém o formato dos buffers de constante, **ConstantBuffers.hlsl**.

**ConstantBuffers.hlsli** é definido da seguinte maneira:

```cpp
Texture2D diffuseTexture : register(t0);
SamplerState linearSampler : register(s0);

cbuffer ConstantBufferNeverChanges : register(b0)
{
    float4 lightPosition[4];
    float4 lightColor;
}

cbuffer ConstantBufferChangeOnResize : register(b1)
{
    matrix projection;
};

cbuffer ConstantBufferChangesEveryFrame : register(b2)
{
    matrix view;
};

cbuffer ConstantBufferChangesEveryPrim : register (b3)
{
    matrix world;
    float4 meshColor;
    float4 diffuseColor;
    float4 specularColor;
    float  specularExponent;
};

struct VertextShaderInput
{
    float4 position : POSITION;
    float4 normal : NORMAL;
    float2 textureUV : TEXCOORD0;
};

struct PixelShaderInput
{
    float4 position : SV_POSITION;
    float2 textureUV : TEXCOORD0;
    float3 vertexToEye : TEXCOORD1;
    float3 normal : TEXCOORD2;
    float3 vertexToLight0 : TEXCOORD3;
    float3 vertexToLight1 : TEXCOORD4;
    float3 vertexToLight2 : TEXCOORD5;
    float3 vertexToLight3 : TEXCOORD6;
};

struct PixelShaderFlatInput
{
    float4 position : SV_POSITION;
    float2 textureUV : TEXCOORD0;
    float4 diffuseColor : TEXCOORD1;
};
```

**VertexShader.hlsl** é definido da seguinte maneira:

VertexShader.hlsl

```cpp
#include "ConstantBuffers.hlsli"

PixelShaderInput main(VertextShaderInput input)
{
    PixelShaderInput output = (PixelShaderInput)0;

    output.position = mul(mul(mul(input.position, world), view), projection);
    output.textureUV = input.textureUV;

    // compute view space normal
    output.normal = normalize (mul(mul(input.normal.xyz, (float3x3)world), (float3x3)view));

    // Vertex pos in view space (normalize in pixel shader)
    output.vertexToEye = -mul(mul(input.position, world), view).xyz;

    // Compute view space vertex to light vectors (normalized)
    output.vertexToLight0 = normalize(mul(lightPosition[0], view ).xyz + output.vertexToEye);
    output.vertexToLight1 = normalize(mul(lightPosition[1], view ).xyz + output.vertexToEye);
    output.vertexToLight2 = normalize(mul(lightPosition[2], view ).xyz + output.vertexToEye);
    output.vertexToLight3 = normalize(mul(lightPosition[3], view ).xyz + output.vertexToEye);

    return output;
}
```

A função **main** em **VertexShader.hlsl** executa a sequência de transformação de vértice que discutimos na seção sobre a câmera. Ela é executada uma vez por vértice. As saídas resultantes são passadas ao código do sombreador de pixel para texturização e efeitos de material.

PixelShader.hlsl

```cpp
#include "ConstantBuffers.hlsli"

float4 main(PixelShaderInput input) : SV_Target
{
    float diffuseLuminance =
        max(0.0f, dot(input.normal, input.vertexToLight0)) +
        max(0.0f, dot(input.normal, input.vertexToLight1)) +
        max(0.0f, dot(input.normal, input.vertexToLight2)) +
        max(0.0f, dot(input.normal, input.vertexToLight3));

    // Normalize view space vertex-to-eye
    input.vertexToEye = normalize(input.vertexToEye);

    float specularLuminance = 
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight0))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight1))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight2))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight3))), specularExponent);

    float4 specular;
    specular = specularColor * specularLuminance * 0.5f;

   return diffuseTexture.Sample(linearSampler, input.textureUV) *  diffuseColor * diffuseLuminance * 0.5f + specular;
}
```

A função **main** em **PixelShader.hlsl** recebe as projeções 2D das superfícies triangulares de cada primitiva na cena e calcula o valor de cor para cada pixel nas superfícies visíveis com base nas texturas e efeitos (no caso, iluminação especular) aplicados a elas.

Agora juntaremos todos esses conceitos (primitivas, câmera e shaders) e veremos como o exemplo de jogo constrói o processo de renderização completo.

## Renderizando o quadro para saída


Esse método foi abordado sucintamente em [Definindo o objeto principal do jogo](tutorial--defining-the-main-game-loop.md). Agora nós o examinaremos com mais detalhes.

```cpp
void GameRenderer::Render()
{
    int renderingPasses = 1;
    if (m_stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        if (m_stereoEnabled && i > 0)
        {
            // Doing the Right Eye View
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetViewRight.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmapRight.Get());
        }
        else
        {
            // Doing the Mono or Left Eye View
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetView.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
        }

        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (m_stereoEnabled)
            {
                ConstantBufferChangeOnResize changesOnResize;
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                        )
                    );

                m_d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }
            // Update variables that change one time per frame

            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            m_d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup Pipeline

            m_d3dContext->IASetInputLayout(m_vertexLayout.Get());
            m_d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            m_d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            m_d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            // Get the all the objects to render from the Game state.
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(m_d3dContext.Get(), m_constantBufferChangesEveryPrim.Get());
            }
        }
        else
        {
            const float ClearColor[4] = {0.1f, 0.1f, 0.1f, 1.0f};

            // Only need to clear the background when not rendering the full 3-D scene because
            // the 3-D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (m_stereoEnabled && i > 0)
            {
                // Doing the Right Eye View
                m_d3dContext->ClearRenderTargetView(m_renderTargetViewRight.Get(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View
                m_d3dContext->ClearRenderTargetView(m_renderTargetView.Get(), ClearColor);
            }
        }

        m_d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        m_d2dContext->SetTransform(m_rotationTransform2D);

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game, m_d2dContext.Get(), m_windowBounds);
        }

        if (m_gameInfoOverlay->Visible())
        {
            m_d2dContext->DrawBitmap(
                m_gameInfoOverlay->Bitmap(),
                D2D1::RectF(
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f,
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f + GameInfoOverlayConstant::Width,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f + GameInfoOverlayConstant::Height
                    )
                );
        }

        HRESULT hr = m_d2dContext->EndDraw();
        if (hr != D2DERR_RECREATE_TARGET)
        {
            // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
            // D3D device.  All subsequent rendering will be ignored until the device is recreated.
            // This error will be propagated and the appropriate D3D error will be returned from the
            // swapchain->Present(...) call.   At that point, the sample will recreate the device
            // and all associated resources.  As a result the D2DERR_RECREATE_TARGET doesn't
            // need to be handled here.
            DX::ThrowIfFailed(hr);
        }
    }
    Present();
}
```

O jogo já dispõe de todas as peças para montar uma visualização de saída: primitivas e regras para o seu comportamento, um objeto de câmera para fornecer a visualização do mundo do jogo pelo jogador e os recursos gráficos para desenhá-lo.

Agora examinaremos o processo que junta tudo isso.

1.  Se 3D estéreo estiver habilitado, defina o processo de renderização a seguir para ser executado duas vezes, uma para cada olho.
2.  Toda a cena está contida em um volume envolvente do mundo; portanto, desenhe cada pixel (mesmo aqueles que não precisamos) para limpar os planos de cor do destino de renderização. Defina o buffer de estêncil de profundidade como o valor padrão.
3.  Atualize o buffer constante de dados de atualização do quadro usando a matriz de visualização e os dados da câmera.
4.  Configure o contexto Direct3D para usar os quatro buffers de conteúdo definidos anteriormente.
5.  Chame o método **Render** em cada objeto de primitiva. Isso resulta em uma chamada de **Draw** ou **DrawIndexed** no contexto para desenhar a geometria dessa primitiva. Especificamente, essa chamada de **Draw** enfileira comandos e dados para a GPU, conforme parametrizados pelos dados do buffer constante. Cada chamada de desenho executa o sombreador de vértice uma vez por vértice e, em seguida, o sombreador de pixel uma vez para cada pixel de cada triângulo da primitiva. As texturas fazem parte do estado que o sombreador de pixel usa para executar a renderização.
6.  Desenhar o HUD e a sobreposição usando o contexto Direct2D.
7.  Chame **DirectXBase::Present**.

Com isso, o jogo acaba de atualizar a tela! Tomado em conjunto, esse é o processo básico de implementação da estrutura gráfica de um jogo. Evidentemente, quando maior for o jogo, mais abstrações terão que ser empregadas para lidar com essa complexidade, tais como hierarquias inteiras de tipos de objeto e comportamentos de animação, e mais complexos serão os métodos para carregar e gerenciar ativos, como malhas e texturas.

## Próximas etapas


Seguindo em frente, examinaremos agora algumas partes importantes do exemplo de jogo que só foram abordadas de passagem: [a sobreposição da interface do usuário](tutorial--adding-a-user-interface.md), [os controles de entrada](tutorial--adding-controls.md) e [o som](tutorial--adding-sound.md).

## Código de exemplo completo desta seção


Camera.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class Camera
{
internal:
    Camera();

    // Call these from client and use Get*Matrix() to read new matrices.
    // Functions to change camera matrices.
    void SetViewParams(_In_ DirectX::XMFLOAT3 eye, _In_ DirectX::XMFLOAT3 lookAt, _In_ DirectX::XMFLOAT3 up);
    void SetProjParams(_In_ float fieldOfView, _In_ float aspectRatio, _In_ float nearPlane, _In_ float farPlane);

    void LookDirection (_In_ DirectX::XMFLOAT3 lookDirection);
    void Eye (_In_ DirectX::XMFLOAT3 position);

    DirectX::XMMATRIX View();
    DirectX::XMMATRIX Projection();
    DirectX::XMMATRIX LeftEyeProjection();
    DirectX::XMMATRIX RightEyeProjection();
    DirectX::XMMATRIX World();
    DirectX::XMFLOAT3 Eye();
    DirectX::XMFLOAT3 LookAt();
    DirectX::XMFLOAT3 Up();
    float NearClipPlane();
    float FarClipPlane();
    float Pitch();
    float Yaw();

protected private:
    DirectX::XMFLOAT4X4 m_viewMatrix;                 // View matrix
    DirectX::XMFLOAT4X4 m_projectionMatrix;           // Projection matrix
    DirectX::XMFLOAT4X4 m_projectionMatrixLeft;       // Projection Matrix for Left Eye Stereo
    DirectX::XMFLOAT4X4 m_projectionMatrixRight;      // Projection Matrix for Right Eye Stereo

    DirectX::XMFLOAT4X4 m_inverseView;

    DirectX::XMFLOAT3 m_eye;                        // Camera eye position
    DirectX::XMFLOAT3 m_lookAt;                     // LookAt position
    DirectX::XMFLOAT3 m_up;                         // Up vector
    float             m_cameraYawAngle;             // Yaw angle of camera
    float             m_cameraPitchAngle;           // Pitch angle of camera

    float             m_fieldOfView;                // Field of view
    float             m_aspectRatio;                // Aspect ratio
    float             m_nearPlane;                  // Near plane
    float             m_farPlane;                   // Far plane
};
```

Camera.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Camera.h"
#include "StereoProjection.h"

using namespace DirectX;

#undef min // use __min instead
#undef max // use __max instead
//--------------------------------------------------------------------------------------
// Constructor
//--------------------------------------------------------------------------------------
Camera::Camera()
{
    // Setup the view matrix.
    SetViewParams(
        XMFLOAT3(0.0f, 0.0f, 0.0f),   // default eye position.
        XMFLOAT3(0.0f, 0.0f, 1.0f),   // default look at position.
        XMFLOAT3(0.0f, 1.0f, 0.0f)    // default up vector.
        );

    // Setup the projection matrix.
    SetProjParams(XM_PI / 4, 1.0f, 1.0f, 1000.0f);
}
//--------------------------------------------------------------------------------------
void Camera::LookDirection (_In_ XMFLOAT3 lookDirection)
{
    XMFLOAT3 lookAt;
    lookAt.x = m_eye.x + lookDirection.x;
    lookAt.y = m_eye.y + lookDirection.y;
    lookAt.z = m_eye.z + lookDirection.z;

    SetViewParams(m_eye, lookAt, m_up);
}
//--------------------------------------------------------------------------------------
void Camera::Eye(_In_ XMFLOAT3 eye)
{
    SetViewParams(eye, m_lookAt, m_up);
}
//--------------------------------------------------------------------------------------
void Camera::SetViewParams(
    _In_ XMFLOAT3 eye,
    _In_ XMFLOAT3 lookAt,
    _In_ XMFLOAT3 up
    )
{
    m_eye = eye;
    m_lookAt = lookAt;
    m_up = up;

    // Calculate the view matrix.
    XMMATRIX view = XMMatrixLookAtLH(
        XMLoadFloat3(&m_eye),
        XMLoadFloat3(&m_lookAt),
        XMLoadFloat3(&m_up)
        );

    XMVECTOR det;
    XMMATRIX inverseView = XMMatrixInverse(&det, view);
    XMStoreFloat4x4(&m_viewMatrix, view);
    XMStoreFloat4x4(&m_inverseView, inverseView);

    // The axis basis vectors and camera position are stored inside the
    // position matrix in the four rows of the camera's world matrix.
    // To figure out the yaw/pitch of the camera, we just need the Z basis vector.
    XMFLOAT3 zBasis;
    XMStoreFloat3(&zBasis, inverseView.r[2]);

    m_cameraYawAngle = atan2f(zBasis.x, zBasis.z);

    float len = sqrtf(zBasis.z * zBasis.z + zBasis.x * zBasis.x);
    m_cameraPitchAngle = atan2f(zBasis.y, len);
}
//--------------------------------------------------------------------------------------
// Calculates the projection matrix based on input params.
//--------------------------------------------------------------------------------------
void Camera::SetProjParams(
    _In_ float fieldOfView,
    _In_ float aspectRatio,
    _In_ float nearPlane,
    _In_ float farPlane
    )
{
    // Set attributes for the projection matrix.
    m_fieldOfView = fieldOfView;
    m_aspectRatio = aspectRatio;
    m_nearPlane = nearPlane;
    m_farPlane = farPlane;
    XMStoreFloat4x4(
        &m_projectionMatrix,
        XMMatrixPerspectiveFovLH(
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane
            )
        );

    STEREO_PARAMETERS* stereoParams = nullptr;
    // Change the projection matrix.
    XMStoreFloat4x4(
        &m_projectionMatrixLeft,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::LEFT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );

    XMStoreFloat4x4(
        &m_projectionMatrixRight,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::RIGHT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );
}
//--------------------------------------------------------------------------------------
DirectX::XMMATRIX Camera::View()
{
    return XMLoadFloat4x4(&m_viewMatrix);
}
DirectX::XMMATRIX Camera::Projection()
{
    return XMLoadFloat4x4(&m_projectionMatrix);
}
DirectX::XMMATRIX Camera::LeftEyeProjection()
{
    return XMLoadFloat4x4(&m_projectionMatrixLeft);
}
DirectX::XMMATRIX Camera::RightEyeProjection()
{
    return XMLoadFloat4x4(&m_projectionMatrixRight);
}
DirectX::XMMATRIX Camera::World()
{
    return XMLoadFloat4x4(&m_inverseView);
}
DirectX::XMFLOAT3 Camera::Eye()
{
    return m_eye;
}
DirectX::XMFLOAT3 Camera::LookAt()
{
    return m_lookAt;
}
float Camera::NearClipPlane()
{
    return m_nearPlane;
}
float Camera::FarClipPlane()
{
    return m_farPlane;
}
float Camera::Pitch()
{
    return m_cameraPitchAngle;
}
float Camera::Yaw()
{
    return m_cameraYawAngle;
}
```

GameRenderer.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "DirectXBase.h"
#include "GameInfoOverlay.h"
#include "GameHud.h"
#include "Simple3DGame.h"

ref class Simple3DGame;
ref class GameHud;

ref class GameRenderer : public DirectXBase
{
internal:
    GameRenderer();

    virtual void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window,
        float dpi
        ) override;

    virtual void CreateDeviceIndependentResources() override;
    virtual void CreateDeviceResources() override;
    virtual void UpdateForWindowSizeChange() override;
    virtual void Render() override;
    virtual void SetDpi(float dpi) override;

    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    GameInfoOverlay^ InfoOverlay()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f
            );
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f + GameInfoOverlayConstant::Width,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f + GameInfoOverlayConstant::Height
            );
    };

protected private:
    bool                                                m_initialized;
    bool                                                m_gameResourcesLoaded;
    bool                                                m_levelResourcesLoaded;
    GameInfoOverlay^                                    m_gameInfoOverlay;
    GameHud^                                            m_gameHud;
    Simple3DGame^                                       m_game;

    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers.
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

GameRenderer.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "DirectXSample.h"
#include "GameRenderer.h"
#include "ConstantBuffers.h"
#include "TargetTexture.h"
#include "BasicLoader.h"
#include "CylinderMesh.h"
#include "FaceMesh.h"
#include "SphereMesh.h"
#include "WorldMesh.h"
#include "Face.h"
#include "Sphere.h"
#include "Cylinder.h"

using namespace concurrency;
using namespace DirectX;
using namespace Microsoft::WRL;
using namespace Windows::UI::Core;

//----------------------------------------------------------------------

GameRenderer::GameRenderer() :
    m_initialized(false),
    m_gameResourcesLoaded(false),
    m_levelResourcesLoaded(false)
{
}

//----------------------------------------------------------------------

void GameRenderer::Initialize(
    _In_ CoreWindow^ window,
    float dpi
    )
{
    if (!m_initialized)
    {
        m_gameHud = ref new GameHud(
            "Windows 8 Samples",
            "DirectX first-person game sample"
            );
        m_gameInfoOverlay = ref new GameInfoOverlay();
        m_initialized = true;
    }

    DirectXBase::Initialize(window, dpi);

    // Initialize could be called multiple times as a result of an error with the hardware device
    // that requires it to be reinitialized.  Since the m_gameInfoOverlay has resources that are
    // dependent on the device, it will need to be reinitialized each time with the new device information.
    m_gameInfoOverlay->Initialize(m_d2dDevice.Get(), m_d2dContext.Get(), m_dwriteFactory.Get(), dpi);
}

//----------------------------------------------------------------------

void GameRenderer::CreateDeviceIndependentResources()
{
    DirectXBase::CreateDeviceIndependentResources();
    m_gameHud->CreateDeviceIndependentResources(m_dwriteFactory.Get(), m_wicFactory.Get());
}

//----------------------------------------------------------------------

void GameRenderer::CreateDeviceResources()
{
    DirectXBase::CreateDeviceResources();

    m_gameHud->CreateDeviceResources(m_d2dContext.Get());

    if (m_game != nullptr)
    {
        // The initial invocation of CreateDeviceResources will occur
        // before the Game State is initialized when the device is first
        // being created, so that the inital loading screen can be displayed.
        // Subsequent invocations of CreateDeviceResources will be a result
        // of an error with the Device that requires the resources to be
        // recreated.  In this case, the game state is already initialized
        // so the game device resources need to be recreated.

        // This sample doesn't gracefully handle all the async recreation
        // of resources, so an exception is thrown.
        throw Platform::Exception::CreateException(
            DXGI_ERROR_DEVICE_REMOVED,
            "GameRenderer::CreateDeviceResources - Recreation of resources after TDR not available\n"
            );
    }
}

//----------------------------------------------------------------------

void GameRenderer::UpdateForWindowSizeChange()
{
    DirectXBase::UpdateForWindowSizeChange();

    m_gameHud->UpdateForWindowSizeChange(m_windowBounds);

    // Update the Projection Matrix and update the associated Constant Buffer.
    if (m_game != nullptr)
    {
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
            0.01f,
            100.0f
            );
        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
            );

        m_d3dContext->UpdateSubresource(
            m_constantBufferChangeOnResize.Get(),
            0,
            nullptr,
            &changesOnResize,
            0,
            0
            );
    }
}

//----------------------------------------------------------------------

void GameRenderer::SetDpi(float dpi)
{
    DirectXBase::SetDpi(dpi);

    if (m_gameInfoOverlay)
    {
        m_gameInfoOverlay->SetDpi(dpi);
    }
}

//----------------------------------------------------------------------

task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Set the Loading state to wait until any async resources have
    // been loaded before proceeding.
    m_game = game;

    // NOTE: Only the m_d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the m_d3dDevice are free-threaded and are safe while any methods
    // in the m_d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.

    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers,
    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    bd.CPUAccessFlags = 0;
    bd.ByteWidth = (sizeof(ConstantBufferNeverChanges) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangeOnResize) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangeOnResize)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryFrame) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryFrame)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryPrim) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryPrim)
        );

    D3D11_SAMPLER_DESC sampDesc;
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;
    sampDesc.MinLOD = 0;
    sampDesc.MaxLOD = FLT_MAX;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    m_cylinderTexture = nullptr;
    m_ceilingTexture = nullptr;
    m_floorTexture = nullptr;
    m_wallsTexture = nullptr;

    // Load Game specific textures.
    tasks.push_back(loader->LoadTextureAsync("seafloor.dds", nullptr, &m_sphereTexture));
    tasks.push_back(loader->LoadTextureAsync("metal_texture.dds", nullptr, &m_cylinderTexture));
    tasks.push_back(loader->LoadTextureAsync("cellceiling.dds", nullptr, &m_ceilingTexture));
    tasks.push_back(loader->LoadTextureAsync("cellfloor.dds", nullptr, &m_floorTexture));
    tasks.push_back(loader->LoadTextureAsync("cellwall.dds", nullptr, &m_wallsTexture));
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Return the task group of all the async tasks for loading the shader and texture assets.
    return when_all(tasks.begin(), tasks.end());
}

//----------------------------------------------------------------------

void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now, associate all the resources with the appropriate
    // Game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[1] = XMFLOAT4( 3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[2] = XMFLOAT4(-3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[3] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);
    m_d3dContext->UpdateSubresource(m_constantBufferNeverChanges.Get(), 0, nullptr, &constantBufferNeverChanges, 0, 0);

    // For the targets, there are two unique generated textures.
    // Each texture image includes the number of the texture.
    // Make sure the 2D rendering is occurring on the same thread
    // as the main rendering.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        m_d3dDevice.Get(),
        m_d2dFactory.Get(),
        m_dwriteFactory.Get(),
        m_d2dContext.Get()
        );

    MeshObject^ cylinderMesh = ref new CylinderMesh(m_d3dDevice.Get(), 26);
    MeshObject^ targetMesh = ref new FaceMesh(m_d3dDevice.Get());
    MeshObject^ sphereMesh = ref new SphereMesh(m_d3dDevice.Get(), 26);

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    Material^ sphereMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        50.0f,
        m_sphereTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldFloorMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldCeilingId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_ceilingTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldCeilingMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldWallsId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_wallsTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldWallsMesh(m_d3dDevice.Get()));
        }
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        else if (Sphere^ sphere = dynamic_cast<Sphere^>(*object))
        {
            sphere->Mesh(sphereMesh);
            sphere->NormalMaterial(sphereMaterial);
        }
    }

    // Ensure that the camera has been initialized with the right Projection
    // matrix.  The camera is not created at the time the first window resize event
    // occurs.
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that Projection matrix has been set in the constant buffer
    // now that all the resources are loaded.
    // DirectXBase is handling screen rotations directly to eliminate an unaligned
    // fullscreen copy.  As a result, it is necessary to post multiply the rotationTransform3D
    // matrix to the camera projection matrix.
    ConstantBufferChangeOnResize changesOnResize;
    XMStoreFloat4x4(
        &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
        );

    m_d3dContext->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    m_gameResourcesLoaded = true;
}

//----------------------------------------------------------------------

task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        // This is where additional async loading of level specific resources
        // would be done.  Because there aren't any to load, just simulate
        // by delaying for some time.
        wait(GameConstants::LevelLoadingDelay);
    });
}

//----------------------------------------------------------------------

void GameRenderer::FinalizeLoadLevelResources()
{
    // After the level specific resources had been loaded, this method is
    // where D3D context specific actions would be handled.  This method
    // runs in the same thread context as the GameRenderer was created.

    m_levelResourcesLoaded = true;
}

//----------------------------------------------------------------------

void GameRenderer::Render()
{
    int renderingPasses = 1;
    if (m_stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        if (m_stereoEnabled && i > 0)
        {
            // Doing the Right Eye View.
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetViewRight.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmapRight.Get());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetView.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
        }

        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (m_stereoEnabled)
            {
                ConstantBufferChangeOnResize changesOnResize;
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                        )
                    );

                m_d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }
            // Update variables that change one time per frame.

            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            m_d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Set up Pipeline.

            m_d3dContext->IASetInputLayout(m_vertexLayout.Get());
            m_d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            m_d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            m_d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            // Get all the objects to render from the Game state.
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(m_d3dContext.Get(), m_constantBufferChangesEveryPrim.Get());
            }
        }
        else
        {
            const float ClearColor[4] = {0.1f, 0.1f, 0.1f, 1.0f};

            // Only need to clear the background when not rendering the full 3-D scene because
            // the 3-D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (m_stereoEnabled && i > 0)
            {
                // Doing the Right Eye View.
                m_d3dContext->ClearRenderTargetView(m_renderTargetViewRight.Get(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                m_d3dContext->ClearRenderTargetView(m_renderTargetView.Get(), ClearColor);
            }
        }

        m_d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        m_d2dContext->SetTransform(m_rotationTransform2D);

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game, m_d2dContext.Get(), m_windowBounds);
        }

        if (m_gameInfoOverlay->Visible())
        {
            m_d2dContext->DrawBitmap(
                m_gameInfoOverlay->Bitmap(),
                D2D1::RectF(
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f,
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f + GameInfoOverlayConstant::Width,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f + GameInfoOverlayConstant::Height
                    )
                );
        }

        HRESULT hr = m_d2dContext->EndDraw();
        if (hr != D2DERR_RECREATE_TARGET)
        {
            // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
            // D3D device.  All subsequent rendering will be ignored until the device is recreated.
            // This error will be propagated and the appropriate D3D error will be returned from the
            // swapchain->Present(...) call.   At that point, the sample will recreate the device
            // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
            // need to be handled here.
            DX::ThrowIfFailed(hr);
        }
    }
    Present();
}
```

GameObject.h

```cpp
/// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MeshObject.h"
#include "SoundEffect.h"
#include "Animate.h"
#include "Material.h"

ref class GameObject
{
internal:
    GameObject();

    // Expect these two functions to be overloaded by subclasses.
    virtual bool IsTouching(
        DirectX::XMFLOAT3 /* point */,
        float /* radius */,
        _Out_ DirectX::XMFLOAT3 *contact,
        _Out_ DirectX::XMFLOAT3 *normal
        )
    {
        *contact = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);
        *normal = DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f);
        return false;
    };

    void Render(
        _In_ ID3D11DeviceContext *context,
        _In_ ID3D11Buffer *primitiveConstantBuffer
        );

    void Active(bool active);
    bool Active();
    void Target(bool target);
    bool Target();
    void Hit(bool hit);
    bool Hit();
    void OnGround(bool ground);
    bool OnGround();
    void TargetId(int targetId);
    int  TargetId();
    void HitTime(float t);
    float HitTime();

    void     AnimatePosition(_In_opt_ Animate^ animate);
    Animate^ AnimatePosition();

    void         HitSound(_In_ SoundEffect^ hitSound);
    SoundEffect^ HitSound();

    void PlaySound(float impactSpeed, DirectX::XMFLOAT3 eyePoint);

    void Mesh(_In_ MeshObject^ mesh);

    void NormalMaterial(_In_ Material^ material);
    Material^ NormalMaterial();
    void HitMaterial(_In_ Material^ material);
    Material^ HitMaterial();

    void Position(DirectX::XMFLOAT3 position);
    void Position(DirectX::XMVECTOR position);
    void Velocity(DirectX::XMFLOAT3 velocity);
    void Velocity(DirectX::XMVECTOR velocity);
    DirectX::XMMATRIX ModelMatrix();
    DirectX::XMFLOAT3 Position();
    DirectX::XMVECTOR VectorPosition();
    DirectX::XMVECTOR VectorVelocity();
    DirectX::XMFLOAT3 Velocity();

protected private:
    virtual void UpdatePosition() {};
    // Object Data.
    bool                m_active;
    bool                m_target;
    int                 m_targetId;
    bool                m_hit;
    bool                m_ground;

    DirectX::XMFLOAT3   m_position;
    DirectX::XMFLOAT3   m_velocity;
    DirectX::XMFLOAT4X4 m_modelMatrix;

    Material^           m_normalMaterial;
    Material^           m_hitMaterial;

    DirectX::XMFLOAT3   m_defaultXAxis;
    DirectX::XMFLOAT3   m_defaultYAxis;
    DirectX::XMFLOAT3   m_defaultZAxis;

    float               m_hitTime;

    Animate^            m_animatePosition;
    MeshObject^         m_mesh;

    SoundEffect^        m_hitSound;
};


__forceinline void GameObject::Active(bool active)
{
    m_active = active;
}

__forceinline bool GameObject::Active()
{
    return m_active;
}

__forceinline void GameObject::Target(bool target)
{
    m_target = target;
}

__forceinline bool GameObject::Target()
{
    return m_target;
}

__forceinline void GameObject::Hit(bool hit)
{
    m_hit = hit;
}

__forceinline bool GameObject::Hit()
{
    return m_hit;
}

__forceinline void GameObject::OnGround(bool ground)
{
    m_ground = ground;
}

__forceinline bool GameObject::OnGround()
{
    return m_ground;
}

__forceinline void GameObject::TargetId(int targetId)
{
    m_targetId = targetId;
}

__forceinline int GameObject::TargetId()
{
    return m_targetId;
}

__forceinline void GameObject::HitTime(float t)
{
    m_hitTime = t;
}

__forceinline float GameObject::HitTime()
{
    return m_hitTime;
}

__forceinline void GameObject::Position(DirectX::XMFLOAT3 position)
{
    m_position = position;
    // Update any internal states that are dependent on the position.
    // UpdatePosition is a virtual function that is specific to the derived class.
    UpdatePosition();
}

__forceinline void GameObject::Position(DirectX::XMVECTOR position)
{
    XMStoreFloat3(&m_position, position);
   // Update any internal states that are dependent on the position.
    // UpdatePosition is a virtual function that is specific to the derived class.
    UpdatePosition();
}

__forceinline DirectX::XMFLOAT3 GameObject::Position()
{
    return m_position;
}

__forceinline DirectX::XMVECTOR GameObject::VectorPosition()
{
    return DirectX::XMLoadFloat3(&m_position);
}

__forceinline void GameObject::Velocity(DirectX::XMFLOAT3 velocity)
{
    m_velocity = velocity;
}

__forceinline void GameObject::Velocity(DirectX::XMVECTOR velocity)
{
    XMStoreFloat3(&m_velocity, velocity);
}

__forceinline DirectX::XMFLOAT3 GameObject::Velocity()
{
    return m_velocity;
}

__forceinline DirectX::XMVECTOR GameObject::VectorVelocity()
{
    return DirectX::XMLoadFloat3(&m_velocity);
}

__forceinline void GameObject::AnimatePosition(_In_opt_ Animate^ animate)
{
    m_animatePosition = animate;
}

__forceinline Animate^ GameObject::AnimatePosition()
{
    return m_animatePosition;
}

__forceinline void GameObject::NormalMaterial(_In_ Material^ material)
{
    m_normalMaterial = material;
}

__forceinline Material^ GameObject::NormalMaterial()
{
    return m_normalMaterial;
}

__forceinline void GameObject::HitMaterial(_In_ Material^ material)
{
    m_hitMaterial = material;
}

__forceinline Material^ GameObject::HitMaterial()
{
    return m_hitMaterial;
}

__forceinline void GameObject::Mesh(_In_ MeshObject^ mesh)
{
    m_mesh = mesh;
}

__forceinline void GameObject::HitSound(_In_ SoundEffect^ hitSound)
{
    m_hitSound = hitSound;
}

__forceinline SoundEffect^ GameObject::HitSound()
{
    return m_hitSound;
}

__forceinline DirectX::XMMATRIX GameObject::ModelMatrix()
{
    return DirectX::XMLoadFloat4x4(&m_modelMatrix);
}
```

GameObject.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "GameObject.h"
#include "ConstantBuffers.h"
#include "GameConstants.h"

using namespace DirectX;

//--------------------------------------------------------------------------------

GameObject::GameObject() :
    m_normalMaterial(nullptr),
    m_hitMaterial(nullptr)
{
    m_active          = false;
    m_target          = false;
    m_targetId        = 0;
    m_hit             = false;
    m_ground          = true;

    m_position        = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_velocity        = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_defaultXAxis    = XMFLOAT3(1.0f, 0.0f, 0.0f);
    m_defaultYAxis    = XMFLOAT3(0.0f, 1.0f, 0.0f);
    m_defaultZAxis    = XMFLOAT3(0.0f, 0.0f, 1.0f);
    XMStoreFloat4x4(&m_modelMatrix, XMMatrixIdentity());

    m_hitTime         = 0.0f;

    m_animatePosition = nullptr;
}

//----------------------------------------------------------------------

void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    m_mesh->Render(context);
}

//----------------------------------------------------------------------

void GameObject::PlaySound(float impactSpeed, XMFLOAT3 eyePoint)
{
    if (m_hitSound != nullptr)
    {
        // Determine the sound volume adjustment based on velocity.
        float adjustment;
        if (impactSpeed < GameConstants::Sound::MinVelocity)
        {
            adjustment = 0.0f;  // Too soft.  Don't play sound.
        }
        else
        {
            adjustment = min(1.0f, impactSpeed / GameConstants::Sound::MaxVelocity);
            adjustment = GameConstants::Sound::MinAdjustment + adjustment * (1.0f - GameConstants::Sound::MinAdjustment);
        }

        // Compute Distance to eyePoint to adjust the volume based on that distance.
        XMVECTOR cameraToPosition = XMLoadFloat3(&eyePoint) - VectorPosition();
        float distToPositionSquared = XMVectorGetX(XMVector3LengthSq(cameraToPosition));

        float volume = min(distToPositionSquared, 1.0f);
        // Scale
        // Sound is proportional to how hard the ball is hitting.
        volume = adjustment * volume;

        m_hitSound->PlaySound(volume);
    }
}
```

Animate.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Animate:
// This is an abstract class for animations.  It defines a set of
// capabilities for animating XMFLOAT3 variables.  An animation has the following
// characteristics:
//     Start - the time for the animation to start.
//     Duration - the length of time the animation is to run.
//     Continuous - whether the animation loops after duration is reached or just
//         stops.
// There are two query functions:
//     IsActive - determines if the animation is active at time t.
//     IsFinished - determines if the animation is done at time t.
// It is expected that each derived class will provide an Evaluate method for the
// specific kind of animation.

ref class Animate abstract
{
internal:
    Animate();

    virtual DirectX::XMFLOAT3 Evaluate (_In_ float t) = 0;

    bool  IsActive(_In_ float t) { return ((t >= m_startTime) && (m_continuous || (t < (m_startTime + m_duration)))); };
    bool  IsFinished(_In_ float t) { return (!m_continuous && (t >= (m_startTime + m_duration))); }
    float Start();
    void  Start(_In_ float start);
    float Duration();
    void  Duration(_In_ float duration);
    bool  Continuous();
    void  Continuous(_In_ bool continuous);

protected private:
    bool  m_continuous;      // if true means at end cycle back to beginning
    float m_startTime;       // time at which animation begins
    float m_duration;        // for continuous, this is the duration of 1 cycle through path
};

//----------------------------------------------------------------------

// AnimateLinePosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a straight line defined in world coordinates from startPosition to endPosition.

ref class AnimateLinePosition: public Animate
{
internal:
    AnimateLinePosition(
        _In_ DirectX::XMFLOAT3 startPosition,
        _In_ DirectX::XMFLOAT3 endPosition,
        _In_ float duration,
        _In_ bool continuous
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    DirectX::XMFLOAT3 m_startPosition;
    DirectX::XMFLOAT3 m_endPosition;
    float             m_length;
};

//----------------------------------------------------------------------

struct LineSegment
{
    DirectX::XMFLOAT3 position;
    float             length;
    float             uStart;
    float             uLength;
};

// AnimateLineListPosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a set of line segments defined by a set of points.  The animation along the path is
// such that the evaluation of the position along the path will be uniform independent of
// the length of each of the line segments.  A continuous loop can be achieved by having the
// first and last points of the list be the same.

ref class AnimateLineListPosition: public Animate
{
internal:
    AnimateLineListPosition(
        _In_ unsigned int count,
        _In_reads_(count) DirectX::XMFLOAT3 position[],
        _In_ float duration,
        _In_ bool continuous
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    unsigned int             m_count;
    float                    m_totalLength;
    std::vector<LineSegment> m_segment;
};

//----------------------------------------------------------------------

// AnimateCirclePosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a circular path centered at 'center' with a starting point of 'startPosition'.  The
// distance between 'center' and 'startPosition' defines the radius of the circle.  The plane
// of the circle will pass through 'center' and 'startPosition' and have a normal of 'planeNormal'.
// The direction of the animation can be either clockwise or counterclockwise based
// on the 'clockwise' parameter.

ref class AnimateCirclePosition: public Animate
{
internal:
    AnimateCirclePosition(
        _In_ DirectX::XMFLOAT3 center,
        _In_ DirectX::XMFLOAT3 startPosition,
        _In_ DirectX::XMFLOAT3 planeNormal,
        _In_ float duration,
        _In_ bool continuous,
        _In_ bool clockwise
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    DirectX::XMFLOAT4X4 m_rotationMatrix;
    DirectX::XMFLOAT3   m_center;
    DirectX::XMFLOAT3   m_planeNormal;
    DirectX::XMFLOAT3   m_startPosition;
    float               m_radius;
    bool                m_clockwise;
};

//----------------------------------------------------------------------
            
            
```

Animate.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Animate::Animate():
    m_continuous(false),
    m_startTime(0.0f),
    m_duration(10.0f)
{
}

//----------------------------------------------------------------------

float Animate::Start()
{
    return m_startTime;
}

//----------------------------------------------------------------------

void Animate::Start(_In_ float start)
{
    m_startTime = start;
}

//----------------------------------------------------------------------

float Animate::Duration()
{
    return m_duration;
}

//----------------------------------------------------------------------

void Animate::Duration(_In_ float duration)
{
    m_duration = duration;
}

//----------------------------------------------------------------------

bool Animate::Continuous()
{
    return m_continuous;
}

//----------------------------------------------------------------------

void Animate::Continuous(_In_ bool continuous)
{
    m_continuous = continuous;
}

//----------------------------------------------------------------------

AnimateLinePosition::AnimateLinePosition(
    _In_ XMFLOAT3 startPosition,
    _In_ XMFLOAT3 endPosition,
    _In_ float duration,
    _In_ bool continuous)
{
    m_startPosition = startPosition;
    m_endPosition = endPosition;
    m_duration = duration;
    m_continuous = continuous;

    m_length = XMVectorGetX(
        XMVector3Length(XMLoadFloat3(&endPosition) - XMLoadFloat3(&startPosition))
        );
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateLinePosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_startPosition;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_endPosition;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration;
    XMFLOAT3 currentPosition;
    currentPosition.x = m_startPosition.x + (m_endPosition.x - m_startPosition.x)*u;
    currentPosition.y = m_startPosition.y + (m_endPosition.y - m_startPosition.y)*u;
    currentPosition.z = m_startPosition.z + (m_endPosition.z - m_startPosition.z)*u;

    return currentPosition;
}

//----------------------------------------------------------------------

AnimateLineListPosition::AnimateLineListPosition(
    _In_ unsigned int count,
    _In_reads_(count) XMFLOAT3 position[],
    _In_ float duration,
    _In_ bool continuous)
{
    m_duration = duration;
    m_continuous = continuous;
    m_count = count;

    std::vector<LineSegment> segment(m_count);
    m_segment = segment;
    m_totalLength = 0.0f;

    m_segment[0].position = position[0];
    for (unsigned int i = 1; i < count; i++)
    {
        m_segment[i].position = position[i];
        m_segment[i - 1].length = XMVectorGetX(
            XMVector3Length(
                XMLoadFloat3(&m_segment[i].position) -
                XMLoadFloat3(&m_segment[i - 1].position)
                )
            );
        m_totalLength += m_segment[i - 1].length;
    }

    // Parameterize the segments to ensure uniform evaluation along the path.
    float u = 0.0f;
    for (unsigned int i = 0; i < (count - 1); i++)
    {
        m_segment[i].uStart = u;
        m_segment[i].uLength = (m_segment[i].length / m_totalLength);
        u += m_segment[i].uLength;
    }
    m_segment[count-1].uStart = 1.0f;
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateLineListPosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_segment[0].position;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_segment[m_count-1].position;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration;
    // Find the right segment.
    unsigned int i = 0;
    while (u > m_segment[i + 1].uStart)
    {
        i++;
    }

    u -= m_segment[i].uStart;
    u /= m_segment[i].uLength;

    XMFLOAT3 currentPosition;
    currentPosition.x = m_segment[i].position.x + (m_segment[i + 1].position.x - m_segment[i].position.x)*u;
    currentPosition.y = m_segment[i].position.y + (m_segment[i + 1].position.y - m_segment[i].position.y)*u;
    currentPosition.z = m_segment[i].position.z + (m_segment[i + 1].position.z - m_segment[i].position.z)*u;

    return currentPosition;
}

//----------------------------------------------------------------------

AnimateCirclePosition:: AnimateCirclePosition(
    _In_ XMFLOAT3 center,
    _In_ XMFLOAT3 startPosition,
    _In_ XMFLOAT3 planeNormal,
    _In_ float duration,
    _In_ bool continuous,
    _In_ bool clockwise)
{
    m_center = center;
    m_planeNormal = planeNormal;
    m_startPosition = startPosition;
    m_duration = duration;
    m_continuous = continuous;
    m_clockwise = clockwise;

    XMVECTOR coordX = XMLoadFloat3(&m_startPosition) - XMLoadFloat3(&m_center);
    m_radius = XMVectorGetX(XMVector3Length(coordX));

    XMVector3Normalize(coordX);

    XMVECTOR coordZ = XMLoadFloat3(&m_planeNormal);
    XMVector3Normalize(coordZ);

    XMVECTOR coordY;
    if (m_clockwise)
    {
        coordY = XMVector3Cross(coordZ, coordX);
    }
    else
    {
        coordY = XMVector3Cross(coordX, coordZ);
    }

    XMVECTOR vectorX = XMVectorSet(1.0f, 0.0f, 0.0f, 1.0f);
    XMVECTOR vectorY = XMVectorSet(0.0f, 1.0f, 0.0f, 1.0f);
    XMMATRIX mat1 = XMMatrixIdentity();
    XMMATRIX mat2 = XMMatrixIdentity();

    if (!XMVector3Equal(coordX, vectorX))
    {
        float angle;
        angle = XMVectorGetX(
            XMVector3AngleBetweenVectors(vectorX, coordX)
            );
        if ((angle * angle) > 0.025)
        {
            XMVECTOR axis1 = XMVector3Cross(vectorX, coordX);

            mat1 = XMMatrixRotationAxis(axis1, angle);
            vectorY = XMVector3TransformCoord(vectorY, mat1);
        }
    }
    if (!XMVector3Equal(vectorY, coordY))
    {
        float angle;
        angle = XMVectorGetX(
            XMVector3AngleBetweenVectors(vectorY, coordY)
            );
        if ((angle * angle) > 0.025)
        {
            XMVECTOR axis2 = XMVector3Cross(vectorY, coordY);
            mat2 = XMMatrixRotationAxis(axis2, angle);
        }
    }
    XMStoreFloat4x4(
        &m_rotationMatrix,
        mat1 *
        mat2 *
        XMMatrixTranslation(m_center.x, m_center.y, m_center.z)
        );
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateCirclePosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_startPosition;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_startPosition;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration * XM_2PI;

    XMFLOAT3 currentPosition;
    currentPosition.x = m_radius * cos(u);
    currentPosition.y = m_radius * sin(u);
    currentPosition.z = 0.0f;

    XMStoreFloat3(
        &currentPosition,
        XMVector3TransformCoord(
            XMLoadFloat3(&currentPosition),
            XMLoadFloat4x4(&m_rotationMatrix)
            )
        );

    return currentPosition;
}

//----------------------------------------------------------------------
            
            
```

BasicLoader.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "BasicReaderWriter.h"

// A simple loader class that provides support for loading shaders, textures,
// and meshes from files on disk. Provides synchronous and asynchronous methods.
ref class BasicLoader
{
internal:
    BasicLoader(
        _In_ ID3D11Device* d3dDevice,
        _In_opt_ IWICImagingFactory2* wicFactory = nullptr
        );

    void LoadTexture(
        _In_ Platform::String^ filename,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView
        );

    concurrency::task<void> LoadTextureAsync(
        _In_ Platform::String^ filename,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11PixelShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11PixelShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11ComputeShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11ComputeShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11GeometryShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11GeometryShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
        _In_ uint32 numEntries,
        _In_reads_opt_(numStrides) const uint32* bufferStrides,
        _In_ uint32 numStrides,
        _In_ uint32 rasterizedStream,
        _Out_ ID3D11GeometryShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
        _In_ uint32 numEntries,
        _In_reads_opt_(numStrides) const uint32* bufferStrides,
        _In_ uint32 numStrides,
        _In_ uint32 rasterizedStream,
        _Out_ ID3D11GeometryShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11HullShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11HullShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11DomainShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11DomainShader** shader
        );

    void LoadMesh(
        _In_ Platform::String^ filename,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount
        );

    concurrency::task<void> LoadMeshAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount
        );

private:
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<IWICImagingFactory2> m_wicFactory;
    BasicReaderWriter^ m_basicReaderWriter;

    template <class DeviceChildType>
    inline void SetDebugName(
        _In_ DeviceChildType* object,
        _In_ Platform::String^ name
        );

    Platform::String^ GetExtension(
        _In_ Platform::String^ filename
        );

    void CreateTexture(
        _In_ bool decodeAsDDS,
        _In_reads_bytes_(dataSize) byte* data,
        _In_ uint32 dataSize,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView,
        _In_opt_ Platform::String^ debugName
        );

    void CreateInputLayout(
        _In_reads_bytes_(bytecodeSize) byte* bytecode,
        _In_ uint32 bytecodeSize,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11InputLayout** layout
        );

    void CreateMesh(
        _In_ byte* meshData,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount,
        _In_opt_ Platform::String^ debugName
        );
};
```

BasicLoader.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "BasicLoader.h"
#include "BasicShapes.h"
#include "DDSTextureLoader.h"
#include "DirectXSample.h"
#include <memory>

using namespace Microsoft::WRL;
using namespace Windows::Storage;
using namespace Windows::Storage::Streams;
using namespace Windows::Foundation;
using namespace Windows::ApplicationModel;
using namespace std;
using namespace concurrency;

BasicLoader::BasicLoader(
    _In_ ID3D11Device* d3dDevice,
    _In_opt_ IWICImagingFactory2* wicFactory
    ) :
    m_d3dDevice(d3dDevice),
    m_wicFactory(wicFactory)
{
    // Create a new BasicReaderWriter to do raw file I/O.
    m_basicReaderWriter = ref new BasicReaderWriter();
}

template <class DeviceChildType>
inline void BasicLoader::SetDebugName(
    _In_ DeviceChildType* object,
    _In_ Platform::String^ name
    )
{
#if defined(_DEBUG)
    // Only assign debug names in debug builds.

    char nameString[1024];
    int nameStringLength = WideCharToMultiByte(
        CP_ACP,
        0,
        name->Data(),
        -1,
        nameString,
        1024,
        nullptr,
        nullptr
        );

    if (nameStringLength == 0)
    {
        char defaultNameString[] = "BasicLoaderObject";
        DX::ThrowIfFailed(
            object->SetPrivateData(
                WKPDID_D3DDebugObjectName,
                sizeof(defaultNameString) - 1,
                defaultNameString
                )
            );
    }
    else
    {
        DX::ThrowIfFailed(
            object->SetPrivateData(
                WKPDID_D3DDebugObjectName,
                nameStringLength - 1,
                nameString
                )
            );
    }
#endif
}

Platform::String^ BasicLoader::GetExtension(
    _In_ Platform::String^ filename
    )
{
    int lastDotIndex = -1;
    for (int i = filename->Length() - 1; i >= 0 && lastDotIndex == -1; i--)
    {
        if (*(filename->Data() + i) == '.')
        {
            lastDotIndex = i;
        }
    }
    if (lastDotIndex != -1)
    {
        std::unique_ptr<wchar_t[]> extension(new wchar_t[filename->Length() - lastDotIndex]);
        for (unsigned int i = 0; i < filename->Length() - lastDotIndex; i++)
        {
            extension[i] = tolower(*(filename->Data() + lastDotIndex + 1 + i));
        }
        return ref new Platform::String(extension.get());
    }
    return "";
}

void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        DX::ThrowIfFailed(
            resource.As(&texture2D)
            );
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            DX::ThrowIfFailed(
                CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory)
                    )
                );
        }

        ComPtr<IWICStream> stream;
        DX::ThrowIfFailed(
            m_wicFactory->CreateStream(&stream)
            );

        DX::ThrowIfFailed(
            stream->InitializeFromMemory(
                data,
                dataSize
                )
            );

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        DX::ThrowIfFailed(
            m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder
                )
            );

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        DX::ThrowIfFailed(
            bitmapDecoder->GetFrame(0, &bitmapFrame)
            );

        ComPtr<IWICFormatConverter> formatConverter;
        DX::ThrowIfFailed(
            m_wicFactory->CreateFormatConverter(&formatConverter)
            );

        DX::ThrowIfFailed(
            formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom
                )
            );

        uint32 width;
        uint32 height;
        DX::ThrowIfFailed(
            bitmapFrame->GetSize(&width, &height)
            );

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        DX::ThrowIfFailed(
            formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get()
                )
            );

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D
                )
            );

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            DX::ThrowIfFailed(
                m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView
                    )
                );
        }
    }

    SetDebugName(texture2D.Get(), debugName);

    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}

void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        DX::ThrowIfFailed(
            m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout
                )
            );
    }
    else
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout
                )
            );
    }
}

void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            )
        );

    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            )
        );

    SetDebugName(*vertexBuffer, Platform::String::Concat(debugName, "_VertexBuffer"));
    SetDebugName(*indexBuffer, Platform::String::Concat(debugName, "_IndexBuffer"));

    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}

void BasicLoader::LoadTexture(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    Platform::Array<byte>^ textureData = m_basicReaderWriter->ReadData(filename);

    CreateTexture(
        GetExtension(filename) == "dds",
        textureData->Data,
        textureData->Length,
        texture,
        textureView,
        filename
        );
}

task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateVertexShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);

    if (layout != nullptr)
    {
        CreateInputLayout(
            bytecode->Data,
            bytecode->Length,
            layoutDesc,
            layoutDescNumElements,
            layout
            );

        SetDebugName(*layout, filename);
    }
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout
                );

            SetDebugName(*layout, filename);
        }
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreatePixelShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11ComputeShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateComputeShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11ComputeShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateComputeShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11GeometryShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateGeometryShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11GeometryShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateGeometryShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
    _In_ uint32 numEntries,
    _In_reads_opt_(numStrides) const uint32* bufferStrides,
    _In_ uint32 numStrides,
    _In_ uint32 rasterizedStream,
    _Out_ ID3D11GeometryShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateGeometryShaderWithStreamOutput(
            bytecode->Data,
            bytecode->Length,
            streamOutDeclaration,
            numEntries,
            bufferStrides,
            numStrides,
            rasterizedStream,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
    _In_ uint32 numEntries,
    _In_reads_opt_(numStrides) const uint32* bufferStrides,
    _In_ uint32 numStrides,
    _In_ uint32 rasterizedStream,
    _Out_ ID3D11GeometryShader** shader
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the streamOutDeclaration array as well as the SemanticName
    // strings, both of which are pointers to data whose lifetimes may be shorter
    // than that of this method's task.
    shared_ptr<vector<D3D11_SO_DECLARATION_ENTRY>> streamOutDeclarationCopy;
    shared_ptr<vector<string>> streamOutDeclarationSemanticNamesCopy;
    if (streamOutDeclaration != nullptr)
    {
        streamOutDeclarationCopy.reset(
            new vector<D3D11_SO_DECLARATION_ENTRY>(
                streamOutDeclaration,
                streamOutDeclaration + numEntries
                )
            );

        streamOutDeclarationSemanticNamesCopy.reset(
            new vector<string>(numEntries)
            );

        for (uint32 i = 0; i < numEntries; i++)
        {
            streamOutDeclarationSemanticNamesCopy->at(i).assign(streamOutDeclaration[i].SemanticName);
        }
    }
    
    // Create a copy of the bufferStrides array, which is a pointer to data
    // whose lifetime may be shorter than that of this method's task.
    shared_ptr<vector<uint32>> bufferStridesCopy;
    if (bufferStrides != nullptr)
    {
        bufferStridesCopy.reset(
            new vector<uint32>(
                bufferStrides,
                bufferStrides + numStrides
                )
            );
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        if (streamOutDeclaration != nullptr)
        {
            // Reassign the SemanticName elements of the streamOutDeclaration array copy to
            // point to the corresponding copied strings. Performing the assignment inside the
            // lambda body ensures that the lambda will take a reference to the shared_ptr
            // that holds the data.  This will guarantee that the data is still valid when
            // CreateGeometryShaderWithStreamOutput is called.
            for (uint32 i = 0; i < numEntries; i++)
            {
                streamOutDeclarationCopy->at(i).SemanticName = streamOutDeclarationSemanticNamesCopy->at(i).c_str();
            }
        }

        DX::ThrowIfFailed(
            m_d3dDevice->CreateGeometryShaderWithStreamOutput(
                bytecode->Data,
                bytecode->Length,
                streamOutDeclaration == nullptr ? nullptr : streamOutDeclarationCopy->data(),
                numEntries,
                bufferStrides == nullptr ? nullptr : bufferStridesCopy->data(),
                numStrides,
                rasterizedStream,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11HullShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateHullShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11HullShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateHullShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11DomainShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateDomainShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11DomainShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDomainShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    Platform::Array<byte>^ meshData = m_basicReaderWriter->ReadData(filename);

    CreateMesh(
        meshData->Data,
        vertexBuffer,
        indexBuffer,
        vertexCount,
        indexCount,
        filename
        );
}

task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

MeshObject.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// MeshObject:
// This class is the generic (abstract) representation of D3D11 Indexed triangle
// list.  Each of the derived classes is just the constructor for the specific
// geometry primitive.  This abstract class does not place any requirements on
// the format of the geometry directly.
// The primary method of the MeshObject is Render.  The default implementation
// just sets the IndexBuffer, VertexBuffer, and topology to a TriangleList and
// makes a  DrawIndexed call on the context.  It assumes all other state has
// already been set on the context.

ref class MeshObject abstract
{
internal:
    MeshObject();

    virtual void Render(_In_ ID3D11DeviceContext *context);

protected private:
    Microsoft::WRL::ComPtr<ID3D11Buffer>  m_vertexBuffer;
    Microsoft::WRL::ComPtr<ID3D11Buffer>  m_indexBuffer;
    int                                   m_vertexCount;
    int                                   m_indexCount;
};
            
            
```

MeshObject.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "MeshObject.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

MeshObject::MeshObject():
    m_vertexCount(0),
    m_indexCount(0)
{
}

//--------------------------------------------------------------------------------

void MeshObject::Render(_In_ ID3D11DeviceContext *context)
{
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->DrawIndexed(m_indexCount, 0, 0);
}

//--------------------------------------------------------------------------------
            
            
```

SphereMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// SphereMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical sphere that is
// positioned at the origin with a radius of 1.0.

#include "MeshObject.h"

ref class SphereMesh: public MeshObject
{
internal:
    SphereMesh(_In_ ID3D11Device *device, uint32 segments);
};
            
            
```

SphereMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "SphereMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

SphereMesh::SphereMesh(_In_ ID3D11Device *device, uint32 segments)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    uint32 slices = segments / 2;
    uint32 numVertices = (slices + 1) * (segments + 1) + 1;
    uint32 numIndices = slices * segments * 3 * 2;

    std::vector<PNTVertex> point(numVertices);
    std::vector<uint16> index(numIndices);

    // To make the texture look right on the top and bottom of the sphere,
    // each slice will have 'segments + 1' vertices.  The top and bottom
    // vertices will all be coincident, but have different U texture cooordinates.
    uint32 p = 0;
    for (uint32 a = 0; a <= slices; a++)
    {
        float angle1 = static_cast<float>(a) / static_cast<float>(slices) * XM_PI;
        float z = cos(angle1);
        float r = sin(angle1);
        for (uint32 b = 0; b <= segments; b++)
        {
            float angle2 = static_cast<float>(b) / static_cast<float>(segments) * XM_2PI;
            point[p].position = XMFLOAT3(r * cos(angle2), r * sin(angle2), z);
            point[p].normal = point[p].position;
            point[p].textureCoordinate = XMFLOAT2((1.0f-z) / 2.0f, static_cast<float>(b) / static_cast<float>(segments));
            p++;
        }
    }
    m_vertexCount = p;

    p = 0;
    for (uint16 a = 0; a < slices; a++)
    {
        uint16 p1 = a * (segments + 1);
        uint16 p2 = (a + 1) * (segments + 1);

        // Generate two triangles for each segment around the slice.
        for (uint16 b = 0; b < segments; b++)
        {
            if (a < (slices - 1))
            {
                // For all but the bottom slice, add the triangle with one
                // vertex in the a slice and two vertices in the a + 1 slice.
                // Skip it for the bottom slice since the triangle would be
                // degenerate as all the vertices in the bottom slice are coincident.
                index[p] = b + p1;
                index[p+1] = b + p2;
                index[p+2] = b + p2 + 1;
                p = p + 3;
            }
            if (a > 0)
            {
                // For all but the top slice, add the triangle with two
                // vertices in the a slice and one vertex in the a + 1 slice.
                // Skip it for the top slice since the triangle would be
                // degenerate as all the vertices in the top slice are coincident.
                index[p] = b + p1;
                index[p+1] = b + p2 + 1;
                index[p+2] = b + p1 + 1;
                p = p + 3;
            }
        }
    }
    m_indexCount = p;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = point.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(uint16) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = index.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

CylinderMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// CylinderMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical cylinder (capped at
// both ends) that is positioned at the origin with a radius of 1.0,
// a height of 1.0 and with its axis in the +Z direction.

#include "MeshObject.h"

ref class CylinderMesh: public MeshObject
{
internal:
    CylinderMesh(_In_ ID3D11Device *device, uint32 segments);
};
            
            
```

CylinderMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "CylinderMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

CylinderMesh::CylinderMesh(_In_ ID3D11Device *device, uint32 segments)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    uint32 numVertices = 6 * (segments + 1) + 1;
    uint32 numIndices = 3 * segments * 3 * 2;

    std::vector<PNTVertex> point(numVertices);
    std::vector<uint16> index(numIndices);

    uint32 p = 0;
    // Top center point (multiple points for texture coordinates).
    for (uint32 a = 0; a <= segments; a++)
    {
        point[p].position = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Top edge of cylinder: Normals point up for lighting of top surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 1.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Top edge of cylinder: Normals point out for lighting of the side surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 1.0f);
        point[p].normal = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Bottom edge of cylinder: Normals point out for lighting of the side surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].normal = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    // Bottom edge of cylinder: Normals point down for lighting of the bottom surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, -1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    // Bottom center of cylinder: Normals point down for lighting on the bottom surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        point[p].position = XMFLOAT3(0.0f, 0.0f, 0.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, -1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    m_vertexCount = p;

    p = 0;
    for (uint16 a = 0; a < 6; a += 2)
    {
        uint16 p1 = a*(segments + 1);
        uint16 p2 = (a+1)*(segments + 1);
        for (uint16 b = 0; b < segments; b++)
        {
            if (a < 4)
            {
                index[p] = b + p1;
                index[p+1] = b + p2;
                index[p+2] = b + p2 + 1;
                p = p + 3;
            }
            if (a > 0)
            {
                index[p] = b + p1;
                index[p+1] = b + p2 + 1;
                index[p+2] = b + p1 + 1;
                p = p + 3;
            }
        }
    }
    m_indexCount = p;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = point.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(uint16) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = index.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

FaceMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// FaceMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical face defined as a
// rectangle at the origin extending 1 unit in the +X and
// 1 unit in the +Y direction.
// The face is defined to be two sided, so it is visible from either
// side.

#include "MeshObject.h"

ref class FaceMesh: public MeshObject
{
internal:
    FaceMesh(_In_ ID3D11Device *device);
};
            
            
```

FaceMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "FaceMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

FaceMesh::FaceMesh(_In_ ID3D11Device *device)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    PNTVertex target_vertices[] =
    {
        {XMFLOAT3(0.0f, 0.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(1.0f, 1.0f)},
        {XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 1.0f)},
        {XMFLOAT3(1.0f, 1.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(1.0f, 0.0f)}
    };
    WORD target_indices[] =
    {
        0, 1, 2,
        0, 2, 3,
        0, 2, 1,
        0, 3, 2
    };

    m_vertexCount = 4;
    m_indexCount = 12;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = target_vertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = target_indices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

WorldMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MeshObject.h"

// WorldCeilingMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the ceiling of the bounding cube
// of the world.
// The vertices are defined by a position, a normal and a single
// 2D texture coordinate.

ref class WorldCeilingMesh: public MeshObject
{
internal:
    WorldCeilingMesh(_In_ ID3D11Device *device);
};

// WorldFloorMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the floor of the bounding cube
// of the world.

ref class WorldFloorMesh: public MeshObject
{
internal:
    WorldFloorMesh(_In_ ID3D11Device *device);
};

// WorldWallsMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the walls of the bounding cube
// of the world.

ref class WorldWallsMesh: public MeshObject
{
internal:
    WorldWallsMesh(_In_ ID3D11Device *device);
};
            
            
```

WorldMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "WorldMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

WorldCeilingMesh::WorldCeilingMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // CEILING
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2(-0.15f, 0.0f)},
        {XMFLOAT3( 4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2( 1.25f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2(-0.15f, 2.1f)},
        {XMFLOAT3( 4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2( 1.25f, 2.1f)},
    };

    WORD cellIndices[] = {
        0, 1, 2,
        1, 3, 2,
    };

    m_vertexCount = 4;
    m_indexCount = 6;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

WorldFloorMesh::WorldFloorMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // FLOOR
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3( 4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(1.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3( 4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(1.0f, 1.5f)},
    };

    WORD cellIndices[] = {
        0, 1, 2,
        1, 3, 2,
    };

    m_vertexCount = 4;
    m_indexCount = 6;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

WorldWallsMesh::WorldWallsMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // WALL
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3( 4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(2.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3( 4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(2.0f, 1.5f)},
        // WALL
        {XMFLOAT3(4.0f,  3.0f,  6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(4.0f,  3.0f, -6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 0.0f)},
        {XMFLOAT3(4.0f, -3.0f,  6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(4.0f, -3.0f, -6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 1.5f)},
        // WALL
        {XMFLOAT3( 4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(2.0f, 0.0f)},
        {XMFLOAT3( 4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(2.0f, 1.5f)},
        // WALL
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 1.5f)},
    };

    WORD cellIndices[] = {
         0,  1,  2,
         1,  3,  2,
         4,  5,  6,
         5,  7,  6,
         8,  9, 10,
         9, 11, 10,
        12, 13, 14,
        13, 15, 14,
    };

    m_vertexCount = 16;
    m_indexCount = 24;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

            
            
```

Level.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level:
// This is an abstract class from which all of the levels of the game are derived.
// Each level potentially overrides up to four methods:
//     Initialize - (required) takes a list of objects and enables the objects that
//         are active for the level as well as setting their positions and
//         any animations associated with the objects.
//     Update - this method is called once per time step and is expected to
//         determine if the level has been completed.  The Level class provides
//         a 'standard' Update method which checks each object that is a target
//         and disables any active targets that have been hit.  It returns true
//         once there are no active targets remaining.
//     SaveState - method to save any Level specific state.  Default is defined as
//         not saving any state.
//     LoadState - method to restore any Level specific state.  Default is defined
//         as not restoring any state.

#include "GameObject.h"
#include "PersistentState.h"

ref class Level abstract
{
internal:
    virtual void Initialize(
        std::vector<GameObject^> objects
        ) = 0;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        );

    virtual void SaveState(PersistentState^ state);
    virtual void LoadState(PersistentState^ state);

    Platform::String^ Objective();
    float TimeLimit();

protected private:
    Platform::String^ m_objective;
    float             m_timeLimit;
};
            
            
```

Level.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level.h"

//----------------------------------------------------------------------

bool Level::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining*/,
    std::vector<GameObject^> objects
    )
{
    int left = 0;

    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && (*object)->Target())
        {
            if ((*object)->Hit())
            {
                (*object)->Active(false);
            }
            else
            {
                left++;
            }
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level::SaveState(PersistentState^ /* state */)
{
}

//----------------------------------------------------------------------

void Level::LoadState(PersistentState^ /* state */)
{
}

//----------------------------------------------------------------------

Platform::String^ Level::Objective()
{
    return m_objective;
}

//----------------------------------------------------------------------

float Level::TimeLimit()
{
    return m_timeLimit;
}

//----------------------------------------------------------------------            
            
```

Level1.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level1:
// This class defines the first level of the game.  There are nine active targets.
// Each of the targets is stationary and can be hit in any order.

#include "Level.h"

ref class Level1: public Level
{
internal:
    Level1();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
```

Level1.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level1.h"
#include "Face.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level1::Level1()
{
    m_timeLimit = 20.0f;
    m_objective =  "Hit each of the targets before time runs out.\nTouch to aim.  Tap in right box to fire.  Drag in left box to move.";
}

//----------------------------------------------------------------------

void Level1::Initialize(std::vector<GameObject^> objects)
{
    XMFLOAT3 position[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3( 1.5f,  0.0f, -3.0f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3( 2.0f,  0.0f,  0.0f),
        XMFLOAT3( 0.0f,  0.0f,  0.0f),
        XMFLOAT3(-2.0f,  0.0f,  0.0f)
    };

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Active(true);
                target->Target(true);
                target->Hit(false);
                target->AnimatePosition(nullptr);
                target->Position(position[targetCount]);
                targetCount++;
            }
            else
            {
                (*object)->Active(false);
            }
        }
        else
        {
            (*object)->Active(false);
        }
    }
}

//----------------------------------------------------------------------
            
            
```

Level2.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level2:
// This class defines the second level of the game.  It derives from the
// first level.  In this level, the targets must be hit in numeric order.

#include "Level1.h"

ref class Level2: public Level1
{
internal:
    Level2();
    virtual void Initialize(std::vector<GameObject^> objects) override;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;

    virtual void SaveState(PersistentState^ state) override;
    virtual void LoadState(PersistentState^ state) override;

private:
    int m_nextId;
};
            
            
```

Level2.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level2.h"
#include "Face.h"

//----------------------------------------------------------------------

Level2::Level2()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the targets in ORDER before time runs out.";
}

//----------------------------------------------------------------------

void Level2::Initialize(std::vector<GameObject^> objects)
{
    Level1::Initialize(objects);

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Target(targetCount == 0 ? true : false);
                targetCount++;
            }
        }
    }
    m_nextId = 1;
}

//----------------------------------------------------------------------

bool Level2::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining */,
    std::vector<GameObject^> objects
    )
{
    int left = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && ((*object)->TargetId() > 0))
        {
            if ((*object)->Hit()  && ((*object)->TargetId() == m_nextId))
            {
                (*object)->Active(false);
                m_nextId++;
            }
            else
            {
                left++;
            }
        }
        if ((*object)->Active() && ((*object)->TargetId() == m_nextId))
        {
            (*object)->Target(true);
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level2::SaveState(PersistentState^ state)
{
    state->SaveInt32(":NextTarget", m_nextId);
}

//----------------------------------------------------------------------

void Level2::LoadState(PersistentState^ state)
{
    m_nextId = state->LoadInt32(":NextTarget", 1);
}

//----------------------------------------------------------------------            
            
```

Level3.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level3:
// This class defines the third level of the game.  In this level, each of the
// nine targets is moving along closed paths and can be hit
// in any order.

#include "Level.h"

ref class Level3: public Level
{
internal:
    Level3();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
            
```

Level3.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level3.h"
#include "Face.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level3::Level3()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets before time runs out.";
}

//----------------------------------------------------------------------

void Level3::Initialize(std::vector<GameObject^> objects)
{
    XMFLOAT3 position[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3( 1.5f,  0.0f, -5.5f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f)
    };
    XMFLOAT3 LineList1[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-0.5f,  1.0f,  1.0f),
        XMFLOAT3(-0.5f, -2.5f,  1.0f),
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
    };
    XMFLOAT3 LineList2[] =
    {
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3(-2.0f,  2.0f, -1.5f),
        XMFLOAT3(-2.0f, -2.5f, -1.5f),
        XMFLOAT3( 1.5f, -2.5f, -1.5f),
        XMFLOAT3( 1.5f, -2.5f, -3.0f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
    };
    XMFLOAT3 LineList3[] =
    {
        XMFLOAT3(1.5f,  0.0f, -5.5f),
        XMFLOAT3(1.5f,  1.0f, -5.5f),
        XMFLOAT3(1.5f, -2.5f, -5.5f),
        XMFLOAT3(1.5f,  0.0f, -5.5f),
    };
    XMFLOAT3 LineList4[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 1.0f, -1.0f, -5.5f),
        XMFLOAT3( 1.0f,  1.0f, -5.5f),
        XMFLOAT3(-2.5f,  1.0f, -5.5f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
    };
    XMFLOAT3 LineList5[] =
    {
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 2.0f, -2.0f, -5.0f),
        XMFLOAT3( 2.0f,  1.0f, -5.0f),
        XMFLOAT3(-2.5f,  1.0f, -5.0f),
        XMFLOAT3(-2.5f, -2.0f, -5.0f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
    };
    XMFLOAT3 LineList6[] =
    {
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3(-2.5f, -2.0f, -5.5f),
        XMFLOAT3(-2.5f,  1.0f, -5.5f),
        XMFLOAT3( 1.5f,  1.0f, -5.5f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
    };

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Active(true);
                target->Target(true);
                target->Hit(false);
                target->Position(position[targetCount]);
                switch (targetCount)
                {
                case 0:
                    target->AnimatePosition(ref new AnimateLineListPosition(4, LineList1, 10.0f, true));
                    break;
                case 1:
                    target->AnimatePosition(ref new AnimateLineListPosition(6, LineList2, 15.0f, true));
                    break;
                case 2:
                    target->AnimatePosition(ref new AnimateLineListPosition(4, LineList3, 15.0f, true));
                    break;
                case 3:
                    target->AnimatePosition(ref new AnimateLineListPosition(5, LineList4, 15.0f, true));
                    break;
                case 4:
                    target->AnimatePosition(ref new AnimateLineListPosition(6, LineList5, 15.0f, true));
                    break;
                case 5:
                    target->AnimatePosition(ref new AnimateLineListPosition(5, LineList6, 15.0f, true));
                    break;
                case 6:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    break;
                case 7:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    target->AnimatePosition()->Start(3.0f);
                    break;
                case 8:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    target->AnimatePosition()->Start(6.0f);
                    break;
                }
                targetCount++;
            }
            else
            {
                target->Active(false);
            }
        }
        else
        {
            (*object)->Active(false);
        }
    }
}

//----------------------------------------------------------------------
            
            
```

Level4.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level4:
// This class defines the fourth level of the game.  It derives from the
// third level.  The targets must be hit in numeric order.

#include "Level3.h"

ref class Level4: public Level3
{
internal:
    Level4();
    virtual void Initialize(std::vector<GameObject^> objects) override;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;

    virtual void SaveState(PersistentState^ state) override;
    virtual void LoadState(PersistentState^ state) override;

private:
    int m_nextId;
};
            
            
```

Level4.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level4.h"
#include "Face.h"

//----------------------------------------------------------------------

Level4::Level4()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets in ORDER before time runs out.";
}

//----------------------------------------------------------------------

void Level4::Initialize(std::vector<GameObject^> objects)
{
    Level3::Initialize(objects);

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Target(targetCount == 0 ? true : false);
                targetCount++;
            }
        }
    }
    m_nextId = 1;
}

//----------------------------------------------------------------------

bool Level4::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining */,
    std::vector<GameObject^> objects
    )
{
    int left = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && ((*object)->TargetId() > 0))
        {
            if ((*object)->Hit() && ((*object)->TargetId() == m_nextId))
            {
                (*object)->Active(false);
                m_nextId++;
            }
            else
            {
                left++;
            }
        }
        if ((*object)->Active() && ((*object)->TargetId() == m_nextId))
        {
            (*object)->Target(true);
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level4::SaveState(PersistentState^ state)
{
    state->SaveInt32(":NextTarget", m_nextId);
}

//----------------------------------------------------------------------

void Level4::LoadState(PersistentState^ state)
{
    m_nextId = state->LoadInt32(":NextTarget", 1);
}

//----------------------------------------------------------------------
            
            
```

Level5.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level5:
// This class defines the fifth level of the game.  It derives from the
// third level.  This level introduces obstacles that move into place
// during game play.  The targets may be hit in any order.

#include "Level3.h"

ref class Level5: public Level3
{
internal:
    Level5();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
            
```

Level5.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level5.h"
#include "Cylinder.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level5::Level5()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets while avoiding the obstacles before time runs out.";
}

//----------------------------------------------------------------------

void Level5::Initialize(std::vector<GameObject^> objects)
{
    Level3::Initialize(objects);

    XMFLOAT3 obstacleStartPosition[] =
    {
        XMFLOAT3(-4.5f, -3.0f, 0.0f),
        XMFLOAT3(4.5f, -3.0f, 0.0f),
        XMFLOAT3(0.0f, 3.01f, -2.0f),
        XMFLOAT3(-1.5f, -3.0f, -6.5f),
        XMFLOAT3(1.5f, -3.0f, -6.5f)
    };
    XMFLOAT3 obstacleEndPosition[] =
    {
        XMFLOAT3(-2.0f, -3.0f, 0.0f),
        XMFLOAT3(2.0f, -3.0f, 0.0f),
        XMFLOAT3(0.0f, -3.0f, -2.0f),
        XMFLOAT3(-1.5f, -3.0f, -4.0f),
        XMFLOAT3(1.5f, -3.0f, -4.0f)
    };
    float obstacleStartTime[] =
    {
        2.0f,
        5.0f,
        8.0f,
        11.0f,
        14.0f
    };

    int obstacleCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Cylinder^ obstacle = dynamic_cast<Cylinder^>(*object))
        {
            if (obstacleCount < 5)
            {
                obstacle->Active(true);
                obstacle->Position(obstacleStartPosition[obstacleCount]);
                obstacle->AnimatePosition(
                    ref new AnimateLinePosition(
                        obstacleStartPosition[obstacleCount],
                        obstacleEndPosition[obstacleCount],
                        10.0,
                        false
                        )
                    );
                obstacle->AnimatePosition()->Start(obstacleStartTime[obstacleCount]);
                obstacleCount ++;
            }
        }
    }
}

//----------------------------------------------------------------------            
            
```

Level6.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level6:
// This class defines the sixth and final level of the game.  It derives from the
// fifth level.  In this level, the targets do not disappear when they are hit.
// The target will stay highlighted for two seconds.  As this is the last level,
// the only criteria for completion is time expiring.

#include "Level5.h"

ref class Level6: public Level5
{
internal:
    Level6();
    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;
};
            
            
```

Level6.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level6.h"

//----------------------------------------------------------------------

Level6::Level6()
{
    m_timeLimit = 20.0f;
    m_objective = "Hit as many moving targets as possible while avoiding the obstacles before time runs out.";
}

//----------------------------------------------------------------------

bool Level6::Update(
    float time,
    float elapsedTime,
    float timeRemaining,
    std::vector<GameObject^> objects
    )
{
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && (*object)->Target())
        {
            if ((*object)->Hit() && ((*object)->HitTime() < (time - 2.0f)))
            {
                (*object)->Hit(false);
            }
        }
    }
    return ((timeRemaining - elapsedTime) <= 0.0f);
}

//----------------------------------------------------------------------            
            
```

TargetTexture.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// TargetTexture:
// This is a helper class to procedurally generate textures for game
// targets.  There are two versions of the textures, one when it is
// hit and the other is when it is not.
// The class creates the necessary resources to draw the texture into
// an off screen resource at initialization time.

ref class TargetTexture
{
internal:
    TargetTexture(
        _In_ ID3D11Device1*      d3dDevice,
        _In_ ID2D1Factory1*      d2dFactory,
        _In_ IDWriteFactory1*    dwriteFactory,
        _In_ ID2D1DeviceContext* d2dContext
        );

    void CreateTextureResourceView(
        _In_ Platform::String^ name,
        _Out_ ID3D11ShaderResourceView** textureResourceView
        );
    void CreateHitTextureResourceView(
        _In_ Platform::String^ name,
        _Out_ ID3D11ShaderResourceView** textureResourceView
        );

protected private:
    Microsoft::WRL::ComPtr<ID3D11Device1>           m_d3dDevice;
    Microsoft::WRL::ComPtr<ID2D1Factory1>           m_d2dFactory;
    Microsoft::WRL::ComPtr<ID2D1DeviceContext>      m_d2dContext;
    Microsoft::WRL::ComPtr<IDWriteFactory1>         m_dwriteFactory;

    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_redBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_blueBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_greenBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_whiteBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_blackBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_yellowBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_clearBrush;

    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry1;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry2;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry3;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry4;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry5;

    Microsoft::WRL::ComPtr<IDWriteTextFormat>       m_textFormat;
};            
            
```

TargetTexture.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "TargetTexture.h"
#include "DirectXSample.h"

using namespace Microsoft::WRL;
using namespace Windows::Graphics::Display;

TargetTexture::TargetTexture(
    _In_ ID3D11Device1* d3dDevice,
    _In_ ID2D1Factory1* d2dFactory,
    _In_ IDWriteFactory1* dwriteFactory,
    _In_ ID2D1DeviceContext* d2dContext
    )
{
    m_d3dDevice = d3dDevice;
    m_d2dFactory = d2dFactory;
    m_dwriteFactory = dwriteFactory;
    m_d2dContext = d2dContext;

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Red, 1.f),
            &m_redBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::CornflowerBlue, 1.0f),
            &m_blueBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Green, 1.f),
            &m_greenBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White, 1.f),
            &m_whiteBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Black, 1.f),
            &m_blackBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Yellow, 1.f),
            &m_yellowBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White, 0.0f),
            &m_clearBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                50.0f,
                50.0f
                ),
            &m_circleGeometry1)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                100.0f,
                100.0f
                ),
            &m_circleGeometry2)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                150.0f,
                150.0f
                ),
            &m_circleGeometry3)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                200.0f,
                200.0f
                ),
            &m_circleGeometry4)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                250.0f,
                250.0f
                ),
            &m_circleGeometry5)
            );

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            425,        // fontsize
            L"en-US",   // locale
            &m_textFormat
            )
        );

    // Center the text horizontally.
    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_CENTER)
        );

    // Center the text vertically.
    DX::ThrowIfFailed(
        m_textFormat->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_CENTER)
        );
}
//----------------------------------------------------------------------
void TargetTexture::CreateTextureResourceView(
    _In_ Platform::String^ name,
    _Out_ ID3D11ShaderResourceView** textureResourceView
    )
{
    // Allocate a offscreen D3D surface for D2D to render our 2D content into
    D3D11_TEXTURE2D_DESC texDesc;
    texDesc.ArraySize = 1;
    texDesc.BindFlags = D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE;
    texDesc.CPUAccessFlags = 0;
    texDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    texDesc.Height = 512;
    texDesc.Width = 512;
    texDesc.MipLevels = 1;
    texDesc.MiscFlags = 0;
    texDesc.SampleDesc.Count = 1;
    texDesc.SampleDesc.Quality = 0;
    texDesc.Usage = D3D11_USAGE_DEFAULT;

    ComPtr<ID3D11Texture2D> offscreenTexture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(&texDesc, nullptr, &offscreenTexture)
        );

    // Convert the Direct2D texture into a Shader Resource View
    ComPtr<ID3D11ShaderResourceView> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(offscreenTexture.Get(), nullptr, &texture)
        );
#if defined(_DEBUG)
    {
        char debugName[100];
        int l = sprintf_s(debugName, sizeof(debugName) / sizeof(debugName[0]), "Simple3DGame Target %ls", name->Data());
        DX::ThrowIfFailed(
            texture->SetPrivateData(WKPDID_D3DDebugObjectName, l, debugName)
            );
    }
#endif

    ComPtr<IDXGISurface> dxgiSurface;
    DX::ThrowIfFailed(
        offscreenTexture.As(&dxgiSurface)
        );

    // Create a D2D render target which can draw into our offscreen D3D
    // surface. Given that we use a constant size for the texture, we
    // fix the DPI at 96.

    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = 96;
    properties.dpiY = 96;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW;
    properties.colorContext = nullptr;

    ComPtr<ID2D1Bitmap1> renderTarget;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiSurface.Get(),
            &properties,
            &renderTarget
            )
        );

    m_d2dContext->SetTarget(renderTarget.Get());
    float saveDpiX;
    float saveDpiY;

    m_d2dContext->GetDpi(&saveDpiX, &saveDpiY);
    m_d2dContext->SetDpi(96.0f, 96.0f);

    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());

    D2D1_SIZE_F renderTargetSize = renderTarget->GetSize();

    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::White));
    m_d2dContext->FillGeometry(m_circleGeometry5.Get(), m_redBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry4.Get(), m_blueBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry3.Get(), m_greenBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry2.Get(), m_yellowBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry1.Get(), m_blackBrush.Get());
    m_d2dContext->DrawText(
        name->Data(),
        name->Length(),
        m_textFormat.Get(),
        D2D1::RectF(0, 0, renderTargetSize.width, renderTargetSize.height),
        m_whiteBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->SetTarget(nullptr);
    m_d2dContext->SetDpi(saveDpiX, saveDpiY);

    *textureResourceView = texture.Detach();
}
//----------------------------------------------------------------------
void TargetTexture::CreateHitTextureResourceView(
    _In_ Platform::String^ name,
    _Out_ ID3D11ShaderResourceView** textureResourceView
    )
{
    // Allocate a offscreen D3D surface for D2D to render our 2D content into
    D3D11_TEXTURE2D_DESC texDesc;
    texDesc.ArraySize = 1;
    texDesc.BindFlags = D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE;
    texDesc.CPUAccessFlags = 0;
    texDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    texDesc.Height = 512;
    texDesc.Width = 512;
    texDesc.MipLevels = 1;
    texDesc.MiscFlags = 0;
    texDesc.SampleDesc.Count = 1;
    texDesc.SampleDesc.Quality = 0;
    texDesc.Usage = D3D11_USAGE_DEFAULT;

    ComPtr<ID3D11Texture2D> offscreenTexture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(&texDesc, nullptr, &offscreenTexture)
        );

    // Convert the Direct2D texture into a Shader Resource View.
    ComPtr<ID3D11ShaderResourceView> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(offscreenTexture.Get(), nullptr, &texture)
        );
#if defined(_DEBUG)
    {
        char debugName[100];
        int l = sprintf_s(debugName, sizeof(debugName) / sizeof(debugName[0]), "Simple3DGame HitTarget %ls", name->Data());
        DX::ThrowIfFailed(
            texture->SetPrivateData(WKPDID_D3DDebugObjectName, l, debugName)
            );
    }
#endif

    ComPtr<IDXGISurface> dxgiSurface;
    DX::ThrowIfFailed(
        offscreenTexture.As(&dxgiSurface)
        );

    // Create a D2D render target which can draw into our offscreen D3D
    // surface. Given that we use a constant size for the texture, we
    // fix the DPI at 96.

    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = 96;
    properties.dpiY = 96;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW;
    properties.colorContext = nullptr;

    ComPtr<ID2D1Bitmap1> renderTarget;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiSurface.Get(),
            &properties,
            &renderTarget
            )
        );

    m_d2dContext->SetTarget(renderTarget.Get());
    float saveDpiX;
    float saveDpiY;

    m_d2dContext->GetDpi(&saveDpiX, &saveDpiY);
    m_d2dContext->SetDpi(96.0f, 96.0f);

    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());

    D2D1_SIZE_F renderTargetSize = renderTarget->GetSize();

    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::Black));
    m_d2dContext->FillGeometry(m_circleGeometry5.Get(), m_yellowBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry4.Get(), m_greenBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry3.Get(), m_blueBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry2.Get(), m_redBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry1.Get(), m_whiteBrush.Get());
    m_d2dContext->DrawText(
        name->Data(),
        name->Length(),
        m_textFormat.Get(),
        D2D1::RectF(0, 0, renderTargetSize.width, renderTargetSize.height),
        m_blackBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->SetTarget(nullptr);
    m_d2dContext->SetDpi(saveDpiX, saveDpiY);

    *textureResourceView = texture.Detach();
}
//----------------------------------------------------------------------
            
            
```

Material.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Material:
// This class maintains the properties that represent how an object will
// look when it is rendered.  This includes the color of the object, the
// texture used to render the object, and the vertex and pixel shader that
// should be used for rendering.
// The RenderSetup method sets the appropriate values into the constantBuffer
// and calls the appropriate D3D11 context methods to set up the rendering pipeline
// in the graphics hardware.

#include "ConstantBuffers.h"

ref class Material
{
internal:
    Material(
        DirectX::XMFLOAT4 meshColor,
        DirectX::XMFLOAT4 diffuseColor,
        DirectX::XMFLOAT4 specularColor,
        float specularExponent,
        _In_ ID3D11ShaderResourceView* textureResourceView,
        _In_ ID3D11VertexShader* vertexShader,
        _In_ ID3D11PixelShader* pixelShader
        );

    void RenderSetup(
        _In_ ID3D11DeviceContext* context,
        _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
        );

    void SetTexture(_In_ ID3D11ShaderResourceView* textureResourceView)
    {
        m_textureRV = textureResourceView;
    }

protected private:
    DirectX::XMFLOAT4   m_meshColor;
    DirectX::XMFLOAT4   m_diffuseColor;
    DirectX::XMFLOAT4   m_hitColor;
    DirectX::XMFLOAT4   m_specularColor;
    float               m_specularExponent;

    Microsoft::WRL::ComPtr<ID3D11VertexShader>       m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>        m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView> m_textureRV;
};
            
            
```

Material.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Material.h"
#include "GameConstants.h"

using namespace DirectX;

//--------------------------------------------------------------------------------

Material::Material(
    XMFLOAT4 meshColor,
    XMFLOAT4 diffuseColor,
    XMFLOAT4 specularColor,
    float specularExponent,
    _In_ ID3D11ShaderResourceView* textureResourceView,
    _In_ ID3D11VertexShader* vertexShader,
    _In_ ID3D11PixelShader* pixelShader
    )
{
    m_meshColor = meshColor;
    m_diffuseColor = diffuseColor;
    m_specularColor = specularColor;
    m_specularExponent = specularExponent;

    m_vertexShader = vertexShader;
    m_pixelShader = pixelShader;
    m_textureRV = textureResourceView;
}

//--------------------------------------------------------------------------------

void Material::RenderSetup(
    _In_ ID3D11DeviceContext* context,
    _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
    )
{
    constantBuffer->meshColor = m_meshColor;
    constantBuffer->specularColor = m_specularColor;
    constantBuffer->specularPower = m_specularExponent;
    constantBuffer->diffuseColor = m_diffuseColor;

    context->PSSetShaderResources(0, 1, m_textureRV.GetAddressOf());
    context->VSSetShader(m_vertexShader.Get(), nullptr, 0);
    context->PSSetShader(m_pixelShader.Get(), nullptr, 0);
}            
            
```

> **Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados


* [Criar um jogo UWP simples com o DirectX](tutorial--create-your-first-metro-style-directx-game.md)

 

 







<!--HONumber=Jun16_HO4-->


