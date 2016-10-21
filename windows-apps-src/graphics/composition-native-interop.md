---
author: scottmill
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: "Interoperação nativa entre DirectX e Direct2D de composição com BeginDraw e EndDraw"
description: "A API Windows.UI.Composition fornece interfaces de interoperação nativa que permitem que o conteúdo seja movido diretamente para o compositor."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 4d1bf75fee06c8f4c31ce23c89bf6267ab9e6394

---
# Interoperação nativa entre DirectX e Direct2D de composição com BeginDraw e EndDraw

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A API Windows.UI.Composition fornece interfaces de interoperação nativa [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068), [**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058) e [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065), que permitem que o conteúdo seja movido diretamente para o compositor.

A interoperação nativa é estruturada em torno de objetos de superfície que contam com texturas do DirectX. As superfícies são criadas a partir de um objeto de fábrica chamado [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749). Esse objeto tem o respaldo de um objeto de dispositivo Direct2D ou Direct3D subjacente, que ele usa para alocar memória de vídeo para superfícies. A API de composição nunca cria o dispositivo DirectX subjacente. É da responsabilidade do aplicativo criar um e passá-lo para o objeto **CompositionGraphicsDevice**. Um aplicativo pode criar mais de um objeto **CompositionGraphicsDevice** por vez e pode usar o mesmo dispositivo DirectX como o dispositivo de renderização para vários objetos **CompositionGraphicsDevice**.

## Criando uma superfície

Cada [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) serve como um alocador de superfícies. Cada superfície é criada com um tamanho inicial (que pode ser 0,0), mas sem pixels válidos. Uma superfície em seu estado inicial pode ser consumida imediatamente em uma árvore visual, por exemplo, via um [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) e um [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433), mas em seu estado inicial, a superfície não tem efeito no resultado da tela. Para todas as finalidades, ela é totalmente transparente, mesmo se o modo alfa especificado for "opaque".

Ocasionalmente, os dispositivos DirectX podem ser considerados inutilizáveis. Isso pode acontecer, entre outros motivos, se o aplicativo passa argumentos inválidos para certas APIs do DirectX, se o adaptador gráfico é reiniciado pelo sistema ou se o driver foi atualizado. O Direct3D tem uma API que um aplicativo que pode ser usado para descobrir, de forma assíncrona, se o dispositivo foi perdido por qualquer motivo. Quando um dispositivo DirectX é perdido, o aplicativo deve descartá-lo, criar um novo e passá-lo para quaisquer objetos [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) associados anteriormente ao dispositivo DirectX com defeito.

## Carregando pixels em uma superfície

Para carregar pixels na superfície, o aplicativo deve chamar o método [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), que retorna uma interface do DirectX que representa uma textura ou contexto do Direct2D, dependendo do que o aplicativo solicita. O aplicativo deve, então, renderizar ou carregar pixels nessa textura. Quando o aplicativo estiver concluído, ele deverá chamar o método [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060). Somente nesse ponto os novos pixels estão disponíveis para composição, mas eles ainda não aparecem na tela até a próxima vez que todas as alterações na árvore visual forem confirmadas. Se a árvore visual for confirmada antes de **EndDraw** ser chamado, a atualização que estiver em andamento não estará visível na tela, e a superfície continuará a exibir o conteúdo que tinha antes de **BeginDraw**. Quando **EndDraw** é chamado, a textura ou o ponteiro de contexto do Direct2D retornado por BeginDraw são invalidados. Um aplicativo nunca deve armazenar esse ponteiro em cache além da chamada a **EndDraw**.

O aplicativo só pode chamar BeginDraw em uma superfície por vez, para qualquer [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) fornecido. Depois de chamar [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), o aplicativo deve chamar [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) nessa superfície antes de chamar **BeginDraw** em outra. Como a API é ágil, o aplicativo é responsável por sincronizar essas chamadas se quiser realizar uma renderização de vários threads de trabalho. Se um aplicativo quiser interromper a renderização de uma superfície e alternar para outra temporariamente, ele poderá usar o método [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx). Isso permite que outro **BeginDraw** seja executado com êxito, mas não disponibiliza a primeira atualização da superfície para na composição na tela. Isso permite que o aplicativo execute várias atualizações de maneira transacional. Quando uma superfície é suspensa, o aplicativo pode continuar a atualização chamando o método [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062), ou pode declarar que a atualização foi concluída chamando **EndDraw**. Isso significa que somente uma superfície pode ser atualizada ativamente por vez para qualquer **CompositionGraphicsDevice** fornecido. Cada dispositivo gráfico mantém esse estado independentemente dos outros, então, um aplicativo pode renderizar nas duas superfícies simultaneamente se pertencerem a dispositivos gráficos diferentes. No entanto, isso impede que as memórias de vídeo dessas duas superfícies sejam agrupados e, dessa forma, é menos eficiente em termos de memória.

Os métodos [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx), [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) e [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) retornam falhas se o aplicativo executar uma operação incorreta (como passar argumentos inválidos ou chamar **BeginDraw** em uma superfície antes de chamar **EndDraw** em outra). Esses tipos de falhas representam bugs do aplicativo e, sendo assim, a expectativa é que eles sejam manipulados com uma falha rápida. **BeginDraw** também poderá retornar uma falha se o dispositivo DirectX subjacente for perdido. Essa falha não é fatal quando o aplicativo pode recriar o dispositivo DirectX e tentar novamente. Dessa forma, o aplicativo deve manipular a perda de dispositivo simplesmente ignorando a renderização. Se **BeginDraw** falhar por qualquer motivo, o aplicativo também não deverá chamar **EndDraw**, já que o início também não teve êxito.

## Rolagem

Por motivos de desempenho, quando um aplicativo chama [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), não é garantido que o conteúdo da textura retornada seja o conteúdo anterior da superfície. O aplicativo deve presumir que o conteúdo seja aleatório e, dessa forma, o aplicativo deve garantir que todos os pixels sejam retocados, seja limpando a superfície antes da renderização ou desenhando conteúdo opaco suficiente para cobrir todo o retângulo atualizado. Isso, combinado com o fato de que o ponteiro de textura só é válido entre chamadas **BeginDraw** e [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060), torna impossível para o aplicativo copiar o conteúdo anterior da superfície. Por esse motivo, oferecemos um método [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063), que permite que o aplicativo executar uma cópia de pixel na mesma superfície.

## Exemplo de uso

O exemplo a seguir ilustra um cenário muito simples onde um aplicativo cria superfícies de desenho e usa [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) e [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) para preencher as superfícies com texto. O aplicativo usa o DirectWrite para definir o layout do texto (detalhes não mostrados) e, em seguida, usa o Direct2D para renderizá-lo. O dispositivo gráficos de composição aceita o dispositivo Direct2D diretamente no momento da inicialização. Isso permite que **BeginDraw** retorne um ponteiro de interface ID2D1DeviceContext, que é consideravelmente mais eficiente do que fazer o aplicativo criar um contexto no Direct2D para encapsular uma interface ID3D11Texture2D retornada em cada operação de desenho.

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```

 

 







<!--HONumber=Aug16_HO3-->


