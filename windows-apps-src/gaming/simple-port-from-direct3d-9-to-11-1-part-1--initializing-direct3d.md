---
title: Inicializar o Direct3D 11
description: Mostra como converter o código de inicialização do Direct3D 9 para o Direct3D 11. Saiba também como obter identificadores para o dispositivo Direct3D e o contexto de dispositivo e como usar DXGI para configurar uma cadeia de troca.
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, direct3d 11, inicialização, fazendo a portabilidade, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: c5a7f33ddbc6d70af5293b92165892c2098e452d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368028"
---
# <a name="initialize-direct3d-11"></a>Inicializar o Direct3D 11



**Resumo**

-   Parte 1: Inicializar o Direct3D 11
-   [Parte 2: Converter o framework de renderização](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [Parte 3: O loop do jogo da porta](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Mostra como converter o código de inicialização do Direct3D 9 para o Direct3D 11. Saiba também como obter identificadores para o dispositivo Direct3D e o contexto de dispositivo e como usar DXGI para configurar uma cadeia de troca. Parte 1 do guia passo a passo de [portabilidade de um aplicativo simples em Direct3D 9 para o DirectX 11 e a Plataforma Universal do Windows (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="initialize-the-direct3d-device"></a>Inicializar o dispositivo Direct3D


No Direct3D 9, criamos um identificador para o dispositivo Direct3D chamando [**IDirect3D9::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-createdevice). Começamos pela obtenção de um ponteiro para [**IDirect3D9 interface**](https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9) e especificamos alguns parâmetros para controlar a configuração do dispositivo Direct3D e da cadeia de troca. Antes disso, chamamos [**GetDeviceCaps**](https://docs.microsoft.com/windows/desktop/api/wingdi/nf-wingdi-getdevicecaps) para garantir que não estávamos solicitando ao dispositivo algo que não pudesse fazer.

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

Primeiramente, criamos o dispositivo. Vamos obter uma lista de níveis de recursos compatíveis com o dispositivo; ela contém a maior parte das informações de que precisamos sobre a GPU. Além disso, não precisamos criar uma interface apenas para acessar o Direct3D. Em vez disso, usamos a API básica [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Ela fornece um identificador para o dispositivo e seu contexto imediato, usado para definir o estado do pipeline e gerar comandos de renderização.

Após a criação do dispositivo Direct3D 11 e de seu contexto, podemos usar a funcionalidade do ponteiro COM para obter as versões mais recentes das interfaces, que incluem recursos adicionais e são sempre recomendadas.

> **Observação**    D3D\_recurso\_nível\_9\_1 (que corresponde ao modelo de sombreador 2.0) é o nível mínimo seu jogo da Microsoft Store é necessário para dar suporte. (Pacotes ARM do seu jogo falhará certificação se não houver suporte 9\_1.) Se seu jogo também inclui um caminho de renderização para recursos de modelo 3 de sombreador, você deve incluir D3D\_recurso\_nível\_9\_3 na matriz.

 

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
    D3D11_SDK_VERSION, // UWP apps must set this to D3D11_SDK_VERSION.
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

> **Observação**    elas são interfaces COM, portanto, sua primeira resposta pode ser usar [ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Porém, em vez disso, use ponteiros inteligentes [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class). Em seguida, chame o método [**As()** ](https://docs.microsoft.com/previous-versions/br230426(v=vs.140)), fornecendo um ponteiro COM vazio do tipo de interface correto.

 

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

Agora que temos a fábrica DXGI, podemos usá-la para criar a cadeia de permuta. É hora de definir os parâmetros da cadeia de troca. É necessário especificar o formato de superfície; Vamos escolher [ **DXGI\_formato\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) porque ele é compatível com o Direct2D. Desativaremos a colocação da exibição em escala, a multiamostragem e a renderização estéreo, pois elas não são usadas neste exemplo. Como a execução é feita diretamente em uma CoreWindow, podemos deixar a largura e altura como 0 e obter os valores de tela inteira automaticamente.

> **Observação**    sempre conjunto a *SDKVersion* parâmetro D3D11\_SDK\_versão para aplicativos UWP.

 

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

Para garantir que não são de renderização com mais frequência que a tela pode mostrar, na verdade, definimos a latência de quadro como 1 e o uso [ **DXGI\_trocar\_efeito\_INVERTER\_SEQUENCIAL** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect). Assim, é possível economizar energia e atender a um requisito de certificação da loja; a parte 2 deste guia passo a passo fala mais sobre apresentação na tela.

> **Observação**    você pode usar multithreading (por exemplo, [ **ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading) itens de trabalho) para continuar a outro trabalho enquanto o thread de renderização é bloqueado.

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

Agora já podemos configurar o buffer de fundo para renderização.

## <a name="configure-the-back-buffer-as-a-render-target"></a>Configure-o como destino de renderização.


Primeiro, precisamos obter um identificador para esse buffer (Observe que o buffer de fundo é de propriedade de cadeia de troca DXGI, enquanto que o DirectX 9 ele pertencia pelo dispositivo Direct3D.) Dizemos que o dispositivo Direct3D para usá-lo como o destino de renderização, criando um destino de renderização *exibição* usando o buffer de fundo.

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

Agora que temos um identificador de dispositivo e um destino de renderização em tela inteira, estamos prontos para carregar a geometria de desenho. Continuar a [parte 2: Renderização](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md).

 

 




