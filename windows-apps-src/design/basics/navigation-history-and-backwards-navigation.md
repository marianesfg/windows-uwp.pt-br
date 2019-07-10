---
Description: Saiba como implementar a navegação regressiva para analisar o histórico de navegação do usuário em um aplicativo UWP.
title: Histórico de navegação e navegação regressiva (aplicativos do Windows)
template: detail.hbs
op-migration-status: ready
ms.date: 4/9/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e3ab6760ed3eff1d284e51205de261796db0fb2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63799119"
---
# <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Histórico de navegação e navegação retroativa para aplicativos UWP

> **APIs importantes**: [evento BackRequested](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [classe SystemNavigationManager](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

A UWP (Plataforma Universal do Windows) oferece um sistema de navegação regressiva consistente a fim de analisar o histórico de navegação do usuário dentro de um aplicativo e, dependendo do dispositivo, de aplicativo para aplicativo.

Para implementar navegação regressiva no aplicativo, coloque um [botão Voltar](#back-button) no canto superior esquerdo da interface do usuário do aplicativo. Se o aplicativo usar o controle [NavigationView](../controls-and-patterns/navigationview.md), você poderá usar o [botão Voltar integrado no NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation).

O usuário espera que o botão Voltar navegue para o local anterior no histórico de navegação do aplicativo. Note que cabe a você decidir quais ações de navegação serão adicionadas ao histórico de navegação e como responder ao pressionar o botão Voltar.

## <a name="back-button"></a>Botão Voltar

Para criar um botão Voltar, use o controle de [Botão](../controls-and-patterns/buttons.md) com o estilo `NavigationBackButtonNormalStyle` e posicione o botão no canto superior esquerdo da interface do usuário do aplicativo. Para saber mais detalhes, confira os exemplos de código XAML abaixo.

![Botão Voltar na parte superior esquerda da interface do usuário do aplicativo](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Se o aplicativo tiver um [CommandBar](../controls-and-patterns/app-bars.md) superior, o controle de botão com 44px de altura não será bem alinhado com AppBarButtons de 48px. No entanto, para evitar inconsistência, alinhe o controle de botão dentro dos limites de 48px.

![Botão Voltar na barra de comandos superior](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Para minimizar a movimentação de elementos de interface do usuário no aplicativo, mostre um botão Voltar desabilitado quando não houver nada no backstack (veja o exemplo de código abaixo). No entanto, se você espera que o aplicativo nunca tenha um backstack, não precisa exibir o botão Voltar.

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

Acima, manipulamos a navegação regressiva em uma única página. Será possível manipular a navegação em cada página se você quiser excluir páginas específicas da navegação regressiva ou quiser executar código no nível da página antes de exibi-la.

Para manipular a navegação regressiva de um aplicativo completo, registre um ouvinte global no evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) no arquivo `App.xaml` do code-behind.

Code-behind App.xaml:

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

## <a name="optimizing-for-different-device-and-form-factors"></a>Otimizar para diferentes dispositivos e fatores forma

Essa diretriz de design de navegação regressiva é aplicável a todos os dispositivos, mas diferentes fatores forma e dispositivos podem se beneficiar com a otimização. Isso também depende do botão Voltar do hardware compatível com shells diferentes.

- **Telefone/Tablet**: Um botão Voltar do hardware ou software está sempre presente em celulares e tablets, mas é recomendável desenhar um botão Voltar no aplicativo para maior clareza.
- **Desktop/Hub**: Desenhe o botão Voltar no aplicativo no canto superior esquerdo da interface do usuário do aplicativo.
- **Xbox/TV**: Não desenhe um botão Voltar, pois isso adicionará confusão desnecessária na interface do usuário. Em vez disso, conte com o botão Gamepad B para navegar para trás.

Se o aplicativo será executado em vários dispositivos, [crie um gatilho visual personalizado para Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox) para alternar a visibilidade do botão. O controle NavigationView alternará automaticamente a visibilidade do botão Voltar se o aplicativo for executado no Xbox. 

É recomendada a compatibilidade com as seguintes entradas para navegação regressiva. Observe que algumas dessas entradas não são compatíveis com o sistema BackRequested e devem ser controladas por eventos separados.

| Entrada | Evento |
| --- | --- |
| Tecla Windows-Backspace | BackRequested |
| Botão Voltar de hardware | BackRequested |
| Botão Voltar do modo de tablet shell | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Tecla Alt + seta para a esquerda | KeyboardAccelerator.BackInvoked |

Os exemplos de código fornecidos acima demonstram como manipular todas essas entradas.

## <a name="system-back-behavior-for-backward-compatibilities"></a>Comportamento do botão Voltar do sistema para compatibilidades com versões anteriores

Anteriormente, os aplicativos UWP usavam [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) para navegação regressiva. A API continuará a ser compatível com versões anteriores, mas não é mais recomendável que conte com [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility). Em vez disso, o aplicativo deve desenhar seu próprio botão Voltar.

Se o aplicativo continuar usando [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility), a interface do usuário do sistema o renderizará o botão Voltar do sistema dentro da barra de título. (A aparência e as interações do usuário com o botão Voltar não foram alteradas em relação aos builds anteriores.)

![Botão Voltar da barra de título](images/nav-back-pc.png)

### <a name="system-back-bar"></a>Barra de voltar do sistema

> [!NOTE]
> "Barra de voltar do sistema" é somente uma descrição, e não um nome oficial.

A barra de voltar do sistema é uma "faixa" inserida entre a faixa de guia e a área de conteúdo do aplicativo. A faixa passa pela largura do aplicativo, com o botão Voltar na borda esquerda. A faixa tem uma altura vertical de 32 pixels para garantir o tamanho certo do alvo de toque para o botão Voltar.

A barra de voltar do sistema é exibida dinamicamente com base na visibilidade do botão Voltar. Quando o botão Voltar estiver visível, a barra de voltar do sistema será inserida, mudando o conteúdo do aplicativo para 32 pixels abaixo da faixa da guia. Quando o botão Voltar estiver oculto, a barra de voltar do sistema será removida dinamicamente, mudando o conteúdo do aplicativo para 32 pixels acima da faixa da guia. Para evitar que a interface do usuário do aplicativo mova-se para cima ou para baixo, é recomendável desenhar um [botão Voltar no aplicativo](#back-button).

As [personalizações da barra de título](../shell/title-bar.md) serão transferidas para a guia do aplicativo e a barra de voltar do sistema. Se o aplicativo especificar as propriedades de cor de primeiro e segundo plano com [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), as cores serão aplicadas à guia e à barra de voltar do sistema.

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
<td style="vertical-align:top;"><strong>Página para página, grupos de pares diferentes</strong></td>
<td style="vertical-align:top;"><strong>Sim</strong>
<p>Nesta ilustração, o usuário navega do nível 1 do aplicativo ao nível 2, cruzando grupos de par, de maneira que a navegação é adicionada ao histórico de navegação.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>Na próxima ilustração, o usuário navega entre dois grupos de par no mesmo nível, novamente cruzando grupos de par, de maneira que a navegação é adicionada ao histórico de navegação.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Página a página, mesmo grupo de pares, sem elemento de navegação na tela</strong>
<p>O usuário navega de uma página para outra com o mesmo grupo de pares. Não há elemento de navegação na tela, como <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>, que forneça navegação direta para ambas as páginas.</p></td>
<td style="vertical-align:top;"><strong>Sim</strong>
<p>Na ilustração a seguir, o usuário navega entre duas páginas no mesmo grupo de pares e a navegação precisa ser adicionada ao histórico de navegação.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Página a página, mesmo grupo de pares, com elemento de navegação na tela</strong>
<p>O usuário navega de uma página para outra no mesmo grupo de pares. Ambas as páginas são mostradas no mesmo elemento de navegação, como <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>.</p></td>
<td style="vertical-align:top;"><strong>Depende</strong>
<p>Sim, adicione ao histórico de navegação, mas com duas exceções notáveis. Se você espera que os usuários do aplicativo alternem entre as páginas no grupo de pares com frequência ou se desejar preservar a hierarquia de navegação, então não adicione ao histórico de navegação. Nesse caso, quando o usuário pressiona Voltar, ele retorna para a última página visitada antes do usuário navegar para o grupo de pares atual. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Exibir uma interface do usuário transitória</strong>
<p>O aplicativo exibe uma janela pop-up ou filho, como uma caixa de diálogo, tela inicial, ou teclado virtual, ou o aplicativo entra em um modo especial, como o modo de seleção múltipla.</p></td>
<td style="vertical-align:top;"><strong>Não</strong>
<p>Quando o usuário pressionar o botão Voltar, descarte a interface do usuário transitória (ocultar o teclado virtual, cancelar a caixa de diálogo, etc) e retorne à página que gerou a interface do usuário transitória.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Enumerar os itens</strong>
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
