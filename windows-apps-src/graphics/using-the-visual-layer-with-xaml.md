---
author: jaster
ms.assetid: 
title: Usando a Camada Visual com XAML
description: "Saiba sobre técnicas para usar as APIs de Camada Visual em combinação com o conteúdo XAML existente para criar animações e efeitos avançados."
translationtype: Human Translation
ms.sourcegitcommit: dfda33c70224f32d9c3e8877eabdfcd965521757
ms.openlocfilehash: 00d663b130202f4513cd1a9d82baed4068d909d3

---

# Usando a Camada Visual com XAML

## Introdução

A maioria dos aplicativos que consomem recursos de Camada Visual usará XAML para definir o conteúdo principal da interface do usuário. Na Atualização de Aniversário do Windows 10, há novos recursos na estrutura XAML e na Camada Visual que tornam fácil combinar essas duas tecnologias para criar experiências incríveis para o usuário.
A funcionalidade "interoperabilidade" de XAML e da Camada Visual pode ser usada para criar animações e efeitos avançados que não estão disponíveis quando se usa somente a API de XAML. Isso inclui:

-              Animações controladas por rolagem e paralaxe
-              Animações de layout automáticas
-              Sombras perfeitas de pixel
-              Efeitos de vidro fosco e desfoque

Esses efeitos e animações podem ser aplicados ao conteúdo XAML existente, portanto, você não precisa reestruturar drasticamente seu aplicativo XAML para tirar proveito das novas funcionalidades.
Animações de layout, sombras e efeitos de desfoque são abordados na seção Receitas abaixo. Para ver um exemplo de código implementando paralaxe, consulte o [Exemplo de ParallaxingListItems](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems). O [repositório WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) também tem vários outros exemplos de implementação de animações, sombras e efeitos.

## A classe **ElementCompositionPreview**

[**ElementCompositionPreview**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) é uma classe estática que fornece a funcionalidade "interoperabilidade" do XAML e da Camada Visual. Para obter uma visão geral da Camada Visual e sua funcionalidade, consulte [Camada Visual](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer). A classe **ElementCompositionPreview** fornece os seguintes métodos:

-   [**GetElementVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): obtém um elemento visual "handout" que é usado para renderizar esse elemento
-   [**SetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx): define um elemento visual "handin" como o último filho da árvore visual desse elemento. Esse elemento visual será desenhado sobre o restante do elemento. 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): recupera o conjunto visual usando **SetElementChildVisual**
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): obtém um objeto que pode ser usado para criar animações 60fps com base no deslocamento de rolagem em um **ScrollViewer**

## Comentários sobre **ElementCompositionPreview.GetElementVisual**

**ElementCompositionPreview.GetElementVisual** retorna um elemento visual "handout" que é usado para renderizar o **UIElement** fornecido. Propriedades como **Visual.Opacity**, **Visual.Offset** e **Visual.Size** são definidas pela estrutura de XAML com base no estado do UIElement. Isso permite técnicas como animações de reposicionamento implícito (consulte *receitas*).

Observe que, como **Offset** e **Size** são definidas como resultado do layout da estrutura de XAML, os desenvolvedores devem ter cuidado ao modificar ou animar essas propriedades. Os desenvolvedores só devem modificar ou animar o deslocamento quando o canto superior esquerdo do elemento tem a mesma posição que seu pai no layout. O tamanho geralmente não deve ser modificado, mas pode ser útil acessar a propriedade. Por exemplo, os exemplos Sombra e Vidro fosco abaixo usam o tamanho de um elemento visual "handout" como entrada para uma animação.

Como limitação adicional, as propriedades atualizados do elemento visual "handout" não serão refletidas no UIElement correspondente. Então, por exemplo, definir **UIElement.Opacity** como 0,5 definirá a opacidade do elemento visual "handout" correspondente como 0,5. Entretanto, configurar a **Opacidade** do elemento visual "handout" como 0,5 fará com que o conteúdo apareça em 50% de opacidade, mas não mudará o valor da propriedade de opacidade do UIElement correspondente.

