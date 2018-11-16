---
author: eliotcowley
title: Captura de tela
description: O namespace Windows.Graphics.Capture fornece APIs para adquirir quadros de uma tela ou de uma janela de aplicativo para criar fluxos de vídeo ou instantâneos para criar experiências colaborativas e interativas.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, captura de tela
ms.localizationpriority: medium
ms.openlocfilehash: d28ed1fce79a815155180ab8a3c708e2c8bf8916
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995024"
---
# <a name="screen-capture"></a>Captura de tela

A partir do Windows 10, versão 1803, o namespace [Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) fornece APIs a fim de obter quadros de uma janela de exibição ou aplicativo para criar streaming de vídeo ou instantâneos e criar experiências colaborativas e interativas.

Com a captura de tela, os desenvolvedores chamam a interface do usuário do sistema para que os usuários finais selecionem a tela ou a janela do aplicativo para captura, e uma borda de notificação amarela é desenhada pelo sistema ao redor do item capturado ativamente. No caso de várias sessões de captura simultânea, uma borda amarela é desenhada em volta de cada item sendo capturado.

> [!NOTE]
> A captura de tela APIs são suportadas apenas em desktops e fones de ouvido imersivos do Windows Mixed Reality.

## <a name="add-the-screen-capture-capability"></a>Adicionar a funcionalidade de captura de tela

As APIs encontradas no namespace **Graphics** exigem uma funcionalidade geral para ser declaradas no manifesto do aplicativo:
    
1. Abra **Package. appxmanifest** no **Gerenciador de soluções**.
2. Selecione a guia **Recursos**.
3. Verifique a **captura de elementos gráficos**.

