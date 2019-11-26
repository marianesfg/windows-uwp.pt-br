---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: Efeitos de composição
description: As APIs de efeito permitem que os desenvolvedores personalizem como sua interface do usuário será renderizada.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 57236b6780a7afe996fb1e68ac474d8d8077ca69
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255900"
---
# <a name="composition-effects"></a>Efeitos de composição

As APIs [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) permitem que efeitos em tempo real sejam aplicados a imagens e interface do usuário com propriedades de efeito animáveis. Nesta visão geral, vamos realizar a execução por meio de funcionalidade disponível que permite adicionar efeitos a serem aplicados a uma composição visual.

Para dar suporte a consistência da [Plataforma Universal do Windows (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) para desenvolvedores descrevendo efeitos em seus aplicativos, os efeitos de composição aproveitam a interface IGraphicsEffect do Win2D para usar as descrições do efeito via Namespace [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) Namespace.

Efeitos de pincel são usados para pintar áreas de um aplicativo pela aplicação de efeitos a um conjunto de imagens existentes. As APIs de efeito de composição do Windows 10 APIs estão concentradas em sprites visuais. O SpriteVisual oferece excelente flexibilidade e relacionamento na criação de cores, imagens e efeitos. O SpriteVisual é um tipo de visual de composição que pode preencher um retângulo 2D com um pincel. O visual define os limites do retângulo e o pincel define os pixels usados para desenhar o retângulo.

Os pincéis de efeito são usados em elementos visuais de árvore de composição cujo conteúdo vem da saída de um gráfico de efeito. Os efeitos podem fazer referência a superfícies/texturas existentes, mas não à saída das outras árvores de composição.

Os efeitos também podem ser aplicados a UIElements XAML usando um pincel de efeito com [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="effect-features"></a>Recursos de efeito

- [Biblioteca de efeitos](./composition-effects.md#effect-library)
- [Efeitos de encadeamento](./composition-effects.md#chaining-effects)
- [Suporte à animação](./composition-effects.md#animation-support)
- [Constantes versus propriedades de efeito animado](./composition-effects.md#constant-vs-animated-effect-properties)
- [Várias instâncias de efeito com propriedades independentes](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### <a name="effect-library"></a>Biblioteca de efeitos

Atualmente, a composição é compatível com os efeitos a seguir:

| Efeito               | Descrição                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Transformação afim 2D  | Aplica uma matriz de transformação afim 2D a uma imagem. Usamos esse efeito para animar a máscara alfa em nossos [exemplos](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects) de efeitos.       |
| Composição aritmética | Combina duas imagens usando uma equação flexível. Usamos a composição aritmética para criar um efeito de fading cruzado em nossos [exemplos](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects). |
| Efeito de mesclagem         | Cria um efeito de mesclagem que combina duas imagens. A composição fornece 21 dos 26 [modos de mesclagem](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.htm) com suporte no Win2D.        |
| Fonte de cor         | Gera uma imagem que contém uma cor sólida.                                                                                                                                                                               |
| Composição            | Combina duas imagens. A composição fornece todos os 13 [modos compostos](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.htm) com suporte no Win2D.                                              |
| Contraste             | Aumenta ou reduz o contraste de uma imagem.                                                                                                                                                                           |
| Exposição             | Aumenta ou reduz a exposição de uma imagem.                                                                                                                                                                           |
| Escala de cinza            | Converte uma imagem em cinza monocromático.                                                                                                                                                                                   |
| Transferência de gama       | Altera as cores de uma imagem aplicando uma função de transferência de gama por canal.                                                                                                                                           |
| Matiz girar           | Altera a cor de uma imagem ao girar seus valores de matiz.                                                                                                                                                                   |
| Invert               | Inverte as cores de uma imagem.                                                                                                                                                                                            |
| Saturar             | Altera a saturação de uma imagem.                                                                                                                                                                                         |
| Sépia                | Converte uma imagem em tons de sépia.                                                                                                                                                                                          |
| Temperatura e tonalidade | Ajusta a temperatura e/ou a tonalidade de uma imagem.                                                                                                                                                                           |

Consulte o Namespace [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) do Win2D para obter informações mais detalhadas. Os efeitos sem suporte na composição são indicados como \[nocomposição\].

### <a name="chaining-effects"></a>Efeitos de encadeamento

Os efeitos podem ser encadeados, o que permite a um aplicativo usar simultaneamente vários efeitos em uma imagem. Os gráficos de efeito podem dar suporte a vários efeitos que podem fazer referência um aos outros. Ao descrever seu efeito, basta adicionar um efeito como entrada para o efeito.

```cs
IGraphicsEffect graphicsEffect =
new Microsoft.Graphics.Canvas.Effects.ArithmeticCompositeEffect
{
  Source1 = new CompositionEffectSourceParameter("source1"),
  Source2 = new SaturationEffect
  {
    Saturation = 0,
    Source = new CompositionEffectSourceParameter("source2")
  },
  MultiplyAmount = 0,
  Source1Amount = 0.5f,
  Source2Amount = 0.5f,
  Offset = 0
}
```

O exemplo acima descreve um efeito de composição aritmética que tem duas entradas. A segunda entrada tem um efeito de saturação com uma propriedade de saturação de 0,5.

### <a name="animation-support"></a>Suporte de animação

As propriedades de efeito fornecem suporte à animação. Durante a compilação de efeito, você pode especificar as propriedades de efeito que podem ser animadas e quais podem ser "incorporadas" como constantes. As propriedades animáveis são especificadas por meio de cadeias de caracteres do formato "effect name.property name". Essas propriedades podem ser animadas de forma independente ao longo de várias instâncias do efeito.

### <a name="constant-vs-animated-effect-properties"></a>Propriedades de efeito constantes versus animadas

Durante a compilação de efeitos, você pode especificar as propriedades do efeito como dinâmicas ou como propriedades que são "incorporadas" como constantes. As propriedades dinâmicas são especificadas por meio de cadeias de caracteres de forma "<effect name>.<property name>". As propriedades dinâmicas pode ser definidas com um valor específico ou podem ser animadas usando o sistema de animação de composição.

Ao compilar a descrição de efeito acima, você tem a flexibilidade de incorporar a saturação para que seja igual a 0,5 ou torná-la dinâmica e defini-la dinamicamente ou animá-la.

Compilar um efeito com saturação incorporada:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
```

Compilar um efeito com saturação dinâmica:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{"SaturationEffect.Saturation"});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

A propriedade de saturação do efeito acima pode ser definida como um valor estático ou animado usando animações Expression ou ScalarKeyFrame.

Você pode criar um ScalarKeyFrame que será usado para animar a propriedade Saturation de um efeito da seguinte forma:

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

Inicie a animação na propriedade Saturation do efeito da seguinte forma:

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

Consulte o [exemplo de dessaturação – animação](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/Desaturation - Animation) para propriedades de efeito animadas com quadros chave e o [exemplo AlphaMask](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/AlphaMask) para uso de efeitos e expressões.

### <a name="multiple-effect-instances-with-independent-properties"></a>Várias instâncias de efeito com propriedades independentes

Ao especificar que um parâmetro deve ser dinâmico durante a compilação do efeito, o parâmetro pode ser alterado para cada instância do efeito. Isso permite que dois elementos visuais usem o mesmo efeito mas que sejam renderizados com propriedades de outro efeito. Veja o [sample](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/ColorSource and Blend) ColorSource e Blend para obter mais informações.

## <a name="getting-started-with-composition-effects"></a>Introdução aos efeitos de composição

Este tutorial de início rápido mostra como fazer uso de alguns dos recursos básicos de efeitos.

- [Instalando o Visual Studio](./composition-effects.md#installing-visual-studio)
- [Criando um novo projeto](./composition-effects.md#creating-a-new-project)
- [Instalando o Win2D](./composition-effects.md#installing-win2d)
- [Definindo suas noções básicas de composição](./composition-effects.md#setting-your-composition-basics)
- [Criando um pincel CompositionSurface](./composition-effects.md#creating-a-compositionsurface-brush)
- [Criando, compilando e aplicando efeitos](./composition-effects.md#creating-compiling-and-applying-effects)

### <a name="installing-visual-studio"></a>Instalando o Visual Studio

- Se você não tiver uma versão compatível do Visual Studio instalado, vá até a página de Downloads do Visual Studio [aqui](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs).

### <a name="creating-a-new-project"></a>Crie um novo projeto

- Vá até Arquivo->Novo->Projeto...
- Selecione 'Visual C#'
- Crie um 'Aplicativo em Branco (Universal do Windows)' (Visual Studio 2015)
- Digite um nome para o projeto de sua escolha
- Clique em 'OK'

### <a name="installing-win2d"></a>Instalando o Win2D

O Win2D é lançado como um pacote Nuget.org e deve ser instalado antes que você use os efeitos.

Há duas versões do pacote, uma para o Windows 10 e outra para o Windows 8.1. Para efeitos de composição, você usará a versão do Windows 10.

- Inicie o Gerenciador de Pacotes NuGet em Ferramentas → Gerenciador de Pacotes NuGet → Gerenciar Pacotes NuGet para Solução.
- Procure "Win2D" e selecione o pacote apropriado para a sua versão do Windows. Como Windows.UI. Composition dá suporte ao Windows 10 (mas não ao 8.1), selecione Win2D.uwp.
- Aceite o contrato de licença
- Clique em 'Fechar'

Nas próximas etapas usaremos APIs de composição para aplicar um efeito de saturação a esta imagem de gato que removerá toda a saturação. Nesse modelo, o efeito foi criado e aplicado a uma imagem.

![Imagem de origem](images/composition-cat-source.png)
### <a name="setting-your-composition-basics"></a>Definindo o básico de composição

Consulte o [Exemplo de árvore visual de composição](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/CompositionImageSample) em nosso GitHub para obter um exemplo de como configurar o Compositor Windows.UI.Composition e a raiz ContainerVisual, além de criar a associação com um Janela Principal.

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### <a name="creating-a-compositionsurface-brush"></a>Criando um pincel CompositionSurface

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush);
```

### <a name="creating-compiling-and-applying-effects"></a>Criando, compilando e aplicando efeitos

1. Crie um efeito gráfico

    ```cs
    var graphicsEffect = new SaturationEffect
    {
      Saturation = 0.0f,
      Source = new CompositionEffectSourceParameter("mySource")
    };
    ```

1. Compile o efeito e crie um pincel de efeito

    ```cs
    var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

    var catEffect = effectFactory.CreateBrush();
    catEffect.SetSourceParameter("mySource", surfaceBrush);
    ```

1. Crie um SpriteVisual na árvore de composição e aplique o efeito

    ```cs
    var catVisual = _compositor.CreateSpriteVisual();
      catVisual.Brush = catEffect;
      catVisual.Size = new Vector2(219, 300);
      _root.Children.InsertAtBottom(catVisual);
    }
    ```

1. Crie sua fonte de imagem a ser carregada.

    ```cs
    CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
    CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
    if (result.Status == CompositionImageLoadStatus.Success)
    ```

1. Dimensione e pincele a superfície no SpriteVisual

    ```cs
    brush.Surface = imageSource.Surface;
    ```

1. Execute o aplicativo – seus resultados devem ser um gato sem saturação:

![Imagem sem saturação](images/composition-cat-desaturated.png)

## <a name="more-information"></a>Mais Informações

- [Microsoft – GitHub de composição](https://github.com/microsoft/WindowsCompositionSamples)
- [**Windows. UI. composição**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
- [Equipe de composição do Windows no Twitter](https://twitter.com/wincomposition)
- [Visão geral da composição](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
- [Noções básicas da árvore visual](composition-visual-tree.md)
- [Pincéis de composição](composition-brushes.md)
- [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)
- [Visão geral da animação](composition-animation.md)
- [Interoperação do DirectX nativo de composição e Direct2D com BeginDraw e EndDraw](composition-native-interop.md)