### Exemplo de animação de **deslocamento**

#### Incorreto

```xml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### Correto

```xml
<Border>
    <Canvas Margin="5">
        <Image x:Name="MyImage" />
    </Canvas>
</Border>
```

```csharp
// This works because the Canvas parent doesn’t generate a layout offset.
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

## O método **ElementCompositionPreview.SetElementChildVisual**

O **ElementCompositionPreview.SetElementChildVisual** permite ao desenvolvedor fornecer um elemento visual "handin" que aparecerá como parte da árvore visual de um elemento. Isso permite que os desenvolvedores criem uma "ilha de composição" onde o conteúdo baseado em elementos visuais possa aparecer dentro de uma interface do usuário de XAML. Os desenvolvedores devem ser conservadores no uso dessa técnica porque o conteúdo baseado em elementos visuais não terá a mesma acessibilidade e garantias da experiência do usuário com o conteúdo XAML. Portanto, geralmente, é recomendável que essa técnica só seja usada quando for necessário implementar efeitos personalizados como aqueles encontrados na seção Receitas abaixo.

## Métodos **GetAlphaMask**

[**Image**](https://msdn.microsoft.com/library/windows/apps/br242752), [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) e [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377) implementam, cada um deles, um método denominado **GetAlphaMask** que retorna um **CompositionBrush** que representa uma imagem em escala de cinza com o formato do elemento. Esse **CompositionBrush** pode servir como uma entrada para uma composição **DropShadow**, portanto, a sombra pode refletir a forma do elemento em vez de um retângulo. Isso permite sombras de pixel baseadas em contorno perfeitas para texto, imagens com alfa e formas. Consulte *Sombra* abaixo para obter um exemplo dessa API.

## Receitas

### Animação de reposição

Ao usar animações de composição implícitas, um desenvolvedor pode animar automaticamente alterações no layout de um elemento em relação ao seu pai. Por exemplo, se você alterar a **margem** do botão abaixo, ela será animada automaticamente para a nova posição do layout.

#### Visão geral da implementação

1.            Obtenha o elemento **visual** "handout" para o elemento de destino
2.            Crie um **ImplicitAnimationCollection** que anime automaticamente as alterações na propriedade **Offset**
3.            Associar o **ImplicitAnimationCollection** ao elemento visual

#### XAML

```xml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeRepositionAnimation(RepositionTarget);
}

private void InitializeRepositionAnimation(UIElement repositionTarget)
{
    var targetVisual = ElementCompositionPreview.GetElementVisual(repositionTarget);
    Compositor compositor = targetVisual.Compositor;

    // Create an animation to animate targetVisual's Offset property to its final value
    var repositionAnimation = compositor.CreateVector3KeyFrameAnimation();
    repositionAnimation.Duration = TimeSpan.FromSeconds(0.66);
    repositionAnimation.Target = "Offset";
    repositionAnimation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");

    // Run this animation when the Offset Property is changed
    var repositionAnimations = compositor.CreateImplicitAnimationCollection();
    repositionAnimations["Offset"] = repositionAnimation;

    targetVisual.ImplicitAnimations = repositionAnimations;
}
```

### Sombra

Aplique uma sombra de pixels perfeita a um **UIElement**, por exemplo, um **Ellipse** que contenha uma imagem. Já que a sombra requer um **SpriteVisual** criado pelo aplicativo, precisamos criar um elemento de "host" que conterá o **SpriteVisual** usando **ElementCompositionPreview.SetElementChildVisual**.

#### Visão geral da implementação

1.            Obtenha o elemento **visual** "handout" para o elemento host
2.            Criar um Windows.UI.Composition **DropShadow**
3.            Configurar o **DropShadow** para obter sua forma do elemento de destino por meio de uma máscara
    - **DropShadow** é retangular por padrão, portanto, não será necessário se o destino for retangular
4.            Anexar sombra a um novo **SpriteVisual**e definir o **SpriteVisual** como filho do elemento host
5.            Associar o tamanho do **SpriteVisual** ao tamanho do host usando um **ExpressionAnimation**

#### XAML

```xml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

private void InitializeDropShadow(UIElement shadowHost, Shape shadowTarget)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(shadowHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a drop shadow
    var dropShadow = compositor.CreateDropShadow();
    dropShadow.Color = Color.FromArgb(255, 75, 75, 80);
    dropShadow.BlurRadius = 15.0f;
    dropShadow.Offset = new Vector3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask = shadowTarget.GetAlphaMask();

    // Create a Visual to hold the shadow
    var shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
   ElementCompositionPreview.SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual.StartAnimation("Size", bindSizeAnimation);
}
```

### Vidro fosco

Crie um efeito que desfoque e tinja o conteúdo de plano de fundo. Observe que os desenvolvedores precisam instalar o pacote Win2D NuGet para usar efeitos. Consulte a [página inicial do Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) para obter instruções de instalação.

#### Visão geral da implementação

1.            Obtenha o elemento **visual** "handout" para o elemento host
2.            Criar uma árvore de efeito de desfoque usando Win2D e **CompositionEffectSourceParameter**
3.            Criar um **CompositionEffectBrush** com base na árvore de efeito
4.            Defina a entrada do **CompositionEffectBrush** como um **CompositionBackdropBrush**, que permite que um efeito seja aplicado ao conteúdo atrás de um **SpriteVisual**
5.            Defina o **CompositionEffectBrush** como o conteúdo de um novo **SpriteVisual**e defina o **SpriteVisual** como filho do elemento host
6.            Associar o tamanho do **SpriteVisual** ao tamanho do host usando um **ExpressionAnimation**

#### XAML

```xml
<Grid Width="300" Height="300" Grid.Column="1">
    <Image
        Source="Assets/Images/2.jpg"
        Width="200"
        Height="200" />
    <Canvas
        x:Name="GlassHost"
        Width="150"
        Height="300"
        HorizontalAlignment="Right" />
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializedFrostedGlass(UIElement glassHost)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(glassHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a glass effect, requires Win2D NuGet package
    var glassEffect = new GaussianBlurEffect
    { 
        BlurAmount = 15.0f,
        BorderMode = EffectBorderMode.Hard,
        Source = new ArithmeticCompositeEffect
        {
            MultiplyAmount = 0,
            Source1Amount = 0.5f,
            Source2Amount = 0.5f,
            Source1 = new CompositionEffectSourceParameter("backdropBrush"),
            Source2 = new ColorSourceEffect
            {
                Color = Color.FromArgb(255, 245, 245, 245)
            }
        }
    };

    //  Create an instance of the effect and set its source to a CompositionBackdropBrush
    var effectFactory = compositor.CreateEffectFactory(glassEffect);
    var backdropBrush = compositor.CreateBackdropBrush();
    var effectBrush = effectFactory.CreateBrush();

    effectBrush.SetSourceParameter("backdropBrush", backdropBrush);

    // Create a Visual to contain the frosted glass effect
    var glassVisual = compositor.CreateSpriteVisual();
    glassVisual.Brush = effectBrush;

    // Add the blur as a child of the host in the visual tree
    ElementCompositionPreview.SetElementChildVisual(glassHost, glassVisual);

    // Make sure size of glass host and glass visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    glassVisual.StartAnimation("Size", bindSizeAnimation);
}
```

## Recursos adicionais:

-   [Visão geral da Camada Visual](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer)
-   [Classe **ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/mt608976)
-   Exemplos de Interface do usuário avançada e composição no [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs)
-   [Exemplo de BasicXamlInterop](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
-   [Exemplo de ParallaxingListItems](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)



<!--HONumber=Aug16_HO3-->


