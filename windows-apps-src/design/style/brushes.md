---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Usar pincéis
description: Os objetos de pincel são usados para pintar os interiores ou os contornos das formas, texto e partes de controles, de maneira que o objeto pintado fique visível em uma interface do usuário.
ms.date: 04/28/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 350b07e96d95b043116f2eb8029a2352360c50d6
ms.sourcegitcommit: 490b563462853f10f14825f2358e4852ee1011fb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82632997"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Usar pincéis para pintar tela de fundo, primeiro plano e contorno

Use objetos [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) para pintar os interiores e os contornos das formas, do texto e dos controles XAML, tornando-os visíveis na interface do usuário do aplicativo.

> **APIs importantes**:  [Classe Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Introdução aos pincéis

Para pintar uma [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape), um texto ou partes de um [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) que é exibido na tela do aplicativo, defina a propriedade [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) da **Shape** ou as propriedades de [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) e [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) de um **Control** com um valor de **Brush**.

Os diferentes tipos de pincéis são: 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 
-   [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Pincéis de cor sólida

Um [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) pinta uma área com uma única [**Color**](/uwp/api/Windows.UI.Color), como vermelho ou azul. Esse é o pincel mais básico. No XAML, há três maneiras de definir um **SolidColorBrush** e a cor que ele especifica: nomes de cores predefinidas, valores de cores hexadecimais ou a sintaxe do elemento da propriedade.

### <a name="predefined-color-names"></a>Nomes de cores predefinidas

Você pode usar um nome de cor predefinido, como [**Yellow**](/uwp/api/windows.ui.colors.yellow) ou [**Magenta**](/uwp/api/windows.ui.colors.magenta). Existem 256 nomes de cores disponíveis. Um analisador XAML converte o nome da cor em uma estrutura de [**Color**](/uwp/api/Windows.UI.Color) com os canais de cor corretos. Os 256 nomes de cores são baseados nos nomes de cores *X11* da especificação CSS3 (Folhas de Estilos em Cascata, Nível 3), portanto, é possível que você já esteja familiarizado com essa lista de nomes de cores se tiver experiência em design ou desenvolvimento para a Web.

Este é um exemplo que define a propriedade [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) de um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) como a cor predefinida [**Red**](/uwp/api/windows.ui.colors.red).

```xaml
<Rectangle Width="100" Height="100" Fill="Red" />
```

![Um SolidColorBrush renderizado](images/brushes-solidcolorbrush.jpg)

*SolidColorBrush aplicado a um Rectangle*

Se você estiver definindo um [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) usando código em vez de XAML, cada cor de nome estará disponível como um valor estático de propriedade da classe [**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors). Por exemplo, para declarar um valor [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) de um **SolidColorBrush** para representar o nome de cor "Orquídea", defina o valor **Color** como o valor estático [**Colors.Orchid**](/uwp/api/windows.ui.colors.orchid).

### <a name="hexadecimal-color-values"></a>Valores de cores em hexadecimal

Você pode usar uma cadeia de caracteres de formato hexadecimal para declarar os valores precisos de cor de 24 bits com canal alfa de 8 bits para um [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Dois caracteres na faixa de 0 a F definem cada valor de componente e a ordem dos valores de componentes da cadeia hexadecimal é: canal alfa (opacidade), canal vermelho, canal verde e canal azul (**ARGB**). Por exemplo, o valor hexadecimal "\#FFFF0000" define o vermelho totalmente opaco (alfa="FF", vermelho="FF", verde="00" e azul="00").

Este exemplo de XAML define a propriedade [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) de um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) como o valor hexadecimal "\#FFFF0000" e produz um resultado idêntico ao usar o nome de cor [**Colors.Red**](/uwp/api/windows.ui.colors.red).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="property-element-syntax"></a>Sintaxe de elemento da propriedade

Você pode usar a sintaxe do elemento da propriedade para definir um [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Esta sintaxe é mais prolixa do que os métodos anteriores, mas você pode especificar configurações adicionais, como o [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) do pincel. Para obter mais informações sobre a sintaxe XAML, incluindo a sintaxe dos elementos de propriedade, confira a [Visão geral sobre o XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview) e o [Guia de sintaxe XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-syntax-guide).

Nos exemplos anteriores, o pincel está sendo criado implícita e automaticamente, como parte de uma linguagem XAML abreviada deliberada que ajuda a manter as definições da interface do usuário simples para os casos mais comuns. O exemplo a seguir cria um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) e cria explicitamente o [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) como um valor de elemento para uma propriedade [**Rectangle.Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill). A [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) do **SolidColorBrush** é definida como [**Blue**](/uwp/api/windows.ui.colors.blue) e a [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) é definida como 0,5.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="linear-gradient-brushes"></a>Pincéis de gradiente lineares

Um [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) pinta uma área com um gradiente que é definido ao longo de uma linha. Esta linha é chamada *eixo do gradiente*. Você especifica as cores do gradiente e suas localizações ao longo do eixo do gradiente usando os objetos [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop). Por padrão, o eixo de gradiente é executado do canto superior esquerdo até o canto inferior direito da área pintada pelo pincel, resultando em um sombreado diagonal.

A [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) é um bloco de construção básico de um pincel de gradiente. Uma marca de gradiente especifica a [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) do pincel que está em um [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) ao longo do eixo do gradiente, quando o pincel é aplicado na área sendo pintada.

A propriedade [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) da marca do gradiente especifica a cor da marca do gradiente. Você pode definir a cor usando um nome de cor predefinido ou especificando os valores hexadecimais de **ARGB**.

A propriedade [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) de uma [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) especifica a posição de cada **GradientStop** ao longo do eixo do gradiente. O **Offset** é um **double** que vai de 0 a 1. Um **Offset** igual a 0 coloca a **GradientStop** no início do eixo do gradiente, em outras palavras, próximo ao seu [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint). Um **Offset** igual a 1 coloca a **GradientStop** no [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint). No mínimo, um [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) útil deve ter dois valores **GradientStop**, em que cada **GradientStop** deve especificar uma [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) diferente e ter um **Offset** diferente entre 0 e 1.

Este exemplo cria um gradiente linear com quatro cores e a usa para pintar um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

A cor de cada ponto entre as marcas do gradiente são linearmente interpoladas como uma combinação da cor especificada pelas duas marcas de gradiente associadas. A imagem a seguir destaca as marcas de gradiente do exemplo anterior. Os círculos marcam a posição das marcas do gradiente e uma linha tracejada mostra o eixo do gradiente.

![Marcas de gradiente](images/linear-gradients-stops.png)

*Combinação de cores especificadas pelas duas marcas de gradiente delimitadoras*

Você pode alterar a linha na qual as marcas de gradiente são posicionadas definindo as propriedades [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) e [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) com valores diferentes dos padrões iniciais `(0,0)` e `(1,1)`. Ao alterar os valores das coordenadas **StartPoint** e **EndPoint**, você pode criar gradientes horizontais ou verticais, inverter a direção do gradiente ou condensar o gradiente espalhado para aplicar a um intervalo menor que a área totalmente pintada. Para condensar o gradiente, você define os valores de **StartPoint** e/ou **EndPoint** como algo entre os valores 0 e 1. Por exemplo, para um gradiente horizontal em que o esmaecimento acontece totalmente na metade esquerda do pincel e o lado direito é sólido como sua última cor [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop), especifique um **StartPoint** de `(0,0)` e um **EndPoint** de `(0.5,0)`.

### <a name="use-tools-to-make-gradients"></a>Usar ferramentas para criar gradientes

Agora que já sabe como gradientes lineares funcionam, você pode usar o Visual Studio ou o Blend para tornar a criação desses gradientes mais fácil. Para criar um gradiente, selecione o objeto ao qual você deseja aplicar um gradiente na superfície de design ou no modo de exibição XAML. Expanda **Pincel** e selecione a guia **Gradiente Linear**.

![Criar um gradiente linear no Visual Studio](images/tool-gradient-brush-1.png)

*Como criar um gradiente linear no Visual Studio*

Agora, você pode mudar as cores das marcas do gradiente e deslizar as posições usando a barra na parte inferior. Você também pode adicionar novas marcas de gradiente clicando na barra e remover arrastando as marcas para fora da barra (veja a próxima captura de tela).

![Barra na parte inferior da janela de propriedades que controla as marcas de gradiente](images/tool-gradient-brush-2.png)

*Controle deslizante da configuração de gradiente*

## <a name="radial-gradient-brushes"></a>Pincéis de gradiente radial

Um [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) é desenhado em uma elipse que é definida pelas propriedades [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) e [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy). As cores do gradiente começam no centro da elipse e terminam no raio.

As cores do gradiente radial são definidas por marcas de cor adicionadas à propriedade da coleção de [**GradientStops**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientstops). Cada marca de gradiente especifica uma cor e um deslocamento ao longo do gradiente.

A origem do gradiente usa como padrão o centro e pode ser deslocada usando a propriedade [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin).

[MappingMode](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) define se [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx), [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) e [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) representam coordenadas relativas ou absolutas.

Quando [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) é definido como `RelativeToBoundingBox`, os valores X e Y das três propriedades são tratados em relação aos limites do elemento, em que `(0,0)` representa a parte superior esquerda e `(1,1)` representa a parte inferior direita dos limites do elemento para as propriedades [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) e [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) e `(0,0)` representa o centro da propriedade [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin).

Quando [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) é definido como `Absolute`, os valores X e Y das três propriedades são tratados como coordenadas absolutas dentro dos limites do elemento.

Este exemplo cria um gradiente linear com quatro cores e a usa para pintar um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<!-- This rectangle is painted with a radial gradient. -->
<Rectangle Width="200" Height="200">
    <Rectangle.Fill>
        <media:RadialGradientBrush>
            <GradientStop Color="Blue" Offset="0.0" />
            <GradientStop Color="Yellow" Offset="0.2" />
            <GradientStop Color="LimeGreen" Offset="0.4" />
            <GradientStop Color="LightBlue" Offset="0.6" />
            <GradientStop Color="Blue" Offset="0.8" />
            <GradientStop Color="LightGray" Offset="1" />
        </media:RadialGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

A cor de cada ponto entre as marcas de gradiente é radialmente interpolada como uma combinação da cor especificada pelas duas marcas de gradiente associadas. A imagem a seguir destaca as marcas de gradiente do exemplo anterior. 

![Marcas de gradiente](images/radial-gradient.png)

*Marcas de gradiente*

## <a name="image-brushes"></a>Pincéis da imagem

Um [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) pinta uma imagem em uma área e a imagem a ser pintada se origina de uma origem de arquivo de imagem. Você define a propriedade [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) com o caminho da imagem a ser carregada. Geralmente, a origem da imagem vem de um item **Content** que faz parte dos recursos do seu aplicativo.

Por padrão, um [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) estica a sua imagem para preencher completamente a área pintada, possivelmente distorcendo a imagem se a área pintada tiver uma proporção diferente da imagem. Você pode mudar este comportamento mudando a propriedade [**Stretch**](/uwp/api/windows.ui.xaml.media.tilebrush.stretch) de seu valor padrão de **Fill** e o definindo como **None**, **Uniform** ou **UniformToFill**.

O exemplo a seguir cria um [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) e define [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) para uma imagem chamada licorice.jpg, que você deve incluir como um recurso no aplicativo. O **ImageBrush** pinta a área definida por uma forma [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

![Um ImageBrush renderizado.](images/brushes-imagebrush.jpg)

*Um ImageBrush renderizado*

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) e [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) fazem referência a um arquivo de origem de imagem pelo URI (Uniform Resource Identifier), em que esse arquivo de origem de imagem usa vários formatos de imagem possíveis. Esses arquivos de origem de imagem são especificados como URIs. Para saber mais sobre a especificação de origens de imagem, os formatos de imagem utilizáveis e o empacotamento deles em um aplicativo, consulte [Image e ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes).

## <a name="brushes-and-text"></a>Pincéis e texto

Você também pode usar pincéis para aplicar características de renderização aos elementos de texto. Por exemplo, a propriedade [**Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) de [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) tem um [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Você pode todas os pincéis descritos aqui em texto. No entanto, tenha cuidado com os pincéis aplicados ao texto, pois qualquer tela de fundo poderá tornar o texto ilegível se você usar pincéis que causam sangramento na tela de fundo. Use [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) para legibilidade dos elementos de texto na maior parte dos casos, a menos que você queira que o elemento de texto seja principalmente decorativo.

Mesmo quando você usa uma cor sólida, verifique se a cor de texto que você escolher tenha contraste suficiente com a cor da tela de fundo do contêiner de layout de texto. O nível de contraste entre o tela de fundo do texto e o primeiro plano do contêiner é uma consideração de acessibilidade.

## <a name="webviewbrush"></a>WebViewBrush

Um [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) é um tipo especial de pincel que pode acessar o conteúdo normalmente exibido em um controle [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView). Em vez de renderizar o conteúdo na área de controle **WebView** retangular, o **WebViewBrush** pinta esse conteúdo em outro elemento que tem uma propriedade -type do [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) para renderizar a superfície. **WebViewBrush** não é adequado para todos os cenários de pincel, mas é útil nas transições de um **WebView**. Para saber mais, confira [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) é uma classe base usada para criar pincéis personalizados que usam [**CompositionBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) para pintar elementos XAML da interface do usuário.

Ele permite a interoperação "suspensa" entre as camadas Windows.UI.Xaml e Windows.UI.Composition conforme descrito na [**Visão geral da Camada Visual**](/windows/uwp/composition/visual-layer). 

Para criar um pincel personalizado, crie uma nova classe que herde de XamlCompositionBrushBase e implemente os métodos necessários.

Por exemplo, ele poderá ser usado para aplicar [**efeitos**](/uwp/composition/composition-effects) a UIElements do XAML usando um [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush), como um **GaussianBlurEffect** ou [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) que controla as propriedades reflexivas de um UIElement do XAML quando estiver sendo aceso por um [**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight).

Para obter exemplos de código, confira [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pincéis como recursos XAML

Você pode declarar qualquer pincel para ser um recurso XAML com chave em um dicionário de recursos XAML. Isso torna mais fácil replicar os valores de pincel quando são aplicados a vários elementos em uma interface do usuário. Os valores de pincel são compartilhados e aplicativos a qualquer caso no qual você faz referência do recurso de pincel como uma utilização de [{StaticResource}](/uwp/xaml-platform/staticresource-markup-extension) no seu XAML. Isso inclui os casos em que você tem um modelo de controle XAML que faz referência ao pincel compartilhado e o modelo de controle é um recurso XAML com chave.

## <a name="brushes-in-code"></a>Pincéis em código

É muito mais comum especificar os pincéis usando XAML do que usando código. Isso acontece porque os pincéis costumam ser definidos como recursos XAML e porque os valores de pincel geralmente são a saída das ferramentas de design ou fazem parte de uma definição da IU XAML. Além disso, se você por acaso quiser definir um pincel usando código, todos os tipos de [**Pincel**](/uwp/api/Windows.UI.Xaml.Media.Brush) estarão disponíveis para instanciação de códigos.

Para criar um [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) em código, use o construtor que leva um parâmetro [**Color**](/uwp/api/Windows.UI.Color). Informe um valor que seja uma propriedade estática da classe [**Colors**](/uwp/api/windows.ui.colors), como este:

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

Para [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) e [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush), use o construtor padrão e, depois, chame outras APIs antes de tentar usar esse pincel para uma propriedade da IU.

-   [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) requer uma [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (não um URI) ao definir [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) usando código. Se a sua origem for um fluxo, use o método [**SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) para iniciar o valor. Se sua origem for um URI que inclui conteúdo no seu aplicativo que usa o esquema **ms-appx** ou **ms-resource**, use o construtor [**BitmapImage**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) que usa um URI. Você também poderá considerar a manipulação do evento [**ImageOpened**](/uwp/api/windows.ui.xaml.media.imagebrush.imageopened) se houver algum problema de temporização com a recuperação ou decodificação da origem da imagem, em que você pode precisar de conteúdo alternativo para exibir até que a origem da imagem esteja disponível.
-   Para [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), talvez seja necessário chamar [**Redraw**](/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw) se você tiver redefinido recentemente a propriedade [**SourceName**](/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename) ou se o conteúdo de [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) também estiver sendo alterado com código.

Para obter exemplos de código, confira [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) e [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 




