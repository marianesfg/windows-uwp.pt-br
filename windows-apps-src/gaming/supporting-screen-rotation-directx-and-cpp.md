---
title: Oferecendo suporte à orientação de tela (DirectX e C++)
description: Aqui, vamos discutir as práticas recomendadas para manipular a rotação da tela em seu aplicativo UWP DirectX, para que o hardware de elementos gráficos do dispositivo Windows 10 são usados de forma eficiente e efetiva.
ms.assetid: f23818a6-e372-735d-912b-89cabeddb6d4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, orientação da tela, directx
ms.localizationpriority: medium
ms.openlocfilehash: 4e2cf915e510c3d6e3d702417b72c097a293f03c
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9051069"
---
# <a name="supporting-screen-orientation-directx-and-c"></a>Oferecendo suporte à orientação de tela (DirectX e C++)



Seu aplicativo UWP (Plataforma Universal do Windows) pode dar suporte a várias orientações de tela quando você manipula o evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268). Aqui, vamos discutir as práticas recomendadas para manipular a rotação da tela em seu aplicativo UWP DirectX, para que o hardware de elementos gráficos do dispositivo Windows 10 são usados de forma eficiente e efetiva.

Antes de começar, lembre-se de que o hardware gráfico sempre emite dados em pixel da mesma maneira, independentemente da orientação do dispositivo. Os dispositivos Windows 10 podem determinar sua orientação de exibição atual (com algum tipo de sensor ou com uma alternância de software) e permitir que os usuários alterem as configurações de vídeo. Devido a esse, Windows 10 em si controla a rotação das imagens para assegurar que elas fiquem "verticais" com base na orientação do dispositivo. Por padrão, seu aplicativo recebe a notificação de que algo mudou na orientação, por exemplo, o tamanho de uma janela. Quando isso acontece, o Windows 10 imediatamente gira a imagem para exibição final. Para três as quatro orientações de tela específicas (explicadas posteriormente), o Windows 10 usa recursos gráficos adicionais e computação para exibir a imagem final.

Para aplicativos UWP DirectX, o objeto [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258) inclui os dados básicos de orientação de exibição que seu aplicativo pode consultar. A orientação padrão é *paisagem*, em que a largura em pixels da exibição é maior que a altura; a orientação alternativa é *retrato*, onde a exibição é girada a 90 graus nas duas direções e a largura torna-se menor que a altura.

Windows 10 define quatro modos de orientação de exibição específicos:

-   Paisagem — a exibição padrão orientação para Windows 10 e é considerada o ângulo base ou de rotação (0 graus).
-   Retrato - o vídeo tem que ser girado no sentido horário 90 graus (ou sentido anti-horário 270 graus).
-   Paisagem, invertida - o tela foi girada 180 graus (virada de cabeça para baixo).
-   Retrato invertido - o vídeo foi girado no sentido horário a 270 graus (ou no sentido anti-horário a 90 graus).

Quando a tela gira de uma orientação para outra, Windows 10 executa internamente uma operação de rotação para alinhar a imagem desenhada com a nova orientação, e o usuário vê uma imagem vertical na tela.

Além disso, o Windows 10 exibe animações de transição automática para criar uma experiência de usuário uniforme ao mudar de uma orientação para outra. À medida que a orientação do vídeo muda, o usuário vê essas mudanças como uma animação de rotação e zoom fixo da imagem da tela exibida. Windows 10 aloca tempo ao aplicativo para o layout na nova orientação.

Em geral, este é o processo para lidar com as alterações na orientação de tela:

1.  Use uma combinação dos valores de limites de janela e os dados de orientação de exibição para manter a cadeia de troca alinhada com a orientação de exibição nativa do dispositivo.
2.  Notifica o Windows 10 sobre a orientação da cadeia de troca usando [**idxgiswapchain1:: SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801).
3.  Mude o código de renderização para gerar imagens alinhadas com a orientação do dispositivo do usuário.

## <a name="resizing-the-swap-chain-and-pre-rotating-its-contents"></a>Redimensionar a cadeia de troca e girar previamente o conteúdo


Para executar um redimensionamento básico de exibição e girar previamente o conteúdo em seu aplicativo UWP DirectX, siga estas etapas:

1.  Manipule o evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268).
2.  Redimensione a cadeia de troca para as novas dimensões da janela.
3.  Chame [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) para definir a orientação da cadeia de troca.
4.  Recrie os recursos que dependem do tamanho da janela, como seus alvos de renderização e outros buffers de dados em pixel.

