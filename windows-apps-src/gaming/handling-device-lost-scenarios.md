---
author: mtoepke
title: "Manipular Cenários removidos de dispositivos no Direct3D 11"
description: "Este tópico explica como recriar a cadeia de interface do dispositivo Direct3D e DXGI quando o adaptador gráfico é removido ou reinicializado."
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, directx 11, dispositivo perdido
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2202d8ea4584f7755464fd3035829d348cfbff8d
ms.lasthandoff: 02/07/2017

---

# <a name="span-iddevgaminghandlingdevice-lostscenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Manipular cenários removidos de dispositivos no Direct3D 11


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico explica como recriar a cadeia de interface do dispositivo Direct3D e DXGI quando o adaptador gráfico é removido ou reinicializado.

No DirectX 9, os aplicativos devem encontrar uma condição "[dispositivo perdido](https://msdn.microsoft.com/library/windows/desktop/bb174714)" em que o dispositivo D3D entra em um estado não operacional. Por exemplo, quando um aplicativo Direct3D 9 de tela inteira perde o foco, o dispositivo Direct3D fica "perdido", e qualquer tentativa de desenhar com um dispositivo perdido falhará silenciosamente. O Direct3D 11 usa interfaces de dispositivo gráfico virtual, permitindo que vários programas compartilhem o mesmo dispositivo gráfico físico e eliminando as condições em que os aplicativos perdem o controle do dispositivo Direct3D. Porém, ainda é possível que a disponibilidade do adaptador gráfico mude. Por exemplo:

-   O driver gráfico é atualizado.
-   O sistema muda de um adaptador gráfico de economia de energia para um adaptador gráfico de desempenho.
-   O dispositivo gráfico para de responder e é reiniciado.
-   Um adaptador gráfico é fisicamente conectado ou removido.

Mediante essas circunstâncias, o DXGI retorna um código de erro indicando que o dispositivo Direct3D deve ser reinicializado e os recursos do dispositivo precisam ser recriados. Este tutorial passo a passo explica como os aplicativos e jogos do Direct3D 11 podem detectar e responder a qualquer circunstância em que o adaptador gráfico seja reiniciado, removido ou alterado. Exemplos de código são fornecidos no modelo de aplicativo DirectX 11 (Windows Universal) fornecido com o Microsoft Visual Studio 2015.

# <a name="instructions"></a>Instruções

### <a name="spanspanstep-1"></a><span></span>Etapa 1:

Inclua uma verificação do erro de dispositivo removido no loop de renderização. Apresente o quadro chamando [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) (ou [**Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) e assim por diante). Em seguida, verifique se [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553) ou **DXGI\_ERROR\_DEVICE\_RESET** é retornado.

Primeiro, o modelo armazena o HRESULT retornado pela cadeia de troca DXGI:

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

Após realizar todo o trabalho necessário para apresentar o quadro, o modelo verifica o erro de dispositivo removido. Se necessário, ele chama um método para manipular a condição de dispositivo removido:

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>Etapa 2:

Inclua também uma verificação do erro de dispositivo removido ao responder a alterações no tamanho da janela. Esse é um bom lugar para verificar [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553) ou **DXGI\_ERROR\_DEVICE\_RESET** por vários motivos:

-   O redimensionamento da cadeia de troca requer uma chamada para o adaptador DXGI subjacente, que pode retornar o erro de dispositivo removido.
-   O aplicativo pode ter sido movido para um monitor que está conectado a um dispositivo gráfico diferente.
-   A resolução da área de trabalho costuma mudar quando um dispositivo gráfico é removido ou reiniciado, resultando em uma alteração no tamanho da janela.

O modelo verifica o HRESULT retornado por [**ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577):

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    0
    );

if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    // If the device was removed for any reason, a new device and swap chain will need to be created.
    HandleDeviceLost();

    // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
    // and correctly set up the new device.
    return;
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-3"></a>Etapa 3:

Toda vez que o app recebe o erro [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553), ele precisa reinicializar o dispositivo Direct3D e recriar todos os recursos dependentes do dispositivo. Libere as referências aos recursos de dispositivo gráfico criados com o dispositivo Direct3D anterior; esses recursos não são mais válidos e todas as referências à cadeia de troca devem ser liberadas antes que uma nova possa ser criada.

O método HandleDeviceLost libera a cadeia de troca e notifica os componentes do aplicativo para liberar os recursos do dispositivo:

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

Em seguida, ele cria uma nova cadeia de troca e reinicializa os recursos dependentes do dispositivo controlados pela classe de gerenciamento de dispositivo:

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

