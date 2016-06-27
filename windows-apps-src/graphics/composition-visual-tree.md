---
author: scottmill
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: "Árvore visual de composição"
description: "Elementos visuais de composição compõem a estrutura da árvore visual que todos os outros recursos da API de composição usam e têm como referência. A API permite que os desenvolvedores definam e criem um ou vários objetos visuais, cada um representando um único nó em uma árvore visual."
ms.sourcegitcommit: b3d198af0c46ec7a2041a7417bccd56c05af760e
ms.openlocfilehash: e31d3b23871bd37cde26fb3e9414e8cfae340d3d

---
# Árvore visual de composição

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Elementos visuais de composição compõem a estrutura da árvore visual que todos os outros recursos da API de composição usam e têm como referência. A API permite que os desenvolvedores definam e criem um ou vários objetos visuais, cada um representando um único nó em uma árvore visual.

## Elementos visuais

Há três tipos de elementos visuais que compõem a estrutura da árvore visual, além de uma classe de pincel de base com várias subclasses que afetam o conteúdo de um elemento visual:

-   [
              **Visual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706858) – objeto base, a maioria das propriedades estão aqui e são herdadas por outros objetos visuais.
-   [
              **ContainerVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706810) – deriva de [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) e adiciona a capacidade de criar filhos.
-   [
              **SpriteVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589433) – deriva de [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) e adiciona a capacidade de associar um pincel para que o elemento visual possa renderizar pixels, incluindo imagens, efeitos ou uma cor sólida.
-   [
              **CompositionBrush**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589398) – permite a aplicação de um efeito ao conteúdo de um elemento visual. Há uma série de subclasses de CompositionBrush.

## Amostra CompositionVisual

Na amostra, há uma série de quadrados de cor sólida que podem ser clicados e arrastados na tela. Quando você clica em um quadrado, ele vai para frente, gira 45 graus e fica opaco quando arrastado.

Isso mostra uma série de conceitos básicos para trabalhar com a API, incluindo:

-   Criar um compositor
-   Criar um SpriteVisual com um ColorBrush
-   Recortar um elemento visual
-   Girar um elemento visual
-   Definir a opacidade
-   Alterar a posição do elemento visual na coleção

Na amostra, também há três elementos visuais diferentes no trabalho:

-   [
              **Visual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706858) – objeto base, a maioria das propriedades estão aqui e são herdadas por outros objetos visuais.
-   [
              **ContainerVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706810) – deriva de Visual e adiciona a capacidade de criar filhos.
-   [
              **SpriteVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589433) – deriva do elemento visual e adiciona a capacidade de associar um pincel para que o elemento visual possa renderizar pixels, incluindo imagens, efeitos ou uma cor sólida.

Embora esta amostra não abranja conceitos como animações ou efeitos mais complexos, ela contém os blocos de construção que todos esses sistemas usam.

## Criar um compositor

Criar um [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) e armazená-lo para uso como alocador em uma variável é uma tarefa simples. O trecho a seguir mostra como criar um novo **Compositor**:

```cs
_compositor = new Compositor();
```

## Criar um SpriteVisual e ColorBrush

Com o [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789), é fácil criar objetos sempre que você precisar deles, como um [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) e um [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399):

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Embora isso seja apenas algumas linhas de código, ele demonstra um conceito útil: os objetos [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) são o coração do sistema de efeitos. O **SpriteVisual** oferece excelente flexibilidade e relacionamento na criação de cores, imagens e efeitos. O **SpriteVisual** é um tipo de elemento visual único que pode preencher um retângulo 2D com um pincel, neste caso, uma cor sólida.

## Recortar um elemento visual

O [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) também pode ser usado para criar clipes para um [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858). A seguir está um exemplo da amostra do uso de [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) para cortar cada lado do elemento visual:

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

Observação: como outros objetos na API, [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) pode ter animações aplicadas às suas propriedades.

## <span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Girar um clipe

Um [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) pode ser transformado com uma rotação. Observe que [**RotationAngle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.rotationangle) aceita radianos e graus. Esse elemento assume radianos como padrão, mas é fácil especificar graus, conforme mostrado no trecho a seguir:

```cs
child.RotationAngleInDegrees = 45.0f;
```

Rotation é apenas um exemplo de um conjunto de componentes de transformação fornecidos pela API para facilitar essas tarefas. Outros exemplos incluem Offset, Scale, Orientation, RotationAxis e 4x4 TransformMatrix.

## Definir a opacidade

Definir a opacidade de um elemento visual é uma operação simples que usa um valor flutuante. Por exemplo, na amostra, todos os quadrados começam na opacidade 0.8:

```cs
visual.Opacity = 0.8f;
```

Assim como a rotação, a propriedade [**Opacity**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.opacity) pode ser animada.

## Alterar a posição do elemento visual na coleção

A API de composição permite que a posição de um elemento visual em um [**VisualCollection**](https://msdn.microsoft.com/library/windows/apps/Dn706858collection) seja alterada de várias formas. Ele pode ser colocado acima de outro elemento visual com [**InsertAbove**](https://msdn.microsoft.com/library/windows/apps/Dn706858collection_insertabove), colocado abaixo com [**InsertBelow**](https://msdn.microsoft.com/library/windows/apps/Dn706858collection_insertbelow), ser movido para o início com [**InsertAtTop**](https://msdn.microsoft.com/library/windows/apps/Dn706858collection_insertattop) ou para o final com [**InsertAtBottom**](https://msdn.microsoft.com/library/windows/apps/Dn706858collection_insertatbottom).

Na amostra, um [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) que foi clicado é classificado para ficar no início:

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## Exemplo completo

No exemplo completo, todos os conceitos acima são usados juntos para construir e movimentar uma árvore simple de objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) para alterar a opacidade sem usar XAML, WWA ou DirectX. Este exemplo mostra como objetos **Visual** filho são criados e adicionados e como as propriedades são alteradas.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing 
            //var visual = _compositor.CreateSolidColorVisual() and 
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```

 

 







<!--HONumber=Jun16_HO3-->


