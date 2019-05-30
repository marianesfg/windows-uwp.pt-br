---
title: Reduzir a latência com cadeias de troca DXGI 1.3
description: Use o DXGI 1.3 para reduzir a latência de quadros eficaz aguardando a cadeia de troca sinalizar o horário apropriado para começar a renderizar um novo quadro.
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, latência, dxgi, cadeias de troca, directx
ms.localizationpriority: medium
ms.openlocfilehash: dbf4935abc543b1c11fbbee32812a7702298cd79
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368179"
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>Reduzir a latência com cadeias de troca DXGI 1.3



Use o DXGI 1.3 para reduzir a latência de quadros eficaz aguardando a cadeia de troca sinalizar o horário apropriado para começar a renderizar um novo quadro. Os jogos normalmente precisam oferecer a menor quantidade de latência possível do momento em que a entrada do jogador é recebida até o momento em que o jogador responde a essa entrada atualizando a tela. Este tópico explica uma técnica disponível a partir do Direct3D 11.2 que pode ser usada para minimizar a latência de quadros eficaz no jogo.

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>Como a espera no buffer de fundo reduz a latência?


Com a cadeia de troca do modelo de inversão, "inversões" do buffer de fundo são enfileiradas sempre que o jogo chama [**IDXGISwapChain::Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present). Quando o loop de renderização chama Present(), o sistema bloqueia o thread até este concluir a apresentação de um quadro anterior, liberando espaço para enfileirar o novo quadro, antes que realmente seja apresentado. Isso gera latência adicional entre o tempo em que o jogo desenha um quadro e o tempo em que o sistema o permite exibir esse quadro. Em muitos casos, o sistema atingirá um ponto de equilíbrio em que o jogo está sempre esperando quase um quadro adicional inteiro entre o tempo em que é renderizado e o tempo em que apresenta cada quadro. É melhor aguardar até que o sistema esteja pronto para aceitar um novo quadro, renderizar o quadro com base nos dados atuais e enfileirá-lo imediatamente.

Criar uma cadeia de troca de espera com o [ **DXGI\_SWAP\_cadeia\_sinalizador\_quadro\_LATÊNCIA\_WAITABLE\_objeto** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) sinalizador. As cadeias de troca criadas dessa maneira podem notificar seu loop de renderização quando o sistema na verdade está pronto para aceitar um novo quadro. Isso permite que o jogo se renderize com base nos dados atuais e coloque o resultado na fila atual imediatamente.

## <a name="step-1-create-a-waitable-swap-chain"></a>Etapa 1: Criar uma cadeia de troca de espera


Especifique o [ **DXGI\_trocar\_cadeia\_sinalizador\_quadro\_LATÊNCIA\_WAITABLE\_objeto** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) Sinalizador quando você chama [ **CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow).

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> **Observação**    em contraste com alguns sinalizadores, esse sinalizador não podem ser adicionado ou removidos usando [ **ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers). O DXGI retornará um código de erro se esse sinalizador for definido de maneira diferente de quando a cadeia de troca foi criada.

 

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## <a name="step-2-set-the-frame-latency"></a>Etapa 2: Definir a latência de quadro


Defina a latência de quadros com a API [**IDXGISwapChain2::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-setmaximumframelatency), em vez de chamar [**IDXGIDevice1::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency).

Por padrão, a latência de quadros para cadeias de troca que podem esperar é definida como 1, o que resulta na menor latência possível porém também reduz o paralelismo CPU-GPU. Se você precisar de maior paralelismo CPU-GPU para atingir 60 FPS (quadros por segundo) – ou seja, se a CPU e a GPU cada uma gastarem menos de 16,7 ms por quadro ao processar o trabalho de renderização, mas sua soma combinada ainda for superior a 16,7 ms, defina a latência de quadros como 2. Isso permite que a GPU processe trabalho enfileirado pela CPU durante o quadro anterior, ao mesmo tempo permitindo que a CPU envie comandos de renderização para o quadro atual de maneira independente.

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>Etapa 3: Obter o objeto de espera de cadeia de troca


Chame [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject) para recuperar o identificador de espera. O identificador de espera é um ponteiro para o objeto de espera. Armazene esse identificador para ser usado pelo loop de renderização.

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>Etapa 4: Aguardar antes de processar cada quadro


O loop de renderização deve esperar a cadeia de troca sinalizar através do objeto de espera antes de começar a renderizar cada quadro. Isso inclui o primeiro quadro renderizado com a cadeia de troca. Use [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex), fornecendo o identificador de espera recuperado na Etapa 2, para sinalizar o início de cada quadro.

O exemplo a seguir mostra o loop de renderização do exemplo de DirectXLatency:

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

O exemplo a seguir mostra a chamada WaitForSingleObjectEx do exemplo de DirectXLatency:

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>O que meu jogo deve fazer enquanto espera a cadeia de troca se apresentar?


Se o jogo não tiver nenhuma tarefa que se bloqueiem no loop de renderização, permitir que ele espere a cadeia de permuta se apresentar pode ser uma vantagem porque gera economia de energia, o que é especialmente importante em dispositivos móveis. Caso contrário, você pode usar multithreading para executar o trabalho enquanto o jogo está esperando a cadeia de troca se apresentar. A seguir está apenas algumas tarefas que o jogo pode concluir:

-   Processar eventos de rede
-   Atualizar a AI
-   Física baseada em CPU
-   Renderização com contexto adiado (em dispositivos com suporte)
-   Carregamento de ativos

Para saber mais sobre programação multithreaded no Windows, consulte os seguintes tópicos relacionados.

## <a name="related-topics"></a>Tópicos relacionados


* [Exemplo de DirectXLatency](https://go.microsoft.com/fwlink/p/?LinkID=317361)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject)
* [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [**Windows.System.Threading**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)
* [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)
* [Processos e Threads](https://docs.microsoft.com/windows/desktop/ProcThread/processes-and-threads)
* [Sincronização](https://docs.microsoft.com/windows/desktop/Sync/synchronization)
* [Usando objetos de evento (Windows)](https://docs.microsoft.com/windows/desktop/Sync/using-event-objects)

 

 




