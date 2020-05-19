---
title: Notas sobre a versão do WinUI 2.1
description: Notas sobre a versão do WinUI 2.1, incluindo novos recursos e correções de bugs.
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: f62dd912d76f8045a46d467f49167cdc426b75c2
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580613"
---
# <a name="windows-ui-library-21"></a>Biblioteca de Interface do Usuário do Windows 2.1

A versão oficial mais recente da Biblioteca de Interface do Usuário do Windows (WinUI 2.1) foi lançada em 8 de abril de 2019. 

O WinUI oferece muitos dos recursos mais recentes da plataforma da Windows UX, incluindo controles e estilos Fluent atualizados, imediatamente compatível com versões anteriores à Atualização de Aniversário do Windows 10 (14393). A [Galeria de Controles XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery) conta com exemplos para você explorar todos os novos recursos legais adicionados à biblioteca.

Baixar o [pacote NuGet do WinUI 2.1](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

É possível usar os pacotes do WinUI no aplicativo por meio do Gerenciador de Pacotes NuGet: confira [Introdução à Biblioteca de Interface do Usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/getting-started) para saber mais.

O WinUI é um projeto de software livre hospedado no GitHub. Relatórios de bugs, solicitações de recursos e contribuições de código da comunidade são bem-vindos no [Repositório da Biblioteca de Interface do Usuário do Windows](https://aka.ms/winui).

## <a name="whats-new-in-this-release"></a>Novidades desta versão

### <a name="itemsrepeater"></a>ItemsRepeater

Use um ItemsRepeater para criar experiências de coleção personalizadas usando um sistema de layout flexível, exibições personalizadas e virtualização.
Diferentemente de ListView, ItemsRepeater não proporciona uma experiência do usuário final abrangente: ele não tem interface do usuário padrão nem fornece políticas relacionada a foco, seleção ou interação do usuário. Em vez disso, ele é um bloco de construção que você pode usar para criar suas próprias experiências baseadas em coleção e controles personalizados exclusivos. É compatível com a criação de experiências mais ricas e de alto desempenho.

![Exemplo](../images/ItemsRepeater%20-%20MSN%20News.gif)

[Documentação](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

O AnimatedVisualPlayer hospeda e controla a reprodução de visuais animados, permitindo que você adicione gráficos de movimento personalizados de alto desempenho ao aplicativo. Por exemplo, o AnimatedVisualPlayer é usado para exibir e controlar animações Lottie.

![Exemplo](../images/AnimatedVisualPlayerUpdated.gif)

[Documentação](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

O TeachingTip é uma maneira atraente e Fluent dos aplicativos para orientar e informar os usuários com dicas não invasivas e ricas em conteúdo. O TeachingTip pode ressaltar recursos novos ou importantes, ensinar aos usuários como executar tarefas e aprimorar o fluxo de trabalho fornecendo informações contextualmente relevantes para a tarefa em questão.

![Exemplo](../images/TeachingTipUpdated.gif)

[Documentação](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

Inclui a capacidade de mostrar as opção do estilo 'Radio Button' em um MenuBar. Isso permite grupos de opções com marcadores que são vinculados como um grupo de botões de opção. A lógica é processada para o desenvolvedor.

![Exemplo](../images/RadioMenuFlyoutItem1.png)

[Documentação](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

O Modo compacto permite aos desenvolvedores criar experiências confortáveis para diversos cenários. Com a simples inclusão de um dicionário de recursos, o aplicativo pode conter aproximadamente 33% mais interface do usuário.

![Exemplo de densidade compactada](../images/CompactDensityUpdated.png)

[Documentação](https://docs.microsoft.com/windows/uwp/design/style/spacing )

### <a name="shadows"></a>Sombras

![Exemplo](../images/shadow.gif)

Criar uma hierarquia visual de elementos torna a interface do usuário fácil de examinar e definir o que é mais importante para se concentrar. A elevação, o ato de trazer os elementos selecionados da interface do usuário para a frente, é geralmente usada para atingir tal hierarquia no software. 

Com a Atualização de maio de 2019 para o Windows 10, muitos dos nossos controles comuns adicionaram a elevação usando a profundidade z e a sombra por padrão. Os controles NavigationView e TeachingTip no WinUI 2.1 também têm sombras padrão quando executados em um sistema operacional com a Atualização de maio de 2019 para o Windows 10. A lista completa de controles que têm sombras padrão e como usar APIs adicionais estará disponível depois que a Atualização de maio de 2019 para o Windows 10 for liberada e o link será postado aqui.

## <a name="examples"></a>Exemplos

O aplicativo de exemplo da Galeria de Controles XAML inclui demonstrações interativas e código de exemplo para usar controles WinUI.

* Instale o aplicativo da Galeria de Controles XAML da [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

* A Galeria de Controles XAML também é de [software livre no GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Documentação

Artigos de instruções sobre os controles da Biblioteca de Interface do Usuário do Windows estão incluídos com a [Documentação de controles da Plataforma Universal do Windows](/windows/uwp/design/controls-and-patterns/).

Os documentos da referência de API estão localizados aqui: [APIs da Biblioteca de Interface do Usuário do Windows](/uwp/api/overview/winui/).

## <a name="microsoftuixaml-21-version-history"></a>Histórico de versão do Microsoft.UI.Xaml 2.1

### <a name="microsoftuixaml-21-official-release"></a>Versão oficial do Microsoft.UI.Xaml 2.1

Abril de 2019

[Página de versão do GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[Download do pacote NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>Novo recurso (não incluído em versões anteriores)

* [CompactDensity](https://docs.microsoft.com/windows/uwp/design/style/spacing): O Modo compacto permite aos desenvolvedores criar experiências confortáveis para diversos cenários. Com a simples inclusão de um dicionário de recursos, o aplicativo pode conter aproximadamente 33% mais interface do usuário.

* Sombras: Criar uma hierarquia visual de elementos torna a interface do usuário fácil de examinar e definir o que é mais importante para se concentrar. A elevação, o ato de trazer os elementos selecionados da interface do usuário para a frente, é geralmente usada para atingir tal hierarquia no software. Muitos dos nossos controles comuns adicionaram a elevação usando a profundidade z e a sombra por padrão.  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-prerelease

Fevereiro de 2019

[Página de versão do GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[Download do pacote NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

Novos recursos experimentais:

* [Controle TeachingTip](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  O novo controle é uma maneira de o aplicativo orientar e informar os usuários com notificações não invasivas e conteúdo avançado. O TeachingTip pode ser usado para ressaltar um recurso novo ou importante, ensinar os usuários a executar uma tarefa ou aprimorar o fluxo de trabalho do usuário fornecendo informações contextualmente relevantes à tarefa em questão.

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-prerelease

Fevereiro de 2019

[Página de versão do GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[Download do pacote NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

Novos recursos experimentais:

* [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  O novo controle permite reproduzir animações de vetor de alto desempenho, incluindo animações [Lottie](https://github.com/airbnb/lottie) criadas com o [Lottie-Windows](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie).

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-prerelease

Dezembro de 2018

[Página de versão do GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[Download do pacote NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

Novos recursos experimentais:

* [ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [RadioButtons](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)