![Captura de elementos gráficos](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Iniciar o interface do usuário do sistema para iniciar a captura de tela

Antes de iniciar a interface do usuário do sistema, verifique se o aplicativo é capaz de fazer capturas de tela. Há vários motivos para o aplicativo não ser capaz de usar a captura de tela, incluindo se o dispositivo não atende aos requisitos de hardware ou se o aplicativo direcionado para a captura de tela bloqueia a captura. Use o método **IsSupported** na classe [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) para determinar se a captura de tela da UWP é suportada:

```cs
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported()) 
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed; 
    }    
}
```

Depois de confirmar se a captura de tela é compatível, use a classe [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) para invocar a interface do usuário do seletor do sistema. O usuário final usa essa interface do usuário para selecionar tela ou a janela do aplicativo deles para fazer capturas de tela. O seletor retornará um [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem) que será usado para criar uma **GraphicsCaptureSession**:

```cs
public async Task StartCaptureAsync() 
{ 
    // The GraphicsCapturePicker follows the same pattern the 
    // file pickers do. 
    var picker = new GraphicsCapturePicker(); 
    GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 

    // The item may be null if the user dismissed the 
    // control without making a selection or hit Cancel. 
    if (item != null) 
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item); 
    } 
}
```

Como esse é o código de interface do usuário, ele precisa ser chamado no thread da interface do usuário. Se você estiver chamando-o em code-behind para uma página do seu aplicativo (por exemplo, **MainPage.xaml.cs**) isso é feito para você automaticamente, mas se não, você pode forçar a ser executado no thread da interface do usuário com o código a seguir:

```cs
CoreWindow window = CoreApplication.MainView.CoreWindow;
           
await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Criar um pool de quadro de captura e uma sessão de captura

Usando o **GraphicsCaptureItem**, você criará um [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) com o formato de pixel compatível com o dispositivo D3D (**DXGI\_FORMAT\_B8G8R8A8\_UNORM**), o número desejado de quadros (que pode ser um inteiro) e um tamanho de quadro. A propriedade **ContentSize** da classe **GraphicsCaptureItem** pode ser usada como tamanho do quadro:

```cs
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item) 
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create( 
        _canvasDevice, // D3D device 
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
        2, // Number of frames 
        _item.Size); // Size of the buffers   
} 
```

Em seguida, obtenha uma instância da classe **GraphicsCaptureSession** para seu **Direct3D11CaptureFramePool** passando o **GraphicsCaptureItem** para o método **CreateCaptureSession**:

```cs
_session = _framePool.CreateCaptureSession(_item);
```

Depois que o usuário consente explicitamente para a captura de uma janela ou tela de aplicativo na interface do usuário, o **GraphicsCaptureItem** podem ser associado a vários objetos **CaptureSession**. Dessa forma, o aplicativo pode optar por capturar o mesmo item para várias experiências.

Para capturar vários itens ao mesmo tempo, o aplicativo deve criar uma sessão de captura para cada item a ser capturado, o que requer a invocação da interface do usuário do seletor para cada item capturado.

## <a name="acquire-capture-frames"></a>Adquirir quadros de captura

Com o pool de quadros e a sessão de captura criada, chame o método **StartCapture** na instância **GraphicsCaptureSession** a fim de notificar o sistema para começar a enviar quadros ao aplicativo:

```cs
_session.StartCapture();
```

Para adquirir esses quadros de captura, que são objetos [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe), você pode usar o evento **Direct3D11CaptureFramePool.FrameArrived**:

```cs
_framePool.FrameArrived += (s, a) => 
{ 
    // The FrameArrived event fires for every frame on the thread that         
    // created the Direct3D11CaptureFramePool. This means we don't have to 
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame()) 
    {
        // We'll define this method later in the document.
        ProcessFrame(frame); 
    }  
}; 
```

É recomendável evitar usar o thread de interface do usuário se possível para **FrameArrived**, pois esse evento será gerado cada vez que um novo quadro estiver disponível, ou seja, frequentemente. Se você optar por ouvir **FrameArrived** no thread da interface do usuário, lembre-se de quanto trabalho você estará executando sempre que o evento for acionado.

Como alternativa, você pode puxar quadros manualmente com o método **Direct3D11CaptureFramePool.TryGetNextFrame** até obter todos os quadros que você precisa.

O objeto **Direct3D11CaptureFrame** contém as propriedades **ContentSize**, **Surface** e **SystemRelativeTime**. O **SystemRelativeTime** é o tempo QPC ([QueryPerformanceCounter](https://msdn.microsoft.com/library/windows/desktop/ms644904)) que pode ser usado para sincronizar outros elementos de mídia.

## <a name="processing-capture-frames"></a>Processamento de quadros de captura

Cada quadro do **Direct3D11CaptureFramePool** é verificado ao chamar **TryGetNextFrame** de acordo com o tempo de vida do objeto **Direct3D11CaptureFrame**. Para aplicativos nativos, a liberação do objeto **Direct3D11CaptureFrame** é suficiente para verificar o quadro novamente no pool de quadros. Para aplicativos gerenciados, é recomendável usar o método **Direct3D11CaptureFrame.Dispose** (**Fechar** em C++). **Direct3D11CaptureFrame** implementa a interface [IFechável](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable), que é projetada como [IDescartável](https://msdn.microsoft.com/library/system.idisposable.aspx) para chamadas de C#.

Os aplicativos não deve salvar referências em objetos **Direct3D11CaptureFrame** nem devem salvar referências na superfície Direct3D subjacente depois que o quadro foi verificado novamente.

Ao processar um quadro, é recomendável que aplicativos usem o bloqueio [ID3D11Multithread](https://msdn.microsoft.com/library/windows/desktop/mt644886) no mesmo dispositivo associado ao objeto **Direct3D11CaptureFramePool**.

A superfície Direct3D subjacente sempre terá o tamanho especificado ao criar (ou recriar) o **Direct3D11CaptureFramePool**. Se o conteúdo for maior do que o quadro, então é cortado para se adequado ao tamanho do quadro. Se o conteúdo for menor do que o quadro, o restante deste contém dados indefinidos. É recomendável que os aplicativos copiem uma sub-retângulo usando a propriedade **ContentSize** desse **Direct3D11CaptureFrame** para evitar a exibição de conteúdo indefinido.

## <a name="react-to-capture-item-resizing-or-device-lost"></a>Reagir ao redimensionamento de item de captura ou dispositivo perdido

Durante o processo de captura, os aplicativos podem desejar alterar aspectos de **Direct3D11CaptureFramePool**. Isso inclui o fornecimento de um novo dispositivo Direct3D, alteração do tamanho dos buffers de quadro ou até mesmo alterar o número de buffers em um pool. Em cada um desses cenários, o método **Recreate** no objeto **Direct3D11CaptureFramePool** é a ferramenta recomendada.

Quando **Recreate** é chamado, todos os quadros existentes são descartados. Isso é para impedir o envio de quadros cujas superfícies subjacentes do Direct3D pertencem a um dispositivo ao qual o aplicativo não tem mais acesso. Por esse motivo, pode ser aconselhável processar todos os quadros pendentes antes de chamar **Recreate**.

## <a name="putting-it-all-together"></a>Reunindo tudo

O trecho de código a seguir está um exemplo de ponta a ponta de como implementar a captura de tela em um aplicativo UWP. Neste exemplo, temos um botão no front-end que, quando clicado, chama o método **Button_ClickAsync** .

> [!NOTE]
> Este trecho de código usa [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm), uma biblioteca para renderização de gráficos 2D. Consulte a documentação para obter informações sobre como configurá-lo para seu projeto.

```cs
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace WindowsGraphicsCapture
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Capture API objects.
        private SizeInt32 _lastSize;
        private GraphicsCaptureItem _item;
        private Direct3D11CaptureFramePool _framePool;
        private GraphicsCaptureSession _session;

        // Non-API related members.
        private CanvasDevice _canvasDevice;
        private CompositionGraphicsDevice _compositionGraphicsDevice;
        private Compositor _compositor;
        private CompositionDrawingSurface _surface;

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();
            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice);
            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with the composition APIs.

            var visual = _compositor.CreateSpriteVisual();
            visual.RelativeSizeAdjustment = Vector2.One;
            var brush = _compositor.CreateSurfaceBrush(_surface);
            brush.HorizontalAlignmentRatio = 0.5f;
            brush.VerticalAlignmentRatio = 0.5f;
            brush.Stretch = CompositionStretch.Uniform;
            visual.Brush = brush;
            ElementCompositionPreview.SetElementChildVisual(this, visual);
        }

        public async Task StartCaptureAsync()
        {
            // The GraphicsCapturePicker follows the same pattern the 
            // file pickers do. 
            var picker = new GraphicsCapturePicker();
            GraphicsCaptureItem item = await picker.PickSingleItemAsync();

            // The item may be null if the user dismissed the 
            // control without making a selection or hit Cancel. 
            if (item != null)
            {
                StartCaptureInternal(item);
            }
        }

        private void StartCaptureInternal(GraphicsCaptureItem item)
        {
            // Stop the previous capture if we had one.
            StopCapture();

            _item = item;
            _lastSize = _item.Size;

            _framePool = Direct3D11CaptureFramePool.Create(
               _canvasDevice, // D3D device 
               DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
               2, // Number of frames 
               _item.Size); // Size of the buffers 

            _framePool.FrameArrived += (s, a) =>
            {
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we 
                // don't have to do a null-check here, as we know we're the only 
                // one dequeueing frames in our application.  

                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.

                using (var frame = _framePool.TryGetNextFrame())
                {
                    ProcessFrame(frame);
                }
            };

            _item.Closed += (s, a) =>
            {
                StopCapture();
            };

            _session = _framePool.CreateCaptureSession(_item);
            _session.StartCapture();
        }

        public void StopCapture()
        {
            _session?.Dispose();
            _framePool?.Dispose();
            _item = null;
            _session = null;
            _framePool = null;
        }

        private void ProcessFrame(Direct3D11CaptureFrame frame)
        {
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids 
            // throwing in the catch block below (device creation could always 
            // fail) along with ensuring that resize completes successfully and 
            // isn’t vulnerable to device-lost.   
            bool needsReset = false;
            bool recreateDevice = false;

            if ((frame.ContentSize.Width != _lastSize.Width) ||
                (frame.ContentSize.Height != _lastSize.Height))
            {
                needsReset = true;
                _lastSize = frame.ContentSize;
            }

            try
            {
                // Take the D3D11 surface and draw it into a  
                // Composition surface.

                // Convert our D3D11 surface into a Win2D object.
                var canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                    _canvasDevice,
                    frame.Surface);

                // Helper that handles the drawing for us.
                FillSurfaceWithBitmap(canvasBitmap);
            }

            // This is the device-lost convention for Win2D.
            catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
            {
                // We lost our graphics device. Recreate it and reset 
                // our Direct3D11CaptureFramePool.  
                needsReset = true;
                recreateDevice = true;
            }

            if (needsReset)
            {
                ResetFramePool(frame.ContentSize, recreateDevice);
            }
        }

        private void FillSurfaceWithBitmap(CanvasBitmap canvasBitmap)
        {
            CanvasComposition.Resize(_surface, canvasBitmap.Size);

            using (var session = CanvasComposition.CreateDrawingSession(_surface))
            {
                session.Clear(Colors.Transparent);
                session.DrawImage(canvasBitmap);
            }
        }

        private void ResetFramePool(SizeInt32 size, bool recreateDevice)
        {
            do
            {
                try
                {
                    if (recreateDevice)
                    {
                        _canvasDevice = new CanvasDevice();
                    }

                    _framePool.Recreate(
                        _canvasDevice,
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,
                        2,
                        size);
                }
                // This is the device-lost convention for Win2D.
                catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
                {
                    _canvasDevice = null;
                    recreateDevice = true;
                }
            } while (_canvasDevice == null);
        }

        private async void Button_ClickAsync(object sender, RoutedEventArgs e)
        {
            await StartCaptureAsync();
        }
    }
}
```

## <a name="see-also"></a>Veja também

* [Namespace Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
