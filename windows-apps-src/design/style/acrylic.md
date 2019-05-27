---
description: Um tipo de pincel que cria uma textura translúcida.
title: Material acrílico
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4731ab089189a8a03656281d1a9a6da6e4d24e89
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984256"
---
# <a name="acrylic-material"></a>Material acrílico

![imagem hero](images/header-acrylic.svg)

Tinta acrílica é um tipo de [pincel](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) que cria uma textura translúcida. Você pode aplicar o acrílico em superfícies de aplicativos para adicionar profundidade e ajudar a estabelecer uma hierarquia visual.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **APIs importantes**: [Classe AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [propriedade do plano de fundo](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

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

 O Sistema de Design Fluente ajuda você a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. O Acrílico é um componente do Sistema de Design Fluent que acrescenta textura física (material) e profundidade ao seu aplicativo. Para saber mais, consulte a [visão geral do Design Fluente para UWP](/windows/apps/fluent-design-system).

 ## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Exemplos

:::row:::
    :::column span:::
![Algumas imagens](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**Galeria de controles XAML**<br>
Se você tiver o aplicativo da Galeria de controles XAML instalado, clique em <a href="xamlcontrolsgallery:/item/Acrylic">aqui</a> para abrir o aplicativo e ver a tinta acrílica em ação.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Tipos de mistura de acrílico
A característica mais notável do acrílico é sua transparência. Existem dois tipos de mistura de acrílico que alteram o que é visível através do material:
 - O **Acrílico de fundo** revela o papel de parede da área de trabalho e outras janelas que estão atrás do aplicativo ativo no momento, adicionando profundidade entre janelas de aplicações, ao mesmo tempo que celebra as preferências de personalização do usuário.
 - O **acrílico de aplicativo** adiciona uma sensação de profundidade dentro do quadro do aplicativo, oferecendo foco e hierarquia.

 ![Acrílico de fundo](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrílico de aplicativo](images/AppAcrylic_DarkTheme.png)

 Camada várias superfícies acrílico com cuidado: várias camadas de tinta acrílica do plano de fundo podem criar incômodas ilusões de óptico.

## <a name="when-to-use-acrylic"></a>Quando usar o acrílico

* Use tinta acrílica de no aplicativo para dar suporte a interface do usuário, como em superfícies que podem se sobrepor conteúdo quando rolado ou interagir com.
* Use tinta acrílica do plano de fundo para elementos de interface do usuário transitórios, como ignorável a luz da interface do usuário, submenus e menus de contexto.<br />Usar tinta Acrílica em cenários transitórios ajuda a manter um relacionamento visual com o conteúdo que disparou a interface do usuário transitório.

Se você estiver usando tinta acrílica de no aplicativo em superfícies de navegação, considere a possibilidade de estender o conteúdo abaixo do painel de acrílico para melhorar o fluxo em seu aplicativo. Usar o NavigationView fará isso para você automaticamente. No entanto, para evitar a criação de um efeito de distribuição, não tente colocar várias partes de acrílico-borda - isso pode criar uma costura indesejada entre as duas superfícies desfocadas. Tinta acrílica é uma ferramenta para trazer harmonia visual para seus designs, mas quando usado incorretamente, pode resultar em poluição visual.

Considere os seguintes padrões de uso para decidir a melhor maneira de incorporar tinta acrílica em seu aplicativo:

### <a name="horizontal-navigation-or-commanding"></a>Navegação horizontal ou dos comandos

Se seu aplicativo não é capaz de aproveitar o NavigationView e você planeja adicionar tinta acrílica por conta própria, é recomendável usar o tinta acrílica relativamente translúcida com opacidade de tonalidade de 60%.
 - Quando o painel é aberto como uma sobreposição sobre outros conteúdos do aplicativo, isso deve ser [60% de acrílico de aplicativo](#acrylic-theme-resources)

![Aplicativo de mapas usando o comando horizontal no aplicativo](images/Maps_In_App_Acrylic_1.png)

Além disso, ter seu conteúdo estender ou rolagem sob a tinta acrílica na parte superior dará seu aplicativo uma experiência mais imersiva e contínua.

### <a name="vertical-panes"></a>Painéis verticais

Para os painéis verticais ou superfícies que ajudam a seção de conteúdo de seu aplicativo, é recomendável que você use um plano de fundo opaco em vez de tinta acrílica. Se seus painéis verticais abrir na parte superior do conteúdo, como do NavigationView **Compact** ou **mínimo** modos, sugerimos que você use tinta acrílica de no aplicativo para ajudar a manter o contexto da página quando o usuário tem nesse painel aberto.

### <a name="transient-surfaces"></a>Superfícies transitórias

Para aplicativos com submenus de menu, popups não modal, ou descarte suave e painéis, é recomendável usar tinta acrílica do plano de fundo.

![Padrão de aplicativo de email usando um submenu informativa](images/Mail_TransientContextMenu.png)

Muitos dos nossos controles usarão a tinta acrílica por padrão. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) e controles semelhantes com descarte suave e pop-ups todos usarão a tinta acrílica transitória quando eles são invocados.

> [!Note]
> A renderização de superfícies acrílico é o uso intenso da GPU, que pode aumentar o consumo de energia do dispositivo e encurtar o tempo de vida útil da bateria. Efeitos de acrílico automaticamente são desabilitados quando dispositivos entrar no modo de economia de bateria e os usuários podem desabilitar os efeitos de acrílico para todos os aplicativos, se desejarem.

## <a name="usability-and-adaptability"></a>Usabilidade e adaptabilidade
O acrílico adapta automaticamente sua aparência para uma variedade de dispositivos e contextos.

No modo Alto Contraste, os usuários continuam visualizando a cor de fundo familiar de sua escolha no lugar do acrílico. Além disso, a tinta acrílica do plano de fundo e tinta acrílica de no aplicativo aparecem como uma cor sólida:
 - Quando o usuário desativa a transparência em Configurações > personalização > cor
 - Quando o modo de economia de bateria é ativado
 - Quando o aplicativo é executado em hardware de capacidade reduzida

Além disso, apenas tinta acrílica do plano de fundo substituirá seu translucência e textura com uma cor sólida:
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
        <th align="center"><a href="color.md">Cor de fallback</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Uso recomendado:</b> Esses são recursos de finalidade geral acrílico que funcionam bem em uma ampla variedade de usos. Se o seu aplicativo utiliza texto secundário de cor AltMedium com tamanho menor que 18px, coloque um recurso acrílico de 80% atrás do texto para <a href="../accessibility/accessible-text-requirements.md">atender os requisitos de taxa de contraste</a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Se seu aplicativo usa texto secundário da cor AltMedium com um tamanho de texto de 18px ou maior, você pode colocar esses recursos de acrílico 70% mais translúcidos atrás do texto. Recomendamos usar esses recursos nas áreas de navegação e comando horizontais superiores do seu aplicativo.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Ao colocar somente texto primário da cor AltHigh ao longo da tinta acrílica, seu aplicativo pode utilizar esses recursos de 60%. Recomendamos pintar o <a href="../controls-and-patterns/navigationview.md">painel de navegação vertical</a> do seu aplicativo, i.e. menu de hambúrgueres, com acrílico de 60%. </td>
    </tr>
</table>

Além do acrílico de cor neutra, também adicionamos recursos que colorem o acrílico utilizando o tom de cor especificado pelo usuário. Recomendamos usar acrílico colorido com moderação. Para as variantes dark1 e dark2 fornecidas, coloque texto branco ou de cores claras consistente com cor de texto de tema escuro sobre esses recursos.
<table>
    <tr>
        <th align="center">Chave do recurso</th>
        <th align="center">Opacidade do tom</th>
        <th align="center"><a href="color.md">Cores de tom e Fallback</a> </th>
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
 - **TintOpacity**: a opacidade da camada de tonalidade. É recomendável 80% de opacidade como ponto de partida, embora cores diferentes podem parecer mais atraentes em outros translucencies.
 - **TintLuminosityOpacity**: controla a quantidade de saturação é permitida por meio da superfície acrílico do plano de fundo.
 - **BackgroundSource**: o marcador para especificar se você deseja acrílico de fundo ou de aplicativo.
 - **FallbackColor**: a cor sólida que substitui a tinta acrílica em economia de bateria. Para acrílico de fundo, a cor de retorno também substitui o acrílico quando seu aplicativo não está na janela da área de trabalho ativa ou quando o aplicativo está sendo executado no telefone ou no Xbox.

![Amostras de acrílico de tema claro](images/CustomAcrylic_Swatches_LightTheme.png)

![Amostras de acrílico de tema escuro](images/CustomAcrylic_Swatches_DarkTheme.png)

![Opactity luminosidade comparado a opacidade de tonalidade](images/LuminosityVersusTint.png)

Para adicionar um pincel acrílico, defina os três recursos para temas escuro, claro e de alto contraste. Observe que em alto contraste, recomendamos usar um SolidColorBrush com a mesma x:Key do AcrylicBrush escuro/claro.

> [!Note] 
> Se você não especificar um valor de TintLuminosityOpacity, o sistema se ajustará automaticamente o seu valor com base em seu TintColor e TintOpacity.

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
* Não coloque a tinta acrílica da área de trabalho nas superfícies de plano de fundo grande do seu aplicativo - Isso interrompe o modelo mental de tinta acrílica que está sendo usada principalmente para superfícies transitórias.
* Não coloque acrílicos de aplicativo e de fundo de forma diretamente adjacente para evitar a tensão visual nas junções.
* Não coloque vários painéis acrílicos com o mesmo tom e opacidade próximos uns aos outros, pois isso gera uma emenda visível não desejada.
* Não coloque texto de cores destacadas sobre superfícies acrílicas.

## <a name="how-we-designed-acrylic"></a>Como projetamos o acrílico

Ajustamos os componentes principais do acrílico para alcançar sua aparência e propriedades únicas. Começamos com translucência, Desfoque e ruído para adicionar profundidade visual e a dimensão à superfícies simples. Adicionamos uma camada de modo de mistura de exclusão para garantir contraste e legibilidade da interface de usuário posicionada em um fundo acrílico. Finalmente, adicionamos tonalidade de cor para oportunidades de personalização. Em conjunto, essas camadas constituem um material novo e utilizável.

![Receita acrílico](images/AcrylicRecipe_Diagram.jpg)
<br/>A receita acrílica: fundo, desfoque, mistura de exclusão, sobreposição de cor/tonalidade, ruído.


## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

[**Revelar realce**](reveal.md)
