---
title: Adicionando conteúdo visual ao exemplo do Marble Maze
description: Este documento descreve como o jogo Marble Maze usa Direct3D e Direct2D no ambiente do aplicativo da Plataforma Universal do Windows (UWP) para que você aprenda e adapte os parâmetros quando trabalhar com o conteúdo do seu próprio jogo.
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.date: 09/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, amostra, directx, elementos gráficos
ms.localizationpriority: medium
ms.openlocfilehash: ce62e065170349523062fbd42d867edfed63f47c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369078"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Adicionando conteúdo visual ao exemplo do Marble Maze




Este documento descreve como o jogo Marble Maze usa Direct3D e Direct2D no ambiente do aplicativo da Plataforma Universal do Windows (UWP) para que você aprenda e adapte os parâmetros quando trabalhar com o conteúdo do seu próprio jogo. Para saber mais sobre como os componentes visuais do jogo se encaixam na estrutura geral do aplicativo Marble Maze, consulte [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md).

Seguimos estas três etapas básicas conforme desenvolvíamos os aspectos visuais do Marble Maze:

1.  Crie uma estrutura básica que inicialize os ambientes Direct3D e Direct2D.
2.  Use programas de edição de modelo e de imagem para criar os ativos 2D e 3D que aparecem no jogo.
3.  Certifique-se de que ativos 2D e 3D corretamente carregar e aparecem no jogo.
4.  Integre sombreadores de vértice e pixels capazes de melhorar a qualidade visual dos ativos do jogo.
5.  Integre a lógica do jogo, como animações e entradas do usuário.

Podemos também concentra-se pela primeira vez na adição de ativos 3D e, em seguida, em ativos 2D. Por exemplo, nós nos concentramos na lógica central do jogo antes de adicionamos o sistema de menus e temporizador.

Também precisamos repetir algumas dessas etapas várias vezes durante o processo de desenvolvimento. Por exemplo, como fizemos alterações para os modelos de malha e marble, precisamos também alterar algum código de sombreador que dá suporte a esses modelos.

