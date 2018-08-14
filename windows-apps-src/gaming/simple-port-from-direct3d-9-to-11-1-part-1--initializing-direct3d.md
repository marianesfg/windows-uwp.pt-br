---
author: mtoepke
title: Inicializar o Direct3D 11.
description: Consulte como converter o código de inicialização do Direct3D 9 para usá-lo no Direct3D 11. Saiba também como obter identificadores para o dispositivo Direct3D e o contexto de dispositivo e como usar DXGI para configurar uma cadeia de troca.
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, direct3d 11, inicialização, fazendo a portabilidade, direct3d 9
ms.openlocfilehash: d4c4c905ad7d7452251ad13d95cbdc53b137c6c8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.locfileid: "199326"
---
# <a name="initialize-direct3d-11"></a>Inicializar o Direct3D 11.


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Resumo**

-   Parte 1: inicializar o Direct3D 11
-   [Parte 2: converter a estrutura de renderização](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [Parte 3: fazer a portabilidade do loop do jogo](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Consulte como converter o código de inicialização do Direct3D 9 para usá-lo no Direct3D 11. Saiba também como obter identificadores para o dispositivo Direct3D e o contexto de dispositivo e como usar DXGI para configurar uma cadeia de troca. Parte 1 do guia passo a passo de [portabilidade de um aplicativo simples em Direct3D 9 para o DirectX 11 e a Plataforma Universal do Windows (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="initialize-the-direct3d-device"></a>Inicializar o dispositivo Direct3D


No Direct3D 9, criamos um identificador para o dispositivo Direct3D chamando [**IDirect3D9::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174313). Começamos pela obtenção de um ponteiro para [**IDirect3D9 interface**](https://msdn.microsoft.com/library/windows/desktop/bb174300) e especificamos alguns parâmetros para controlar a configuração do dispositivo Direct3D e da cadeia de troca. Antes disso, chamamos [**GetDeviceCaps**](https://msdn.microsoft.com/library/windows/desktop/dd144877) para garantir que não estávamos solicitando ao dispositivo algo que não pudesse fazer.

Direct3D 9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

No Direct3D 11, o contexto de dispositivo e a infraestrutura gráfica são considerados separadamente do dispositivo em si. A inicialização é dividida em várias etapas.

Primeiramente, criamos o dispositivo. Vamos obter uma lista de níveis de recursos compatíveis com o dispositivo; ela contém a maior parte das informações de que precisamos sobre a GPU. Além disso, não precisamos criar uma interface apenas para acessar o Direct3D. Em vez disso, usamos a API básica [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Ela fornece um identificador para o dispositivo e seu contexto imediato, usado para definir o estado do pipeline e gerar comandos de renderização.

Após a criação do dispositivo Direct3D 11 e de seu contexto, podemos usar a funcionalidade do ponteiro COM para obter as versões mais recentes das interfaces, que incluem recursos adicionais e são sempre recomendadas.

> **Observação**   D3D\_FEATURE\_LEVEL\_9\_1 (correspondente ao modelo de sombreador 2.0) é o nível mínimo de suporte exigido para seu jogo da Windows Store. (Ocorrerá falha na certificação dos pacotes ARM de seu jogo se você não der suporte a 9\_1.) Se o seu jogo também incluir um caminho de renderização de recursos do modelo 3 de sombreador, você deverá incluir D3D\_FEATURE\_LEVEL\_9\_3 na matriz.

 

Direct3D 11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // Windows Store apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## <a name="create-a-swap-chain"></a>Criar uma cadeia de troca


O Direct3D 11 inclui uma API de dispositivo chamada DXGI (infraestrutura de elementos gráficos do DirectX). A interface DXGI nos permite (por exemplo) controlar a configuração da cadeia de permuta e configurar dispositivos compartilhados. Nesta etapa da inicialização do Direct3D, vamos usar DXGI para criar uma cadeia de permuta. Como criamos o dispositivo, podemos seguir uma cadeia da interface até o adaptador DXGI.

O dispositivo Direct3D implementa uma interface COM para DXGI. Primeiro precisamos obter essa interface e usá-la para solicitar o adaptador DXGI que hospeda o dispositivo. Depois, usamos o adaptador DXGI para criar uma fábrica DXGI.

> **Observação**   Tratam-se de interfaces COM; por isso, como primeira resposta, é possível usar [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Porém, em vez disso, use ponteiros inteligentes [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx). Em seguida, chame o método [**As()**](https://msdn.microsoft.com/library/windows/apps/br230426.aspx), fornecendo um ponteiro COM vazio do tipo de interface correto.

 

**Direct3D 11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

Agora que temos a fábrica DXGI, podemos usá-la para criar a cadeia de permuta. É hora de definir os parâmetros da cadeia de troca. Precisamos especificar o formato da superfície; escolheremos [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059)por ser compatível com Direct2D. Desativaremos a colocação da exibição em escala, a multiamostragem e a renderização estéreo, pois elas não são usadas neste exemplo. Como a execução é feita diretamente em uma CoreWindow, podemos deixar a largura e altura como 0 e obter os valores de tela inteira automaticamente.

> **Observação**   Sempre defina o parâmetro *SDKVersion* de aplicativos UWP como D3D11\_SDK\_VERSION.

 

**Direct3D 11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

Para garantir que não estejamos realizando mais renderizações do que a tela pode exibir, definimos a latência de quadros como 1 e usamos [**DXGI\_SWAP\_EFFECT\_FLIP\_SEQUENTIAL**](https://msdn.microsoft.com/library/windows/desktop/bb173077). Assim, é possível economizar energia e atender a um requisito de certificação da loja; a parte 2 deste guia passo a passo fala mais sobre apresentação na tela.

> **Observação**   Você pode usar multithreading (por exemplo, itens de trabalho [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229642) para dar continuidade a outros trabalhos enquanto o thread de renderização está bloqueado.

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

Agora já podemos configurar o buffer de fundo para renderização.

## <a name="configure-the-back-buffer-as-a-render-target"></a>Configure-o como destino de renderização.


Primeiro, precisamos obter um identificador para esse buffer (Observe que o buffer de fundo pertence à cadeia de troca DXGI; no DirectX 9 ele pertencia ao dispositivo Direct3D.) Em seguida, pediremos que o dispositivo Direct3D use-o como o destino de renderização, criando um destino de renderização *exibição* usando o buffer de fundo.

**Direct3D 11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

Agora o contexto de dispositivo entra em ação. Diremos ao Direct3D para usar a exibição de destino de renderização recém-criada, utilizando a interface do contexto de dispositivo. Recuperaremos a largura e altura do buffer de fundo para podermos definir toda a janela como visor. Lembre-se de que o buffer de fundo é conectado à cadeia de permuta; por isso, se o tamanho da tela mudar (por exemplo, o usuário arrasta a tela do jogo para outro monitor), será necessário redimensionar o buffer de fundo e mudar algumas configurações.

**Direct3D 11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

Agora que temos um identificador de dispositivo e um destino de renderização em tela inteira, estamos prontos para carregar a geometria de desenho. Vá para a [parte 2: renderização](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md).

 

 




