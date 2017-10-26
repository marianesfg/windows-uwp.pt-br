---
author: mijacobs
description: 
title: "Material acrílico"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.openlocfilehash: 01c8d1bd961a5246a052d1dc7a746257687104e4
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2017
---
# <a name="acrylic-material"></a>Material acrílico
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> Este artigo descreve funcionalidades que ainda não foram lançadas e que podem ser modificadas substancialmente antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

O acrílico é um tipo de [Pincel](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) que cria uma textura parcialmente transparente. Você pode aplicar o acrílico em superfícies de aplicativos para adicionar profundidade e ajudar a estabelecer uma hierarquia visual.  <!-- By allowing user-selected wallpaper or colors to shine through, Acrylic keeps users in touch with the OS personalization they've chosen. -->

> **APIs importantes**: [classe AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [Propriedade de fundo](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Background)


![Acrílico em tema claro](images/Acrylic_DarkTheme_Base.png)

![Acrílico em tema escuro](images/Acrylic_LightTheme_Base.png)

## <a name="acrylic-and-the-fluent-design-system"></a>Acrílico e o Sistema de Design Fluent

 O Sistema de Design Fluent ajuda você a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. O Acrílico é um componente do Sistema de Design Fluent que acrescenta textura física (material) e profundidade ao seu aplicativo. 

## <a name="when-to-use-acrylic"></a>Quando usar o acrílico

Recomendamos que você coloque interface de usuário de suporte, como a navegação dentro do aplicativo ou elementos de comando, em uma superfície acrílica. Esse material também é útil para elementos de interface de usuário transientes, como diálogos e submenus, pois ele ajuda a manter uma relação visual com o conteúdo que acionou a UI transiente. Desenvolvemos o acrílico para ser usado como um material de fundo e mostra em painéis visualmente discretos, portanto não aplique o acrílico em elementos detalhados em primeiro plano.

As superfícies atrás do conteúdo primário do aplicativo devem usar fundos sólidos e opacos.

Considere a possibilidade de estender o acrílico à uma ou mais bordas do seu aplicativo, incluindo a barra de título da janela, para melhorar o fluxo visual. Evite criar um efeito de tiragem empilhando acrílicos de diferentes tipos de mistura adjacentes um ao outro. O acrílico é uma ferramenta para trazer harmonia visual aos seus designs, no entanto, quando usado incorretamente, pode resultar em ruído visual.

Considere os seguintes padrões de uso para decidir a melhor maneira de incorporar o acrílico em seu aplicativo.

### <a name="vertical-acrylic-pane"></a>Painel acrílico vertical

Para aplicativos com navegação vertical, recomendamos aplicar o acrílico ao painel secundário contendo elementos de navegação.

![Padrão de aplicativo usando um único painel acrílico vertical](images/acrylic_app-pattern_vertical.png)

[NavigationView](../controls-and-patterns/navigationview.md) é um novo controle comum para adicionar navegação ao seu aplicativo e inclui acrílico em seu design visual. O painel do NavigationView exibe acrílico ao fundo quando o painel é aberto lado a lado com um conteúdo primário, e muda automaticamente para o acrílico do aplicativo quando o painel é aberto como uma sobreposição.

Se o seu aplicativo não é consegue aproveitar o NavigationView e você pretende adicionar um acrílico por conta própria, recomendamos utilizar um acrílico relativamente transparente com opacidade de tonalidade de 60%.
 - Quando o painel é aberto como uma sobreposição sobre outros conteúdos do aplicativo, isso deve ser [60% de acrílico de aplicativo](#acrylic-theme-resources)
 - Quando o painel é aberto lado a lado com conteúdo principal do aplicativo, isso deve ser [60% de acrílico de fundo](#acrylic-theme-resources)

### <a name="multiple-acrylic-panes"></a>Múltiplos painéis acrílicos

Para aplicativos com três painéis verticais distintos, recomendamos adicionar acrílico em conteúdo não principal.
 - Para o painel secundário mais próximo ao conteúdo primário, utilize [80% de acrílico de fundo](#acrylic-theme-resources)
 - Para o painel terciário mais distante do conteúdo primário, utilize [60% de acrílico de fundo](#acrylic-theme-resources)

![Padrão do aplicativo utilizando painel acrílico vertical duplo](images/acrylic_app-pattern_double-vertical.png)

### <a name="horizontal-acrylic-pane"></a>Painel acrílico horizontal

Para aplicativos com navegação horizontal, comando ou outros fortes elementos horizontais na parte superior do aplicativo, recomendamos aplicar [70% de acrílico](#acrylic-theme-resources) nesse elemento visual.

![Padrão do aplicativo utilizando um painel acrílico horizontal](images/acrylic_app-pattern_horizontal.png)

Aplicativos de tela com ênfase em conteúdo contínuo passível de zoom devem utilizar acrílico de aplicativo na barra superior para permitir que os usuários se conectem com esse conteúdo. Exemplos de aplicativos de tela incluem mapas, pintura e desenho.

Para aplicativos sem uma única tela contínua, recomendamos usar acrílico de fundo para conectar usuários aos seus ambientes gerais de trabalho.

### <a name="acrylic-in-utility-apps"></a>Acrílico em aplicativos de utilidade

Widgets ou aplicativos leves podem reforçar seu uso como aplicativos de utilidade aplicando o acrílico a toda a janela do aplicativo. Aplicativos que pertencem a essa categoria costumam apresentar momentos breves de envolvimento do usuário e provavelmente ocuparão toda a tela da área de trabalho do usuário. Os exemplos incluem calculadora e central de ações.

![O aplicativo de utilidade Calculadora com acrílico com seu contexto completo.](images/acrylic_app-pattern_full.png)

> [!Note]
> A renderização de superfícies acrílicas exige muito da GPU, o que pode aumentar o consumo energético do dispositivo e reduzir a duração da bateria. Os efeitos acrílicos são automaticamente desativados quando dispositivos entram no modo de economia de bateria, e os usuários podem desativar os efeitos acrílicos para todos os aplicativos, caso desejem.


## <a name="acrylic-blend-types"></a>Tipos de mistura de acrílico
A característica mais notável do acrílico é sua transparência. Existem dois tipos de mistura de acrílico que alteram o que é visível através do material:
 - O **Acrílico de fundo** revela o papel de parede da área de trabalho e outras janelas que estão atrás do aplicativo ativo no momento, adicionando profundidade entre janelas de aplicações, ao mesmo tempo que celebra as preferências de personalização do usuário.
 - O **acrílico de aplicativo** adiciona uma sensação de profundidade dentro do quadro do aplicativo, oferecendo foco e hierarquia.

 ![Acrílico de fundo](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrílico de aplicativo](images/AppAcrylic_DarkTheme.png)

 Coloque diversas superfícies de acrílico com cuidado. O acrílico de fundo, como seu nome sugere, não deve ser o mais próximo do usuário na ordem z. Múltiplas camadas de acrílico de fundo costumam resultar em ilusões ópticas não desejadas e devem também ser evitadas. Se optar por sobrepor acrílicos, utilize o acrílico de aplicativo e considere usar um tom de acrílico mais claro para ajudar a apresentar as camadas ao visualizador.


## <a name="usability-and-adaptability"></a>Usabilidade e adaptabilidade
O acrílico adapta automaticamente sua aparência para uma variedade de dispositivos e contextos.

No modo Alto Contraste, os usuários continuam visualizando a cor de fundo familiar de sua escolha no lugar do acrílico. Além disso, ambos os acrílicos de fundo e de aplicativo aparecem como uma cor sólida.
 - Quando o usuário desativa a transparência nas configurações de Personalização
 - Quando o modo de economia de bateria está ativado
 - Quando o aplicativo é executado em hardware de capacidade reduzida

Além disso, apenas o acrílico de fundo irá substituir sua transparência e textura por uma cor sólida
 - Quando uma janela de aplicativo na área de trabalho é desativada
 - Quando o aplicativo UWP está em execução no telefone, Xbox, HoloLens ou modo tablet

### <a name="legibility-considerations"></a>Considerações de legibilidade
É importante garantir que qualquer texto que seu aplicativo apresente aos usuários [atenda as taxas de contraste](../accessibility/accessible-text-requirements.md). Nós otimizamos a receita do acrílico para que pretos e brancos de alta intensidade, ou até mesmo textos de cor média cinza atendam as taxas de contraste mesmo com acrílico. Os recursos de tema fornecidos pela plataforma contrastam, por padrão, tons de cores com opacidade de 80%. Ao posicionar texto de corpo com cor de alta intensidade no acrílico, você pode reduzir a opacidade do tom mantendo legibilidade. No modo escuro, a opacidade de tom pode ser 70%, enquanto acrílico no modo claro irá atender taxas de contraste com 50% de opacidade.

Não é recomendável colocar texto de cores acentuadas em suas superfícies acrílicas pois essas combinações provavelmente não atenderão os requisitos de contraste mínimo para tamanho de fonte 15px. Tente evitar colocar [hiperlinks](../controls-and-patterns/hyperlinks.md) sobre elementos acrílicos. Além disso, se você optar por personalizar o tom da cor ou nível de opacidade do acrílico fora dos padrões da plataforma fornecidos pelo recurso de tema, mantenha o impacto sobre a legibilidade em mente.

## <a name="acrylic-theme-resources"></a>Recursos de tema acrílico
Você pode facilmente aplicar acrílico às suas superfícies do aplicativo utilizando o novo XAML AcrylicBrush ou recursos de tema AcrylicBrush predefinidos. Primeiro, você precisará decidir se deseja usar acrílico de aplicativo ou de fundo. Certifique-se de examinar padrões comuns de aplicativos descritos anteriormente neste artigo para obter recomendações.

Nós criamos uma coleção de recursos de tema de pincel para tipos de acrílico de fundo e de aplicativo que respeitam o tema do aplicativo e retornam para cores sólidas quando necessário. Recursos chamados *Acrylic\*WindowBrush* representam acrílico de fundo, enquanto *Acrylic\*ElementBrush* se refere ao acrílico de aplicativo.

<table>
    <tr>
        <th align="center">Chave do recurso</th>
        <th align="center">Opacidade do tom</th>
        <th align="center">[Cor de retorno](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush<br/>SystemControlAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium </td>
    </tr>
    </tr>
        <td> **Uso recomendado:** Esses são recursos acrílicos com finalidades gerais que funcionam bem em uma ampla variedade de aplicações. Se o seu aplicativo utiliza texto secundário de cor AltMedium com tamanho menor que 18px, coloque um recurso acrílico de 80% atrás do texto para [atender os requisitos de taxa de contraste](../accessibility/accessible-text-requirements.md). </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicMediumHighWindowBrush<br/>SystemControlAcrylicMediumHighElementBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium </td>
    </tr>
    <tr>
        <td> **Uso recomendado:** Se o seu aplicativo utiliza texto secundário de cor AltMedium com um tamanho de 18px ou maior, você pode colocar esses recursos acrílicos de 70% atrás do texto. Recomendamos usar esses recursos nas áreas de navegação e comando horizontais superiores do seu aplicativo.  </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicMediumWindowBrush<br/>SystemControlAcrylicMediumElementBrush </td>
        <td align="center"> 60% </td>
        <td> ChromeMediumLow </td>
    </tr>
    <tr>
        <td> **Uso recomendado:** Ao colocar apenas texto primário de cor AltHigh sobre o acrílico, seu aplicativo pode utilizar esses recursos de 60%. Recomendamos pintar o [painel de navegação vertical](../controls-and-patterns/navigationview.md) do seu aplicativo, i.e. menu de hambúrgueres, com acrílico de 60%. </td>
    </tr>
</table>

Além do acrílico de cor neutra, também adicionamos recursos que colorem o acrílico utilizando o tom de cor especificado pelo usuário. Recomendamos usar acrílico colorido com moderação. Para as variantes dark1 e dark2 fornecidas, coloque texto branco ou de cores claras consistente com cor de texto de tema escuro sobre esses recursos.
<table>
    <tr>
        <th align="center">Chave do recurso</th>
        <th align="center">Opacidade do tom</th>
        <th align="center">[Tom e cores de Retorno](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAcrylicAccentMediumHighWindowBrush<br/>SystemControlAcrylicAccentMediumHighElementBrush </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicAccentDark1WindowBrush<br/>SystemControlAcrylicAccentDark1ElementBrush </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicAccentDark2MediumHighWindowBrush<br/>SystemControlAcrylicAccentDark2MediumHighElementBrush </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


Para pintar uma superfície específica, aplique um dos recursos de tema acima ao fundo dos elementos, da mesma forma que você aplicaria qualquer outro recurso de pincel.

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>Pincel acrílico personalizado
Você pode optar por adicionar uma tonalidade de cor ao acrílico do seu aplicativo para mostrar a identidade visual ou garantir equilíbrio visual em relação aos outros elementos da página. Para exibir cores ao invés da escala de cinza, você deverá definir seus próprios pincéis acrílicos utilizando as seguintes propriedades.
 - **TintColor**: a cor/tonalidade da camada de sobreposição. Considere especificar o valor RGB da cor e a opacidade do canal alfa.
 - **TintOpacity**: a opacidade da camada de tonalidade. É recomendável 80% de opacidade como ponto de partida, embora cores diferentes possam parecer mais atraentes em outras transparências.
 - **BackgroundSource**: o marcador para especificar se você deseja acrílico de fundo ou de aplicativo.
 - **FallbackColor**: a cor sólida que substitui o acrílico no modo bateria fraca. Para acrílico de fundo, a cor de retorno também substitui o acrílico quando seu aplicativo não está na janela da área de trabalho ativa ou quando o aplicativo está sendo executado no telefone ou no Xbox.


![Amostras de acrílico de tema claro](images/CustomAcrylic_Swatches_LightTheme.png)

![Amostras de acrílico de tema escuro](images/CustomAcrylic_Swatches_DarkTheme.png)

Para adicionar um pincel acrílico, defina os três recursos para temas escuro, claro e de alto contraste. Observe que em alto contraste, recomendamos usar um SolidColorBrush com a mesma x:Key do AcrylicBrush escuro/claro.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

O exemplo a seguir mostra como declarar AcrylicBrush no código. Se seu aplicativo suportar diversos sistemas operacionais, certifique-se de verificar que o API está disponível no computador do usuário.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.XamlCompositionBrushBase"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extending-acrylic-into-your-title-bar"></a>Estendendo o acrílico até sua barra de título

Para obter um visual fluido e integrado na janela do seu aplicativo, recomendamos colocar acrílico em sua área da barra de título. Para fazer isso, adicione o seguinte código ao seu App.xaml.cs.

```csharp
CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
titleBar.ButtonBackgroundColor = Colors.Transparent;
titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
```

Além disso, você precisará desenhar o título do seu aplicativo, que normalmente aparece automaticamente na barra de título, com um TextBlock usando `CaptionTextBlockStyle`.

## <a name="dos-and-donts"></a>O que fazer e o que não fazer
* Use acrílico como material de fundo em superfícies não primárias do aplicativo, como painéis de navegação.
* Estenda o acrílico para pelo menos uma borda do seu aplicativo para criar uma experiência perfeita, combinando de maneira sutil com os entornos do aplicativo.
* Não coloque acrílicos de aplicativo e de fundo de forma diretamente adjacente para evitar a tensão visual nas junções.
* Não coloque vários painéis acrílicos com o mesmo tom e opacidade próximos uns aos outros, pois isso gera uma emenda visível não desejada.
* Não coloque texto de cores destacadas sobre superfícies acrílicas.

## <a name="how-we-designed-acrylic"></a>Como projetamos o acrílico

Ajustamos os componentes principais do acrílico para alcançar sua aparência e propriedades únicas. Começamos com transparência, desfoque e ruído para adicionar profundidade visual e dimensão a superfícies planas. Adicionamos uma camada de modo de mistura de exclusão para garantir contraste e legibilidade da interface de usuário posicionada em um fundo acrílico. Finalmente, adicionamos tonalidade de cor para oportunidades de personalização. Em conjunto, essas camadas constituem um material novo e utilizável.

![Receita acrílica](images/AcrylicRecipe_Diagram.png)
<br/>A receita acrílica: fundo, desfoque, mistura de exclusão, sobreposição de cor/tonalidade, ruído.

<!--
<div class="microsoft-internal-note">
When designing your app, please utilize these [design resources](http://uni/DesignDepot.FrontEnd/#/Search?t=Resources%7CNeon%7CToolkit&f=Acrylic%20Material) to show acrylic in comps. The linked templates are the most accurate way to represent acrylic material in Photoshop and Illustrator. The ordering, as noted in the recipe diagram above, should start from the top: <br/>
 - Noise asset (tiled) at 4% opacity <br/>
 - Base color/tint/alpha layer <br/>
 - Exclusion blend (white @ 10% opacity) <br/>
 - Gaussian blur (30px radius) <br/>
</div>
-->


## <a name="related-articles"></a>Artigos relacionados
[**Revelar**](reveal.md)