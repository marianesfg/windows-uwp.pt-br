---
author: serenaz
Description: The Universal Windows Platform (UWP) provides a consistent back navigation system for traversing the user's navigation history within an app and, depending on the device, from app to app.
title: Histórico de navegação e navegação retroativa (aplicativos do Windows)
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/22/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 824f0e83408893bf95d856067282b1fea1313876
ms.sourcegitcommit: 588171ea8cb629d2dd6aa2080e742dc8ce8584e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
ms.locfileid: "1895403"
---
#  <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Histórico de navegação e navegação retroativa para apps UWP

> **APIs importantes**: [BackRequested event](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [SystemNavigationManager class](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

A Plataforma Universal do Windows (UWP) oferece um sistema de navegação regressiva consistente a fim de percorrer o histórico de navegação do usuário dentro de um aplicativo e, dependendo do dispositivo, de aplicativo para aplicativo.

Para implementar navegação retroativa no seu aplicativo, coloque um [botão Voltar](#Back-button) no canto superior esquerdo da interface do usuário do seu aplicativo. Se seu aplicativo usa o controle [NavigationView](../controls-and-patterns/navigationview.md), você pode usar o [botão Voltar integrado no NavigationView ](../controls-and-patterns/navigationview.md#backwards-navigation). 

O usuário espera que o botão Voltar navegue para o local anterior no histórico de navegação do aplicativo. Note que cabe a você decidir quais ações de navegação serão adicionadas ao histórico de navegação e como responder ao pressionar o botão Voltar.

## <a name="back-button"></a>Botão Voltar

Para criar um botão Voltar, use o controle [Botão](../controls-and-patterns/buttons.md) com o `NavigationBackButtonNormalStyle` estilo e posicione o botão no canto superior esquerdo da interface do usuário do seu aplicativo.

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

Para minimizar a movimentação em seu aplicativo de elementos de interface do usuário, mostre um botão Voltar desabilitado quando não há nada no backstack (veja o exemplo de código abaixo).

![Estados do botão Voltar](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir demonstra como implementar o comportamento de navegação regressiva com um botão Voltar. O código responde ao evento do botão [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) e desabilita/habilita a visibilidade do botão no [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_), que é chamado ao navegar para uma nova página. O exemplo de código também manipula entradas de teclas Voltar de hardware e sistema de software registrando um ouvinte para o evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested).

```xaml
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

Code-behind:

```csharp
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

Aqui, nós registramos um ouvinte global para evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) no arquivo de code-behind `App.xaml`. É possível se registrar para esse evento em cada página se você quiser excluir páginas específicas da navegação regressiva ou quiser executar código no nível da página antes de exibi-la.

code-behind App.xaml:

```csharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed = e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>Otimizando para diferentes dispositivos e fatores forma

Esse design de navegação regressiva é aplicável a todos os dispositivos, mas diferentes fatores forma e dispositivos podem se beneficiar com a otimização. Isso também depende do botão Voltar do hardware compatível com capas diferentes.

- **Telefone/Tablet**: um botão Voltar do hardware ou software está sempre presente em celulares e tablet, mas nós recomendamos desenhar um botão Voltar no aplicativo para maior clareza.
- **Desktop/Hub**: desenhe o botão Voltar no aplicativo no canto superior esquerdo da interface do usuário do seu aplicativo.
- **Xbox/TV**: não desenhe um botão Voltar, pois isso adicionará confusão desnecessária na interface do usuário. Em vez disso, conte com o botão Gamepad B para navegar para trás.

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

Anteriormente, os aplicativos UWP usavam [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) para navegação regressiva. A API continuará a ter suporte para compatibilidade com versões anteriores, mas não recomendamos mais que conte com o botão Voltar da barra de título. Em vez disso, seu aplicativo deve desenhar seu próprio botão Voltar no aplicativo.

Se seu aplicativo continua a usar [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility), o botão Voltar será renderizado dentro da barra de título, como de costume.

![botão Voltar da barra de título](images/nav-back-pc.png)

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
<td style="vertical-align:top;"><strong>Página a página, mesmo grupo de par, sem elemento de navegação na tela</strong>
<p>O usuário navega de uma página para outra com o mesmo grupo de pares. Não há qualquer elemento de navegação que seja sempre presente (como o painel de navegação superior ou um painel de navegação esquerdo encaixado) que ofereça navegação direta para as duas páginas.</p></td>
<td style="vertical-align:top;"><strong>Sim</strong>
<p>Na ilustração a seguir, o usuário navega entre duas páginas no mesmo grupo de pares. As páginas não usam uma barra de navegação superior ou um painel de navegação esquerdo encaixado, então a navegação é adicionada ao histórico de navegação.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Página a página, mesmo grupo de par, com elemento de navegação na tela</strong>
<p>O usuário navega de uma página para outra no mesmo grupo de pares. Ambas as páginas são mostradas no mesmo elemento de navegação. Por exemplo, as duas páginas usam o mesmo elemento do painel superior guias/pivôs ou ambas as páginas aparecem em um painel de navegação esquerdo encaixado.</p></td>
<td style="vertical-align:top;"><strong>Depende</strong>
<p>Sim, adicione ao histórico de navegação, mas com duas exceções notáveis. Se você espera que os usuários do seu aplicativo alternem entre as páginas no grupo de pares com frequência ou se desejar preservar o estado/histórico de navegação em uma página do grupo de pares, então não adicione ao histórico de navegação. Nesse caso, quando o usuário pressiona Voltar, ele retorna para a última página visitada antes do usuário navegar para o grupo de pares atual. </p>
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

Quando o usuário alternar para outro aplicativo e retornar ao seu aplicativo, recomendamos retornar para a última página no histórico de navegação

## <a name="related-articles"></a>Artigos relacionados

- [Noções básicas de navegação](navigation-basics.md)