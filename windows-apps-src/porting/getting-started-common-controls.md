---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: Introdução aos controles comuns
title: Introdução aos controles comuns
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a64dbd6a9530f81c55b0d4b52e4c0fd55c4b9956
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372834"
---
# <a name="getting-started-common-controls"></a>Introdução: Controles comuns


## <a name="common-controls-list"></a>Lista de controles comuns

Na seção anterior, você trabalhou com apenas dois controles: botões e blocos de texto. É claro, há muito mais controles que estão disponíveis para você. Aqui estão alguns controles comuns que você usará em seus aplicativos e seus equivalentes do iOS. Os controles de iOS estão listados em ordem alfabética, perto dos controles da Plataforma Universal do Windows (UWP) mais similares.

O que há de mais inteligente com relação aos controles UWP é que eles podem perceber em qual tipo de dispositivo estão sendo executados e mudar a aparência e a funcionalidade deles de acordo. Por exemplo, se seu projeto usa o controle [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)), é bastante inteligente ele se otimizar para ter aparência e comportamento diferentes em um desktop e em um telefone. Você não precisa fazer nada: os controles se ajustam no tempo de execução.

| Controle iOS (classe/protocolo) | Controle equivalente da UWP |
|------------------------------|--------------------------------------|
| Indicador de atividade (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> Consulte também [Guia de início rápido: adicionando controles de progresso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Visualização da barra de notificação de anúncio (**ADBannerView**) e representante de visualização da barra de notificação de anúncio (**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> Consulte também [exibir anúncios em seu aplicativo](../monetize/display-ads-in-your-app.md) |
| Botão (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> Consulte também [guia de início rápido: Adicionando controles de botão](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| Seletor de data (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| Visualização de imagem (UIImageView) | [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> Consulte também [Image and ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| Rótulo (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> Consulte também [Guia de início rápido: exibindo texto](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Visualização de mapa (MKMapView) e representante de visualização de mapa (MKMapViewDelegate) | Consulte [do Bing Maps para aplicativos UWP](https://go.microsoft.com/fwlink/p/?LinkId=263496) |
| Controlador de navegação (UINavigationController) e representante de controlador de navegação (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> Consulte também [Navegação](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Controle de página (UIPageControl) | [Página](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> Consulte também [Navegação](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Visualização de seletor (UIPickerView) e representante de visualização de seletor (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> Consulte também [Adicionando caixas de combinação e caixas de listagem](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| Barra de progresso (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> Consulte também [Guia de início rápido: adicionando controles de progresso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Visualização de rolagem (UIScrollView) e representante de visualização de rolagem (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  Consulte também [Extensible Application Markup Language (XAML) scrolling, panning, and zooming sample](https://go.microsoft.com/fwlink/p/?LinkId=238577) |
| Barra de pesquisa (UISearchBar) e representante da barra de pesquisa (UISearchBarDelegate) | Consulte [Adicionando pesquisa a um aplicativo](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  Consulte também [guia de início rápido: Adicionando pesquisa a um aplicativo](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| Controle segmentado (UISegmentedControl) | Nenhum |
| Controle deslizante (UISlider) | [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  Consulte também [Como adicionar um controle deslizante](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| Controlador de modo divisão (UISplitViewController) e representante do controlador de modo divisão (UISplitViewControllerDelegate) | Nenhum |
| Botão de alternância (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  Consulte também [Como adicionar um botão de alternância](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| Controlador de barra de guia (UITabBarController) e representante do controlador de barra de guia (UITabBarControllerDelegate) | Nenhum |
| Controlador de visualização em tabela (UITableViewController), visualização em tabela (UITableView), representante de visualização em tabela (UITableViewDelegate) e célula de tabela (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  Consulte também [Início rápido: adicionando controles ListView e GridView](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| Campo de texto (UITextField) e representante do campo de texto (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  Consulte também [Exibir e editar texto](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| Visualização de texto (UITextView) e representante de visualização de texto (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  Consulte também [Guia de início rápido: exibindo texto](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Visualização (UIView) e controlador de visualização (UIViewController) | [Página](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  Consulte também [Navegação](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Exibição da Web (UIWebView) e representante de exibição da Web (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  Consulte também [XAML WebView control sample](https://go.microsoft.com/fwlink/p/?LinkId=238582) |
| Janela (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  Consulte também [Navegação](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |

Para ainda mais controles, consulte [Lista de controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/).

**Observação**  para obter uma lista de controles para aplicativos UWP usando JavaScript e HTML, consulte [lista controles](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>Próximas etapas

[Guia de Introdução: Navegação](getting-started-navigation.md)

## <a name="related-topics"></a>Tópicos relacionados

* [Build 2014: E o XAML da interface do usuário e controles?](https://go.microsoft.com/fwlink/p/?LinkID=397897)
* [Build 2014: Desenvolver aplicativos usando a estrutura de interface do usuário XAML comum](https://go.microsoft.com/fwlink/p/?LinkID=397898)
* [Build 2014: Usando o Visual Studio para criar o XAML aplicativos convergidos](https://go.microsoft.com/fwlink/p/?LinkID=397876)
