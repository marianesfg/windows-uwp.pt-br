---
Description: Saiba como implementar a navegação para percorrer o histórico de navegação do usuário dentro de um aplicativo UWP com versões anteriores.
title: Histórico de navegação e navegação retroativa (aplicativos do Windows)
template: detail.hbs
op-migration-status: ready
ms.date: 06/21/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c74d4ebd08dfeddfb4a0149cffcd7bb845ceff11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595041"
---
# <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Histórico de navegação e navegação retroativa para aplicativos UWP

> **APIs importantes**: [Evento BackRequested](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [classe SystemNavigationManager](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

A Plataforma Universal do Windows (UWP) oferece um sistema de navegação regressiva consistente a fim de percorrer o histórico de navegação do usuário dentro de um aplicativo e, dependendo do dispositivo, de aplicativo para aplicativo.

Para implementar navegação retroativa no seu aplicativo, coloque um [botão Voltar](#Back-button) no canto superior esquerdo da interface do usuário do seu aplicativo. Se seu aplicativo usa o controle [NavigationView](../controls-and-patterns/navigationview.md), você pode usar o [botão Voltar integrado no NavigationView ](../controls-and-patterns/navigationview.md#backwards-navigation).

O usuário espera que o botão Voltar navegue para o local anterior no histórico de navegação do aplicativo. Note que cabe a você decidir quais ações de navegação serão adicionadas ao histórico de navegação e como responder ao pressionar o botão Voltar.

## <a name="back-button"></a>Botão Voltar

Para criar um botão Voltar, use o [botão](../controls-and-patterns/buttons.md) controlar com o `NavigationBackButtonNormalStyle` de estilo e colocar o botão no canto superior esquerdo da interface de usuário do seu aplicativo (para obter detalhes, consulte os exemplos de código XAML abaixo).

![Botão Voltar na parte superior esquerda da interface do usuário do aplicativo](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Se seu aplicativo tiver um [CommandBar](../controls-and-patterns/app-bars.md) superior, o controle de botão com 44px de altura não será alinhado com AppBarButtons de 48px. No entanto, para evitar inconsistência, alinhe o controle do botão dentro dos limites de 48px.

![Botão Voltar na barra de comandos superior](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Para minimizar a movimentação em seu aplicativo de elementos de interface do usuário, mostre um botão Voltar desabilitado quando não há nada no backstack (veja o exemplo de código abaixo). No entanto, se você espera que seu aplicativo nunca terá uma backstack, você não precisa exibir o botão Voltar em todos os.

![Estados do botão Voltar](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir demonstra como implementar o comportamento de navegação regressiva com um botão Voltar. O código responde ao evento do botão [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) e desabilita/habilita a visibilidade do botão no [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_), que é chamado ao navegar para uma nova página. O exemplo de código também manipula entradas de teclas Voltar de hardware e sistema de software registrando um ouvinte para o evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested).

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

Code-behind:

```csharp
// MainPage.xaml.cs
public MainPage()
{
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    BackButton.IsEnabled = this.Frame.CanGoBack;
}

private void Back_Click(object sender, RoutedEventArgs e)
{
    On_BackRequested();
}

// Handles system-level BackRequested events and page-level back button Click events
private bool On_BackRequested()
{
    if (this.Frame.CanGoBack)
    {
        this.Frame.GoBack();
        return true;
    }
    return false;
}

private void BackInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Navigation.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;

namespace winrt::PageNavTest::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        altLeft.Key(Windows::System::VirtualKey::Left);
        altLeft.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);
        KeyboardAccelerators().Append(altLeft);
        // ALT routes here.
        altLeft.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
    }

    void MainPage::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
    {
        BackButton().IsEnabled(Frame().CanGoBack());
    }

    void MainPage::Back_Click(IInspectable const&, RoutedEventArgs const&)
    {
        On_BackRequested();
    }

    // Handles system-level BackRequested events and page-level back button Click events.
    bool MainPage::On_BackRequested()
    {
        if (Frame().CanGoBack())
        {
            Frame().GoBack();
            return true;
        }
        return false;
    }

    void MainPage::BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& sender,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        args.Handled(On_BackRequested());
    }
}
```

Acima, lidamos com versões anteriores com a navegação para uma única página. Se você deseja excluir páginas específicas da navegação de retorno, ou você deseja executar o código de nível de página antes de exibir a página, você pode manipular a navegação em cada página.

Para lidar com versões anteriores a navegação para um aplicativo inteiro, você irá registrar um ouvinte global para o [ **BackRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) evento no `App.xaml` arquivo code-behind.

code-behind App.xaml:

```csharp
// App.xaml.cs
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}

private bool On_BackRequested()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// App.cpp
#include <winrt/Windows.UI.Core.h>
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"

#include "App.h"
#include "MainPage.h"

using namespace winrt;
...

    Windows::UI::Core::SystemNavigationManager::GetForCurrentView().BackRequested({ this, &App::App_BackRequested });
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }
    rootFrame.PointerPressed({ this, &App::On_PointerPressed });
...

void App::App_BackRequested(IInspectable const& /* sender */, Windows::UI::Core::BackRequestedEventArgs const& e)
{
    e.Handled(On_BackRequested());
}

void App::On_PointerPressed(IInspectable const& sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender.as<UIElement>()).Properties().PointerUpdateKind() == Windows::UI::Input::PointerUpdateKind::XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled(On_BackRequested());
    }
}

// Handles system-level BackRequested events.
bool App::On_BackRequested()
{
    if (Frame().CanGoBack())
    {
        Frame().GoBack();
        return true;
    }
    return false;
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>Otimizando para diferentes dispositivos e fatores forma

Esse design de navegação regressiva é aplicável a todos os dispositivos, mas diferentes fatores forma e dispositivos podem se beneficiar com a otimização. Isso também depende do botão Voltar do hardware compatível com capas diferentes.

- **Telefone/Tablet**: Um botão Voltar do hardware ou software está sempre presente em dispositivos móveis e tablet, mas é recomendável a desenhar um botão de voltar no aplicativo para maior clareza.
- **Área de trabalho para o Hub**: Desenhe o botão de voltar no aplicativo no canto superior esquerdo da interface de usuário do seu aplicativo.
- **Xbox/TV**: Não desenhe um botão Voltar, para que ele adicionará uma desordem desnecessária da interface do usuário. Em vez disso, conte com o botão Gamepad B para navegar para trás.

Se seu aplicativo será executado em vários dispositivos, [crie um gatilho visual personalizado para Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox) para alternar a visibilidade do botão. O controle NavigationView alternará automaticamente a visibilidade do botão Voltar se seu aplicativo for executado no Xbox. 

Recomendamos suporte às seguintes entradas para navegação regressiva. (Observe que algumas dessas entradas não são compatíveis com o sistema BackRequested e devem ser controladas por eventos separados).

| Entrada | Evento |
| --- | --- |
| Tecla Windows-Backspace | BackRequested |
| Botão Voltar de hardware | BackRequested |
| Botão Voltar do modo de tablet shell | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Tecla Alt + seta para a esquerda | KeyboardAccelerator.BackInvoked |

Os exemplos de código fornecidos acima demonstram como manipular todas essas entradas.

## <a name="system-back-behavior-for-backward-compatibilities"></a>Comportamento de regresso do sistema para compatibilidades com versões anteriores

Anteriormente, os aplicativos UWP usavam [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) para navegação regressiva. A API continuará a ter suporte para garantir a compatibilidade com versões anteriores, mas recomendamos não precisa mais depender [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility). Em vez disso, seu aplicativo deve desenhar seu próprio botão Voltar no aplicativo.

Se seu aplicativo continua usando [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility), em seguida, o sistema de interface do usuário será renderizado o botão Voltar do sistema dentro da barra de título. (As interações de aparência e o usuário para o botão Voltar são as mesmas compilações anteriores.)

![Título do botão Voltar da barra](images/nav-back-pc.png)

### <a name="system-back-bar"></a>Backup de sistema barra

> [!NOTE]
> "Backup de sistema barra" é apenas uma descrição, e não um nome oficial.

O sistema para trás barra é uma "banda" que é inserida entre a faixa de guia e a área de conteúdo do aplicativo. A faixa passa pela largura do aplicativo, com o botão Voltar na borda esquerda. A banda tem uma altura vertical de 32 pixels para garantir que o tamanho de destino de toque adequado para o botão Voltar.

A barra de voltar do sistema é exibida dinamicamente com base na visibilidade do botão Voltar. Quando o botão Voltar é visível, o sistema para trás barra é inserida, mudando o conteúdo do aplicativo para baixo por 32 pixels abaixo da faixa de guia. Quando o botão Voltar é oculto, o sistema para trás barra dinamicamente é removida, aumentando de conteúdo do aplicativo 32 pixels para atender a faixa da guia. Para evitar que o deslocamento de interface do usuário do seu aplicativo para cima ou para baixo, é recomendável que o desenho de um [botão de voltar no aplicativo](#back-button).

[Personalizações da barra de título](../shell/title-bar.md) serão transferidos para a guia aplicativo e o sistema volta barra. Se seu aplicativo especifica as propriedades de cor de primeiro plano e plano de fundo com [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), em seguida, serão aplicadas as cores para o guia e do sistema de volta barra.

## <a name="guidelines-for-custom-back-navigation-behavior"></a>Diretrizes para o comportamento da navegação regressiva personalizada

Se você optar por fornecer sua própria pilha Voltar de navegação, a experiência deve ser consistente com outros aplicativos. Recomendamos que você siga os seguintes padrões de ações de navegação:

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Ação de navegação</th>
<th align="left">Adicionar ao histórico de navegação?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>Página para página grupos diferentes de pares</strong></td>
<td style="vertical-align:top;"><strong>Sim</strong>
<p>Nesta ilustração, o usuário navega do nível 1 do aplicativo ao nível 2, cruzando grupos de par, de maneira que a navegação é adicionada ao histórico de navegação.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>Na próxima ilustração, o usuário navega entre dois grupos de par no mesmo nível, novamente cruzando grupos de par, de maneira que a navegação é adicionada ao histórico de navegação.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Página para página, mesmo emparelhar grupo, não na tela elemento de navegação</strong>
<p>O usuário navega de uma página para outra com o mesmo grupo de pares. Não na tela não há elemento de navegação (como <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>) que fornece navegação direta para ambas as páginas.</p></td>
<td style="vertical-align:top;"><strong>Sim</strong>
<p>Na ilustração a seguir, o usuário navega entre duas páginas no mesmo grupo de mesmo nível e a navegação deve ser adicionada ao histórico de navegação.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Página a página, o mesmo grupo de ponto a ponto, com na tela elemento de navegação</strong>
<p>O usuário navega de uma página para outra no mesmo grupo de pares. Ambas as páginas são mostradas no mesmo elemento de navegação, como <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>.</p></td>
<td style="vertical-align:top;"><strong>Ele depende</strong>
<p>Sim, adicione ao histórico de navegação, com duas exceções notáveis. Se você espera que os usuários do seu aplicativo para alternar entre as páginas no grupo de ponto a ponto com frequência, ou se você desejar preservar a hierarquia de navegação, em seguida, não adicione ao histórico de navegação. Nesse caso, quando o usuário pressiona Voltar, ele retorna para a última página visitada antes do usuário navegar para o grupo de pares atual. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Mostrar uma interface do usuário transitório</strong>
<p>O aplicativo exibe uma janela pop-up ou filho, como uma caixa de diálogo, tela inicial, ou teclado virtual, ou o aplicativo entra em um modo especial, como o modo de seleção múltipla.</p></td>
<td style="vertical-align:top;"><strong>Não</strong>
<p>Quando o usuário pressionar o botão Voltar, descarte a interface do usuário transitória (ocultar o teclado virtual, cancelar a caixa de diálogo, etc) e retorne à página que gerou a interface do usuário transitória.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Enumerar itens</strong>
<p>O aplicativo exibe o conteúdo para um item virtual, como os detalhes de um item selecionado na lista mestre/de detalhes.</p></td>
<td style="vertical-align:top;"><strong>Não</strong>
<p>A enumeração de itens é semelhante à navegação dentro de um grupo de pares. Quando o usuário pressionar Voltar, navegue até a página anterior à página atual com a enumeração de item.</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>Retomando

Quando o usuário alternar para outro aplicativo e retornar ao seu aplicativo, recomendamos retornar para a última página no histórico de navegação.

## <a name="related-articles"></a>Artigos relacionados

- [Noções básicas de navegação](navigation-basics.md)
