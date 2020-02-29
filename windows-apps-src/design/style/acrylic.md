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
ms.openlocfilehash: c6995ab6116d4e3bda8e21c397ab3b4985732763
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77639763"
---
# <a name="acrylic-material"></a>Material acrílico

![imagem Hero](images/header-acrylic.svg)

O acrílico é um tipo de [Pincel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) que cria uma textura translúcida. Você pode aplicar o acrílico a superfícies de aplicativos para adicionar profundidade e ajudar a estabelecer uma hierarquia visual.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **APIs importantes**: [classe AcrylicBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.acrylicbrush), [propriedade Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
Acrílico em tema claro ![Acrílico em tema claro](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
Acrílico em tema escuro ![Acrílico em tema escuro](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acrílico e o Sistema de Design Fluente

 O Sistema de Design Fluente ajuda a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. O acrílico é um componente do Sistema de Design Fluente que acrescenta textura física (material) e profundidade ao aplicativo. Para saber mais, confira a [visão geral do Design Fluente para UWP](/windows/apps/fluent-design-system).

 ## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Exemplos

:::row:::
    :::column span:::
![Algumas imagens](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**XAML Controls Gallery**<br>
Se você tem o aplicativo XAML Controls Gallery instalado, clique <a href="xamlcontrolsgallery:/item/Acrylic">aqui</a> para abri-lo e veja o acrílico em funcionamento.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Tipos de mistura de acrílico
A característica mais notável do acrílico é a sua transparência. Existem dois tipos de mistura de acrílico que alteram o que é visível através do material:
 - O **Acrílico de fundo** revela o papel de parede da área de trabalho e outras janelas que estão atrás do aplicativo ativo naquele momento, adicionando profundidade entre as janelas dos aplicativos, ao mesmo tempo em que promove as preferências de personalização do usuário.
 - O **Acrílico do aplicativo** acrescenta uma sensação de profundidade dentro do quadro do aplicativo, oferecendo foco e hierarquia.

 ![Acrílico de fundo](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrílico do aplicativo](images/AppAcrylic_DarkTheme.png)

 Tenha cautela ao usar várias camadas de superfícies com acrílico: muitas camadas de acrílico de fundo podem criar ilusões de ótica incômodas.

## <a name="when-to-use-acrylic"></a>Quando usar o acrílico

* Use o acrílico do aplicativo para auxiliar a interface do usuário, como em superfícies que podem se sobrepor ou interagir quando a tela é rolada.
* Use o acrílico de fundo para elementos transitórios da interface do usuário, como os menus de contexto, submenus e interface do usuário light dismiss.<br />Usar o acrílico em cenários transitórios ajuda a manter uma relação visual com o conteúdo que acionou a interface do usuário transitória.

Se você estiver usando o acrílico do aplicativo em superfícies de navegação, considere estender o conteúdo abaixo do painel do acrílico para melhorar o fluxo no aplicativo. Usar o NavigationView fará isso automaticamente. No entanto, para evitar um efeito com listras, não tente colocar várias partes acrílicas de uma borda à outra, pois isso pode criar um fio indesejado entre as duas superfícies desfocadas. O acrílico é uma ferramenta para proporcionar harmonia visual aos seus designs; no entanto, quando usado incorretamente, pode causar um ruído visual.

Considere os seguintes padrões de uso para decidir a melhor maneira de incorporar o acrílico em seu aplicativo:

### <a name="horizontal-navigation-or-commanding"></a>Navegação horizontal ou pelos comandos

Se não for possível usar o NavigationView em seu aplicativo e você pretender adicionar o acrílico por conta própria, recomendamos utilizar um acrílico relativamente translúcido com opacidade de tonalidade de 60%.
 - Quando o painel abrir como uma sobreposição sobre outros conteúdos do aplicativo, deverá ser [60% de acrílico do aplicativo](#acrylic-theme-resources)

![Aplicativo de mapas usando os comandos horizontais do aplicativo](images/Maps_In_App_Acrylic_1.png)

Além disso, ao estender o conteúdo ou fazê-lo rolar por baixo do acrílico, na parte superior, oferecerá uma experiência mais imersiva e contínua com seu aplicativo.

### <a name="vertical-panes"></a>Painéis verticais

Para os painéis verticais ou superfícies que ajudam a separar o conteúdo por seção em seu aplicativo, é recomendável que você use uma tela de fundo opaca em vez do acrílico. Se seus painéis verticais abrem na parte superior do conteúdo, como os modos **Compacto** ou **Mínimo** do NavigationView, sugerimos usar o acrílico do aplicativo para ajudar a manter o contexto da página quando o usuário estiver com esse painel aberto.

### <a name="transient-surfaces"></a>Superfícies transitórias

Para aplicativos com submenus de menu, pop-ups não modais ou painéis light dismiss, é recomendável usar o acrílico de tela de fundo.

![Padrão de aplicativo de e-mail usando um submenu informativo](images/Mail_TransientContextMenu.png)

Muitos dos controles usarão o acrílico por padrão. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) e controles semelhantes com pop-ups light dismiss usarão o acrílico transitório quando ao serem invocados.

> [!Note]
> A renderização de superfícies acrílicas exige muito da GPU, o que pode aumentar o consumo de energia do dispositivo e reduzir a vida útil da bateria. Os efeitos acrílicos são automaticamente desativados quando os dispositivos entram no modo de economia de bateria, e os usuários podem desativar os efeitos acrílicos em todos os aplicativos, caso desejem.

## <a name="usability-and-adaptability"></a>Usabilidade e adaptabilidade
O acrílico adapta automaticamente sua aparência para uma variedade de dispositivos e contextos.

No modo Alto Contraste, os usuários continuam visualizando a cor de fundo familiar de sua escolha no lugar do acrílico. Além disso, o acrílico de fundo e o acrílico do aplicativo aparecem como uma cor sólida:
 - Quando o usuário desativa a transparência em Configurações > Personalização > Cor
 - Quando o modo de Economia de Bateria está ativado
 - Quando o aplicativo é executado em hardware de baixa gama

Além disso, apenas o acrílico de fundo irá substituir a translucência e textura por uma cor sólida:
 - Quando uma janela do aplicativo é desativada na área de trabalho
 - Quando o aplicativo UWP está em execução no telefone, no Xbox, no HoloLens ou no modo tablet

### <a name="legibility-considerations"></a>Considerações de legibilidade
É importante garantir que qualquer texto que o aplicativo apresente ao usuário [atenda aos índices de contraste](../accessibility/accessible-text-requirements.md). Nós otimizamos a receita do acrílico para que pretos e brancos de alta intensidade, ou até mesmo textos de cor cinza-médio atendam as taxas de contraste acima do acrílico. Os recursos de tema fornecidos pela plataforma contrastam, por padrão, tons de cores com opacidade de 80%. Ao posicionar o texto com corpo high-color sobre o acrílico, você poderá reduzir a opacidade do tom mantendo legibilidade. No modo escuro, a opacidade da tonalidade pode ser de 70%, enquanto o acrílico no modo claro irá atender às taxas de contraste de 50% de opacidade.

Não é recomendável colocar texto com cores acentuadas sobre as superfícies de acrílico, pois essas combinações provavelmente não atenderão aos requisitos mínimos de contraste em tamanho de fonte de 15px. Evite colocar [hiperlinks](../controls-and-patterns/hyperlinks.md) sobre elementos acrílicos. Além disso, se você optar por personalizar a cor de tonalidade ou o nível de opacidade do acrílico fora dos padrões da plataforma fornecidos pelo recurso de tema, tenha em mete o impacto sobre a legibilidade.

## <a name="acrylic-theme-resources"></a>Recursos do tema acrílico
Você pode facilmente aplicar o acrílico às superfícies do aplicativo utilizando o novo XAML AcrylicBrush ou os recursos de tema AcrylicBrush predefinidos. Primeiro, você precisará decidir se deseja usar o acrílico do aplicativo ou o acrílico de fundo. Examine os padrões comuns de aplicativos descritos anteriormente neste artigo para obter recomendações.

Criamos uma coleção de recursos de tema de pincel para tipos de acrílico de fundo e para tipos de acrílico do aplicativo que respeitam o tema do aplicativo e retornam para cores sólidas quando necessário. Recursos chamados *AcrylicWindow* representam o acrílico de fundo, enquanto o *AcrylicElement* refere-se ao acrílico do aplicativo.

<table>
    <tr>
        <th align="center">Chave de recurso</th>
        <th align="center">Opacidade da tonalidade</th>
        <th align="center"><a href="color.md">Cor de fallback</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Uso recomendado:</b> Esses são recursos de acrílico com finalidades gerais que funcionam bem em uma ampla variedade de aplicações. Se o seu aplicativo utiliza texto secundário de cor AltMedium com tamanho inferior a 18px, coloque um recurso acrílico de 80% atrás do texto para <a href="../accessibility/accessible-text-requirements.md">atender aos requisitos de índice de contraste</a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Se o seu aplicativo utiliza texto secundário de cor AltMedium com um texto de tamanho 18px ou superior, você pode colocar esses recursos acrílicos com transluscência de 70% atrás do texto. Recomendamos usar esses recursos nas áreas de navegação e comando horizontais superiores do seu aplicativo.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Ao colocar apenas texto primário de cor AltHigh sobre o acrílico, seu aplicativo pode utilizar esses recursos de 60%. Recomendamos pintar o <a href="../controls-and-patterns/navigationview.md">painel de navegação vertical</a> do seu aplicativo, por exemplo, menu de hambúrguer, com acrílico a 60%. </td>
    </tr>
</table>

Além do acrílico de cor neutra, também adicionamos recursos que colorem o acrílico utilizando a cor de destaque especificada pelo usuário. Recomendamos usar acrílico colorido com moderação. Para as variantes dark1 e dark2 fornecidas, coloque texto branco ou de cores claras, compatível com a cor de texto do tema escuro, por cima desses recursos.
<table>
    <tr>
        <th align="center">Chave de recurso</th>
        <th align="center">Opacidade da tonalidade</th>
        <th align="center"><a href="color.md">Tonalidade e cores de fallback</a> </th>
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


Para pintar uma superfície específica, aplique um dos recursos de tema acima aos fundos do elemento, da mesma forma que faria com qualquer outro recurso de pincel.

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>Pincel acrílico personalizado
Você pode optar por adicionar uma tonalidade de cor ao acrílico do aplicativo para mostrar a identidade visual ou garantir equilíbrio visual em relação aos outros elementos da página. Para exibir cores em vez da escala de cinza, você deverá definir seus próprios pincéis acrílicos utilizando as seguintes propriedades.
 - **TintColor**: camada sobreposta de cor/tonalidade. Considere especificar o valor RGB da cor e a opacidade do canal alfa.
 - **TintOpacity**: a opacidade da camada de tonalidade. Recomendamos 80% de opacidade como ponto de partida, embora cores diferentes possam parecer mais atraentes em outras transparências.
 - **TintLuminosityOpacity**: controla a quantidade de saturação que é permitida através da superfície do acrílico da tela de fundo.
 - **BackgroundSource**: o marcador para especificar se você deseja o acrílico de fundo ou o acrílico do aplicativo.
 - **FallbackColor**: a cor sólida que substitui o acrílico no modo de Economia de bateria. Para o acrílico de fundo, a cor de fallback também substituirá o acrílico quando o aplicativo não estiver na janela da área de trabalho ativa ou quando o aplicativo estiver sendo executado no telefone ou no Xbox.

![Amostras de acrílico de tema claro](images/CustomAcrylic_Swatches_LightTheme.png)

![Amostras de acrílico de tema escuro](images/CustomAcrylic_Swatches_DarkTheme.png)

![Opacidade de luminosidade comparada à opacidade de tonalidade](images/LuminosityVersusTint.png)

Para adicionar um pincel acrílico, defina os três recursos para os temas escuro, claro e de alto contraste. Observe que em alto contraste, recomendamos usar um SolidColorBrush com a mesma x:Key do AcrylicBrush escuro/claro.

> [!Note] 
> Se você não especificar um valor para TintLuminosityOpacity, o sistema ajustará automaticamente o seu valor com base no TintColor e TintOpacity.

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

O exemplo a seguir mostra como declarar o AcrylicBrush no código. Se seu aplicativo dá suporte a diversos sistemas operacionais, certifique-se de verificar se a API está disponível no computador do usuário.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.AcrylicBrush"))
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

Para dar uma aparência perfeita à janela do aplicativo, é possível usar o acrílico na área da barra de título. Este exemplo estende o acrílico até a barra de título, definindo as propriedades do objeto [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar)[ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) e [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) como [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent).

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Este código pode ser colocado no método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) (_App.xaml.cs_) do aplicativo, após a chamada para [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), como mostrado aqui, ou na primeira página do aplicativo.

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

Além disso, você precisará desenhar o título do seu aplicativo, que em geral aparece automaticamente na barra de título, com um TextBlock usando `CaptionTextBlockStyle`. Para saber mais, confira a [personalização da barra de título](../shell/title-bar.md).

## <a name="dos-and-donts"></a>O que fazer e o que não fazer
* Use acrílico como material de fundo de superfícies não primárias do aplicativo, como os painéis de navegação.
* Estenda o acrílico para pelo menos uma borda do seu aplicativo para criar uma experiência perfeita, combinando de maneira sutil com os entornos do aplicativo.
* Não coloque acrílico da área de trabalho em superfícies grandes de tela de fundo de seu aplicativo, pois isso interromperá o modelo mental do acrílico que está sendo usado principalmente para as superfícies transitórias.
* Não coloque os acrílicos do aplicativo e de fundo diretamente adjacentes para evitar tensão visual nas junções.
* Não coloque vários painéis acrílicos com a mesma tonalidade e opacidade próximos uns aos outros, pois isso gera uma emenda visível não desejada.
* Não coloque texto de cores destacadas sobre superfícies acrílicas.

## <a name="how-we-designed-acrylic"></a>Como projetamos o acrílico

Ajustamos os componentes principais do acrílico para alcançar uma aparência e propriedades únicas. Começamos com a translucência, desfoque e ruído para adicionar profundidade visual e dimensão a superfícies planas. Adicionamos uma camada de modo de mistura de exclusão para garantir contraste e legibilidade da interface de usuário posicionada em um fundo acrílico. Por fim, adicionamos uma tonalidade de cor para promover oportunidades de personalização. Em conjunto, essas camadas constituem um material novo e utilizável.

![Receita do acrílico](images/AcrylicRecipe_Diagram.jpg)
<br/>Receita do acrílico: fundo, desfoque, mistura de exclusão, sobreposição de cor/tonalidade, ruído


## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

[**Realce de revelação**](reveal.md)
