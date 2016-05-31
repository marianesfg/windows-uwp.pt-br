---
author: mtoepke
title: Adicionando conteúdo visual ao exemplo do Marble Maze
description: Este documento descreve como o jogo Marble Maze usa Direct3D e Direct2D no ambiente do aplicativo da Plataforma Universal do Windows (UWP) para que você aprenda e adapte os parâmetros quando trabalhar com o conteúdo do seu próprio jogo.
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
---

# Adicionando conteúdo visual ao exemplo do Marble Maze


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este documento descreve como o jogo Marble Maze usa Direct3D e Direct2D no ambiente do aplicativo da Plataforma Universal do Windows (UWP) para que você aprenda e adapte os parâmetros quando trabalhar com o conteúdo do seu próprio jogo. Para saber mais sobre como os componentes visuais do jogo se encaixam na estrutura geral do aplicativo Marble Maze, consulte [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md).

Seguimos estas três etapas básicas conforme desenvolvíamos os aspectos visuais do Marble Maze:

1.  Crie uma estrutura básica que inicialize os ambientes Direct3D e Direct2D.
2.  Use programas de edição de imagens e modelos para projetar os ativos 2D e 3D que aparecem no jogo.
3.  Verifique se os ativos 2D e 3D são carregados corretamente e aparecem no jogo.
4.  Integre sombreadores de vértice e pixels capazes de melhorar a qualidade visual dos ativos do jogo.
5.  Integre a lógica do jogo, como animações e entradas do usuário.

Também nos concentramos primeiro em adicionar ativos 3D e, em seguida, ativos 2D. Por exemplo, nós nos concentramos na lógica central do jogo antes de adicionamos o sistema de menus e temporizador.

Também precisamos repetir algumas dessas etapas várias vezes durante o processo de desenvolvimento. Por exemplo, conforme fazíamos mudanças aos modelos de malha e de bola de gude, também tivemos que mudar parte do código de sombreador que dá suporte a tais modelos.

