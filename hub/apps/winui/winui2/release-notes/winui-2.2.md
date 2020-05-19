---
title: Notas sobre a versão do WinUI 2.2
description: Notas sobre a versão do WinUI 2.2, incluindo novos recursos e correções de bugs.
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: 2272046bc59865ebcf7958a165805d9ca4c7efce
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580463"
---
# <a name="windows-ui-library-22"></a>Biblioteca de Interface do Usuário do Windows 2.2

O WinUI 2.2 é a versão oficial mais recente da Biblioteca de Interface do Usuário do Windows.

É possível adicionar pacotes do WinUI para o seu aplicativo usando o Gerenciador de Pacotes NuGet: confira [Introdução à Biblioteca de Interface do Usuário do Windows](../getting-started.md) para saber mais.

O WinUI é um projeto de software livre hospedado no GitHub. Relatórios de bugs, solicitações de recursos e contribuições de código da comunidade são bem-vindos no [Repositório da Biblioteca de Interface do Usuário do Windows](https://aka.ms/winui).

## <a name="microsoftuixaml-22-version-history"></a>Histórico de versão do Microsoft.UI.Xaml 2.2

### <a name="windows-ui-library-22-official-release"></a>Versão oficial da Biblioteca de Interface do Usuário do Windows 2.2

AGOSTO DE 2019

[Página de versão do GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

[Download do pacote NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>Novos recursos

#### <a name="tabview"></a>TabView

![Exemplo](../images/tabview-gif.gif)

#### <a name="description"></a>Descrição

O controle TabView é uma coleção de guias que representa uma nova página ou documento em seu aplicativo. O TabView é útil quando o aplicativo tem várias páginas de conteúdo e o usuário quer adicionar, fechar e reorganizar as guias. O novo [Terminal do Windows](https://github.com/Microsoft/Terminal) usa o TabView para mostrar várias interfaces de linha de comando.

#### <a name="documentation"></a>Documentação

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>Atualizações do NavigationView

##### <a name="a-navigationviews-back-button-update"></a>a) Atualização do botão Voltar do NavigationView

![Exemplo](../images/navigationview-back-button.gif)

##### <a name="description"></a>Descrição

No modo mínimo do NavigationView, o botão Voltar não desaparece mais. Ao abrir e fechar o painel, os usuários não precisam mais mover o cursor para clicar no botão de hambúrguer. Esse recurso funcionará por padrão. Não é necessário alterar o código para que ele funcione.

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView – Sem preenchimento automático

![Exemplo](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>Descrição

Os desenvolvedores de aplicativos agora podem recuperar todos os pixels dentro de sua janela de aplicativo usando o controle NavigationView e se estendendo para a área da barra de título.

##### <a name="documentation"></a>Documentação

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>Atualizações de estilo visual

##### <a name="a-corner-radius-update"></a>a) Atualização do raio de canto

![Exemplo](../images/corner-radius.png)

##### <a name="description"></a>Descrição

O atributo CornerRadius foi adicionado. Os controles padrão foram atualizados para usar cantos ligeiramente arredondados. Os desenvolvedores podem personalizar facilmente o raio de canto para dar ao aplicativo uma aparência única, se desejado.

##### <a name="github-spec-link"></a>Link de especificação do GitHub

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) Atualização da espessura da borda

![Exemplo](../images/border-thickness.png)

##### <a name="description"></a>Descrição

Agora é mais fácil personalizar a propriedade BorderThickness. Os controles padrão foram atualizados para reduzir os contornos para serem mais finos e proporcionar uma aparência mais limpa e familiar.

##### <a name="github-spec-link"></a>Link de especificação do GitHub

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) Atualização do visual do botão

![Exemplo](../images/button-hover-visual-update.png)

##### <a name="description"></a>Descrição: 
O visual do botão padrão foi atualizado para remover a estrutura de tópicos exibida durante o foco para dar uma aparência mais limpa.

##### <a name="github-spec-link"></a>Link de especificação do GitHub:  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>d) Atualização visual do SplitButton

![Exemplo](../images/splitbutton-visual-update.png)

##### <a name="description"></a>Descrição: 
O visual do SplitButton padrão foi atualizado para torná-lo mais distinto de DropDownButton.

##### <a name="github-spec-link"></a>Link de especificação do GitHub: 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) Atualização visual do ToggleSwitch

![Exemplo](../images/toggleswitch-update.png)

##### <a name="description"></a>Descrição: 
A largura padrão de ToggleSwitch foi reduzida de 44 px para 40 px, de modo que o visual seja equilibrado e mantenha a usabilidade.

##### <a name="github-spec-link"></a>Link de especificação do GitHub: 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) Atualização visual de CheckBox e RadioButton

![Exemplo](../images/checkbox-radiobutton.png)

##### <a name="description"></a>Descrição: 
O visual de CheckBox e RadioButton foi atualizado para manter a consistência com o restante da alteração de estilo visual.

##### <a name="github-spec-link"></a>Link de especificação do GitHub: 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>Exemplos

O aplicativo de exemplo da Galeria de Controles XAML inclui demonstrações interativas e código de exemplo para usar controles WinUI.

* Instale o aplicativo da Galeria de Controles XAML da [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

* A Galeria de Controles XAML também é de [software livre no GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Documentação

Artigos de instruções sobre os controles da Biblioteca de Interface do Usuário do Windows estão incluídos com a [Documentação de controles da Plataforma Universal do Windows](/windows/uwp/design/controls-and-patterns/).

Os documentos da referência de API estão localizados aqui: [APIs da Biblioteca de Interface do Usuário do Windows](/uwp/api/overview/winui/).

## <a name="microsoftuixaml-22-version-history"></a>Histórico de versão do Microsoft.UI.Xaml 2.2

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-prerelease

Julho de 2019

[Página de versão do GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[Download do pacote NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>Recurso experimental

* [TabView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-prerelease

Abril de 2019

[Página de versão do GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[Download do pacote NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>Recursos experimentais

* [FlowLayout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.scrollviewer)