> [!NOTE]
> O exemplo de código que corresponde a este documento pode ser encontrado no [Exemplo do jogo Marble Maze em DirectX](https://go.microsoft.com/fwlink/?LinkId=624011).

 
Seguem alguns dos principais pontos discutidos neste documento para quando você trabalhar com DirectX e conteúdo visual de jogos, ou seja, quando inicializar as bibliotecas de elementos gráficos do DirectX, carregar recursos de cena e atualizar e renderizar a cena.

-   Em geral, adicionar conteúdo de jogo envolve várias etapas. Muitas vezes, essas etapas também exigem iteração. Os desenvolvedores de jogos geralmente se concentrar primeiro na adição de conteúdo de jogo 3D e, em seguida, na adição de conteúdo 2D.
-   Conquiste mais clientes e proporcione uma ótima experiência a todos eles dando suporte para a maior variedade possível de componentes de hardware gráfico.
-   Separe transparentemente os formatos de tempo de design e de tempo de execução. Estruture seus ativos de tempo de design para maximizar a flexibilidade e permitir rápidas iterações no conteúdo. Formate e compacte seus ativos para que eles sejam carregados e processados da maneira mais eficiente possível em tempo de execução.
-   Você cria os dispositivos Direct3D e Direct2D em um aplicativo UWP de maneira bastante semelhante à que costuma fazer em um aplicativo de área de trabalho clássico do Windows. Uma diferença importante é a forma como a cadeia de troca é associada à janela de saída.
-   Ao criar seu jogo, certifique-se de que o formato de malha escolhido dê suporte aos cenários principais. Por exemplo, se o seu jogo requer colisão, verifique se é possível obter dados de colisão a partir das suas malhas.
-   Separe a lógica do jogo da lógica de renderização atualizando primeiramente todos os objetos de cena antes de renderizá-los.
-   Você normalmente desenha seus objetos de cena 3D e, em seguida, qualquer 2D objetos que aparecem na frente da cena.
-   Sincronize o desenho com o espaço em branco vertical para garantir que o jogo não perca tempo desenhando quadros que não chegarão a aparecer na tela. Um *vertical está em branco* é o tempo entre quando um quadro termina o desenho para o monitor e começa o próximo quadro.

## <a name="getting-started-with-directx-graphics"></a>Introdução a elementos gráficos DirectX


Quando planejamos o jogo Marble Maze Universal Windows Platform (UWP), escolhemos o C++ e Direct3D 11.1 porque eles são excelentes opções para criar jogos 3D que exigem o máximo controle sobre a renderização e de alto desempenho. O DirectX 11.1 dá suporte para componentes de hardware do DirectX 9, até o DirectX 11 e, portanto, pode ajudar você a conquistar mais clientes com maior eficiência, pois não é necessário reescrever o código para cada uma das versões anteriores do DirectX.

O Marble Maze usa Direct3D 11.1 para renderizar os ativos de jogos 3D, ou seja, o marble e labirinto. Também Marble Maze usa Direct2D, DirectWrite e Windows Imaging Component (WIC) para desenhar os ativos de jogos 2D, como os menus e o temporizador.

O processo de desenvolvimento de jogos requer planejamento. Se você for novo no DirectX graphics, é recomendável que você leia [DirectX: Introdução ao](directx-getting-started.md) de se familiarizar com os conceitos básicos da criação de um jogo UWP DirectX. Conforme você lê este documento e trabalhar com o código de origem do Marble Maze, você pode consultar os seguintes recursos para obter informações mais detalhadas sobre elementos gráficos do DirectX:

-   [Direct3D 11 gráficos](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11): Descreve o Direct3D 11, um poderoso acelerada por hardware, gráficos 3D API para a renderização de geometria 3D na plataforma Windows.
-   [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal): Descreve o Direct2D, uma API de gráficos 2D, acelerada por hardware que fornece alto desempenho e renderização de alta qualidade para geometria 2D, bitmaps e texto.
-   [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal): Descreve o DirectWrite, que dá suporte à renderização de texto de alta qualidade.
-   [Windows Imaging Component](https://docs.microsoft.com/windows/desktop/wic/-wic-lh): Descreve o WIC, uma plataforma extensível que fornece a API de nível inferior para imagens digitais.

### <a name="feature-levels"></a>Níveis de recursos

Direct3D 11 introduz um paradigma denominado *níveis de recursos*. Um nível de recurso é um conjunto bem definido de funcionalidades de GPU. Use níveis de recursos para direcionar seu jogo para execução em versões anteriores de componentes de hardware Direct3D. O Marble Maze dá suporte ao nível de recurso 9.1, pois não necessita dos recursos avançados dos níveis mais altos. Recomendamos que você dê suporte para a maior variedade possível de componentes de hardware e dimensione o conteúdo do seu jogo para que tanto os clientes com os computadores mais simples até aqueles com os computadores mais modernos possam ter uma grande experiência. Para saber mais sobre níveis de recursos, consulte [Direct3D 11 em componentes de hardware inferiores](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel).

## <a name="initializing-direct3d-and-direct2d"></a>Inicializando o Direct3D e o Direct2D


Um dispositivo representa o adaptador de vídeo. Você cria os dispositivos Direct3D e Direct2D em um aplicativo UWP de maneira bastante semelhante à que costuma fazer em um aplicativo de área de trabalho clássico do Windows. A principal diferença é em como você conecta a cadeia de troca do Direct3D ao sistema de janelas.

A classe **DeviceResources** é uma base para o gerenciamento do Direct3D e do Direct2D. Essa classe manipula a infraestrutura geral, ativos não específicos de jogo. Marble Maze define o **MarbleMazeMain** classe para manipular ativos específicos de jogos, que tem uma referência para um **DeviceResources** objeto para dar a ela acesso ao Direct3D e Direct2D.

Durante a inicialização, o **DeviceResources** construtor cria recursos independentes de dispositivo e os dispositivos Direct3D e Direct2D.

```cpp
// Initialize the Direct3D resources required to run. 
DX::DeviceResources::DeviceResources() :
    m_screenViewport(),
    m_d3dFeatureLevel(D3D_FEATURE_LEVEL_9_1),
    m_d3dRenderTargetSize(),
    m_outputSize(),
    m_logicalSize(),
    m_nativeOrientation(DisplayOrientations::None),
    m_currentOrientation(DisplayOrientations::None),
    m_dpi(-1.0f),
    m_deviceNotify(nullptr)
{
    CreateDeviceIndependentResources();
    CreateDeviceResources();
}
```

A classe **DeviceResources** separa essa funcionalidade para que ela possa responder mais facilmente quando o ambiente mudar. Por exemplo, ela chama o método **CreateWindowSizeDependentResources** quando o tamanho da janela muda.

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Inicializando as fábricas do Direct2D, DirectWrite e WIC

O método **DeviceResources::CreateDeviceIndependentResources** cria as fábricas do Direct2D, DirectWrite e WIC. Nos elementos gráficos do DirectX, as fábricas são os pontos de partida na criação de recursos de elementos gráficos. Especifica Marble Maze **D2D1\_fábrica\_tipo\_único\_SEGMENTADAS** porque ele executa todos os desenhos no thread principal.

```cpp
// These are the resources required independent of hardware. 
void DX::DeviceResources::CreateDeviceIndependentResources()
{
    // Initialize Direct2D resources.
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
    // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    // Initialize the Direct2D Factory.
    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory2),
            &options,
            &m_d2dFactory
            )
        );

    // Initialize the DirectWrite Factory.
    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory2),
            &m_dwriteFactory
            )
        );

    // Initialize the Windows Imaging Component (WIC) Factory.
    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory2,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>Criando os dispositivos Direct3D e Direct2D

O **DeviceResources::CreateDeviceResources** chamadas de método [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) para criar o objeto de dispositivo que representa o adaptador de vídeo do Direct3D. Porque o Marble Maze dá suporte ao nível de recurso 9.1 e acima, o **DeviceResources::CreateDeviceResources** método especifica níveis 9.1 por meio de 11.1 na **featureLevels** matriz. O Direct3D passa pela lista em ordem e dá ao aplicativo o primeiro nível de recurso que estiver disponível. Portanto, o **D3D\_recurso\_nível** as entradas de matriz são listadas do mais alto ao mais baixo para que o aplicativo obterá o nível de recurso mais alto disponível. O método **DeviceResources::CreateDeviceResources** obtém o dispositivo Direct3D 11.1 consultando o dispositivo Direct3D 11 que volta do **D3D11CreateDevice**.

```cpp
// This flag adds support for surfaces with a different color channel ordering
// than the API default. It is required for compatibility with Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
    if (DX::SdkLayersAvailable())
    {
        // If the project is in a debug build, enable debugging via SDK Layers 
        // with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
    }
#endif

// This array defines the set of DirectX hardware feature levels this app will support.
// Note the ordering should be preserved.
// Don't forget to declare your application's minimum required feature level in its
// description.  All applications are assumed to support 9.1 unless otherwise stated.
D3D_FEATURE_LEVEL featureLevels[] =
{
    D3D_FEATURE_LEVEL_11_1,
    D3D_FEATURE_LEVEL_11_0,
    D3D_FEATURE_LEVEL_10_1,
    D3D_FEATURE_LEVEL_10_0,
    D3D_FEATURE_LEVEL_9_3,
    D3D_FEATURE_LEVEL_9_2,
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;

HRESULT hr = D3D11CreateDevice(
    nullptr,                    // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,   // Create a device using the hardware graphics driver.
    0,                          // Should be 0 unless the driver is D3D_DRIVER_TYPE_SOFTWARE.
    creationFlags,              // Set debug and Direct2D compatibility flags.
    featureLevels,              // List of feature levels this app can support.
    ARRAYSIZE(featureLevels),   // Size of the list above.
    D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for UWP apps.
    &device,                    // Returns the Direct3D device created.
    &m_d3dFeatureLevel,         // Returns feature level of device created.
    &context                    // Returns the device immediate context.
    );

if (FAILED(hr))
{
    // If the initialization fails, fall back to the WARP device.
    // For more information on WARP, see:
    // https://go.microsoft.com/fwlink/?LinkId=286690
    DX::ThrowIfFailed(
        D3D11CreateDevice(
            nullptr,
            D3D_DRIVER_TYPE_WARP, // Create a WARP device instead of a hardware device.
            0,
            creationFlags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &device,
            &m_d3dFeatureLevel,
            &context
            )
        );
}

// Store pointers to the Direct3D 11.1 API device and immediate context.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );

DX::ThrowIfFailed(
    context.As(&m_d3dContext)
    );
```

O método **DeviceResources::CreateDeviceResources** cria, em seguida, o dispositivo Direct2D. O Direct2D usa o DirectX Graphic Infrastructure (DXGI) da Microsoft para interoperar com o Direct3D. O DXGI permite que superfícies de memória de vídeo sejam compartilhadas entre tempos de execução de elementos gráficos. O Marble Maze usa o dispositivo DXGI subjacente do dispositivo Direct3D para criar o dispositivo Direct2D a partir do alocador Direct2D.

```cpp
// Create the Direct2D device object and a corresponding context.
ComPtr<IDXGIDevice3> dxgiDevice;
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

Para saber mais sobre o DXGI e a interoperabilidade entre Direct2D e Direct3D, consulte [DXGI Overview](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi) e [Direct2D and Direct3D Interoperability Overview](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-and-direct3d-interoperation-overview).

### <a name="associating-direct3d-with-the-view"></a>Associando o Direct3D com a exibição

O método **DeviceResources::CreateWindowSizeDependentResources** cria os recursos de elementos gráficos que dependem de um dado tamanho de janela, como a cadeia de troca e os destinos de renderização do Direct3D e do Direct2D. Uma característica importante que diferencia um aplicativo UWP DirectX de um aplicativo de área de trabalho é a forma como a cadeia de troca é associada à janela de saída. Uma cadeia de troca é responsável por exibir o buffer para o qual o dispositivo é renderizado no monitor. [Marble estrutura de aplicativo do labirinto](marble-maze-application-structure.md) descreve como o sistema de janelas para um aplicativo UWP difere de um aplicativo da área de trabalho. Como um aplicativo UWP não funciona com [HWND](https://docs.microsoft.com/windows/desktop/WinProg/windows-data-types) objetos, Marble Maze deve usar o [IDXGIFactory2::CreateSwapChainForCoreWindow](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) método para associar a saída do dispositivo para o modo de exibição. O exemplo a seguir mostra a parte do método **DeviceResources::CreateWindowSizeDependentResources** que cria a cadeia de troca.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &m_swapChain
        )
    );
```

Para minimizar o consumo de energia, que é importante fazer em dispositivos alimentado por bateria, como laptops e tablets, o **DeviceResources::CreateWindowSizeDependentResources** chamadas de método a [IDXGIDevice1:: SetMaximumFrameLatency](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) método para garantir que o jogo será renderizado somente após o espaço em branco vertical. Sincronizando com o espaço em branco vertical é descrita mais detalhadamente na seção [apresentando a cena](#presenting-the-scene) neste documento.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both 
// reduces latency and ensures that the application will only render after each
// VSync, minimizing power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

O método **DeviceResources::CreateWindowSizeDependentResources** inicializa os recursos de elementos gráficos de forma que funcione para a maioria dos jogos.

> [!NOTE]
> O termo *exibição* tem um significado diferente no tempo de execução do Windows que ele tem no Direct3D. No Windows Runtime, uma exibição refere-se ao conjunto de configurações de interface do usuário para um aplicativo, incluindo a área de exibição e os comportamentos de entrada, além do thread utilizado para processamento. Você pode especificar as configurações necessárias ao criar uma exibição. O processo de configurar uma exibição de aplicativo é descrito em [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md).
> No Direct3D, o termo exibição tem vários significados. Um modo de exibição de recurso define os sub-recursos que um recurso pode acessar. Por exemplo, quando um objeto de textura está associado a uma exibição de recurso de sombreador, esse sombreador pode acessar a textura mais tarde. Uma das vantagens de uma exibição de recurso é que você pode interpretar os dados de diferentes maneiras em diferentes estágios no pipeline de renderização. Para obter mais informações sobre modos de exibição de recursos, consulte [modos de exibição de recurso](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-intro).
> Quando usada no contexto de uma transformação de exibição ou de uma matriz de transformação de exibição, uma exibição refere-se à localização e à orientação da câmara. Uma transformação de exibição realoca objetos ao redor da posição e da orientação da câmera. Para saber mais sobre transformações de exibição, consulte [Transformação de exibição (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/view-transform). Como o Marble Maze usa exibições de matriz e recurso é descrito com mais detalhes neste tópico.

 

## <a name="loading-scene-resources"></a>Carregando recursos de cena


Marble Maze usa a **BasicLoader** classe, que é declarado no **Basicloader**, para carregar as texturas e sombreadores. Marble Maze usa a **SDKMesh** malhas de classe para carregar o 3D para o Labirinto e o marble.

Para garantir um aplicativo com respostas rápidas, o Marble Maze carrega recursos de cena de forma assíncrona ou no fundo. Como os ativos são carregados em segundo plano, seu jogo pode responder a eventos de janela. Esse processo é explicado com mais detalhes em [Carregando ativos de jogo em segundo plano](marble-maze-application-structure.md#loading-game-assets-in-the-background), neste guia.

###  <a name="loading-the-2d-overlay-and-user-interface"></a>Ao carregar a interface de usuário e a sobreposição 2D

No Marble Maze, a sobreposição é a imagem que aparece na parte superior da tela. A sobreposição sempre aparece na frente da cena. Marble Maze, a sobreposição contém o logotipo do Windows e a cadeia de caracteres de texto **exemplo de jogo de labirinto de mármore do DirectX**. O gerenciamento da sobreposição é executado o **SampleOverlay** classe, que é definido no **SampleOverlay.h**. Apesar de usarmos a sobreposição como parte das amostras do Direct3D, você pode adaptar esse código para exibir qualquer imagem que apareça na frente da sua cena.

Um aspecto importante da sobreposição é que, porque seu conteúdo não alterar, o **SampleOverlay** classe desenha, ou os caches, seu conteúdo para um [ID2D1Bitmap1](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1bitmap1) objeto durante a inicialização. Neste momento, a classe **SampleOverlay** só desenhar o bitmap na tela. Assim, rotinas caras como a de desenho de texto não precisam ser executadas para cada quadro.

A interface do usuário (IU) consiste em componentes 2D, como menus e heads-up exibe (HUDs), que aparecem na frente de sua cena. O Marble Maze define os seguintes elementos de interface do usuário:

-   Itens de menu que permitem ao usuário iniciar o jogo ou visualizar as pontuações mais altas.
-   Um temporizador com uma contagem regressiva de três segundos antes de o jogo começar.
-   Um temporizador que controla o tempo de jogo decorrido.
-   Uma tabela que lista os tempos de finalização mais rápidos.
-   Texto que lê **pausado** quando o jogo é pausado.

Marble Maze define elementos de interface do usuário do jogo específico em **UserInterface.h**. O Marble Maze define a classe **ElementBase** como um tipo base para todos os elementos de interface do usuário. A classe **ElementBase** define atributos como tamanho, posição, alinhamento e visibilidade de um elemento de interface do usuário. Ela também controla como os elementos são atualizados e renderizados.

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

Ao fornecer uma classe de base comum para elementos de interface do usuário, a classe **UserInterface**, que gerencia a interface do usuário, precisa apenas armazenar uma coleção de objetos **ElementBase**, o que simplifica o gerenciamento de interface do usuário que é reutilizável. O Marble Maze define tipos que derivam do **ElementBase** que implementam comportamentos específicos do jogo. Por exemplo, **HighScoreTable** define o comportamento da tabela de recordes. Para saber mais sobre esses tipos, vá ao código-fonte.

> [!NOTE]
> Porque o XAML permite que você crie com mais facilidade interfaces do usuário complexas, como as encontradas no simulação e jogos de estratégia, considere usar XAML para definir sua interface do usuário. Para obter informações sobre como desenvolver uma interface do usuário em XAML em um jogo UWP do DirectX, consulte [estender o exemplo do jogo](tutorial-resources.md), que se refere a 3D DirectX solução de exemplo do jogo.

 

###  <a name="loading-shaders"></a>Carregando sombreadores

O Marble Maze usa o método **BasicLoader::LoadShader** para carregar um sombreador de um arquivo.

Sombreadores são a unidade fundamental de Programação de GPU em jogos hoje em dia. Quase todo o processamento de gráficos em 3D é orientada por meio de sombreadores, seja de iluminação de cena e transformação de modelo ou processamento da estrutura de caractere para o mosaico de geometria mais complexa. Para saber mais sobre o modelo de programação de sombreadores, consulte [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl).

O Marble Maze usa sombreadores de vértice e de pixel. Um sombreador de vértice sempre opera em um vértice de entrada e produz um vértice como saída. Um sombreador de pixel usa valores numéricos, dados de textura, valores interpolados por vértice e outros dados para produzir uma cor de pixel como saída. Como um sombreador transforma um elemento de cada vez, o hardware de elementos gráficos que fornece vários pipelines de sombreador pode processar conjuntos de elementos em paralelo. O número de pipelines paralelos que estão disponíveis para a GPU pode ser muito maior que o número disponível para a CPU. Portanto, até os sombreadores básicos podem melhorar muito a taxa de transferência.

O **MarbleMazeMain::LoadDeferredResources** método carrega sombreador de um vértice e sombreador de pixel de um depois que ele carrega a sobreposição. As versões de tempo de design desses sombreadores são definidas em **BasicVertexShader.hlsl** e **BasicPixelShader.hlsl**, respectivamente. O Marble Maze aplica esses sombreadores à bola e ao labirinto durante a fase de renderização.

O projeto do Marble Maze inclui tanto a versão .hlsl (o formato em tempo de design) quanto a versão .cso (o formato em tempo de execução) dos arquivos de sombreador. Em tempo de compilação, o Visual Studio usa o compilador de efeitos fxc.exe para compilar seu arquivo de origem .hlsl em um sombreador .cso. Para saber mais sobre a ferramenta de compilador de efeitos, consulte [Ferramenta de Compilador de Efeitos](https://docs.microsoft.com/windows/desktop/direct3dtools/fxc).

O sombreador de vértice usa as matrizes fornecidas de modelo, exibição e projeção para transformar a geometria de entrada. Os dados de posição da geometria de entrada são transformados e processados duas vezes: uma vez no espaço da tela, o que é necessário para o processamento, e novamente no espaço universal para permitir que o sombreador de pixel realize cálculos de iluminação. O vetor da normal da superfície é transformado em espaço universal, que também é usado pelo sombreador de pixel para iluminação. As coordenadas de textura são transmitidas em formato inalterado ao sombreador de pixel.

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

O sombreador de pixel recebe a saída do sombreador de vértice como entrada. Este sombreador realiza cálculos de iluminação para simular uma luz de projetor com contornos suaves que paira sobre o labirinto e fica alinhada à posição do mármore. A iluminação é mais forte para superfícies que apontam diretamente para a luz. O componente difuso é afunilado até zero à medida que a normal da superfície se torna perpendicular à luz, e o termo ambiente diminui à medida que a normal aponta para longe da luz. Os pontos mais próximos da bolinha (e, portanto, mais próximos do centro da luz do projetor) são iluminados com mais intensidade. No entanto, a iluminação é modulada para pontos abaixo da bolinha para simular uma sombra suave. Em um ambiente real, um objeto como a bolinha branca refletiria difusamente a luz de projetor em outros objetos na cena. Isso é aproximado para as superfícies que estão em exibição na metade iluminada da bolinha. Os fatores de iluminação adicionais estão a um ângulo e a uma distância relativos à bolinha. A cor do pixel resultante é uma composição da amostra da textura com o resultado dos cálculos de iluminação.

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> [!WARNING]
> O sombreador de pixel compilado contém 32 instruções aritméticas e instruções de textura 1. Esse sombreador terá um bom desempenho em desktops e em tablets modernos. No entanto, um computador mais simples pode não ser capaz de processar esse sombreador e ainda fornecer uma taxa de quadros interativa. Considere o hardware típico de seu público alvo e projete seus sombreadores para atender às capacidades do mesmo.

 

O **MarbleMazeMain::LoadDeferredResources** método usa o **BasicLoader::LoadShader** método para carregar os sombreadores. O exemplo a seguir carrega o sombreador de vértice. É o formato de tempo de execução para este sombreador **BasicVertexShader.cso**. O **m\_vertexShader** variável de membro é um [ID3D11VertexShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader) objeto.

```cpp
BasicLoader^ loader = ref new BasicLoader(m_deviceResources->GetD3DDevice());

D3D11_INPUT_ELEMENT_DESC layoutDesc [] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
m_vertexStride = 44; // must set this to match the size of layoutDesc above

Platform::String^ vertexShaderName = L"BasicVertexShader.cso";
loader->LoadShader(
    vertexShaderName,
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

O **m\_inputLayout** variável de membro é um [ID3D11InputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout) objeto. O objeto do layout de entrada encapsula o estado do estágio de assembler de entrada (IA) Um trabalho do estágio IA é deixar os sombreadores mais eficazes usando valores gerados pelo sistema, também conhecidos como *semântica*, para processar apenas os primitivos ou vértices que ainda não foram processados.

Use o [ID3D11Device::CreateInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) método para criar um layout de entrada de uma matriz de descrições de elemento de entrada. A matriz contém um ou mais elementos de entrada, e cada um destes descreve um elemento de dados de vértice a partir de um buffer de vértices. O conjunto inteiro de descrições de elementos de entrada descreve todos os elementos de dados de vértice de todos os buffers de vértices que serão associados ao estágio IA. 

**layoutDesc** no código acima o trecho de código mostra a descrição de layout que Marble Maze usa. Essa descrição de layout descreve um buffer de vértices que contém quatro elementos de dados de vértice. As partes importantes de cada entrada na matriz são o nome da semântica, o formato de data e o deslocamento de byte. Por exemplo, o elemento **POSITION** especifica a posição do vértice no espaço de objeto. Ele inicia no deslocamento de byte 0 e contém três componentes de ponto flutuante (para um total de 12 bytes). O elemento **NORMAL** especifica o vetor normal. Ele inicia no deslocamento de byte 12 porque aparece logo depois de **POSITION** no layout, que exige 12 bytes. O elemento **NORMAL** contém um inteiro sem sinal de 32 bits e quatro componentes.

Compare o layout de entrada com a estrutura **sVSInput** que foi definida pelo sombreador de vértice, como mostrado no exemplo a seguir. A estrutura **sVSInput** define os elementos **POSITION**, **NORMAL**, e **TEXCOORD0**. O tempo de execução do DirectX mapeia cada elemento no layout para inserir a estrutura definida pelo sombreador.

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

O documento [Semantics](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) descreve cada semântica disponível com mais detalhes.

> [!NOTE]
> Em um layout, você pode especificar componentes adicionais que não são usados para habilitar vários sombreadores compartilhar o mesmo layout. Por exemplo, o elemento **TANGENT** não é usado pelo sombreador. Você pode usar o elemento **TANGENT** se quiser testar técnicas como mapeamento normal. Usando o mapeamento de normais, também conhecido como mapeamento de impacto, você pode criar o efeito de impactos nas superfícies de objetos. Para saber mais sobre o mapeamento de impacto, consulte [Mapeamento de impacto (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/bump-mapping).

 

Para obter mais informações sobre o estágio de assembly de entrada, consulte [estágio do Assembler de entrada](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage) e [Introdução ao estágio de entrada do Assembler](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage-getting-started).

O processo de usar os sombreadores de vértice e de pixel para renderizar a cena está descrito na secção [Renderizando a cena](#rendering-the-scene), mais adiante neste documento.

### <a name="creating-the-constant-buffer"></a>Criando o buffer constante

O buffer Direct3D agrupa uma coleção de dados. Um buffer constante é uma espécie de buffer que pode ser usado para transmitir dados a sombreadores. O Marble Maze usa um buffer constante para armazenar a exibição modelo (ou do mundo) e as matrizes de projeção para um objeto de cena ativo.

A exemplo a seguir mostra como o **MarbleMazeMain::LoadDeferredResources** método cria um buffer de constantes que mais tarde conterá dados de matriz. O exemplo cria um **D3D11\_BUFFER\_DESC** estrutura que usa o **D3D11\_ASSOCIAR\_constante\_BUFFER** sinalizador como Especifique o uso como um buffer de constantes. Neste exemplo, em seguida, passa essa estrutura para o [ID3D11Device::CreateBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) método. O **m\_constantBuffer** variável é uma [ID3D11Buffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) objeto.

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};

// Multiple of 16 bytes
constantBufferDesc.ByteWidth = ((sizeof(ConstantBuffer) + 15) / 16) * 16;

constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;

// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &constantBufferDesc,
        nullptr,    // leave the buffer uninitialized
        &m_constantBuffer
        )
    );
```

O **MarbleMazeMain::Update** método atualiza posterior **ConstantBuffer** objetos, um para o Labirinto e outra para o marble. O **MarbleMazeMain::Render** método, em seguida, associa cada **ConstantBuffer** objeto para o buffer de constantes antes de cada objeto é renderizado. A exemplo a seguir mostra a **ConstantBuffer** estrutura, que está no **MarbleMazeMain.h**.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;

    XMFLOAT3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Para entender melhor como constante buffers mapa para o código de sombreador, compare o **ConstantBuffer** estruturar no **MarbleMazeMain.h** para o **ConstantBuffer** buffer de constantes que é definido pelo sombreador de vértice **BasicVertexShader.hlsl**:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

O layout da estrutura **ConstantBuffer** corresponde ao objeto **cbuffer**. A variável **cbuffer** especifica registro b0, o que significa que os dados de buffer constante ficam armazenados no registro 0. O **MarbleMazeMain::Render** método especifica registrar 0 quando ele ativa o buffer de constantes. Esse processo é descrito com mais detalhes em uma seção posterior deste documento.

Para saber mais sobre buffers constantes, consulte [Introduction to Buffers in Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro). Para obter mais informações sobre a palavra-chave do registro, consulte [registrar](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-register).

###  <a name="loading-meshes"></a>Carregando malhas

O Marble Maze usa o SDK-Mesh como formato do tempo de execução porque esse formato fornece uma forma básica de carregar dados de malha para aplicativos de exemplo. Para uso de produto, você deve usar um formato de malha que atenda às exigências específicas do seu jogo.

O **MarbleMazeMain::LoadDeferredResources** método carrega dados de malha depois que ele carrega os sombreadores de pixel e vértice. Uma malha é uma coleção de dados de vértice que muitas vezes inclui informações como posições, dados de normais, cores, materiais e coordenadas de textura. As malhas normalmente são criadas em 3D de software de criação e mantidas em arquivos separados do código do aplicativo. A bola de gude e o labirinto são dois exemplos de malhas que o jogo usa.

O Marble Maze usa a classe **SDKMesh** para carregar as malhas 3D para o labirinto e a bola de gude. Essa classe é declarada em **SDKMesh.h**. O **SDKMesh** fornece métodos de carregar, renderizar e destruir dados de malha.

> [!IMPORTANT]
> Marble Maze usa o formato de malha do SDK e fornece o **SDKMesh** classe apenas para fins ilustrativos. Apesar de o formato SDK-Mesh ser útil para o aprendizado e para a criação de protótipos, é um formato muito básico que pode não atender às exigências da maior parte do desenvolvimento de jogos. Recomendamos que você use um formato de malha que atenda às exigências específicas do seu jogo.

 

A exemplo a seguir mostra como o **MarbleMazeMain::LoadDeferredResources** método usa o **SDKMesh::Create** método para carregar dados para o Labirinto e para a bola de malha.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  <a name="loading-collision-data"></a>Carregando dados de colisão

Embora esta seção não se concentre em como o Marble Maze implementa a simulação de física entre a bolinha e o labirinto, observe que a geometria de malha para o sistema de física é lida quando as malhas são carregadas.

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

A maneira que você carrega dados de colisão em grande parte depende do formato de tempo de execução que você usa. Para obter mais informações sobre como o Marble Maze carrega a geometria de colisão de um arquivo de malha do SDK, consulte o **MarbleMazeMain::ExtractTrianglesFromMesh** método no código-fonte.

## <a name="updating-game-state"></a>Atualizando o estado do jogo


O Marble Maze separa lógica de jogo e lógica de renderização ao atualizar primeiro todos os objetos de cena antes de renderizá-los.

[Marble estrutura de aplicativo do labirinto](marble-maze-application-structure.md) descreve o loop principal do jogo. Atualizar a cena, que faz parte do loop de jogo, acontece depois que os eventos e a entrada do Windows são processados e antes de a cena ser renderizada. O **MarbleMazeMain::Update** método lida com a atualização de interface do usuário e o jogo.

### <a name="updating-the-user-interface"></a>Atualizando a interface do usuário

O **MarbleMazeMain::Update** chamadas de método de **UserInterface::Update** método para atualizar o estado da interface do usuário.

```cpp
UserInterface::GetInstance().Update(
    static_cast<float>(m_timer.GetTotalSeconds()), 
    static_cast<float>(m_timer.GetElapsedSeconds()));
```

O método **UserInterface::Update** atualiza cada elemento na coleção da interface do usuário.

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

Classes que derivam de **ElementBase** (definidos no **UserInterface.h**) implementam a **atualização** método executar comportamentos específicos. Por exemplo, o método **StopwatchTimer::Update** atualiza o tempo decorrido pela quantidade fornecida e atualiza o texto que ele exibe depois.

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  <a name="updating-the-scene"></a>Atualizando uma cena

O **MarbleMazeMain::Update** método atualiza o jogo com base no estado atual da máquina de estado (o **GameState**, armazenado no **m_gameState**). Quando o jogo estiver no estado ativo (**GameState::InGameActive**), atualiza a câmera para seguir o marble Marble Maze, atualiza a parte da matriz de exibição dos buffers de constantes e atualiza a simulação de física.

A exemplo a seguir mostra como o **MarbleMazeMain::Update** método atualiza a posição da câmera. Marble Maze usa a **m\_resetCamera** variável para que a câmera deve ser redefinida para estar localizado diretamente acima de mármore do sinalizador. A câmera é redefinida quando o jogo começa ou quando a bolinha cai no labirinto. Quando a tela de exibição do menu principal ou recorde estiver ativa, a câmera estará definida em localização constante. Do contrário, o Marble Maze usará o parâmetro *timeDelta* para interpolar a posição da câmera entre sua posição atual e pretendida. A posição de destino está ligeiramente acima e na frente da bolinha. Usar o tempo decorrido permite que a câmera siga, ou persiga, a bola de gude de forma gradual.

```cpp
static float eyeDistance = 200.0f;
static XMFLOAT3A eyePosition = XMFLOAT3A(0, 0, 0);

// Gradually move the camera above the marble.
XMFLOAT3A targetEyePosition;
XMStoreFloat3A(
    &targetEyePosition, 
    XMLoadFloat3A(&marblePosition) - (XMLoadFloat3A(&g) * eyeDistance));

if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&eyePosition) 
            + ((XMLoadFloat3A(&targetEyePosition) - XMLoadFloat3A(&eyePosition)) 
                * min(1, static_cast<float>(m_timer.GetElapsedSeconds()) * 8)
            )
    );
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&marblePosition) + XMVectorSet(75.0f, -150.0f, -75.0f, 0.0f));

    m_camera->SetViewParameters(
        eyePosition, 
        marblePosition, 
        XMFLOAT3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, XMFLOAT3(0.0f, 1.0f, 0.0f));
}
```

A exemplo a seguir mostra como o **MarbleMazeMain::Update** método atualiza os buffers de constantes para o marble e labirinto. A matriz de modelo, ou universal, do labirinto é sempre a matriz de identidade. Exceto para a diagonal principal, cujos elementos são todos 1, a matriz de identidade é uma matriz quadrada formada por zeros. A matriz modelo da bola de gude é baseada na sua matriz de posição vezes sua matriz de rotação.

```cpp
// Update the model matrices based on the simulation.
XMStoreFloat4x4(&m_mazeConstantBufferData.model, XMMatrixIdentity());