> **Observação**   O código de exemplo que corresponde a este documento pode ser encontrado em [DirectX Marble Maze game sample](http://go.microsoft.com/fwlink/?LinkId=624011).

 
Seguem alguns dos principais pontos discutidos neste documento para quando você trabalhar com DirectX e conteúdo visual de jogos, ou seja, quando inicializar as bibliotecas de elementos gráficos do DirectX, carregar recursos de cena e atualizar e renderizar a cena.

-   Em geral, adicionar conteúdo de jogo envolve várias etapas. Muitas vezes, essas etapas também exigem iteração. Frequentemente, desenvolvedores de jogos se concentram primeiro em adicionar conteúdo de jogo 3D e, em seguida, em adicionar conteúdo 2D.
-   Conquiste mais clientes e proporcione uma ótima experiência a todos eles dando suporte para a maior variedade possível de componentes de hardware gráfico.
-   Separe transparentemente os formatos de tempo de design e de tempo de execução. Estruture seus ativos de tempo de design para maximizar a flexibilidade e permitir rápidas iterações no conteúdo. Formate e compacte seus ativos para que eles sejam carregados e processados da maneira mais eficiente possível em tempo de execução.
-   Você cria os dispositivos Direct3D e Direct2D em um aplicativo UWP de maneira bastante semelhante à que costuma fazer em um aplicativo de área de trabalho clássico do Windows. Uma diferença importante é a forma como a cadeia de troca é associada à janela de saída.
-   Ao criar seu jogo, certifique-se de que o formato de malha escolhido dê suporte aos cenários principais. Por exemplo, se o seu jogo requer colisão, verifique se é possível obter dados de colisão a partir das suas malhas.
-   Separe a lógica do jogo da lógica de renderização atualizando primeiramente todos os objetos de cena antes de renderizá-los.
-   Em geral, você desenha seus objetos de cena 3D e, em seguida, desenha todos os objetos 2D que aparecem na frente da cena.
-   Sincronize o desenho com o espaço em branco vertical para garantir que o jogo não perca tempo desenhando quadros que não chegarão a aparecer na tela.

## Introdução a elementos gráficos DirectX


Quando planejamos o jogo Marble Maze da UWP (Plataforma Universal do Windows), escolhemos as linguagens C++ e Direct3D 11.1 porque elas são as melhores opções para criar jogos 3D que exigem alto desempenho e o máximo de controle sobre o processo de renderização. O DirectX 11.1 dá suporte para componentes de hardware do DirectX 9, até o DirectX 11 e, portanto, pode ajudar você a conquistar mais clientes com maior eficiência, pois não é necessário reescrever o código para cada uma das versões anteriores do DirectX.

O Marble Maze usa o Direct3D 11.1 para renderizar os ativos de jogo 3D, ou seja, a bolinha e o labirinto. O Marble Maze também usa as linguagens Direct2D, DirectWrite e WIC (Windows Imaging Component) para desenhar os ativos de jogo 2D, como os menus e o temporizador. Por último, o Marble Maze usa o XAML para fornecer uma barra de aplicativos e permite a você adicionar controles XAML.

O processo de desenvolvimento de jogos requer planejamento. Se você não conhecer o elementos gráficos do DirectX, pode ler Creating a DirectX game para familiarizar-se com os conceitos básicos da criação de um jogo DirectX UWP. Ao ler esse documento e trabalhar com o código-fonte do Marble Maze, você pode consultar os seguintes recursos para obter informações mais detalhadas sobre elementos gráficos DirectX.

-   [Elementos gráficos Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080) Descreve o Direct3D 11, uma poderosa API de elementos gráficos 3D com aceleração de hardware para a renderização da geometria 3D na plataforma Windows.
-   [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) Descreve o Direct2D, uma API de elementos gráficos 2D com aceleração de hardware que fornece alto desempenho e renderização de alta qualidade para geometria 2D, bitmaps e texto.
-   [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) Descreve a DirectWrite, que dá suporte à renderização de texto de alta qualidade.
-   [Windows Imaging Component](https://msdn.microsoft.com/library/windows/desktop/ee719902) Descreve o WIC, uma plataforma extensível que fornece uma API de baixo nível para imagens digitais.

### Níveis de recursos

O Direct3D 11 apresenta um paradigma conhecido com níveis de recursos. Um nível de recurso é um conjunto bem definido de funcionalidades de GPU. Use níveis de recursos para direcionar seu jogo para execução em versões anteriores de componentes de hardware Direct3D. O Marble Maze dá suporte ao nível de recurso 9.1, pois não necessita dos recursos avançados dos níveis mais altos. Recomendamos que você dê suporte para a maior variedade possível de componentes de hardware e dimensione o conteúdo do seu jogo para que tanto os clientes com os computadores mais simples até aqueles com os computadores mais modernos possam ter uma grande experiência. Para saber mais sobre níveis de recursos, consulte [Direct3D 11 em componentes de hardware inferiores](https://msdn.microsoft.com/library/windows/desktop/ff476872).

## Inicializando o Direct3D e o Direct2D


Um dispositivo representa o adaptador de vídeo. Você cria os dispositivos Direct3D e Direct2D em um aplicativo UWP de maneira bastante semelhante à que costuma fazer em um aplicativo de área de trabalho clássico do Windows. A principal diferença é em como você conecta a cadeia de troca do Direct3D ao sistema de janelas.

O *aplicativo DirectX 11 e XAML (Universal Windows)* fatora um sistema operacional genérico e funções de renderização 3D das funções específicas de jogo. A classe **DeviceResources** é uma base para o gerenciamento do Direct3D e do Direct2D. Essa classe lida com infraestrutura em geral e não tem ativos específicos de jogo. O Marble Maze define a classe **MarbleMaze** para lidar com ativos específicos de jogo, que tem uma referência a um objeto **DeviceResources** para dar a ele acesso ao Direct3D e ao Direct2D.

Durante a inicialização, o método **DeviceResources::Initialize** cria recursos independentes do dispositivo e os dispositivos Direct3D e Direct2D.

```cpp
// Initialize the Direct3D resources required to run. 
void DeviceResources::DeviceResources(CoreWindow^ window, float dpi)
{
    m_window = window;

    CreateDeviceIndependentResources();
    CreateDeviceResources();
    CreateWindowSizeDependentResources();
    SetDpi(dpi);
}
```

A classe **DeviceResources** separa essa funcionalidade para que ela possa responder mais facilmente quando o ambiente mudar. Por exemplo, ela chama o método **CreateWindowSizeDependentResources** quando o tamanho da janela muda.

###  Inicializando as fábricas do Direct2D, DirectWrite e WIC

O método **DeviceResources::CreateDeviceIndependentResources** cria as fábricas do Direct2D, DirectWrite e WIC. Nos elementos gráficos do DirectX, as fábricas são os pontos de partida na criação de recursos de elementos gráficos. O Marble Maze especifica o **D2D1\_FACTORY\_TYPE\_SINGLE\_THREADED** porque executa todos os desenhos no thread principal.

```cpp
// These are the resources required independent of hardware. 
void DeviceResources::CreateDeviceIndependentResources()
{
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
     // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory1),
            &options,
            &m_d2dFactory
            )
        );

    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory),
            &m_dwriteFactory
            )
        );

    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  Criando os dispositivos Direct3D e Direct2D

O método **DeviceResources::CreateDeviceResources** chama o [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) para criar o objeto de dispositivo para representar o adaptador de vídeo do Direct3D. Como o Marble Maze dá suporte a recursos a partir do nível 9.1, o método **DeviceResources::CreateDeviceResources** especifica os níveis 9.1 a 11.1 na matriz de valores **\\**. O Direct3D passa pela lista em ordem e dá ao aplicativo o primeiro nível de recurso que estiver disponível. Portanto, as entradas de matriz **D3D\_FEATURE\_LEVEL** são listadas da maior para a menor para que o aplicativo obtenha o recurso de nível mais alto disponível. O método **DeviceResources::CreateDeviceResources** obtém o dispositivo Direct3D 11.1 consultando o dispositivo Direct3D 11 que volta do **D3D11CreateDevice**.

```cpp
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

// Create the DX11 API device object, and get a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
DX::ThrowIfFailed(
    D3D11CreateDevice(
        nullptr,                    // Specify null to use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                          // Leave as 0 unless it is a software device.
        creationFlags,              // Optionally, set debug and Direct2D compatibility flags.
        featureLevels,              // A list of feature levels that this app can support.
        ARRAYSIZE(featureLevels),   // The number of entries in the above list.
        D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for modern.
        &device,                    // Returns the Direct3D device created.
        &m_featureLevel,            // Returns the feature level of the device created.
        &context                    // Returns the device immediate context.
        )
    );    

// Get the Direct3D 11.1 device by querying the Direct3D 11 device.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );
```

O método **DeviceResources::CreateDeviceResources** cria, em seguida, o dispositivo Direct2D. O Direct2D usa o DirectX Graphic Infrastructure (DXGI) da Microsoft para interoperar com o Direct3D. O DXGI permite que superfícies de memória de vídeo sejam compartilhadas entre tempos de execução de elementos gráficos. O Marble Maze usa o dispositivo DXGI subjacente do dispositivo Direct3D para criar o dispositivo Direct2D a partir do alocador Direct2D.

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

Para saber mais sobre o DXGI e a interoperabilidade entre Direct2D e Direct3D, consulte [DXGI Overview](https://msdn.microsoft.com/library/windows/desktop/bb205075) e [Direct2D and Direct3D Interoperability Overview](https://msdn.microsoft.com/library/windows/desktop/dd370966).

### Associando o Direct3D com a exibição

O método **DeviceResources::CreateWindowSizeDependentResources** cria os recursos de elementos gráficos que dependem de um dado tamanho de janela, como a cadeia de troca e os destinos de renderização do Direct3D e do Direct2D. Uma característica importante que diferencia um aplicativo UWP DirectX de um aplicativo de área de trabalho é a forma como a cadeia de troca é associada à janela de saída. Uma cadeia de troca é responsável por exibir o buffer para o qual o dispositivo é renderizado no monitor. O documento da estrutura do aplicativo Marble Maze descreve como o sistema de janelas de um aplicativo UWP diferencia-se de um aplicativo da área de trabalho. Devido ao aplicativo da Windows Store não trabalhar com objetos **HWND**, o Marble Maze deve usar o método [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) para associar o dispositivo de saída à exibição. O exemplo a seguir mostra a parte do método **DeviceResources::CreateWindowSizeDependentResources** que cria a cadeia de troca.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window),
        &swapChainDesc,
        nullptr,    // Allow on all displays.
        &m_swapChain
        )
    );
```

Para minimizar o consumo de energia, o que é importante em dispositivos alimentados por bateria, como laptops e tablets, o método **DeviceResources::CreateWindowSizeDependentResources** chama o método [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) para garantir que o jogo seja renderizado somente após o vazio vertical. A sincronia com o vazio vertical é descrita com mais detalhes na seção Apresentando a cena deste documento.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both reduces  
// latency and ensures that the application will only render after each VSync, minimizing  
// power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

O método **DeviceResources::CreateWindowSizeDependentResources** inicializa os recursos de elementos gráficos de forma que funcione para a maioria dos jogos.

> **Observação**   O termo *exibição* tem significados diferentes no Windows Runtime e no Direct3D. No Windows Runtime, uma exibição refere-se ao conjunto de configurações de interface do usuário para um aplicativo, incluindo a área de exibição e os comportamentos de entrada, além do thread utilizado para processamento. Você pode especificar as configurações necessárias ao criar uma exibição. O processo de configurar uma exibição de aplicativo é descrito em [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md). No Direct3D, o termo exibição tem vários significados. Em primeiro lugar, uma exibição de recurso define os sub-recursos que um recurso pode acessar. Por exemplo, quando um objeto de textura está associado a uma exibição de recurso de sombreador, esse sombreador pode acessar a textura mais tarde. Uma das vantagens de uma exibição de recurso é que você pode interpretar os dados de diferentes maneiras em diferentes estágios no pipeline de renderização. Para saber mais sobre exibições de recursos, consulte [Exibições de textura (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128). Quando usada no contexto de uma transformação de exibição ou de uma matriz de transformação de exibição, uma exibição refere-se à localização e à orientação da câmara. Uma transformação de exibição realoca objetos ao redor da posição e da orientação da câmera. Para saber mais sobre transformações de exibição, consulte [Transformação de exibição (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb206342). Como o Marble Maze usa exibições de matriz e recurso é descrito com mais detalhes neste tópico.

 

## Carregando recursos de cena


O Marble Maze usa a classe **BasicLoader**, declarada no BasicLoader.h, para carregar texturas e sombreadores. O Marble Maze usa a classe **SDKMesh** para carregar as malhas 3D para o labirinto e a bola de gude.

Para garantir um aplicativo com respostas rápidas, o Marble Maze carrega recursos de cena de forma assíncrona ou no fundo. Como os ativos são carregados em segundo plano, seu jogo pode responder a eventos de janela. Esse processo é explicado com mais detalhes em [Carregando ativos de jogo em segundo plano](marble-maze-application-structure.md#loading_game_assets), neste guia.

###  Carregando a sobreposição 2D e a interface do usuário

No Marble Maze, a sobreposição é a imagem que aparece na parte superior da tela. A sobreposição sempre aparece na frente da cena. No Marble Maze, a sobreposição contém o logo do Windows e a cadeia de texto "DirectX Marble Maze game sample". O gerenciamento da sobreposição é executado pela classe **SampleOverlay**, que é definida em SampleOverlay.h. Apesar de usarmos a sobreposição como parte das amostras do Direct3D, você pode adaptar esse código para exibir qualquer imagem que apareça na frente da sua cena.

Um aspecto importante da sobreposição é que, por seus conteúdos não mudarem, a classe **SampleOverlay** atrai, ou armazena em cache, seus conteúdos para um objeto [**ID2D1Bitmap1**](https://msdn.microsoft.com/library/windows/desktop/hh404349) durante a inicialização. Neste momento, a classe **SampleOverlay** só desenhar o bitmap na tela. Assim, rotinas caras como a de desenho de texto não precisam ser executadas para cada quadro.

A interface do usuário consiste em componentes 2D, como menus e HUDs (exibições de alerta), que aparecem na frente da sua cena. O Marble Maze define os seguintes elementos de interface do usuário:

-   Itens de menu que permitem ao usuário iniciar o jogo ou visualizar as pontuações mais altas.
-   Um temporizador com uma contagem regressiva de três segundos antes de o jogo começar.
-   Um temporizador que controla o tempo de jogo decorrido.
-   Uma tabela que lista os tempos de finalização mais rápidos.
-   O texto que indica "Pausado" quando o jogo está pausado.

O Marble Maze define elementos de interface do usuário específicos do jogo em UserInterface.h. O Marble Maze define a classe **ElementBase** como um tipo base para todos os elementos de interface do usuário. A classe **ElementBase** define atributos como tamanho, posição, alinhamento e visibilidade de um elemento de interface do usuário. Ela também controla como os elementos são atualizados e renderizados.

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

> **Observação**   Devido ao XAML permitir que você crie mais facilmente interfaces do usuário complexas, como as encontradas em jogos de simulação e estratégia, considere usar ou não o XAML para definir a sua interface do usuário. Para obter informações sobre como desenvolver uma interface do usuário em XAML em um jogo DirectX da UWP, consulte [Estender a amostra do jogo (Windows)](tutorial-resources.md). Este documento refere-se à amostra do jogo de tiro 3D do DirectX.

 

###  Carregando sombreadores

O Marble Maze usa o método **BasicLoader::LoadShader** para carregar um sombreador de um arquivo.

Sombreadores são a unidade fundamental de Programação de GPU em jogos hoje em dia. Quase todo o processamento de elementos gráficos 3D é realizado através de sombreadores, independentemente de ser uma transformação de modelo e uma iluminação de cena ou um processamento de geometria mais complexo, desde a descamação de personagens até a infusão de mosaicos. Para saber mais sobre o modelo de programação de sombreadores, consulte [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561).

O Marble Maze usa sombreadores de vértice e de pixel. Um sombreador de vértice sempre opera em um vértice de entrada e produz um vértice como saída. Um sombreador de pixel usa valores numéricos, dados de textura, valores interpolados por vértice e outros dados para produzir uma cor de pixel como saída. Como um sombreador transforma um elemento de cada vez, o hardware de elementos gráficos que fornece vários pipelines de sombreador pode processar conjuntos de elementos em paralelo. O número de pipelines paralelos que estão disponíveis para a GPU pode ser muito maior que o número disponível para a CPU. Portanto, até os sombreadores básicos podem melhorar muito a taxa de transferência.

O método **MarbleMaze::LoadDeferredResources** carrega um sombreador de vértice e um sombreador de pixel depois de carregar a sobreposição. As versões em tempo de design desses sombreadores estão definidas em BasicVertexShader.hlsl e BasicPixelShader.hlsl, respectivamente. O Marble Maze aplica esses sombreadores à bola e ao labirinto durante a fase de renderização.

O projeto do Marble Maze inclui tanto a versão .hlsl (o formato em tempo de design) quanto a versão .cso (o formato em tempo de execução) dos arquivos de sombreador. Em tempo de compilação, o Visual Studio usa o compilador de efeitos fxc.exe para compilar seu arquivo de origem .hlsl em um sombreador .cso. Para saber mais sobre a ferramenta de compilador de efeitos, consulte [Ferramenta de Compilador de Efeitos](https://msdn.microsoft.com/library/windows/desktop/bb232919).

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

> **Cuidado**  O sombreador de pixel compilado contém 32 instruções aritméticas e 1 instrução de textura. Esse sombreador terá um bom desempenho em desktops e em tablets modernos. No entanto, um computador mais simples pode não ser capaz de processar esse sombreador e ainda fornecer uma taxa de quadros interativa. Considere o hardware típico de seu público alvo e projete seus sombreadores para atender às capacidades do mesmo.

 

O método **MarbleMaze::LoadDeferredResources** usa o método **BasicLoader::LoadShader** para carregar os sombreadores. O exemplo a seguir carrega o sombreador de vértice. O tempo de execução desse sombreador é BasicVertexShader.cso. A variável do membro **m\_vertexShader** é um objeto [**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641).

```cpp
\loader->LoadShader(
    L"BasicVertexShader.cso",
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

A variável do membro **m\_inputLayout** é um objeto [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575). O objeto do layout de entrada encapsula o estado do estágio de assembler de entrada (IA) Um trabalho do estágio IA é deixar os sombreadores mais eficazes usando valores gerados pelo sistema, também conhecidos como *semântica*, para processar apenas os primitivos ou vértices que ainda não foram processados. Use o método [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) para criar um layout de entrada a partir de uma matriz de descrições de elementos de entrada. A matriz contém um ou mais elementos de entrada, e cada um destes descreve um elemento de dados de vértice a partir de um buffer de vértices. O conjunto inteiro de descrições de elementos de entrada descreve todos os elementos de dados de vértice de todos os buffers de vértices que serão associados ao estágio IA. O exemplo a seguir mostra a descrição de layout usada pelo Marble Maze. Essa descrição de layout descreve um buffer de vértices que contém quatro elementos de dados de vértice. As partes importantes de cada entrada na matriz são o nome da semântica, o formato de data e o deslocamento de byte. Por exemplo, o elemento **POSITION** especifica a posição do vértice no espaço de objeto. Ele inicia no deslocamento de byte 0 e contém três componentes de ponto flutuante (para um total de 12 bytes). O elemento **NORMAL** especifica o vetor normal. Ele inicia no deslocamento de byte 12 porque aparece logo depois de **POSITION** no layout, que exige 12 bytes. O elemento **NORMAL** contém um inteiro sem sinal de 32 bits e quatro componentes.

```cpp
D3D11_INPUT_ELEMENT_DESC layoutDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD",  0, DXGI_FORMAT_R32G32_FLOAT,   0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT,  0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 }, 
};
m_vertexStride = 44; // You must set this to match the size of layoutDesc above.
```

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

O documento [Semantics](https://msdn.microsoft.com/library/windows/desktop/bb509647) descreve cada semântica disponível com mais detalhes.

> **Observação**   Em um layout, você pode especificar componentes adicionais que não são usados para habilitar vários sombreadores para compartilhar o mesmo layout. Por exemplo, o elemento **TANGENT** não é usado pelo sombreador. Você pode usar o elemento **TANGENT** se quiser testar técnicas como mapeamento normal. Usando o mapeamento de normais, também conhecido como mapeamento de impacto, você pode criar o efeito de impactos nas superfícies de objetos. Para saber mais sobre o mapeamento de impacto, consulte [Mapeamento de impacto (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb172379).

 

Para saber mais sobre o estado do estágio do assembly de entrada, veja [Estágio do Assembler de Entrada](https://msdn.microsoft.com/library/windows/desktop/bb205116) e [Introdução ao estágio do Assembler de Entrada](https://msdn.microsoft.com/library/windows/desktop/bb205117).

O processo de usar os sombreadores de vértice e de pixel para renderizar a cena está descrito na secção [Renderizando a cena](#rendering_the_scene), mais adiante neste documento.

### Criando o buffer constante

O buffer Direct3D agrupa uma coleção de dados. Um buffer constante é uma espécie de buffer que pode ser usado para transmitir dados a sombreadores. O Marble Maze usa um buffer constante para armazenar a exibição modelo (ou do mundo) e as matrizes de projeção para um objeto de cena ativo.

O exemplo a seguir mostra como o método **MarbleMaze::LoadDeferredResources** cria um buffer constante que armazenará os dados da matriz depois. O exemplo cria uma estrutura **D3D11\_BUFFER\_DESC** que usa o sinalizador **D3D11\_BIND\_CONSTANT\_BUFFER** para especificar o uso como um buffer constante. Em seguida, esse exemplo passa aquela estrutura para o método [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). A variável **m\_constantBuffer** é um objeto [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351).

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth           = ((sizeof(ConstantBuffer) + 15) / 16) * 16; // Multiple of 16 bytes
constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;
// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_d3dDevice->CreateBuffer(
        &constantBufferDesc,
        nullptr,             // Leave the buffer uninitialized.
        &m_constantBuffer
        )
    );
```

O método **MarbleMaze::Update** atualiza depois objetos **ConstantBuffer**, um para o labirinto e outro para a bola de gude. O método **MarbleMaze::Render**, então, liga cada objeto **ConstantBuffer** ao buffer constante antes de cada objeto ser renderizado. O exemplo a seguir mostra a estrutura **ConstantBuffer**, que fica no MarbleMaze.h.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    float4x4 model;
    float4x4 view;
    float4x4 projection;

    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Para entender melhor como buffers constantes mapeiam para o código de sombreador, compare a estrutura **ConstantBuffer** ao buffer constante **SimpleConstantBuffer**, que é definido pelo sombreador de vértice no BasicVertexShader.hlsl:

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

O layout da estrutura **ConstantBuffer** corresponde ao objeto **cbuffer**. A variável **cbuffer** especifica registro b0, o que significa que os dados de buffer constante ficam armazenados no registro 0. O método **MarbleMaze::Render** especifica o registro 0 quando ele ativa o buffer constante. Esse processo é descrito com mais detalhes em uma seção posterior deste documento.

Para saber mais sobre buffers constantes, consulte [Introduction to Buffers in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898). Para saber mais a palavra-chave de registro, consulte [**register**](https://msdn.microsoft.com/library/windows/desktop/dd607359).

###  Carregando malhas

O Marble Maze usa o SDK-Mesh como formato do tempo de execução porque esse formato fornece uma forma básica de carregar dados de malha para aplicativos de exemplo. Para uso de produto, você deve usar um formato de malha que atenda às exigências específicas do seu jogo.

O método **MarbleMaze::LoadDeferredResources** carrega dados de malha depois de carregar sombreadores de pixel e vértice. Uma malha é uma coleção de dados de vértice que muitas vezes inclui informações como posições, dados de normais, cores, materiais e coordenadas de textura. Malhas são tipicamente criadas em softwares de autoria de 3D e mantidos em arquivos que são separados do código do aplicativo. A bola de gude e o labirinto são dois exemplos de malhas que o jogo usa.

O Marble Maze usa a classe **SDKMesh** para carregar as malhas 3D para o labirinto e a bola de gude. Essa classe está declarada no SDKMesh.h. O **SDKMesh** fornece métodos de carregar, renderizar e destruir dados de malha.

> **Importante**   O Marble Maze usa o formato SDK-Mesh e fornece a classe **SDKMesh** somente para ilustração. Apesar de o formato SDK-Mesh ser útil para o aprendizado e para a criação de protótipos, é um formato muito básico que pode não atender às exigências da maior parte do desenvolvimento de jogos. Recomendamos que você use um formato de malha que atenda às exigências específicas do seu jogo.

 

O exemplo a seguir mostra como o método **MarbleMaze::LoadDeferredResources** usa o método **SDKMesh::Create** para carregar dados de malha para o labirinto e para a bola.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  Carregando dados de colisão

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

A maneira como você carrega dados de colisões depende amplamente do formato de tempo de execução utilizado. Para saber mais sobre como o Marble Maze carrega a geometria de colisão a partir de um arquivo SDK-Mesh, consulte o método **MarbleMaze::ExtractTrianglesFromMesh** no código-fonte.

## Atualizando o estado do jogo


O Marble Maze separa lógica de jogo e lógica de renderização ao atualizar primeiro todos os objetos de cena antes de renderizá-los.

A documento sobre a estrutura do aplicativo Marble Maze descreve o loop principal do jogo. Atualizar a cena, que faz parte do loop de jogo, acontece depois que os eventos e a entrada do Windows são processados e antes de a cena ser renderizada. O método **MarbleMaze::Update** lida com a atualização de interface do usuário e do jogo.

### Atualizando a interface do usuário

O método **MarbleMaze::Update** chama o método **UserInterface::Update** para atualizar o estado de interface do usuário.

```cpp
UserInterface::GetInstance().Update(timeTotal, timeDelta);
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

As classes que derivam do **ElementBase** implementam o método **Update** para executar comportamentos específicos do jogo. Por exemplo, o método **StopwatchTimer::Update** atualiza o tempo decorrido pela quantidade fornecida e atualiza o texto que ele exibe depois.

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

###  Atualizando uma cena

O método **MarbleMaze::Update** atualiza o jogo com base no estado atual da máquina. Quando o jogo está no estado ativo, o Marble Maze atualiza a câmera para seguir a bola de gude, a parte de matriz de exibição dos buffers constantes e a simulação da Física.

O exemplo a seguir mostra como o método **MarbleMaze::Update** atualiza a posição da câmera. O Marble Maze usa a variável **m\_resetCamera** para sinalizar que a câmera deve ser redefinida para ser localizada acima da bola de gude. A câmera é redefinida quando o jogo começa ou quando a bolinha cai no labirinto. Quando a tela de exibição do menu principal ou recorde estiver ativa, a câmera estará definida em localização constante. Do contrário, o Marble Maze usará o parâmetro *timeDelta* para interpolar a posição da câmera entre sua posição atual e pretendida. A posição de destino está ligeiramente acima e na frente da bolinha. Usar o tempo decorrido permite que a câmera siga, ou persiga, a bola de gude de forma gradual.

```cpp
static float eyeDistance = 200.0f;
static float3 eyePosition = float3(0, 0, 0);

// Gradually move the camera above the marble.
float3 targetEyePosition = marblePosition - (eyeDistance * float3(g.x, g.y, g.z));
if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    eyePosition = eyePosition + ((targetEyePosition - eyePosition) * min(1, timeDelta * 8));
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    eyePosition = marblePosition + float3(75.0f, -150.0f, -75.0f);
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 1.0f, 0.0f));
}
```

O exemplo a seguir mostra como o método **MarbleMaze::Update** atualiza os buffers constantes para a bola de gude e o labirinto. A matriz de modelo, ou universal, do labirinto é sempre a matriz de identidade. Exceto para a diagonal principal, cujos elementos são todos 1, a matriz de identidade é uma matriz quadrada formada por zeros. A matriz modelo da bola de gude é baseada na sua matriz de posição vezes sua matriz de rotação. As funções **mul** e **translation** são definidas em BasicMath.h.

```cpp
// Update the model matrices based on the simulation.
m_mazeConstantBufferData.model = identity();
m_marbleConstantBufferData.model = mul(
    translation(marblePosition.x, marblePosition.y, marblePosition.z),
    marbleRotationMatrix
    );

// Update the view matrix based on the camera.
float4x4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

Para saber mais sobre como o método **MarbleMaze::Update** lê a entrada do usuário e simula o movimento da bola de gude, consulte [Adding input and interactivity to the Marble Maze sample](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## Renderizando a cena


Quando a cena é renderizada, estas etapas são normalmente incluídas.

1.  Defina o buffer do estêncil de profundidade de destino da renderização atual.
2.  Limpe as exibições de estêncil e renderização.
3.  Prepare os sombreadores de vértice e pixel para o desenho.
4.  Renderize os objetos 3D na cena.
5.  Renderize qualquer objeto 2D que você quiser que apareça na frente da cena.
6.  Apresente a imagem renderizada ao monitor.

O método **MarbleMaze::Render** liga os modos de exibição do destino de renderização e de estêncil de profundidade, limpa esses modos, desenha uma cena e, então, desenha a sobreposição.

###  Preparando os destinos de renderização

Antes de renderizar a sua cena, você deve definir o buffer do estêncil de profundidade de destino da renderização atual. Se não houver garantia de que a cena seja desenhada em cada pixel da tela, limpe também as exibições de renderização e estêncil. O Marble Maze limpa os modos de exibição de renderização e de estêncil a cada quadro para garantir que não haja artefatos visíveis do quadro anterior.

O exemplo a seguir mostra como o método **MarbleMaze::Render** chama o método [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) para definir o destino de renderização e o buffer de estêncil de profundidade como atuais. A variável do membro **m\_renderTargetView**, um objeto [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) e a variável do membro **m\_depthStencilView**, um objeto [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) são definidos e inicializados pela classe **DirectXBase**.

```cpp
// Bind the render targets.
m_d3dContext->OMSetRenderTargets(
    1,
    m_renderTargetView.GetAddressOf(),
    m_depthStencilView.Get()
    );

// Clear the render target and depth stencil to default values. 
const float clearColor[4] = { 0.0f, 0.0f, 0.0f, 1.0f };

m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    clearColor
    );

m_d3dContext->ClearDepthStencilView(
    m_depthStencilView.Get(),
    D3D11_CLEAR_DEPTH,
    1.0f,
    0
    );
```

As interfaces [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) e [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) dão suporte ao mecanismo de exibição de textura que é fornecido a partir do Direct3D 10 em diante. Para saber mais sobre exibições de textura, consulte [Texture Views (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128). O método [**OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) prepara o estágio de fusão de saída do pipeline do Direct3D. Para saber mais sobre o estágio do agente de mesclagem de saída, consulte [Estágio do agente de mesclagem de saída](https://msdn.microsoft.com/library/windows/desktop/bb205120).

### Preparando os sombreadores de vértice e pixel

Antes de renderizar os objetos de cena, execute as seguintes etapas para preparar os sombreadores de vértice e pixel para desenho:

1.  Defina o layout de entrada do sombreador como o layout atual.
2.  Defina os sombreadores de vértice e pixel como os sombreadores atuais.
3.  Atualize todos os buffers constantes com dados que você tenha para passar os sombreadores.

> **Importante**  O Marble Maze usa um par de sombreadores de vértice e pixel para todos os objetos 3D. Se o seu jogo usar mais de um par de sombreadores, você deverá realizar essas etapas sempre que desenhar objetos que usam sombreadores diferentes. Para reduzir a sobrecarga associada à mudança do estado do sombreador, convém agrupar as chamadas de renderização para todos os objetos que usam os mesmos sombreadores.

 

A seção [Carregando sombreadores](#loading_shaders) neste documento descreve como o layout de entrada é criado quando o sombreador de vértice é criado. O exemplo a seguir mostra como o método **MarbleMaze:: Render** usa o método [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) para definir esse layout como o layout atual.

```cpp
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

O exemplo a seguir mostra como o método **MarbleMaze::Render** usa os métodos [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) e [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) para definir os sombreadores de vértice e pixel como atuais, respectivamente.

```cpp
// Set the vertex shader stage state.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),   // Use this vertex shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

// Set the pixel shader stage state.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),    // Use this pixel shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

m_d3dContext->PSSetSamplers(
    0,                       // Starting at the first sampler slot
    1,                       // set one sampler binding
    m_sampler.GetAddressOf() // to use this sampler.
    );
```

Depois que o **MarbleMaze::Render** define os sombreadores e seu layout de entrada, ele usa o método [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) para atualizar o buffer constante com as matrizes de modelo, de exibição e de projeção do labirinto. O método **UpdateSubresource** copia os dados de matriz da memória de CPU para a memória de GPU. Lembre-se de que os componentes de modelo e exibição da estrutura **ConstantBuffer** são atualizados no método **MarbleMaze::Update**. O método **MarbleMaze:: Render** chama os métodos [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) e [**ID3D11DeviceContext::PSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476470) para definir esse buffer constante como o atual.

```cpp
// Update the constant buffer with the new data.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0
    );

m_d3dContext->VSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );

m_d3dContext->PSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );
```

O método **MarbleMaze::Render** executa etapas semelhantes para preparar a bola de gude para ser renderizada.

### Renderizando o labirinto e a bolinha

Depois que você ativar os sombreadores atuais, você pode desenhar os objetos de cena. O método **MarbleMaze:: Render** chama o método **SDKMesh::Render** para renderizar a malha do labirinto.

```cpp
m_mazeMesh.Render(m_d3dContext.Get(), 0, INVALID_SAMPLER_SLOT, INVALID_SAMPLER_SLOT);
```

O método **MarbleMaze::Render** executa etapas semelhantes para renderizar a bola de gude.

Como já dito neste documento, a classe **SDKMesh** é fornecida para fins de demonstração, mas não aconselhamos seu uso em um jogo de qualidade de produção. Entretanto, observe que o método **SDKMesh::RenderMesh**, que é chamado pelo **SDKMesh::Render**, usa os métodos [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) e [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) para definir os buffers de vértice e de índice atuais que definem a malha, e o método [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476410) para desenhar os buffers. Para saber mais sobre como trabalhar com buffers de índice e de vértice, consulte [Introduction to Buffers in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898).

### Desenhando a interface do usuário e a sobreposição

Depois de desenhar objetos de cena 3D, o Marble Maze desenha os elementos de interface do usuário 2D que aparecem na frente da cena.

O método **MarbleMaze::Render** termina desenhando a interface do usuário e a sobreposição.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render();

m_sampleOverlay->Render();
```

O método **UserInterface::Render** usa um objeto [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) para desenhar os elementos de interface do usuário. Esse método define um estado de desenho, desenha todos os elementos ativos de interface do usuário e, em seguida, restaura o estado de desenho anterior.

```cpp
void UserInterface::Render()
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    m_d2dContext->EndDraw();
    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

O método **SampleOverlay::Render** usa uma técnica semelhante para desenhar o bitmap de sobreposição.

###  Apresentando uma cena

Depois de desenhar todos os objetos de cena 2D e 3D, o Maze Marble apresenta a imagem renderizada no monitor. Ele sincroniza o desenho com o espaço em branco vertical para garantir que não haja perda de tempo com o desenho de quadros que nunca chegarão a aparecer na tela. O Marble Maze também lida com mudanças de dispositivo quando apresenta uma cena.

Depois que o método **MarbleMaze::Render** retorna, o loop do jogo chama o método **MarbleMaze::Present** para enviar a imagem renderizada para o monitor ou exibição. A classe **MarbleMaze** não substitui o método **DirectXBase::Present**. O método **DirectXBase::Present** chama o [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797) para executar a operação de apresentação, como mostrado no seguinte exemplo.

```cpp
// The application may optionally specify "dirty" or "scroll" rects 
// to improve efficiency in certain scenarios. 
// In this sample, however, we do not utilize those features.
DXGI_PRESENT_PARAMETERS parameters = {0};
parameters.DirtyRectsCount = 0;
parameters.pDirtyRects = nullptr;
parameters.pScrollRect = nullptr;
parameters.pScrollOffset = nullptr;

// The first argument instructs DXGI to block until VSync, putting the  
// application to sleep until the next VSync.  
// This ensures we don't waste any cycles rendering frames that will  
// never be displayed to the screen.
HRESULT hr = m_swapChain->Present1(1, 0, &parameters);
```

Neste exemplo, **m\_swapChain** é um objeto [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631). A inicialização deste objeto é descrita na seção [Inicializando o Direct3D e o Direct2D](#initializing) deste documento.

O primeiro parâmetro para o [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797), *SyncInterval*, especifica o número de vazios verticais a se esperar antes de se apresentar o quadro. O Marble Maze especifica 1 e, portanto, aguarda até o próximo espaço em branco vertical. Um vazio vertical é o tempo entre o fim do desenho de um quadro no monitor e o início do próximo quadro.

O método [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) retorna um código de erro que indica que o dispositivo foi removido ou falhou. Nesse caso, o Marble Maze reinicializa o dispositivo.

```cpp
// Reinitialize the renderer if the device was disconnected  
// or the driver was upgraded. 
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    Initialize(m_window, m_dpi);
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## Próximas etapas


Leia [Adicionando entrada e interatividade ao exemplo do Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) para saber mais sobre algumas das principais práticas que você deve ter em mente ao trabalhar com dispositivos de entrada. Este documento discute como o Marble Maze dá suporte a touch, acelerômetro, controle do Xbox 360 e entrada de mouse.

## Tópicos relacionados


* [Adicionando entrada e interatividade ao exemplo do Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md)
* [Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 






<!--HONumber=May16_HO2-->


