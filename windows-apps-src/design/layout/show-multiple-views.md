---
Description: Visualize partes diferentes de seu aplicativo em janelas separadas.
title: Mostrar vários modos de exibição para um aplicativo
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68729515"
---
# <a name="show-multiple-views-for-an-app"></a>Mostrar vários modos de exibição para um aplicativo

![Wireframe mostrando um aplicativo com várias janelas](images/multi-view.gif)

Ajude os usuários a serem mais produtivos permitindo que eles exibam partes independentes do aplicativo em janelas separadas. Quando você cria várias janelas para um aplicativo, a barra de tarefas mostra cada janela separadamente. Os usuários podem mover, redimensionar, mostrar e ocultar janelas do aplicativo de maneira independente e alternar janelas do aplicativo como se elas fossem aplicativos separados.

> **APIs importantes**: [namespace Windows.UI.ViewManagement](/uwp/api/windows.ui.viewmanagement), [namespace Windows.UI.WindowManagement](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>Quando um aplicativo deve usar várias exibições?

Há diversos cenários que podem se beneficiar do uso de várias exibições. Veja aqui alguns exemplos:

- Um aplicativo de email que permite aos usuários exibir uma lista de mensagens recebidas ao redigir um novo email
- Um aplicativo de catálogo de endereços que permite aos usuários comparar as informações de contato de várias pessoas lado a lado
- Um player de música que permite aos usuários ver o que está sendo reproduzido durante a navegação em uma lista de outras músicas disponíveis
- Um aplicativo de anotações que permite aos usuários copiar informações de uma página de anotações para outra
- Um aplicativo de leitura que permite aos usuários abrir vários artigos para leitura posterior, após a oportunidade de ver todos os títulos principais

Embora cada layout de aplicativo seja exclusivo, recomendamos incluir um botão de "nova janela" em um local previsível, como o canto superior direito do conteúdo que pode ser aberto em uma nova janela. Considere também incluir uma opção de [menu de contexto](../controls-and-patterns/menus.md) em "Abrir em uma nova janela".

Para criar instâncias separadas do seu aplicativo (em vez de janelas separadas para a mesma instância), confira [Criar um aplicativo UWP de várias instâncias](../../launch-resume/multi-instance-uwp.md).

## <a name="windowing-hosts"></a>Hosts de janelas

Há diferentes formas de hospedar conteúdo da UWP em um aplicativo.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     Modo de exibição do aplicativo é o emparelhamento 1:1 de um thread e uma janela que o aplicativo usa para exibir conteúdo. O primeiro modo de exibição criado quando o aplicativo é iniciado é chamado de *modo de exibição principal*. Cada CoreWindow/ApplicationView opera em seu próprio thread. Ter que trabalhar em diferentes threads da interface do usuário pode complicar aplicativos com várias janelas.

    A exibição principal do seu aplicativo é sempre hospedada em um ApplicationView. O conteúdo em uma janela secundária pode ser hospedado em um ApplicationView ou em um AppWindow.

    Para saber como usar o ApplicationView para mostrar janelas secundárias no seu aplicativo, confira [Use ApplicationView](application-view.md).
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    O AppWindow simplifica a criação de aplicativos UWP com várias janelas porque opera no mesmo thread da interface do usuário do qual ele foi criado.

    A classe AppWindow e outras APIs no namespace [WindowManagement](/uwp/api/windows.ui.windowmanagement) estão disponíveis a partir do Windows 10, versão 1903 (SDK 18362). Caso seu aplicativo tenha como alvo versões anteriores do Windows 10, use o ApplicationView para criar janelas secundárias.

    Para saber como usar o AppWindow para mostrar janelas secundárias no seu aplicativo, confira [Use AppWindow](app-window.md).

    > [!NOTE]
    > AppWindow está atualmente em versão prévia. Isso significa que você pode enviar aplicativos que usam o AppWindow à Store, mas alguns componentes de plataforma e estrutura são conhecidos por não funcionar com o AppWindow (confira [Limitações](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (Ilhas XAML)

     O conteúdo de XAML UWP em um aplicativo Win32 (usando HWND), também conhecido como Ilhas XAML, é hospedado em um DesktopWindowXamlSource.

    Para saber mais sobre Ilhas XAML, confira [Usar a API de hospedagem de XAML UWP em um aplicativo da área de trabalho](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)

### <a name="make-code-portable-across-windowing-hosts"></a>Viabilizar a portabilidade de código em hosts de janelas

Quando o conteúdo XAML é exibido em um [CoreWindow](/uwp/api/windows.ui.core.corewindow), sempre há um [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) e uma [Janela](/uwp/api/windows.ui.xaml.window) XAML associados. Você pode usar APIs nessas classes para obter informações como os limites de janela. Para recuperar uma instância dessas classes, use o método estático [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread), o método [ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) ou a propriedade [Window.Current](/uwp/api/windows.ui.xaml.window.current). Além disso, há muitas classes que usam o padrão de `GetForCurrentView` para recuperar uma instância da classe, como [DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview).

Essas APIs funcionam porque há apenas uma árvore de conteúdo XAML para um CoreWindow/ApplicationView, portanto, o XAML sabe que o contexto no qual ele está hospedado é o CoreWindow/ApplicationView.

Quando o conteúdo XAML está em execução dentro de um AppWindow ou DesktopWindowXamlSource, você pode ter várias árvores de conteúdo XAML em execução no mesmo thread ao mesmo tempo. Nesse caso, essas APIs não fornecem as informações corretas, já que o conteúdo não é mais executado dentro do CoreWindow/ApplicationView atual (e da Janela XAML).

Para garantir que seu código funcione corretamente em todos os hosts de janelas, você deve substituir as APIs que dependem de [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) e [Window](/uwp/api/windows.ui.xaml.window) por novas APIs que obtêm o contexto da classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot).
A classe XamlRoot representa uma árvore de conteúdo XAML e informações sobre o contexto no qual ele está hospedado, seja um CoreWindow, AppWindow ou DesktopWindowXamlSource. Essa camada de abstração permite que você escreva o mesmo código independentemente do host de janelas em que o XAML é executado.

Esta tabela mostra o código que não funciona corretamente em hosts de janelas e o novo código portátil pelo qual você pode substituí-lo, bem como algumas APIs que você não precisa alterar.

| Se você usou... | Substitua por... |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Inalterado. Isso tem suporte no AppWindow e no DesktopWindowXamlSource. |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Inalterado. Isso tem suporte no AppWindow e no DesktopWindowXamlSource. |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | Retorna o principal objeto da Janela XAML associado ao CoreWindow atual. Confira a Observação após esta tabela. |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | Inalterado. Isso tem suporte no AppWindow e no DesktopWindowXamlSource. |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>Em aplicativos de Ilhas XAML, isso resultará em um erro. Em aplicativos AppWindow, isso retornará pop-ups abertas na janela principal. | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> Para o conteúdo XAML em um DesktopWindowXamlSource, existe um CoreWindow/Window no thread, mas ele sempre está invisível e tem um tamanho de 1x1. Ele ainda é acessível para o aplicativo, mas não retornará limites ou visibilidade significativas.
>
>Para conteúdo XAML em um AppWindow, sempre haverá exatamente um CoreWindow no mesmo thread. Se você chamar uma API `GetForCurrentView` ou `GetForCurrentThread`, essa API retornará um objeto que reflete o estado do CoreWindow no thread, e não qualquer AppWindows que possa estar em execução nesse thread.


## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Forneça um ponto de entrada claro para a exibição secundária utilizando o glifo "abrir nova janela".
- Comunique aos usuários a finalidade da exibição secundária.
- Assegure que o aplicativo seja totalmente funcional em uma única exibição e que os usuários abram uma exibição secundária apenas por conveniência.
- Não conte com a exibição secundária para fornecer notificações ou outros elementos visuais transitórios.

## <a name="related-topics"></a>Tópicos relacionados

- [Usar AppWindow](app-window.md)
- [Usar ApplicationView](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