Depois que o dispositivo e a cadeia de troca são restabelecidos, ele notifica os componentes do aplicativo para reinicializar os recursos dependentes do dispositivo:

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

Quando o método HandleDeviceLost é encerrado, o controle retorna ao loop de renderização que continua ativo para desenhar o próximo quadro.

## <a name="remarks"></a>Comentários


### <a name="investigating-the-cause-of-device-removed-errors"></a>Investigando a causa dos erros de dispositivo removido

Problemas repetitivos com erros de dispositivo DXGI removido podem indicar que o seu código gráfico está criando condições inválidas durante uma rotina de desenho. Também podem indicar uma falha de hardware ou um bug no driver gráfico. Para investigar a causa dos erros de dispositivo removido, chame [**ID3D11Device::GetDeviceRemovedReason**](https://msdn.microsoft.com/library/windows/desktop/ff476526) antes de liberar o dispositivo Direct3D. Esse método retorna um de seis códigos de erro DXGI possíveis, indicando o motivo do erro de dispositivo removido:

-   **DXGI\_ERROR\_DEVICE\_HUNG**: o driver gráfico parou de responder devido a uma combinação inválida de comandos gráficos enviados pelo aplicativo. Se esse erro ocorrer repetidamente, é uma indicação de que provavelmente seu aplicativo fez o dispositivo parar e precisa ser depurado.
-   **DXGI\_ERROR\_DEVICE\_REMOVED**: o dispositivo gráfico foi fisicamente removido, desativado ou um driver foi atualizado. Isso acontece ocasionalmente e é normal, seu aplicativo ou jogo deve recriar os recursos do dispositivo conforme descrito neste tópico.
-   **DXGI\_ERROR\_DEVICE\_RESET**: o dispositivo gráfico falhou devido a um comando malformado. Se esse erro ocorrer repetidamente, pode significar que seu código está enviando comandos inválidos de desenho.
-   **DXGI\_ERROR\_DRIVER\_INTERNAL\_ERROR**: o driver gráfico encontrou um erro e restaurou o dispositivo.
-   **DXGI\_ERROR\_INVALID\_CALL**: o aplicativo forneceu dados de parâmetro inválidos. Se esse erro ocorrer mesmo que seja uma única vez, significa que o seu código causou a condição de dispositivo removido e deve ser depurado.
-   **S\_OK**: retornado quando um dispositivo gráfico foi habilitado, desabilitado ou redefinido sem invalidar o dispositivo gráfico atual. Por exemplo, esse código de erro pode ser retornado se um aplicativo estiver usando a [WARP (Plataforma de Rasterização Avançada do Windows)](https://msdn.microsoft.com/library/windows/desktop/gg615082) e um adaptador de hardware ficar indisponível.

O código a seguir recuperará o código de erro [**DXGI\_ERROR\_DEVICE\_REMOVED**](https://msdn.microsoft.com/library/windows/desktop/bb509553) e o imprimirá no console de depuração. Insira esse código no início do método HandleDeviceLost:

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

Para obter mais detalhes, consulte [**GetDeviceRemovedReason**](https://msdn.microsoft.com/library/windows/desktop/ff476526) e [**DXGI\_ERROR**](https://msdn.microsoft.com/library/windows/desktop/bb509553).

### <a name="testing-device-removed-handling"></a>Testando a manipulação de dispositivo removido

O Prompt de Comando do Desenvolvedor do Visual Studio dá suporte a uma ferramenta de linha de comando chamada 'dxcap' para captura de eventos Direct3D e reprodução relacionada ao Diagnóstico de Gráficos do Visual Studio. Você pode usar a opção de linha de comando "-forcetdr" enquanto seu aplicativo estiver em execução, o que forçará um evento de recuperação e detecção de tempo limite de GPU, disparando DXGI\_ERROR\_DEVICE\_REMOVED e permitindo que você teste seu código de tratamento de erro.

> **Note**  O DXCap e suas DLLs de suporte são instalados em system32/syswow64 como parte das Ferramentas Gráficas para Windows 10, que não são mais distribuídas por meio do SDK do Windows. Em vez disso, elas são fornecidas através do Recurso de Ferramentas de Gráficos sob Demanda que é um componente opcional do sistema operacional e deve ser instalado para que as Ferramentas de Gráficos sejam habilitadas e usadas no Windows 10. Mais informações sobre como instalar as Ferramentas de Gráficos para Windows 10 podem ser encontradas aqui: <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>

 

 

 