Agora vamos examinar essas etapas um pouco mais detalhadamente.

A primeira etapa é registrar um manipulador para o evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268). Esse evento é acionado no aplicativo sempre que a orientação da tela muda, por exemplo, quando ela é girada.

Para manipular o evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268), conecte o manipulador para **DisplayInformation::OrientationChanged** no método [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) exigido, que é um dos métodos da interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) que seu provedor de exibição deve implementar.

Nesse exemplo de código, o manipulador de eventos para [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) é um método chamado **OnOrientationChanged**. Quando **DisplayInformation::OrientationChanged** é acionado, ele chama um método denominado **SetCurrentOrientation** que, em seguida, chama **CreateWindowSizeDependentResources**.

```cpp
void App::SetWindow(CoreWindow^ window)
{
  // ... Other UI event handlers assigned here ...
  
    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);

  // ...
}
}
```

```cpp
void App::OnOrientationChanged(DisplayInformation^ sender, Object^ args)
{
    m_deviceResources->SetCurrentOrientation(sender->CurrentOrientation);
    m_main->CreateWindowSizeDependentResources();
}

// This method is called in the event handler for the OrientationChanged event.
void DX::DeviceResources::SetCurrentOrientation(DisplayOrientations currentOrientation)
{
    if (m_currentOrientation != currentOrientation)
    {
        m_currentOrientation = currentOrientation;
        CreateWindowSizeDependentResources();
    }
}
```

Em seguida, redimensione a cadeia de troca para a nova orientação de tela e prepare-a para girar o conteúdo do pipeline de elemento gráfico quando a renderização for executada. Neste exemplo, **DirectXBase::CreateWindowSizeDependentResources** é um método que manipula chamando IDXGISwapChain::ResizeBuffers, definindo uma matriz de rotação 2D e 3D, chamando SetRotation e recriando os recursos.

```cpp
void DX::DeviceResources::CreateWindowSizeDependentResources() 
{
    // Clear the previous window size specific context.
    ID3D11RenderTargetView* nullViews[] = {nullptr};
    m_d3dContext->OMSetRenderTargets(ARRAYSIZE(nullViews), nullViews, nullptr);
    m_d3dRenderTargetView = nullptr;
    m_d2dContext->SetTarget(nullptr);
    m_d2dTargetBitmap = nullptr;
    m_d3dDepthStencilView = nullptr;
    m_d3dContext->Flush();

    // Calculate the necessary render target size in pixels.
    m_outputSize.Width = DX::ConvertDipsToPixels(m_logicalSize.Width, m_dpi);
    m_outputSize.Height = DX::ConvertDipsToPixels(m_logicalSize.Height, m_dpi);
    
    // Prevent zero size DirectX content from being created.
    m_outputSize.Width = max(m_outputSize.Width, 1);
    m_outputSize.Height = max(m_outputSize.Height, 1);

    // The width and height of the swap chain must be based on the window's
    // natively-oriented width and height. If the window is not in the native
    // orientation, the dimensions must be reversed.
    DXGI_MODE_ROTATION displayRotation = ComputeDisplayRotation();

    bool swapDimensions = displayRotation == DXGI_MODE_ROTATION_ROTATE90 || displayRotation == DXGI_MODE_ROTATION_ROTATE270;
    m_d3dRenderTargetSize.Width = swapDimensions ? m_outputSize.Height : m_outputSize.Width;
    m_d3dRenderTargetSize.Height = swapDimensions ? m_outputSize.Width : m_outputSize.Height;

    if (m_swapChain != nullptr)
    {
        // If the swap chain already exists, resize it.
        HRESULT hr = m_swapChain->ResizeBuffers(
            2, // Double-buffered swap chain.
            lround(m_d3dRenderTargetSize.Width),
            lround(m_d3dRenderTargetSize.Height),
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
    }
    else
    {
        // Otherwise, create a new one using the same adapter as the existing Direct3D device.
        DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

        swapChainDesc.Width = lround(m_d3dRenderTargetSize.Width); // Match the size of the window.
        swapChainDesc.Height = lround(m_d3dRenderTargetSize.Height);
        swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
        swapChainDesc.Stereo = false;
        swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
        swapChainDesc.SampleDesc.Quality = 0;
        swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
        swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
        swapChainDesc.Flags = 0;    
        swapChainDesc.Scaling = DXGI_SCALING_NONE;
        swapChainDesc.AlphaMode = DXGI_ALPHA_MODE_IGNORE;

        // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForCoreWindow(
                m_d3dDevice.Get(),
                reinterpret_cast<IUnknown*>(m_window.Get()),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }

    // Set the proper orientation for the swap chain, and generate 2D and
    // 3D matrix transformations for rendering to the rotated swap chain.
    // Note the rotation angle for the 2D and 3D transforms are different.
    // This is due to the difference in coordinate spaces.  Additionally,
    // the 3D matrix is specified explicitly to avoid rounding errors.

    switch (displayRotation)
    {
    case DXGI_MODE_ROTATION_IDENTITY:
        m_orientationTransform2D = Matrix3x2F::Identity();
        m_orientationTransform3D = ScreenRotation::Rotation0;
        break;

    case DXGI_MODE_ROTATION_ROTATE90:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(90.0f) *
            Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
        m_orientationTransform3D = ScreenRotation::Rotation270;
        break;

    case DXGI_MODE_ROTATION_ROTATE180:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(180.0f) *
            Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
        m_orientationTransform3D = ScreenRotation::Rotation180;
        break;

    case DXGI_MODE_ROTATION_ROTATE270:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(270.0f) *
            Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
        m_orientationTransform3D = ScreenRotation::Rotation90;
        break;

    default:
        throw ref new FailureException();
    }


    //SDM: only instance of SetRotation
    DX::ThrowIfFailed(
        m_swapChain->SetRotation(displayRotation)
        );

    // Create a render target view of the swap chain back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );

    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT, 
        lround(m_d3dRenderTargetSize.Width),
        lround(m_d3dRenderTargetSize.Height),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &depthStencilDesc,
            nullptr,
            &depthStencil
            )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2D);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
            depthStencil.Get(),
            &depthStencilViewDesc,
            &m_d3dDepthStencilView
            )
        );
    
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);

    // Create a Direct2D target bitmap associated with the
    // swap chain back buffer and set it as the current target.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    ComPtr<IDXGISurface2> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

    // Grayscale text anti-aliasing is recommended for all UWP apps.
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

}

```

