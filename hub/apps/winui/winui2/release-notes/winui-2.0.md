---
title: Notas sobre a versão do WinUI 2.0
description: Notas sobre a versão do WinUI 2.0.
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: 300a8eaa0077b3a59ef2ac01213cd82b0c76c343
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580653"
---
# <a name="windows-ui-library-20"></a>Biblioteca de Interface do Usuário do Windows 2.0

O WinUI 2.0 é a primeira versão pública da Biblioteca de Interface do Usuário do Windows.

O WinUI é a maneira mais fácil de criar grandes experiências de Fluent Design para o Windows.

O WinUI inclui dois pacotes NuGet:

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml): Controles e Design Fluente para aplicativos UWP. Este é o pacote principal do WinUI.

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct): APIs de nível baixo para uso em componentes de middleware.

Você pode baixar e usar pacotes do WinUI em seu aplicativo usando o gerenciador de pacotes NuGet: confira [Introdução à Biblioteca de Interface do Usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/getting-started) para obter mais informações.

O WinUI é um projeto de software livre hospedado no GitHub. Relatórios de bugs, solicitações de recursos e contribuições de código da comunidade são bem-vindos no [Repositório da Biblioteca de Interface do Usuário do Windows](https://aka.ms/winui).

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

Outubro de 2018

É a primeira versão do [pacote NuGet Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). Inclui recursos e controles nativos oficiais do Fluent para aplicativos UWP do Windows.

### <a name="new-features"></a>Novos recursos

Os controles e os padrões desta versão incluem:

| Recurso | Descrição |
| --- | --- |
|[AcrylicBrush]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.acrylicbrush)| Pinta uma área com material semitransparente que usa vários efeitos, incluindo desfoque e uma textura de ruído.|
|[BitmapIconSource]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| É uma origem de ícone que usa um bitmap como conteúdo.|
|[ColorPicker]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.colorpicker)| É um controle que permite ao usuário escolher uma cor em um espectro de cores, controles deslizantes e entradas de texto.|
|[CommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|É um submenu especializado que fornece o layout para o AppBarButton e elementos de comando relacionados.|
|[DropDownButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|É um botão com uma divisa destinada a abrir um menu.|
|[FontIconSource](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|É uma origem de ícone que usa um glifo da fonte especificada.|
|[MenuBar](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)|É um contêiner especializado que apresenta um conjunto de menus em uma linha horizontal, normalmente na parte superior da janela de um aplicativo.|
|[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)|É um menu de alto nível em um controle MenuBar.|
|[NavigationView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.navigationview)|É um contêiner que habilita a navegação no conteúdo do aplicativo. Tem um cabeçalho, uma exibição do conteúdo principal e um painel de menu para os comandos de navegação.|
|[ParallaxView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.parallaxview)|É um contêiner que vincula a posição de rolagem de um elemento do primeiro plano, como uma lista, a um elemento em segundo plano, como uma imagem. Enquanto você navega no elemento do primeiro plano, ele anima o elemento em segundo plano para criar um efeito paralaxe.|
|[PersonPicture](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.personpicture)|É um controle que exibe a imagem de avatar de uma pessoa, se existir uma disponível. Caso contrário, exibe as iniciais da pessoa ou um glifo genérico.|
|[RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|É um controle que permite ao usuário inserir uma classificação por estrelas.|
|[RefreshContainer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|É um controle de contêiner que fornece um RefreshVisualizer e uma funcionalidade de deslizar para atualizar para o conteúdo rolável.|
|[RefreshVisualizer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|É um controle que fornece indicadores de estado animado para a atualização do conteúdo.|
|[RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|Pinta o fundo de um controle com um efeito "revelar" usando o pincel de composição e efeitos de luz.|
|[RevealBorderBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|Pinta a borda de um controle com um efeito "revelar" usando o pincel de composição e efeitos de luz.|
|[RevealBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbrush)|Classe base para os pincéis que usam efeitos de composição e luz para implementar a o tratamento de design visual de revelação.|
|[SplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.splitbutton)|É um botão com duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão padrão e a outra invoca um submenu.|
|[SwipeControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|É um contêiner que dá acesso a comandos contextuais por meio de interações de toque.|
|[SymbolIconSource](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|É uma origem de ícone que usa um glifo da fonte Segoe MDL2 Assets como conteúdo.|
|[TextCommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|É um submenu da barra de comandos especializada que contém comandos de edição de texto.|
|[ToggleSplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|É um botão com duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão de alternância e a outra invoca um submenu.|
|[TreeView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.treeview)|É uma lista hierárquica com nós em expansão e em colapso que contêm itens aninhados.|

## <a name="examples"></a>Exemplos

O aplicativo de exemplo da Galeria de Controles XAML inclui demonstrações interativas e código de exemplo para usar controles WinUI.

* Instale o aplicativo da Galeria de Controles XAML da [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

* A Galeria de Controles XAML também é de [software livre no GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Documentação

Artigos de instruções sobre os controles da Biblioteca de Interface do Usuário do Windows estão incluídos com a [Documentação de controles da Plataforma Universal do Windows](/windows/uwp/design/controls-and-patterns/).

Os documentos da referência de API estão localizados aqui: [APIs da Biblioteca de Interface do Usuário do Windows](/uwp/api/overview/winui/).