XMStoreFloat4x4(
    &m_marbleConstantBufferData.model, 
    XMMatrixTranspose(
        XMMatrixMultiply(
            marbleRotationMatrix, 
            XMMatrixTranslationFromVector(XMLoadFloat3A(&marblePosition))
        )
    )
);

// Update the view matrix based on the camera.
XMFLOAT4X4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

Para obter informações sobre como o **MarbleMazeMain::Update** método lê a entrada do usuário e simula o movimento de mármore, consulte [adicionando entrada e interatividade ao exemplo Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## <a name="rendering-the-scene"></a>Renderizando a cena


Quando a cena é renderizada, estas etapas são normalmente incluídas.

1.  Defina o buffer do estêncil de profundidade de destino da renderização atual.
2.  Limpe as exibições de estêncil e renderização.
3.  Prepare os sombreadores de vértice e pixel para o desenho.
4.  Renderizar objetos 3D da cena.
5.  Processa qualquer objeto 2D que você deseja que apareça na frente da cena.
6.  Apresente a imagem renderizada ao monitor.

O **MarbleMazeMain::Render** método associa o destino de renderização e estêncil de profundidade exibições, limpa esses modos de exibição, desenha a cena e, em seguida, desenha a sobreposição.

###  <a name="preparing-the-render-targets"></a>Preparando os destinos de renderização

Antes de renderizar a sua cena, você deve definir o buffer do estêncil de profundidade de destino da renderização atual. Se não houver garantia de que a cena seja desenhada em cada pixel da tela, limpe também as exibições de renderização e estêncil. O Marble Maze limpa os modos de exibição de renderização e de estêncil a cada quadro para garantir que não haja artefatos visíveis do quadro anterior.

A exemplo a seguir mostra como o **MarbleMazeMain::Render** chamadas de método de [ID3D11DeviceContext::OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) método para definir o destino de renderização e o buffer de estêncil de profundidade atual aqueles.

```cpp
auto context = m_deviceResources->GetD3DDeviceContext();

// Reset the viewport to target the whole screen.
auto viewport = m_deviceResources->GetScreenViewport();
context->RSSetViewports(1, &viewport);

// Reset render targets to the screen.
ID3D11RenderTargetView *const targets[1] = 
    { m_deviceResources->GetBackBufferRenderTargetView() };

context->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

// Clear the back buffer and depth stencil view.
context->ClearRenderTargetView(
    m_deviceResources->GetBackBufferRenderTargetView(), 
    DirectX::Colors::Black);

context->ClearDepthStencilView(
    m_deviceResources->GetDepthStencilView(), 
    D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 
    1.0f, 
    0);
```

O [ID3D11RenderTargetView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) e [ID3D11DepthStencilView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) interfaces suportam o mecanismo de exibição de textura é fornecido pelo Direct3D 10 e posterior. Para saber mais sobre exibições de textura, consulte [Texture Views (Direct3D 10)](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-access-views). O [OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) método prepara o estágio de fusão de saída do pipeline de Direct3D. Para saber mais sobre o estágio do agente de mesclagem de saída, consulte [Estágio do agente de mesclagem de saída](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage).

### <a name="preparing-the-vertex-and-pixel-shaders"></a>Preparando os sombreadores de vértice e pixel

Antes de renderizar os objetos de cena, execute as seguintes etapas para preparar os sombreadores de vértice e pixel para desenho:

1.  Defina o layout de entrada do sombreador como o layout atual.
2.  Defina os sombreadores de vértice e pixel como os sombreadores atuais.
3.  Atualize todos os buffers constantes com dados que você tenha para passar os sombreadores.

> [!IMPORTANT]
> O Marble Maze usa um par de sombreadores de pixel e vértice para todos os objetos 3D. Se o seu jogo usar mais de um par de sombreadores, você deverá realizar essas etapas sempre que desenhar objetos que usam sombreadores diferentes. Para reduzir a sobrecarga associada à mudança do estado do sombreador, convém agrupar as chamadas de renderização para todos os objetos que usam os mesmos sombreadores.

 

A seção [Carregando sombreadores](#loading-shaders) neste documento descreve como o layout de entrada é criado quando o sombreador de vértice é criado. A exemplo a seguir mostra como o **MarbleMazeMain::Render** método usa o [ID3D11DeviceContext::IASetInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) método para definir esse layout como o layout atual.

```cpp
m_deviceResources->GetD3DDeviceContext()->IASetInputLayout(m_inputLayout.Get());
```

A exemplo a seguir mostra como o **MarbleMazeMain::Render** método usa o [ID3D11DeviceContext::VSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) e [ID3D11DeviceContext::PSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) métodos para definir os sombreadores de pixel e vértice como os sombreadores atuais, respectivamente.

```cpp
// Set the vertex shader stage state.
m_deviceResources->GetD3DDeviceContext()->VSSetShader(
    m_vertexShader.Get(),   // use this vertex shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetShader(
    m_pixelShader.Get(),    // use this pixel shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetSamplers(
    0,                          // starting at the first sampler slot
    1,                          // set one sampler binding
    m_sampler.GetAddressOf());  // to use this sampler
```

Após **MarbleMazeMain::Render** define os sombreadores e o seu layout de entrada, ele usa o [ID3D11DeviceContext::UpdateSubresource](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) método para atualizar o buffer de constantes com o modelo, exibição, e matrizes de projeção para o labirinto. O método **UpdateSubresource** copia os dados de matriz da memória de CPU para a memória de GPU. Lembre-se de que os componentes de modelo e exibição o **ConstantBuffer** estrutura são atualizadas na **MarbleMazeMain::Update** método. O **MarbleMazeMain::Render** , em seguida, chama um método de [ID3D11DeviceContext::VSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) e [ID3D11DeviceContext::PSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers) métodos para Defina esse buffer de constantes que o atual.

```cpp
// Update the constant buffer with the new data.
m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0);

m_deviceResources->GetD3DDeviceContext()->VSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer

m_deviceResources->GetD3DDeviceContext()->PSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer
```

O **MarbleMazeMain::Render** método executa etapas semelhantes para preparar o marble a ser renderizado.

### <a name="rendering-the-maze-and-the-marble"></a>Renderizando o labirinto e a bolinha

Depois que você ativar os sombreadores atuais, você pode desenhar os objetos de cena. O **MarbleMazeMain::Render** chamadas de método de **SDKMesh::Render** método para renderizar a malha de labirinto.

```cpp
m_mazeMesh.Render(
    m_deviceResources->GetD3DDeviceContext(), 
    0, 
    INVALID_SAMPLER_SLOT, 
    INVALID_SAMPLER_SLOT);
```

O **MarbleMazeMain::Render** método executa etapas semelhantes para renderizar o marble.

Como já dito neste documento, a classe **SDKMesh** é fornecida para fins de demonstração, mas não aconselhamos seu uso em um jogo de qualidade de produção. No entanto, observe que o **SDKMesh::RenderMesh** método, que é chamado pelo **SDKMesh::Render**, usa o [ID3D11DeviceContext::IASetVertexBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) e [ID3D11DeviceContext::IASetIndexBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) métodos para definir o vértice atual e buffers de índice que definem a malha, e o [ID3D11DeviceContext::DrawIndexed](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) método para desenhar os buffers. Para saber mais sobre como trabalhar com buffers de índice e de vértice, consulte [Introduction to Buffers in Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro).

### <a name="drawing-the-user-interface-and-overlay"></a>Desenhando a interface do usuário e a sobreposição

Depois de desenhar objetos da cena 3D, o Marble Maze desenha os elementos de interface do usuário 2D que aparecem na frente da cena.

O **MarbleMazeMain::Render** método termina quando a interface do usuário e a sobreposição de desenho.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render(m_deviceResources->GetOrientationTransform2D());

m_deviceResources->GetD3DDeviceContext()->BeginEventInt(L"Render Overlay", 0);
m_sampleOverlay->Render();
m_deviceResources->GetD3DDeviceContext()->EndEvent();
```

O **UserInterface::Render** método usa um [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) objeto para desenhar os elementos de interface do usuário. Esse método define um estado de desenho, desenha todos os elementos ativos de interface do usuário e, em seguida, restaura o estado de desenho anterior.

```cpp
void UserInterface::Render(D2D1::Matrix3x2F orientation2D)
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(orientation2D);

    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

O método **SampleOverlay::Render** usa uma técnica semelhante para desenhar o bitmap de sobreposição.

###  <a name="presenting-the-scene"></a>Apresentando uma cena

Depois de desenhar objetos todos 2D e 3D da cena, o Marble Maze apresenta a imagem renderizada para o monitor. Ele sincroniza o desenho com o espaço em branco vertical para garantir que não haja perda de tempo com o desenho de quadros que nunca chegarão a aparecer na tela. O Marble Maze também lida com mudanças de dispositivo quando apresenta uma cena.

Após o **MarbleMazeMain::Render** método retorna, o loop do jogo chama o **DX::DeviceResources::Present** método para enviar a imagem renderizada para o monitor ou exibir. O **DX::DeviceResources::Present** chamadas de método [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) para executar a operação de presente, conforme mostrado no exemplo a seguir:

```cpp
// The first argument instructs DXGI to block until VSync, putting the application
// to sleep until the next VSync. This ensures we don't waste any cycles rendering
// frames that will never be displayed to the screen.
HRESULT hr = m_swapChain->Present(1, 0);
```

Neste exemplo, **m\_swapChain** é uma [IDXGISwapChain1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) objeto. A inicialização deste objeto é descrita na seção [Inicializando o Direct3D e o Direct2D](#initializing-direct3d-and-direct2d) deste documento.

O primeiro parâmetro para [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1), *SyncInterval*, especifica o número de espaços em branco verticais de espera antes de apresentar o quadro. O Marble Maze especifica 1 e, portanto, aguarda até o próximo espaço em branco vertical.

O [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) método retorna um código de erro que indica que o dispositivo foi removido ou caso contrário, falha. Nesse caso, o Marble Maze reinicializa o dispositivo.

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## <a name="next-steps"></a>Próximas etapas


Leia [Adicionando entrada e interatividade ao exemplo do Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) para saber mais sobre algumas das principais práticas que você deve ter em mente ao trabalhar com dispositivos de entrada. Este documento discute como o Marble Maze dá suporte a toque, acelerômetro, controladores do Xbox e entrada do mouse.

## <a name="related-topics"></a>Tópicos relacionados


* [Adicionando entrada e interatividade ao exemplo Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Estrutura de aplicativo do Marble Maze](marble-maze-application-structure.md)
* [Desenvolvendo o Marble Maze, um jogo UWP em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




