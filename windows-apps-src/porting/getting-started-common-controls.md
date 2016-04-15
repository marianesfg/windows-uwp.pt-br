---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: Introdução a controles comuns
title: Introdução a controles comuns
---

# Introdução: controles comuns

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Lista de controles comuns

Na seção anterior, você trabalhou com apenas dois controles: botões e blocos de texto. Evidentemente, há muito mais controles disponíveis. Aqui estão alguns controles comuns que você usará em seus aplicativos e seus equivalentes do iOS. Os controles de iOS estão listados em ordem alfabética, perto dos controles da Plataforma Universal do Windows (UWP) mais similares.

O que há de mais inteligente com relação aos controles UWP é que eles podem perceber em qual tipo de dispositivo estão sendo executados e mudar a aparência e a funcionalidade deles de acordo. Por exemplo, se seu projeto usa o controle [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681), é bastante inteligente ele se otimizar para ter aparência e comportamento diferentes em um desktop e em um telefone. Você não precisa fazer nada: os controles se ajustam no tempo de execução.

| Controle iOS (classe/protocolo) | Controle de aplicativo da Windows Store equivalente |
|------------------------------|--------------------------------------|
| Indicador de atividade (**UIActivityIndicatorView**) | [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) <br/> Consulte também [Guia de início rápido: adicionando controles de progresso](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| Visualização da barra de notificação de anúncio (**ADBannerView**) e representante de visualização da barra de notificação de anúncio (**ADBannerViewDelegate**) | Consulte [Microsoft Advertising SDK](http://go.microsoft.com/fwlink/p/?LinkId=263494) |
| Botão (UIButton) | [Button](https://msdn.microsoft.com/library/windows/apps/br209265) <br/> Consulte também [Guia de início rápido: adicionando controles de botão](https://msdn.microsoft.com/library/windows/apps/xaml/jj153346) |
| Seletor de data (UIDatePicker) | [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681) |
| Visualização de imagem (UIImageView) | [Image](https://msdn.microsoft.com/library/windows/apps/br242752) <br/> Consulte também [Image and ImageBrush](https://msdn.microsoft.com/library/windows/apps/mt280382) |
| Rótulo (UILabel) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/> Consulte também [Guia de início rápido: exibindo texto](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| Visualização de mapa (MKMapView) e representante de visualização de mapa (MKMapViewDelegate) | Consulte [Bing Mapas para aplicativos da Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=263496) |
| Controlador de navegação (UINavigationController) e representante de controlador de navegação (UINavigationControllerDelegate) | [Frame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/> Consulte também [Navegação](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| Controle de página (UIPageControl) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/> Consulte também [Navegação](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| Visualização de seletor (UIPickerView) e representante de visualização de seletor (UIPickerViewDelegate) | [ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348) <br/> Consulte também [Adicionando caixas de combinação e caixas de listagem](https://msdn.microsoft.com/library/windows/apps/xaml/hh780616) |
| Barra de progresso (UIProgressView) | [ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529) <br/> Consulte também [Guia de início rápido: adicionando controles de progresso](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| Visualização de rolagem (UIScrollView) e representante de visualização de rolagem (UIScrollViewDelegate) | [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527) <br/>  Consulte também [Extensible Application Markup Language (XAML) scrolling, panning, and zooming sample](http://go.microsoft.com/fwlink/p/?LinkId=238577) |
| Barra de pesquisa (UISearchBar) e representante da barra de pesquisa (UISearchBarDelegate) | Consulte [Adicionando pesquisa a um aplicativo](https://msdn.microsoft.com/library/windows/apps/xaml/jj130767) <br/>  Consulte também [Guia de início rápido: adicionando pesquisa a um aplicativo](https://msdn.microsoft.com/library/windows/apps/xaml/hh868180) |
| Controle segmentado (UISegmentedControl) | Nenhum |
| Controle deslizante (UISlider) | [Slider](https://msdn.microsoft.com/library/windows/apps/br209614) <br/>  Consulte também [Como adicionar um controle deslizante](https://msdn.microsoft.com/library/windows/apps/xaml/hh868197) |
| Controlador de modo divisão (UISplitViewController) e representante do controlador de modo divisão (UISplitViewControllerDelegate) | Nenhum |
| Botão de alternância (UISwitch) | [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/br209712) <br/>  Consulte também [Como adicionar um botão de alternância](https://msdn.microsoft.com/library/windows/apps/xaml/hh868198) |
| Controlador de barra de guia (UITabBarController) e representante do controlador de barra de guia (UITabBarControllerDelegate) | Nenhum |
| Controlador de visualização em tabela (UITableViewController), visualização em tabela (UITableView), representante de visualização em tabela (UITableViewDelegate) e célula de tabela (UITableViewCell) | [ListView](https://msdn.microsoft.com/library/windows/apps/br242878) <br/>  Consulte também [Início rápido: adicionando controles ListView e GridView](https://msdn.microsoft.com/library/windows/apps/xaml/hh780650) |
| Campo de texto (UITextField) e representante do campo de texto (UITextFieldDelegate) | [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683) <br/>  Consulte também [Exibir e editar texto](https://msdn.microsoft.com/library/windows/apps/mt280218) |
| Visualização de texto (UITextView) e representante de visualização de texto (UITextViewDelegate) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/>  Consulte também [Guia de início rápido: exibindo texto](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| Visualização (UIView) e controlador de visualização (UIViewController) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/>  Consulte também [Navegação](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| Exibição da Web (UIWebView) e representante de exibição da Web (UIWebViewDelegate) | [WebView](https://msdn.microsoft.com/library/windows/apps/br227702) <br/>  Consulte também [XAML WebView control sample](http://go.microsoft.com/fwlink/p/?LinkId=238582) |
| Janela (UIWindow) | [Frame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/>  Consulte também [Navegação](https://msdn.microsoft.com/library/windows/apps/mt187344) |

Para ainda mais controles, consulte [Lista de controles](https://msdn.microsoft.com/library/windows/apps/mt185406).

**Observação**  Para uma lista de controles para aplicativos da Windows Store usando JavaScript e HTML, consulte [Lista de controles](https://msdn.microsoft.com/library/windows/apps/hh465453).

### Próxima etapa

[Introdução: navegação](getting-started-navigation.md)

## Tópicos relacionados

* [Compilação 2014: e quanto a IU XAML e controles?](http://go.microsoft.com/fwlink/p/?LinkID=397897)
* [Compilação 2014: desenvolvendo aplicativos usando a estrutura da IU XAML comum](http://go.microsoft.com/fwlink/p/?LinkID=397898)
* [build 2014: usando o Visual Studio para compilar aplicativos convergidos para XAML](http://go.microsoft.com/fwlink/p/?LinkID=397876)


<!--HONumber=Mar16_HO1-->


