---
title: Programação assíncrona (DirectX e C++)
description: Este tópico cobre vários pontos a serem considerados quando se utiliza programação assíncrona e threading com DirectX.
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, programação assíncrona, directx
ms.localizationpriority: medium
ms.openlocfilehash: 3fc2722c8db40aaabd4313dac60f676b93478d69
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369039"
---
# <a name="asynchronous-programming-directx-and-c"></a>Programação assíncrona (DirectX e C++)



Este tópico cobre vários pontos a serem considerados quando se utiliza programação assíncrona e threading com DirectX.

## <a name="async-programming-and-directx"></a>Programação assíncrona e DirectX


Se você estiver apenas aprendendo sobre o DirectX, ou mesmo que já tenha experiência com ele, considere colocar todo o pipeline de processamento de elementos gráficos em um único thread. Em qualquer cena de um jogo, há recursos comuns como bitmaps, sombreadores e outros ativos que exigem acesso exclusivo. Esses mesmos recursos exigem que você sincronize qualquer acesso a esses recursos em todos os threads paralelos. A renderização é um processo difícil para colocar em paralelo vários threads.

Entretanto, se o jogo for suficientemente complexo ou se você estiver tentando obter um maior desempenho, poderá usar a programação assíncrona para colocar em paralelo alguns dos componentes que não sejam específicos para o pipeline de renderização. O hardware moderno contém vários cores e CPUs com hyperthread e o aplicativo deve aproveitar isso! Você pode assegurar isso usando programação assíncrona para alguns dos componentes do jogo que não precisam de acesso direto ao contexto de dispositivo Direct3D, como:

-   E/S de arquivo
-   física
-   Inteligência Artificial
-   rede
-   áudio
-   controles
-   Componentes da interface do usuário baseados em XAML

O aplicativo pode manipular esses componentes em vários threads simultâneos. A E/S de arquivo, principalmente o carregamento de ativos, se beneficia bastante com o carregamento assíncrono, pois o jogo ou o aplicativo podem estar em um estado interativo enquanto milhares (ou centenas de milhares) de megabytes de ativos estão sendo carregados ou transmitidos. A maneira mais fácil de criar e gerenciar esses threads é usando a [Biblioteca de padrões paralelos](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl) e o padrão **task**, como contido no namespace **concurrency** definido em PPLTasks.h. O uso da [Biblioteca de padrões paralelos](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl) aproveita-se diretamente de CPUs com multinúcleo e hiperprocessamento, e pode melhorar tudo, desde tempos de carregamento percebidos até os engates e atrasos que ocorrem com cálculos de CPU ou processamento de rede intensos.

> **Observação**    executa o aplicativo em um Universal Windows Platform (UWP), a interface do usuário inteiramente em um single-threaded apartment (STA). Se você estiver criando uma interface do usuário para o jogo em DirectX que usa [interoperabilidade XAML](directx-and-xaml-interop.md), só será possível acessar os controles usando STA.

 

## <a name="multithreading-with-direct3d-devices"></a>Multithreading com dispositivos Direct3D


O multithreading para contextos de dispositivo só está disponível em dispositivos de gráficos que dão suporte a um nível de recurso do Direct3D de 11\_0 ou superior. Contudo, você talvez queira maximizar o uso da poderosa GPU em muitas plataformas, como plataformas dedicadas a jogos. No caso mais simples, você talvez queira separar a renderização de uma sobreposição de exibição heads-up (HUD) da renderização e projeção de cena 3D e fazer com que os dois componentes usem pipelines paralelos separados. Os dois threads devem usar o mesmo [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) para criar e gerenciar os objetos de recurso (as texturas, as malhas, os sombreadores e outros ativos), o que for single-thread e o que exigir a implementação de algum tipo de mecanismo de sincronização (como seções críticas), para acesso seguro. E, embora você possa criar listas de comando separadas para o contexto de dispositivo em threads diferentes (para renderização diferida), não é possível reproduzir essas listas de comando simultaneamente na mesma instância **ID3D11DeviceContext**.

Agora, seu aplicativo também pode usar [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device), que é seguro para multithreading, para criar objetos de recurso. Então, por que não usar sempre **ID3D11Device**, em vez de [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)? No momento, o suporte de driver para multithreading pode não estar disponível para algumas interfaces gráficas. Você pode questionar o dispositivo e descobrir se ele oferece suporte para multithreading, mas se quiser alcançar um público mais amplo, é bom ficar com o **ID3D11DeviceContext** de single-thread para gerenciamento de objeto de recurso. Assim sendo, quando o driver de dispositivo gráfico não oferece suporte para multithreading ou listas de comando, o Direct3D 11 tenta manipular o acesso sincronizado ao contexto de dispositivo internamente e, se não houver suporte para listas de comando, ele fornece uma implementação de software. Como resultado, você pode escrever o código com multithread que será executado em plataformas com interfaces gráficas que não possuem suporte de driver para acesso de contexto de dispositivo com multithread.

Se o seu aplicativo oferece suporte para threads separados para processar listas de comando e exibir quadros, você provavelmente quer manter a GPU ativa, processando as listas de comando enquanto exibe quadros oportunamente sem engasgos ou atrasos perceptíveis. Nesse caso, você pode usar um separado [ **ID3D11DeviceContext** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) para cada thread e para compartilhar recursos (como texturas) por criá-los com o D3D11\_recurso\_MISC \_Sinalizador compartilhado. Neste cenário, o [**ID3D11DeviceContext::Flush**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-flush) deve ser chamado no thread de processamento para completar a execução da lista de comando antes de exibir os resultados do processamento do objeto de recurso no thread de exibição.

## <a name="deferred-rendering"></a>Renderização adiada


A renderização diferida grava os comandos gráficos em uma lista de comandos para que possam ser reproduzidos em outro momento e foi criada para oferecer suporte à renderização em um thread, ao mesmo tempo em que grava comandos para renderização em threads adicionais. Após a conclusão desses comandos, eles podem ser executados no thread que gera o objeto de exibição final (frame buffer, textura ou outra saída de elementos gráficos).

Crie um contexto adiado usando [**ID3D11Device::CreateDeferredContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createdeferredcontext) (em vez de [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) ou [**D3D11CreateDeviceAndSwapChain**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain), que cria um contexto imediato). Para obter mais informações, consulte [Immediate and Deferred Rendering](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-render).

## <a name="related-topics"></a>Tópicos relacionados


* [Introdução ao Multithreading em Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)

 

 