Depois de salvar os valores de altura e largura atuais da janela para a próxima vez que esse método for chamado, converta os valores do DIP (pixel independente de dispositivo) para os limites de exibição em pixels. Na amostra, chame **ConvertDipsToPixels**, que é uma função simples que executa esse código:

` floor((dips * dpi / 96.0f) + 0.5f);`

Adicione o 0.5f para assegurar o arredondamento para o valor inteiro mais próximo.

Fazendo um aparte, as coordenadas do [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) são sempre definidas em DIPs. Para Windows 10 e versões anteriores do Windows, um DIP é definido como 1/96 de uma polegada e alinhado à definição do sistema operacional de *cima*. Quando a orientação de exibição gira para o modo retrato, o aplicativo inverte a largura e a altura do **CoreWindow**, e o tamanho de destino da renderização (limites) deve mudar de acordo com isso. Como as coordenadas do Direct3D são sempre em pixels físicos, você deve converter os valores de DIP do **CoreWindow** para valores inteiros de pixels antes de passar esses valores para o Direct3D configurar a cadeia de troca.

Nesse processo, você está trabalhando um pouco mais do que trabalharia se simplesmente redimensionasse a cadeia de permuta: você está efetivamente girando os componentes do Direct2D e Direct3D de sua imagem antes de compô-las para a apresentação e está informando a cadeia de permuta que renderizou os resultados em uma nova orientação. Veja a seguir mais detalhes sobre esse processo, conforme mostrado no exemplo de código para **DX::DeviceResources::CreateWindowSizeDependentResources**:

-   Determine a nova orientação de exibição. Se a exibição tiver sido invertida de paisagem para retrato ou vice-versa, troque os valores de altura e largura  alterados de valores DIP para pixels para os limites de exibição.

-   Depois, verifique se a cadeia de troca foi criada. Se ele não tiver sido criado, crie-o chamando [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559). Caso contrário, redimensione os buffers da cadeia de troca existentes para as novas dimensões de exibição chamando [**IDXGISwapchain:ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577). Embora não seja necessário redimensionar a cadeia de troca para o evento de rotação afinal, você está emitindo o conteúdo já girado por seu pipeline de renderização, existem outros eventos de mudança de dimensionamento, como eventos de ajuste e preenchimento, que exigem redimensionamento.

