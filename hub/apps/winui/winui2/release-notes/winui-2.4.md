---
title: Notas sobre a versão do WinUI 2.4
description: Notas sobre a versão do WinUI 2.4, incluindo novos recursos e correções de bugs.
ms.date: 05/08/2020
ms.topic: reference
ms.openlocfilehash: 88b75ae5ec0fbda545106ec0e7f7b86033808f70
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580303"
---
# <a name="windows-ui-library-24"></a>Biblioteca de Interface do Usuário do Windows 2.4

O WinUI 2.4 é a versão oficial mais recente da WinUI (Biblioteca de Interface do Usuário do Windows).

O WinUI é um projeto de software livre hospedado no GitHub no [repositório da Biblioteca de Interface do Usuário do Windows](https://aka.ms/winui). Registre todos os relatórios de bugs, solicitações de recursos e contribuições de código da comunidade nesse repositório.

Versões do WinUI: [Página de versão do GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

É possível adicionar os pacotes do WinUI aos projetos do Visual Studio por meio do Gerenciador de Pacotes do NuGet. Para saber mais, confira [Introdução à Biblioteca de Interface do Usuário do Windows](../getting-started.md).

Download do pacote NuGet: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Novos recursos

### <a name="radialgradientbrush"></a>RadialGradientBrush

Um RadialGradientBrush é desenhado em uma elipse que é definida pelas propriedades Center, RadiusX e RadiusY. As cores do gradiente começam no centro da elipse e terminam no raio.

![Pincel de gradiente radial](../images/radialgradientbrush.gif)<br>
*Pincel de gradiente radial*

[Diretrizes de uso](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[Referência de API](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

O controle ProgressRing é usado para interações modais em que o usuário é bloqueado até que o ProgressRing desapareça. Use esse controle se uma operação exige que a maior parte da interação com o aplicativo seja suspensa até que a operação seja concluída.

![Controle ProgressRing](../images/progressring.gif)<br>
*Controle ProgressRing*

[Diretrizes de uso](/windows/uwp/design/controls-and-patterns/progress-controls)

[Referência de API](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>Atualizações do TabView

As atualizações do controle TabView fornecem mais controle sobre como renderizar guias.

Você pode definir a largura de guias não selecionadas e mostrar apenas um ícone para economizar o espaço de tela:

![Tamanhos de guias do controle TabView](..\images\tabview-sizing.gif)<br>
*Tamanhos de guias do controle TabView*

Você também pode ocultar o botão fechar em guias não selecionadas até que o usuário passe o mouse sobre uma delas (em versões anteriores, ele sempre aparecia):

![Recurso do controle TabView de passar o mouse sobre a guia para fechá-la](..\images\tabview-closebuttononhover.gif)<br>
*Recurso do controle TabView de passar o mouse sobre a guia para fechá-la*

[Diretrizes de uso](/windows/uwp/design/controls-and-patterns/tab-view)

[Referência de API](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>Atualizações do tema escuro para a família de controles TextBox

Quando o tema escuro está habilitado, a cor da tela de fundo dos controles da família TextBox permanece escura por padrão durante a inserção de texto (em versões anteriores, a cor da tela de fundo muda para o tema claro durante a inserção de texto).

| Antes | Depois |
| - | - |
| ![Atualizações do tema escuro do TextBox (antes)](..\images\textbox-darkthemeupdates-before1.gif)<br>*Atualizações do tema escuro do TextBox (antes)* | ![Atualizações do tema escuro do TextBox (depois)](..\images\textbox-darkthemeupdates-after1.gif)<br>*Atualizações do tema escuro do TextBox (depois)* |
| ![Atualizações do tema escuro do TextBox (antes)](..\images\textbox-darkthemeupdates-before2.gif)<br>*Atualizações do tema escuro do TextBox (antes)* | ![Atualizações do tema escuro do TextBox (depois)](..\images\textbox-darkthemeupdates-after2.gif)<br>*Atualizações do tema escuro do TextBox (depois)* |

Estes são alguns controles incluídos na família de controles TextBox:

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>Navegação hierárquica

O controle [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4) agora dá suporte à navegação hierárquica e inclui os modos de exibição Esquerda, Parte superior e LeftCompact. Um NavigationView hierárquico é útil para exibir categorias de páginas, para identificar páginas com páginas secundárias relacionadas ou para usar em aplicativos que têm páginas no estilo de hub que vinculam a muitas outras páginas.

![Controle NavigationView hierárquico](..\images\HierarchicalNavView.gif)<br>*Controle NavigationView hierárquico*

[Diretrizes de uso](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[Referência de API](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>Amostras

Exemplos de cada um dos recursos do WinUI 2.4 descritos estão disponíveis no **XAML Controls Gallery**.

Caso você não tenha o aplicativo XAML Controls Gallery instalado, obtenha-o na [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

Você também pode ver o código-fonte do XAML Controls Gallery no [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery).
