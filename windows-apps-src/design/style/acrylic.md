---
description: Um tipo de pincel que cria uma textura translúcida.
title: Material acrílico
template: detail.hbs
ms.date: 08/9/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b47991add56868fe534b0460ec65336e94f433e9
ms.sourcegitcommit: 1391190e27907f0c7cd40e4bb56cdead387a53cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2019
ms.locfileid: "9035526"
---
# <a name="acrylic-material"></a>Material acrílico

![imagem hero](images/header-acrylic.svg)

Acrílico é um tipo de [Pincel](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) que cria uma textura translúcida. Você pode aplicar o acrílico em superfícies de aplicativos para adicionar profundidade e ajudar a estabelecer uma hierarquia visual.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **APIs importantes**: [classe AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [Propriedade de fundo](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
        Acrylic in light theme
        ![Acrylic in light theme](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
        Acrylic in dark theme
        ![Acrylic in dark theme](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acrílico e o Sistema de Design Fluent

 O Sistema de Design Fluent ajuda você a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. O Acrílico é um componente do Sistema de Design Fluent que acrescenta textura física (material) e profundidade ao seu aplicativo. Para saber mais, consulte a [visão geral do Fluent Design para UWP](../fluent-design-system/index.md).

 ## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Exemplos

:::row:::
    :::column span:::
        ![Some image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
        **XAML Controls Gallery**<br>
        If you have the XAML Controls Gallery app installed, click <a href="xamlcontrolsgallery:/item/Acrylic">here</a> to open the app and see acrylic in action.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Tipos de mistura de acrílico
A característica mais notável do acrílico é sua transparência. Existem dois tipos de mistura de acrílico que alteram o que é visível através do material:
 - O **Acrílico de fundo** revela o papel de parede da área de trabalho e outras janelas que estão atrás do aplicativo ativo no momento, adicionando profundidade entre janelas de aplicações, ao mesmo tempo que celebra as preferências de personalização do usuário.
 - O **acrílico de aplicativo** adiciona uma sensação de profundidade dentro do quadro do aplicativo, oferecendo foco e hierarquia.

 ![Acrílico de fundo](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrílico de aplicativo](images/AppAcrylic_DarkTheme.png)

 Coloque diversas superfícies de acrílico com cuidado: várias camadas de acrílico de fundo podem criar ópticas distração.

## <a name="when-to-use-acrylic"></a>Quando usar o acrílico

* Use acrílico no aplicativo para dar suporte a interface do usuário, como em superfícies que podem se sobrepor conteúdo quando rolada ou interage.
* Use acrílico de fundo para elementos de interface do usuário transitórios, como luz dimsissable da interface do usuário, submenus e menus de contexto.<br />Usar o acrílico em cenários transitórios ajuda a manter uma relação visual com o conteúdo que acionou a UI Transiente.

Se você estiver usando no aplicativo acrílico em superfícies de navegação, considere a possibilidade de estender o conteúdo sob o painel acrílico para melhorar o fluxo em seu aplicativo. Usar o NavigationView fará isso para você automaticamente. No entanto, para evitar a criação de um efeito de tiragem, não tente colocar várias partes de acrílico-borda - isso pode criar uma fenda indesejada entre as duas superfícies desfocadas. Acrílico é uma ferramenta para trazer harmonia visual aos seus designs, mas quando usado incorretamente, pode resultar em ruído visual.

Considere os seguintes padrões de uso para decidir a melhor maneira de incorporar o acrílico em seu aplicativo:

### <a name="horizontal-navigation-or-commanding"></a>Navegação horizontal ou comandos

Se seu aplicativo não é capaz de aproveitar o NavigationView e você pretende adicionar um acrílico por conta própria, recomendamos usar acrílico relativamente transparente com opacidade de tonalidade de 60%.
 - Quando o painel é aberto como uma sobreposição sobre outros conteúdos do aplicativo, isso deve ser [60% de acrílico de aplicativo](#acrylic-theme-resources)

![Aplicativo de mapas usando o comando horizontal no aplicativo](images/Maps_In_App_Acrylic_1.png)

Além disso, ter seu conteúdo estender ou rolagem sob o acrílico na parte superior dará seu aplicativo uma experiência mais imersiva e perfeita.

### <a name="vertical-panes"></a>Painéis verticais

Para painéis verticais ou superfícies que ajudam a seção desativar o conteúdo do seu aplicativo, recomendamos que você use um plano de fundo opaco em vez de acrílico. Se abrir seus painéis verticais sobre conteúdo, como em do NavigationView **compacto** ou modos **mínimo** , sugerimos que usar o acrílico de aplicativo para ajudar a manter o contexto da página quando o usuário tem esse painel aberto.

### <a name="transient-surfaces"></a>Superfícies transitórios

Para aplicativos com menu submenus, pop-ups não modal, ou ignorar painéis, é recomendável usar acrílico de fundo.

![Usando um submenu informativo padrão do aplicativo de email](images/Mail_TransientContextMenu.png)

Muitos dos nossos controles usará o acrílico por padrão. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) e controles semelhantes com luz dimiss pop-ups todos usarão o acrílico transitório quando eles são invocados.

> [!Note]
> Renderização de superfícies é GPU intensivo, que pode aumentar o consumo de energia do dispositivo e reduzir a duração da bateria. Os efeitos acrílicos são automaticamente desativados quando dispositivos entram no modo de economia de bateria, e os usuários podem desativar os efeitos acrílicos para todos os aplicativos, se preferirem.

## <a name="usability-and-adaptability"></a>Usabilidade e adaptabilidade
O acrílico adapta automaticamente sua aparência para uma variedade de dispositivos e contextos.

No modo Alto Contraste, os usuários continuam visualizando a cor de fundo familiar de sua escolha no lugar do acrílico. Além disso, ambos os acrílicos de plano de fundo e no aplicativo aparecem como uma cor sólida:
 - Quando o usuário desativa a transparência em Configurações gt _ gt _ personalização cor
 - Quando o modo de economia de bateria está ativado
 - Quando o aplicativo é executado em hardware de capacidade reduzida

Além disso, apenas o acrílico de fundo irá substituir sua translucency e textura com uma cor sólida:
 - Quando uma janela de aplicativo na área de trabalho é desativada
 - Quando o aplicativo UWP está em execução no telefone, Xbox, HoloLens ou modo tablet

### <a name="legibility-considerations"></a>Considerações de legibilidade
É importante garantir que qualquer texto que seu aplicativo apresente aos usuários [atenda as taxas de contraste](../accessibility/accessible-text-requirements.md). Nós otimizamos a receita do acrílico para que pretos e brancos de alta intensidade, ou até mesmo textos de cor média cinza atendam as taxas de contraste mesmo com acrílico. Os recursos de tema fornecidos pela plataforma contrastam, por padrão, tons de cores com opacidade de 80%. Ao posicionar texto de corpo com cor de alta intensidade no acrílico, você pode reduzir a opacidade do tom mantendo legibilidade. No modo escuro, a opacidade de tom pode ser 70%, enquanto acrílico no modo claro irá atender taxas de contraste com 50% de opacidade.

Não é recomendável colocar texto de cores acentuadas em suas superfícies acrílicas pois essas combinações provavelmente não atenderão os requisitos de contraste mínimo para tamanho de fonte 15px. Tente evitar colocar [hiperlinks](../controls-and-patterns/hyperlinks.md) sobre elementos acrílicos. Além disso, se você optar por personalizar o tom da cor ou nível de opacidade do acrílico fora dos padrões da plataforma fornecidos pelo recurso de tema, mantenha o impacto sobre a legibilidade em mente.

## <a name="acrylic-theme-resources"></a>Recursos de tema acrílico
Você pode facilmente aplicar acrílico às suas superfícies do aplicativo utilizando o novo XAML AcrylicBrush ou recursos de tema AcrylicBrush predefinidos. Primeiro, você precisará decidir se deseja usar acrílico de aplicativo ou de fundo. Certifique-se de examinar padrões comuns de aplicativos descritos anteriormente neste artigo para obter recomendações.

Nós criamos uma coleção de recursos de tema de pincel para tipos de acrílico de fundo e de aplicativo que respeitam o tema do aplicativo e retornam para cores sólidas quando necessário. Recursos chamados *AcrylicWindow* representam acrílico de fundo, enquanto *AcrylicElement* se refere ao acrílico de aplicativo.

<table>
    <tr>
        <th align="center">Chave do recurso</th>
        <th align="center">Opacidade do tom</th>
        <th align="center"><a href="color.md">Cor de retorno</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Uso recomendado:</b> Esses são recursos acrílicos com finalidades gerais que funcionam bem em uma ampla variedade de aplicações. Se o seu aplicativo utiliza texto secundário de cor AltMedium com tamanho menor que 18px, coloque um recurso acrílico de 80% atrás do texto para <a href="../accessibility/accessible-text-requirements.md">atender os requisitos de taxa de contraste</a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Se seu aplicativo utiliza texto secundário de cor AltMedium com um tamanho de 18px ou maior, você pode colocar esses recursos acrílicos de 70% mais translúcidos atrás do texto. Recomendamos usar esses recursos nas áreas de navegação e comando horizontais superiores do seu aplicativo.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Ao colocar apenas texto primário de cor AltHigh sobre o acrílico, seu aplicativo pode utilizar esses recursos de 60%. Recomendamos pintar o <a href="../controls-and-patterns/navigationview.md">painel de navegação vertical</a> do seu aplicativo, i.e. menu de hambúrgueres, com acrílico de 60%. </td>
    </tr>
</table>

Além do acrílico de cor neutra, também adicionamos recursos que colorem o acrílico utilizando o tom de cor especificado pelo usuário. Recomendamos usar acrílico colorido com moderação. Para as variantes dark1 e dark2 fornecidas, coloque texto branco ou de cores claras consistente com cor de texto de tema escuro sobre esses recursos.
<table>
    <tr>
        <th align="center">Chave do recurso</th>
        <th align="center">Opacidade do tom</th>
        <th align="center"><a href="color.md">Tom e cores de Retorno</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
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
 - **TintOpacity**: a opacidade da camada de tonalidade. É recomendável 80% de opacidade como ponto de partida, embora cores diferentes possam parecer mais atraentes em outras translucencies.
 - **TintLuminosityOpacity**: controla a quantidade de saturação é permitida por meio de superfície acrílico do plano de fundo.
 - **BackgroundSource**: o marcador para especificar se você deseja acrílico de fundo ou de aplicativo.
 - **FallbackColor**: a cor sólida que substitui o acrílico em economia de bateria. Para acrílico de fundo, a cor de retorno também substitui o acrílico quando seu aplicativo não está na janela da área de trabalho ativa ou quando o aplicativo está sendo executado no telefone ou no Xbox.

![Amostras de acrílico de tema claro](images/CustomAcrylic_Swatches_LightTheme.png)

![Amostras de acrílico de tema escuro](images/CustomAcrylic_Swatches_DarkTheme.png)

![Luminosidade opactity em comparação comparada opacidade de tonalidade](images/LuminosityVersusTint.png)

Para adicionar um pincel acrílico, defina os três recursos para temas escuro, claro e de alto contraste. Observe que em alto contraste, recomendamos usar um SolidColorBrush com a mesma x:Key do AcrylicBrush escuro/claro.

> [!Note] 
> Se você não especificar um valor de TintLuminosityOpacity, o sistema ajustará automaticamente seu valor com base em seu TintColor e TintOpacity.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
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
            TintLuminosityOpacity="0.5"
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

## <a name="extend-acrylic-into-the-title-bar"></a>Estender o acrílico até a barra de título

Para dar uma aparência perfeita à janela do seu aplicativo, é possível usar o acrílico na área da barra de título. Este exemplo estende o acrílico até a barra de título definindo as propriedades [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) e [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) do objeto [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) como [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent).

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Este código pode ser colocado em seu método de aplicativo [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) (_App.xaml.cs_), após a chamada [Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), como mostrado aqui, ou na primeira página do seu aplicativo.

```csharp
// Call your extend acrylic code in the OnLaunched event, after
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

Além disso, você precisará desenhar o título do seu aplicativo, que normalmente aparece automaticamente na barra de título, com um TextBlock usando `CaptionTextBlockStyle`. Para saber mais, consulte [Personalização da barra de título](../shell/title-bar.md).

## <a name="dos-and-donts"></a>O que fazer e o que não fazer
* Use acrílico como material de fundo em superfícies não primárias do aplicativo, como painéis de navegação.
* Estenda o acrílico para pelo menos uma borda do seu aplicativo para criar uma experiência perfeita, combinando de maneira sutil com os entornos do aplicativo.
* Não coloque arylic da área de trabalho em superfícies grandes em segundo plano do seu aplicativo - Isso interrompe o modelo mental da acrílico sendo usado principalmente para superfícies transitórios.
* Não coloque acrílicos de aplicativo e de fundo de forma diretamente adjacente para evitar a tensão visual nas junções.
* Não coloque vários painéis acrílicos com o mesmo tom e opacidade próximos uns aos outros, pois isso gera uma emenda visível não desejada.
* Não coloque texto de cores destacadas sobre superfícies acrílicas.

## <a name="how-we-designed-acrylic"></a>Como projetamos o acrílico

Ajustamos os componentes principais do acrílico para alcançar sua aparência e propriedades únicas. Começamos com translucency, Desfoque e ruído para adicionar profundidade visual e dimensão a superfícies planas. Adicionamos uma camada de modo de mistura de exclusão para garantir contraste e legibilidade da interface de usuário posicionada em um fundo acrílico. Finalmente, adicionamos tonalidade de cor para oportunidades de personalização. Em conjunto, essas camadas constituem um material novo e utilizável.

![Receita acrílica](images/AcrylicRecipe_Diagram.jpg)
<br/>A receita acrílica: fundo, desfoque, mistura de exclusão, sobreposição de cor/tonalidade, ruído.


## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

[**Realce do Revelação**](reveal.md)
