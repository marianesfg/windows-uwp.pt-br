---
title: Comparar código EGL com DXGI e Direct3D
description: A DXGI (interface gráfica do DirectX) e várias APIs Direct3D desempenham a mesma função que EGL. Este tópico o ajuda a entender a DXGI e o Direct3D 11 do ponto de vista do EGL.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, egl, dxgi, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 3a93f78cdc0d716f9b421fdc493317208f9d17b2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368369"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>Comparar código EGL com DXGI e Direct3D




**APIs importantes**

-   [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)
-   [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)

A DXGI (interface gráfica do DirectX) e várias APIs Direct3D desempenham a mesma função que EGL. Este tópico o ajuda a entender a DXGI e o Direct3D 11 do ponto de vista do EGL.

Assim como o EGL, a DXGI e o Direct3D fornecem métodos de configuração de recursos gráficos, obtenção de um contexto de renderização para desenho dos sombreadores e exibição de resultados em uma janela. No entanto, DXGI e Direct3D têm muitas opções adicionais e exigem mais esforço para serem configurados corretamente em caso de migração da EGL.

> **Observação**    essa orientação é baseada em especificações do open do grupo Khronos para EGL 1.4, encontrados aqui: [Interface gráfica de plataforma nativa Khronos (EGL versão 1.4 - 6 de abril de 2011) \[PDF\]](https://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf). Diferenças na sintaxe específica de outras plataformas e linguagens de programação não são abordadas por esta orientação.

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>Comparação entre a DXGI e o Direct3D


A grande vantagem da EGL em relação a DXGI e a Direct3D é que é relativamente simples começar a desenhar em uma superfície de janela. Isso ocorre porque a OpenGL ES 2.0, e consequentemente a EGL, é uma especificação implementada por vários fornecedores de plataforma, ao passo que DXGI e Direct3D são uma única referência com a qual os drivers de fornecedores de hardware devem estar em conformidade. Isso significa que a Microsoft deve implementar um conjunto de APIs que permitem o conjunto mais amplo possível de recursos dos fornecedores, em vez de focar em um subconjunto funcional oferecido por um fornecedor específico ou combinar comandos de configuração específicos a esses fornecedores em APIs mais simples. Por outro lado, o Direct3D fornece um conjunto único de APIs que abrangem uma variedade muito grande de níveis de recurso e plataformas de hardware gráfico, oferecendo mais flexibilidade para desenvolvedores experientes na plataforma.

Assim como o EGL, a DXGI e o Direct3D fornecem APIs para os seguintes comportamentos:

-   Obtenção, leitura e gravação em um buffer de quadros (chamado de "cadeia de troca" no DXGI).
-   Associação do buffer de quadros a uma janela da interface do usuário.
-   Obtenção e configuração de contextos de renderização para desenhar.
-   Emissão de comandos no pipeline gráfico para um contexto de renderização específico.
-   Criação e gerenciamento de recursos de sombreador e associação deles a um conteúdo de renderização.
-   Renderização em destinos específicos (como texturas).
-   Atualização da superfície de exibição da janela com os resultados da renderização com recursos gráficos.

Para ver o processo básico de Direct3D para configurar o pipeline de gráficos, confira o modelo de aplicativo do DirectX 11 (Universal Windows) no Microsoft Visual Studio 2015. A classe de renderização básica nele é um bom parâmetro para a configuração da infraestrutura de elementos gráficos do Direct3D 11 e para a configuração de recursos básicos nela, bem como para o suporte a recursos de aplicativos da Plataforma Universal do Windows (UWP), como a rotação da tela.

O EGL tem pouquíssimas APIs relativas ao Direct3D 11, e navegar nele pode ser difícil se você não conhece a nomenclatura e os jargões específicos à plataforma. Consulte um panorama simples para você se orientar.

Primeiramente, examine o mapeamento do objeto EGL básico para a interface Direct3D:

| Abstração EGL | Representação Direct3D semelhante                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | No Direct3D (para aplicativos UWP), o identificador de exibição é obtido por meio da API [**Windows::UI::CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) (ou da interface **ICoreWindowInterop** que expõe o HWND). O adaptador e a configuração do hardware são definidas com as interfaces COM [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) e [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2).                                                                                                                                                                                                                                                           |
| **EGLSurface**  | Em Direct3D, os buffers e outros recursos de janela (visíveis ou fora da tela) são criados e configurados por interfaces DXGI específicas, incluindo [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) (uma implementação de padrão de fábrica usada para adquirir recursos DXGI, como [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (buffers de exibição). O [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) que representa o dispositivo gráfico e seus recursos são adquiridos com [**D3D11Device::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Para destinos de renderização, use a interface [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview). |
| **EGLContext**  | Em Direct3D, você configura e emite comandos para o pipeline gráfico com a interface [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | Em Direct3D 11, você cria e configura recursos gráficos como buffers, texturas, estênceis e sombreadores com métodos na interface [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1).                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

Este é o processo mais básico para configurar uma exibição simples de elementos gráficos, recursos e contexto em DXGI e em Direct3D para um aplicativo UWP.

1.  Obtenha um identificador para o objeto [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) para o thread de interface do usuário principal do aplicativo chamando [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread).
2.  Para aplicativos UWP, adquira uma cadeia de troca de [**IDXGIAdapter2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2) com [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow), e passe-a para a referência [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) obtida na etapa 1. Você obterá uma instância de [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) em retorno. Analise-a como o objeto renderizador e seu thread de renderização.
3.  Obtenha instâncias de [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) e [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) chamando o método [**D3D11Device::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Analise-os como seu objeto renderizador, também.
4.  Crie sombreadores, texturas e outros recursos usando métodos no objeto [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) do seu renderizador.
5.  Defina buffers, execute sombreadores e gerencie estágios de pipeline usando métodos no objeto [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) do seu renderizador.
6.  Quando o pipeline tiver sido executado e um quadro for desenhado no buffer de fundo, apresente-o na tela com [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

Para examinar esse processo mais detalhadamente, consulte [Getting started with DirectX graphics](https://docs.microsoft.com/windows/desktop/getting-started-with-directx-graphics). O restante deste artigo abrange muitas etapas comuns para a configuração e o gerenciamento de pipeline gráfico básico.
> **Observação**    aplicativos de área de trabalho do Windows apresentam diferentes APIs para a obtenção de uma cadeia de troca do Direct3D, tais como [ **D3D11Device::CreateDeviceAndSwapChain**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain)e não use um [ **CoreWindow** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) objeto.

 

## <a name="obtaining-a-window-for-display"></a>Obtendo uma janela para exibição


Neste exemplo, eglGetDisplay recebe um HWND para um recurso de janela específico da plataforma Microsoft Windows. Outras plataformas, como o Apple iOS (Cocoa) e o Google Android, têm manipuladores ou referências a recursos de janela diferentes e podem ter uma sintaxe de chamada distinta no geral. Após a obtenção de uma exibição, você deve inicializá-la, definir a configuração preferida e criar uma superfície com um buffer de fundo no qual possa desenhar.

Obtendo uma exibição e configurando-a com EGL.

``` syntax
// Obtain an EGL display object.
EGLDisplay display = eglGetDisplay(GetDC(hWnd));
if (display == EGL_NO_DISPLAY)
{
  return EGL_FALSE;
}

// Initialize the display
if (!eglInitialize(display, &majorVersion, &minorVersion))
{
  return EGL_FALSE;
}

// Obtain the display configs
if (!eglGetConfigs(display, NULL, 0, &numConfigs))
{
  return EGL_FALSE;
}

// Choose the display config
if (!eglChooseConfig(display, attribList, &config, 1, &numConfigs))
{
  return EGL_FALSE;
}

// Create a surface
surface = eglCreateWindowSurface(display, config, (EGLNativeWindowType)hWnd, NULL);
if (surface == EGL_NO_SURFACE)
{
  return EGL_FALSE;
}
```

Em Direct3D, a janela principal de um aplicativo UWP é representada pelo objeto [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), que pode ser obtido do objeto do aplicativo chamando [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) como parte do processo de inicialização do "provedor da exibição" que você constrói para Direct3D. (Se você estiver usando a interoperabilidade de Direct3D XAML, você usar o provedor de exibição da estrutura XAML.) O processo para a criação de um provedor de exibição do Direct3D é abordado [como configurar seu aplicativo para exibir uma exibição](https://docs.microsoft.com/previous-versions/windows/apps/hh465077(v=win.10)).

Obtendo um CoreWindow para Direct3D.

``` syntax
CoreWindow::GetForCurrentThread();
```

Depois de obtida a referência a [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), a janela deve ser ativada, o que executa o método **Run** do seu objeto principal e inicia o processamento do evento de janela. Depois disso, crie uma [ **ID3D11Device1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) e um [ **ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)e usá-los para obter subjacente [ **IDXGIDevice1** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgidevice1) e [ **IDXGIAdapter** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) para que você possa obter um [ **IDXGIFactory2** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) objeto para criar um recurso de cadeia de troca com base no seu [ **DXGI\_troca\_cadeia\_DESC1** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) configuração.

Configurando a cadeia de troca DXGI no CoreWindow para Direct3D.

``` syntax
// Called when the CoreWindow object is created (or re-created).
void SimpleDirect3DApp::SetWindow(CoreWindow^ window)
{
  // Register event handlers with the CoreWindow object.
  // ...

  // Obtain your ID3D11Device1 and ID3D11DeviceContext1 objects
  // In this example, m_d3dDevice contains the scoped ID3D11Device1 object
  // ...

  ComPtr<IDXGIDevice1>  dxgiDevice;
  // Get the underlying DXGI device of the Direct3D device.
  m_d3dDevice.As(&dxgiDevice);

  ComPtr<IDXGIAdapter> dxgiAdapter;
  dxgiDevice->GetAdapter(&dxgiAdapter);

  ComPtr<IDXGIFactory2> dxgiFactory;
  dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2), 
    &dxgiFactory);

  DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
  swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
  swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
  swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
  swapChainDesc.Stereo = false;
  swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
  swapChainDesc.SampleDesc.Quality = 0;
  swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
  swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
  swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
  swapChainDesc.Flags = 0;

  // ...

  Windows::UI::Core::CoreWindow^ window = m_window.Get();
  dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr, // Allow on all displays.
    &m_swapChainCoreWindow);
}
```

Chame o método [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) depois de preparar um quadro para exibi-lo.

Observe que em Direct3D 11, não há uma abstração idêntica a EGLSurface. (Há [ **IDXGISurface1**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1), mas ele é usado de forma diferente.) O mais próximo conceitual é o [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) que usamos para atribuir uma textura de objeto ([**ID3D11Texture2D** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)) como o buffer de fundo que nosso pipeline sombreador desenhará em.

Configurando o buffer de fundo para a cadeia de troca no Direct3D 11

``` syntax
ComPtr<ID3D11RenderTargetView>    m_d3dRenderTargetViewWin; // scoped to renderer object

// ...

ComPtr<ID3D11Texture2D> backBuffer2;
    
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&backBuffer2));

m_d3dDevice->CreateRenderTargetView(
  backBuffer2.Get(),
  nullptr,
    &m_d3dRenderTargetViewWin);
```

Uma prática recomendada é chamar esse código sempre que a janela for criada ou mudar de tamanho. Durante a renderização, defina a exibição de destino da renderização com [**ID3D11DeviceContext1::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) antes de configurar qualquer outro sub-recurso, como buffers de vértices ou sombreadores.

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## <a name="creating-a-rendering-context"></a>Criando um contexto de renderização


No EGL 1.4, uma "exibição" representa um conjunto de recursos de janela. Normalmente, você deve configurar uma "superfície" para exibição fornecendo um conjunto de atributos ao objeto de exibição e obtendo uma superfície em troca. Gere um contexto para exibição do conteúdo da superfície criando esse contexto e vinculando-o à superfície e à exibição.

Geralmente, o fluxo de chamada é parecido com o seguinte:

-   Chamar eglGetDisplay com o manipulador para um recurso de janela ou exibição e obter um objeto de exibição.
-   Inicializar a exibição com eglInitialize.
-   Obtenha a configuração de exibição disponível e selecione uma com eglGetConfigs e eglChooseConfig.
-   Criar uma superfície de janela com eglCreateWindowSurface.
-   Criar um contexto de exibição para desenhar com eglCreateContext.
-   Vincular o contexto à exibição e à superfície com eglMakeCurrent.

Na seção anterior, criamos EGLDisplay e EGLSurface; agora, usamos EGLDisplay para criar um contexto e associá-lo à exibição, usando a EGLSurface configurada para definir parâmetros de saída.

Obtendo um contexto de renderização com EGL 1.4

```cpp
// Configure your EGLDisplay and obtain an EGLSurface here ...
// ...

// Create a drawing context from the EGLDisplay
context = eglCreateContext(display, config, EGL_NO_CONTEXT, contextAttribs);
if (context == EGL_NO_CONTEXT)
{
  return EGL_FALSE;
}   
   
// Make the context current
if (!eglMakeCurrent(display, surface, surface, context))
{
  return EGL_FALSE;
}
```

Um contexto de renderização em Direct3D 11 é representado por um objeto [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), o qual representa o adaptador e permite que você crie recursos Direct3D, como buffers e sombreadores, e pelo objeto [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1), o qual permite que você gerencie o pipeline gráfico e execute os sombreadores.

Fique atento aos níveis de recursos do Direct3D! Eles são usados para oferecer suporte a plataformas de hardware Direct3D mais antigas, do DirectX 9.1 ao DirectX 11. Muitas plataformas que usam hardware gráfico de baixa potência, como tablets, somente têm acesso a recursos do DirectX 9.1, e hardware gráfico compatíveis mais antigos podem ser do 9.1 ao 11.

Criando um contexto de renderização com DXGI e Direct3D

```cpp

// ... 

UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;
ComPtr<IDXGIDevice> dxgiDevice;

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
ComPtr<ID3D11DeviceContext> d3dContext;

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
  &d3dContext // Returns the device immediate context.
);
```

## <a name="drawing-into-a-texture-or-pixmap-resource"></a>Desenhando em uma textura ou um recurso pixmap


Para desenhar em uma textura com OpenGL ES 2.0, configure um buffer de pixels, ou PBuffer. Após a configuração e criação de uma EGLSurface para ele, você poderá fornecer um contexto de renderização e executar o pipeline do sombreador para desenhar na textura.

Desenhe em um buffer de pixels com OpenGL ES 2.0

``` syntax
// Create a pixel buffer surface to draw into
EGLConfig pBufConfig;
EGLint totalpBufAttrs;

const EGLint pBufConfigAttrs[] =
{
    // Configure the pBuffer here...
};
 
eglChooseConfig(eglDsplay, pBufConfigAttrs, &pBufConfig, 1, &totalpBufAttrs);
EGLSurface pBuffer = eglCreatePbufferSurface(eglDisplay, pBufConfig, EGL_TEXTURE_RGBA); 
```

Em Direct3D 11, você cria um recurso [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) e o transforma em um destino de renderização. Configurar o destino de renderização usando [ **D3D11\_RENDERIZAR\_destino\_exibição\_DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc). Quando você chama o [ **ID3D11DeviceContext::Draw** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) método (ou um desenho semelhante\* operação no contexto de dispositivo) usando este destino de renderização, os resultados são desenhados em uma textura.

Desenhar em uma textura com Direct3D 11

``` syntax
ComPtr<ID3D11Texture2D> renderTarget1;

D3D11_RENDER_TARGET_VIEW_DESC renderTargetDesc = {0};
// Configure renderTargetDesc here ...

m_d3dDevice->CreateRenderTargetView(
  renderTarget1.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);

// Later, in your render loop...

// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

Essa textura pode ser passada para um sombreador se ela está associada a uma [**ID3D11ShaderResourceView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview).

## <a name="drawing-to-the-screen"></a>Desenhando na tela


Depois que você usar EGLContext para configurar seus buffers e atualizar seus dados, execute os sombreadores vinculados a ele e desenhe os resultados no buffer de fundo com glDrawElements. O buffer de fundo é exibido chamando eglSwapBuffers.

Open GL ES 2.0: Desenhando na tela.

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

Em Direct3D 11, configure seus buffers e vincule sombreadores com a sua [**IDXGISwapChain::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1). Em seguida, chamar um dos [ **ID3D11DeviceContext1::Draw** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) \* métodos para executar os sombreadores e desenhar os resultados para um destino de renderização configurado como o buffer de fundo da cadeia de troca. Depois disso, basta apresentar o buffer de fundo para a exibição chamando **IDXGISwapChain::Present1**.

Direct3D 11: Desenhando na tela.

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## <a name="releasing-graphics-resources"></a>Liberando recursos gráficos


No EGL, lance os recursos de janela passando EGLDisplay para eglTerminate.

Terminando uma exibição com EGL 1.4

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

Em um aplicativo UWP, você pode fechar a CoreWindow com [**CoreWindow::Close**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.close), embora isso só possa ser usado para janelas da interface do usuário secundária. O thread da interface do usuário principal e sua CoreWindow associada não podem ser fechados; em vez disso, eles são expirados pelo sistema operacional. No entanto, quando uma CoreWindow secundária é fechada, é gerado o evento [**CoreWindow::Closed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.closed).

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>Mapeamento da referência de API de EGL para Direct3D 11


| API do EGL                          | API ou comportamento Direct3D 11 semelhante                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | N/D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | Chame [**ID3D11Device::CreateTexture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) para definir uma textura 2D.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | O Direct3D não fornece um conjunto de configurações padrão de buffer de quadro. A configuração da cadeia de troca                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | Para copiar dados de um buffer, chame [**ID3D11DeviceContext::CopyStructureCount**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copystructurecount). Para copiar um recurso, chame [**ID3DDeviceCOntext::CopyResource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Crie um contexto de dispositivo Direct3D chamando [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice), que retorna o identificador de um dispositivo Direct3D e um contexto Direct3D padrão imediato (objeto [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)). Você também pode criar um contexto Direct3D adiado chamando [**ID3D11Device2::CreateDeferredContext**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-createdeferredcontext2) no objeto [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) retornado. |
| eglCreatePbufferFromClientBuffer | Todos os buffers são lidos e gravados como um sub-recurso Direct3D, como uma [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d). Copie de um tipo de sub-recurso compatível para outro com métodos como [**ID3D11DeviceContext1:CopyResource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | Para criar um dispositivo Direct3D sem cadeia de troca, chame o método estático [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Para uma exibição de destino de renderização Direct3D, chame [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | Para criar um dispositivo Direct3D sem cadeia de troca, chame o método estático [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Para uma exibição de destino de renderização Direct3D, chame [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | Obtenha uma [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (para os buffers de exibição) e um [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) (uma interface virtual para o dispositivo gráfico e seus recursos). Use **ID3D11Device1** para definir uma [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) que você pode usar para criar o buffer de quadro fornecido para a **IDXGISwapChain1**.                                                                                         |
| eglDestroyContext                | N/D. Use [**ID3D11DeviceContext::DiscardView1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-discardview1) para se desfazer de uma exibição de destino de renderização. Para fechar o [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) pai, defina a instância como nula e aguarde até que a plataforma recupere seus recursos. Você não pode eliminar o contexto do dispositivo de forma direta.                                                                                                                                                |
| eglDestroySurface                | N/D. Os recursos gráficos são limpos quando a CoreWindow do aplicativo UWP é fechada pela plataforma.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | Chame [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) para obter uma referência à janela do aplicativo principal da janela atual.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | Isso é a [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) atual. Geralmente, isso é analisado como o objeto do renderizador.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | Os erros são obtidos como HRESULTs retornados pela maioria dos métodos nas interfaces DirectX. Se o método não retorna um HRESULT, chame [**GetLastError**](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror). Para converter um erro de sistema em um valor HRESULT, use o [**HRESULT\_FROM\_WIN32**](https://docs.microsoft.com/windows/desktop/api/winerror/nf-winerror-hresult_from_win32) macro.                                                                                                                                                                                                  |
| eglInitialize                    | Chame [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) para obter uma referência à janela do aplicativo principal da janela atual.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | Defina um destino de renderização para desenhar no contexto atual com [**ID3D11DeviceContext1::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets).                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | N/D. No entanto, você pode adquirir destinos de renderização de uma instância [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), bem como alguns dados de configuração. (Siga o link da lista de métodos disponíveis.)                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | N/D. No entanto, você pode adquirir dados sobre visores e o hardware gráfico atual de métodos em uma instância [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1). (Siga o link da lista de métodos disponíveis.)                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | N/D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | Para multithreading geral da GPU, leia [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | Use [ **D3D11\_RENDERIZAR\_destino\_exibição\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc) para configurar uma exibição de destino de renderização do Direct3D,                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | Use [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | Consulte [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1).                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | A CoreWindow usada para exibir a saída do pipeline gráfico é gerenciada pelo sistema operacional.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | Para superfícies compartilhadas, use IDXGIKeyedMutex. Para multithreading geral da GPU, leia [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | Para superfícies compartilhadas, use IDXGIKeyedMutex. Para multithreading geral da GPU, leia [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | Para superfícies compartilhadas, use IDXGIKeyedMutex. Para multithreading geral da GPU, leia [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |

 

 

 