-   Depois disso, defina a transformação de matriz 2D ou 3D adequada para aplicar aos pixels ou aos vértices (respectivamente) no pipeline de elemento gráfico ao renderizá-los para a cadeia de troca. Temos 4 métricas de rotação possíveis:

    -   paisagem (DXGI\_MODE\_ROTATION\_IDENTITY)
    -   retrato (DXGI\_MODE\_ROTATION\_ROTATE270)
    -   paisagem invertida (DXGI\_MODE\_ROTATION\_ROTATE180)
    -   retrato invertido (DXGI\_MODE\_ROTATION\_ROTATE90)

    A matriz correta é selecionada com base nos dados fornecidos pelo Windows 10 (como os resultados do [**displayinformation:: orientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268)) para determinar a orientação de exibição, e ela será multiplicada pelas coordenadas de cada pixel (Direct2D) ou vértice (Direct3D) na cena, girando-as efetivamente para se alinhar à orientação da tela. (Note que no Direct2D, a origem da tela é definida como o canto superior esquerdo, enquanto em Direct3D a origem é definida como o centro lógico da janela.)

> **Observação**  para obter mais informações sobre as transformações 2-D usadas para rotação e como defini-las, consulte [definindo métricas para rotação da tela (2-D)](#appendix-a-applying-matrices-for-screen-rotation-2-d). Para obter mais informações sobre as transformações 3-D usadas para rotação, veja [Definindo as matrizes para a rotação da tela (3-D)](#appendix-b-applying-matrices-for-screen-rotation-3-d).

 

Aviso importante: chame [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) e forneça a ele sua matriz de rotação atualizada, dessa maneira:

`m_swapChain->SetRotation(rotation);`

Você também armazena a matriz de rotação selecionada onde seu método de renderização pode acessá-la quando calcula a nova projeção. Você usará essa matriz quando renderizar sua projeção final 3-D ou compor seu layout 2-D final. (Ele não o aplica automaticamente para você.)

Depois disso, crie um novo destino de renderização para o modo de exibição 3-D girado, bem como um novo buffer de estênceis de profundidade para o modo de exibição. Defina o visor de renderização 3-D para a cena girada chamando [**ID3D11DeviceContext:RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480).

Por fim, se você tiver imagens 2-D para girar ou definir layout, crie um destino de renderização 2-D como um bitmap gravável para a cadeia de troca redimensionada usando [**ID2D1DeviceContext::CreateBitmapFromDxgiSurface**](https://msdn.microsoft.com/library/windows/desktop/hh404482) e componha seu novo layout para a orientação atualizada. Defina quaisquer propriedades necessárias no destino de renderização, como o modo de suavização (conforme visto no exemplo de código).

Agora, apresente a cadeia de permuta.

## <a name="reduce-the-rotation-delay-by-using-corewindowresizemanager"></a>Reduzir o atraso de rotação usando CoreWindowResizeManager


Por padrão, Windows 10 fornece uma breve, mas considerável de tempo para qualquer aplicativo, independentemente do modelo de aplicativo ou idioma, para concluir a rotação da imagem. Entretanto, há chances de que quando seu aplicativo executa o cálculo de rotação usando uma das técnicas descritas aqui, ele será feito bem antes desse período ser encerrado. Você gostaria de recuperar o tempo e concluir a animação, certo? É nesse ponto que o [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603) entra.

Veja como usar o [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603): quando um evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) é acionado, chame o [**CoreWindowResizeManager::GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh404170) no manipulador para o evento obter uma instância do **CoreWindowResizeManager** e, quando o layout da nova orientação for concluído e apresentado, chame [**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605) para permitir que o Windows saiba que ele pode concluir a animação de rotação e exiba a tela do aplicativo.

Veja a aparência do código em seu manipulador de eventos para [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268):

```cpp
CoreWindowResizeManager^ resizeManager = Windows::UI::Core::CoreWindowResizeManager::GetForCurrentView();

// ... build the layout for the new display orientation ...

resizeManager->NotifyLayoutCompleted();
```

Quando um usuário gira a orientação da tela, Windows 10 mostra uma animação independente de seu aplicativo como feedback para o usuário. Existem três partes para essa animação que ocorrem na seguinte ordem:

-   Windows 10 reduz a imagem original.
-   Windows 10 mantém a imagem durante o tempo que leva para recriar o novo layout. Esse é o período que você gostaria de reduzir, porque seu aplicativo provavelmente não precisará dele inteiro.
-   Quando a janela do layout expira, ou quando uma notificação da conclusão de layout e recebida, o Windows gira a imagem e efetua fading cruzado de zooms para nova orientação.

Conforme sugerido no terceiro marcador, quando um aplicativo chama [**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605), Windows 10 interrompe o período de tempo limite, conclui a animação de rotação e retorna o controle ao seu aplicativo, que agora está apresentando a nova orientação de exibição. O efeito geral é que seu aplicativo agora está um pouco mais fluido, responsivo e eficiente!

## <a name="appendix-a-applying-matrices-for-screen-rotation-2-d"></a>Apêndice A: Aplicando métricas para rotação da tela (2-D)


No código de exemplo em [Redimensionando a cadeia de troca e fazendo a pré-rotação de seu conteúdo](#resizing-the-swap-chain-and-pre-rotating-its-contents) (e no [Exemplo de rotação de cadeia de troca DXGI](https://go.microsoft.com/fwlink/p/?linkid=257600)), você pode ter percebido que tínhamos matrizes de rotação separadas para a saída de Direct2D e Direct3D. Vamos examinar as matrizes 2-D, primeiro.

Há dois motivos pelos quais não podemos aplicar as mesmas matrizes de rotação ao conteúdo do Direct2D e Direct3D:

-   Primeiro, elas usam diferentes modelos de coordenada cartesiana. O Direct2D usa a regra à direita, onde a coordenada y aumenta em valor positivo movimento para cima a partir da origem. Entretanto, o Direct3D isa a regra à esquerda, onde a coordenada y aumenta em valor positivo à direita a partir da origem. O resultado é a origem das coordenadas da tela no canto superior esquerdo para o Direct2D, enquanto a origem da tela (o plano de projeção) está no canto inferior esquerdo para o Direct3D. (Consulte [Sistemas de coordenadas 3D](https://msdn.microsoft.com/library/windows/apps/bb324490.aspx) para saber mais.)

    ![sistema de coordenadas direct3d.](images/direct3d-origin.png)![sistema de coordenadas direct2d.](images/direct2d-origin.png)

-   Segundo, as matrizes de rotação 3-D devem ser especificadas explicitamente para evitar os erros de arredondamento.

A cadeia de permuta assume que a origem esteja localizada no canto inferior esquerdo, portanto você tem que executar uma rotação para alinhar o sistema de coordenadas do Direct2D à direita com o sistema à esquerda usado pela cadeia de permuta. Especificamente, você reposiciona a imagem sob a nova orientação à esquerda multiplicando a matriz de rotação pela matriz de translação para a origem do sistema de coordenadas giradas e transforma a imagem do espaço da coordenadas do [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) no espaço de coordenadas da cadeia de troca. Além disso, seu aplicativo deve aplicar consistentemente esta transformação quando o destino de renderização do Direct2D é conectado com a cadeia de troca. Entretanto, se o seu aplicativo estiver desenhando para intermediar as superfícies que não são associadas diretamente com a cadeia de troca, não aplique essa transformação de espaço de coordenada.

Seu código para selecionar a matriz correta a partir de quatro rotações possíveis pode se assemelhar a isso (lembre-se da conversão para a nova origem do sistema de coordenadas):

```cpp
   
// Set the proper orientation for the swap chain, and generate 2D and
// 3D matrix transformations for rendering to the rotated swap chain.
// Note the rotation angle for the 2D and 3D transforms are different.
// This is due to the difference in coordinate spaces.  Additionally,
// the 3D matrix is specified explicitly to avoid rounding errors.

switch (displayRotation)
{
case DXGI_MODE_ROTATION_IDENTITY:
    m_orientationTransform2D = Matrix3x2F::Identity();
    m_orientationTransform3D = ScreenRotation::Rotation0;
    break;

case DXGI_MODE_ROTATION_ROTATE90:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(90.0f) *
        Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
    m_orientationTransform3D = ScreenRotation::Rotation270;
    break;

case DXGI_MODE_ROTATION_ROTATE180:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(180.0f) *
        Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
    m_orientationTransform3D = ScreenRotation::Rotation180;
    break;

case DXGI_MODE_ROTATION_ROTATE270:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(270.0f) *
        Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
    m_orientationTransform3D = ScreenRotation::Rotation90;
    break;

default:
    throw ref new FailureException();
}
    
```

Depois que você tiver a matriz e a origem de rotação corretas para a imagem 2D, a defina com uma chamada para [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857) entre suas chamadas para [**ID2D1DeviceContext::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371768) e [**ID2D1DeviceContext::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371924).

**Aviso**  Direct2D não tem uma pilha de transformação. Se o seu aplicativo também estiver usando o [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857) como parte de seu código de desenho, essa matriz precisa ser multiplicada posteriormente em qualquer outra transformação que você tenha aplicado.

 

```cpp
    ID2D1DeviceContext* context = m_deviceResources->GetD2DDeviceContext();
    Windows::Foundation::Size logicalSize = m_deviceResources->GetLogicalSize();

    context->SaveDrawingState(m_stateBlock.Get());
    context->BeginDraw();

    // Position on the bottom right corner.
    D2D1::Matrix3x2F screenTranslation = D2D1::Matrix3x2F::Translation(
        logicalSize.Width - m_textMetrics.layoutWidth,
        logicalSize.Height - m_textMetrics.height
        );

    context->SetTransform(screenTranslation * m_deviceResources->GetOrientationTransform2D());

    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_TRAILING)
        );

    context->DrawTextLayout(
        D2D1::Point2F(0.f, 0.f),
        m_textLayout.Get(),
        m_whiteBrush.Get()
        );

    // Ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = context->EndDraw();
```

Na próxima vez que você apresentar a cadeia de troca, sua imagem 2D será girada para corresponder à nova orientação de exibição.

## <a name="appendix-b-applying-matrices-for-screen-rotation-3-d"></a>Apêndice B: Aplicando métricas para rotação da tela (3D)


No código de exemplo em [Redimensionando a cadeia de troca e fazendo a pré-rotação de seu conteúdo](#resizing-the-swap-chain-and-pre-rotating-its-contents) (e no [Exemplo de rotação de cadeia de permuta DXGI](https://go.microsoft.com/fwlink/p/?linkid=257600)), definimos uma matriz de transformação específica para cada orientação de tela possível. Agora, vamos examinar as matrizes para girar cenas 3D. Como antes, crie um conjunto de matrizes para cada uma das 4 orientações possíveis. Para evitar erros de arredondamento e, portanto, artefatos visuais secundários, declare explicitamente as matrizes em seu código.

Configure essas matrizes de rotação 3D como a seguir. As matrizes exibidas no seguinte exemplo de código são matrizes de rotação padrão para rotações de 0, 90, 180 e 270 graus das vértices que definem pontos no espaço da cena 3D da câmera. Cada valor de coordenada \[x, y, z\] do vértice na cena é multiplicado por esta matriz de rotação quando a projeção 2D da cena é calculada.

```cpp
   
// 0-degree Z-rotation
static const XMFLOAT4X4 Rotation0( 
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 90-degree Z-rotation
static const XMFLOAT4X4 Rotation90(
    0.0f, 1.0f, 0.0f, 0.0f,
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 180-degree Z-rotation
static const XMFLOAT4X4 Rotation180(
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, -1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 270-degree Z-rotation
static const XMFLOAT4X4 Rotation270( 
    0.0f, -1.0f, 0.0f, 0.0f,
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );            
    }
```

Defina o tipo de rotação na cadeia de troca com uma chamada para [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801), semelhante a isto:

`   m_swapChain->SetRotation(rotation);`

Agora, em seu método de renderização, implemente algum código similar a isso:

``` syntax
struct ConstantBuffer // This struct is provided for illustration.
{
    // Other constant buffer matrices and data are defined here.

    float4x4 projection; // Current matrix for projection
} ;
ConstantBuffer  m_constantBufferData;          // Constant buffer resource data

// ...

// Rotate the projection matrix as it will be used to render to the rotated swap chain.
m_constantBufferData.projection = mul(m_constantBufferData.projection, m_rotationTransform3D);
```

Agora, quando você chamar seu método de renderização, ela multiplicará a matriz de rotação atual (conforme especificado pela variável de classe **m\_orientationTransform3D**) com a matriz de projeção atual e atribuirá os resultados daquela operação como a nova matriz de projeção para seu renderizador. Apresente a cadeia de troca para ver a cena na orientação de exibição atualizada.

 

 




