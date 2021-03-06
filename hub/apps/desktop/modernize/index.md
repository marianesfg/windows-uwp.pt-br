---
Description: Adicione interfaces de usuário modernas do XAML, crie pacotes MSIX e incorpore outros componentes modernos em seu aplicativo da área de trabalho.
title: Modernize seus aplicativos da área de trabalho para Windows
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 31d7805e7ae936e5c7427b54f2eb9b0ad4b4c3e9
ms.sourcegitcommit: 37e4af3ba203295c7e88448414cf7ea537ab5402
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84257377"
---
# <a name="modernize-your-desktop-apps"></a>Modernize seus aplicativos da área de trabalho

O Windows 10 e a UWP (Plataforma Universal do Windows) oferecem muitos recursos que você pode usar para fornecer uma experiência moderna em seus aplicativos da área de trabalho. A maioria desses recursos está disponível como componentes modulares que você pode adotar em seus aplicativos da área de trabalho em seu próprio ritmo, sem reescrever seu aplicativo para uma plataforma diferente. Você pode aprimorar seus aplicativos da área de trabalho existentes, escolhendo quais partes do Windows 10 e da UWP adotar.

Este artigo descreve os recursos do Windows 10 e da UWP que você pode usar em seus aplicativos da área de trabalho atualmente. Para ver um tutorial que demonstra como modernizar um aplicativo existente a fim de usar muitos dos recursos descritos neste artigo, consulte o tutorial [Modernizar um aplicativo WPF](modernize-wpf-tutorial.md).

> [!NOTE]
> Você precisa de assistência para migrar os aplicativos da área de trabalho para o Windows 10? O serviço [Garantia de Aplicativo da Área de Trabalho](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure) fornece suporte direto e sem custos para os desenvolvedores que estão compatibilizando seus aplicativos com o Windows 10. Esse programa está disponível para todos os ISVs e empresas qualificadas. Para obter mais detalhes sobre qualificação e sobre o programa propriamente dito, visite [https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered](https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered). Para começar agora mesmo, [envie sua solicitação](https://fasttrack.microsoft.com/dl/daa).

## <a name="windows-ui-library"></a>Biblioteca de Interface do Usuário do Windows

A Biblioteca de Interface do Usuário do Windows é um conjunto de pacotes NuGet que fornecem controles e outros elementos de interface do usuário para aplicativos do Windows 10. O WinUI começou como um kit de ferramentas que fornecia versões novas e atualizadas de controles da UWP para aplicativos UWP voltados para versões de nível inferior do Windows 10. O WinUI cresceu em escopo e agora é a plataforma moderna de interface do usuário nativa para aplicativos do Windows 10 que usam a UWP, o .NET e o Win32.

Você pode usar o WinUI das seguintes maneiras em aplicativos da área de trabalho:

* Você pode atualizar os aplicativos existentes do WPF, Windows Forms e C++ /Win32 para usar [Ilhas XAML](xaml-islands.md) a fim de hospedar controles do WinUI 2. x nos aplicativos.
* Começando com o [WinUI 3.0 Versão prévia 1](../../winui/winui3/index.md), você pode criar [Aplicativos .NET e C++/Win32 que usam uma interface do usuário totalmente baseada no WinUI](../../winui/winui3/get-started-winui3-for-desktop.md).

Confira [Biblioteca de Interface do Usuário do Windows (WinUI)](../../winui/index.md).

## <a name="msix-packages"></a>Pacotes de MSIX

MSIX é um formato de pacote do aplicativo do Windows moderno, que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows, incluindo aplicativos UWP, WPF, Windows Forms e Win32. O MSIX reúne os melhores aspectos das tecnologias de instalação MSI, .appx, App-V e ClickOnce para oferecer uma experiência de empacotamento moderna e confiável.

Empacotar seus aplicativos da área de trabalho do Windows em pacotes MSIX fornece a você acesso a uma experiência robusta de instalação e atualização, um modelo de segurança gerenciado com um sistema de capacidade flexível, suporte à Microsoft Store, gerenciamento empresarial e muitos modelos de distribuição personalizados.

Para obter mais informações, consulte [empacotar aplicativos da área de trabalho](/windows/msix/desktop/desktop-to-uwp-root) na documentação do MSIX.

## <a name="net-core-3"></a>.NET Core 3

O .NET Core 3 é a versão principal mais recente do .NET Core. O destaque dessa versão é o suporte para aplicativos da área de trabalho do Windows, incluindo aplicativos Windows Forms e WPF. Você pode executar aplicativos da área de trabalho do Windows novos e existentes no .NET Core 3 e aproveitar todos os benefícios que o .NET Core tem a oferecer. Controles da UWP que são hospedados nas [ilhas de XAML](xaml-islands.md) também podem ser usados em aplicativos do Windows Forms e WPF destinados ao .NET Core 3.

Para obter mais informações, consulte [Novidades do .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).

## <a name="windows-runtime-apis"></a>APIs do Windows Runtime

É possível chamar várias APIs do Windows Runtime diretamente no seu aplicativo da área de trabalho Win32 do WPF, do Windows Forms ou do C++ para integrar experiências modernas interessantes para usuários do Windows 10. Por exemplo, você pode chamar APIs do Windows Runtime para adicionar notificações do sistema ao seu aplicativo da área de trabalho.

Para saber mais, confira [Usar APIs do Windows Runtime em aplicativos de área de trabalho](desktop-to-uwp-enhance.md).

## <a name="host-uwp-controls-xaml-islands"></a>Controles de host UWP (ilhas de XAML)

Começando com o Windows 10 versão 1903, você pode adicionar [controles XAML UWP](/windows/uwp/design/controls-and-patterns/controls-by-function) diretamente para qualquer elemento de interface do usuário em um aplicativo Win32, WPF, Windows Forms ou C++ que esteja associado com um identificador de janela (HWND). Isso significa que você pode integrar totalmente os recursos mais recentes da UWP, tais como o [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) e os controles que dão suporte ao [Sistema de Design Fluente](/windows/uwp/design/fluent-design-system/index), com janelas e outras superfícies de exibição em seus aplicativos da área de trabalho. Às vezes, esse cenário de desenvolvedor é chamado de *ilhas de XAML*.

Para obter mais informações, consulte [Controles UWP em aplicativos da área de trabalho](xaml-islands.md)

## <a name="use-the-visual-layer-in-desktop-apps"></a>Usar a camada Visual em aplicativos da área de trabalho

Agora é possível usar as APIs do Windows Runtime em aplicativos de área de trabalho não UWP para aprimorar a aparência e a funcionalidade de seus aplicativos Win32 do WPF, do Windows Forms e do C++, além de tirar proveito dos recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio da UWP. Isso é útil quando você precisa criar experiências personalizadas que vão além dos controles internos da UWP que você pode hospedar usando ilhas de XAML.

Para obter mais informações, consulte [Modernize seu aplicativo da área de trabalho usando a camada Visual](visual-layer-in-desktop-apps.md).

## <a name="additional-features-available-to-apps-with-package-identity"></a>Recursos adicionais disponíveis para aplicativos com identidade de pacote

Algumas experiências modernas do Windows 10 estão disponíveis apenas em aplicativos da área de trabalho com [identidade de pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Esses recursos incluem certas APIs do Windows Runtime, extensões de pacote e componentes UWP. Para obter mais informações, consulte [recursos que exigem a identidade do pacote](modernize-packaged-apps.md).

Há várias maneiras de conceder identidade a um aplicativo da área de trabalho:

* Empacote-o em um [pacote MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX é um formato moderno de pacote de aplicativo que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows, como aplicativos WPF, Windows Forms e Win32. Ele fornece a você acesso a uma experiência robusta de instalação e atualização, um modelo de segurança gerenciado com um sistema de capacidade flexível, suporte à Microsoft Store, gerenciamento corporativo e muitos modelos de distribuição personalizados. Para obter mais informações, consulte [empacotar aplicativos da área de trabalho](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) na documentação do MSIX.
* Se você não puder adotar o empacotamento MSIX para implantar seu aplicativo da área de trabalho, no Windows 10, versão 2004 em diante, conceda o identificador de pacote criando um *pacote MSIX esparso* que contenha apenas um manifesto do pacote. Para obter mais informações, consulte [Conceder identidade a aplicativos da área de trabalho não empacotados](grant-identity-to-nonpackaged-apps.md).

<a id="desktop-uwp-controls"></a>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>Controles UWP otimizados para aplicativos de área de trabalho

Esteja você criando um aplicativo UWP destinado exclusivamente à família de dispositivos de desktop ou desejando usar controles da UWP em um aplicativo da área de trabalho Win32 WPF, Windows Forms, ou C++, os seguintes controles da UWP novos e atualizados são projetados para oferecer experiências de otimização de área de trabalho com o [Sistema de Design Fluente](/windows/uwp/design/fluent-design-system/index). Esses controles foram introduzidos no Windows 10, versão 1809 (atualização de outubro de 2018 ou versão 10.0.17763).

| Control |  Descrição |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | Fornece uma maneira rápida e simples de expor um conjunto de comandos para aplicativos que podem precisar de mais agrupamento ou organização do que uma **CommandBar** permite. |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | Mostra uma divisa como um indicador visual de que ele tem um submenu anexado que contém mais opções.  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | Fornece que um botão com duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão padrão e invoca uma ação imediata. A outra parte invoca um submenu que contém opções adicionais dentre as quais o usuário pode escolher.|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | Fornece que um botão com duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão de alternância que pode ser ativado ou desativado. A outra parte invoca um submenu que contém opções adicionais dentre as quais o usuário pode escolher. |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  Permite que você mostre tarefas de usuário comuns no contexto de um item na tela da interface do usuário. |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | Agora você pode tornar uma caixa de combinação editável para que o usuário possa inserir valores que não estão listados no controle.  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | Agora você pode configurar um modo de exibição de árvore para habilitar associação de dados, modelos de item e funcionalidade de arrastar e soltar.  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   Oferece uma maneira flexível de exibir um conjunto de dados em linhas e colunas. Esse controle está disponível no [Kit de Ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).  |

## <a name="other-technologies-for-modern-desktop-apps"></a>Outras tecnologias para aplicativos modernos de área de trabalho

### <a name="microsoft-graph"></a>Microsoft Graph

O Microsoft Graph é uma coleção de APIs que você pode usar para criar aplicativos para empresas e consumidores que interagem com os dados de milhões de usuários. O Microsoft Graph expõe APIs REST e bibliotecas de cliente para acessar dados no seguinte:
* Active Directory do Azure
* Serviços do Office 365: SharePoint, OneDrive, Outlook/Exchange, Microsoft Teams, OneNote, Planner e Excel
* Serviços de segurança e mobilidade corporativa: Identity Manager, Intune, Advanced Threat Analytics e Proteção Avançada Contra Ameaças.
* Serviços do Windows 10: atividades e dispositivos

Para obter mais informações, consulte os [docs do Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/overview).

### <a name="adaptive-cards"></a>Cartões Adaptáveis

Os Cartões Adaptáveis são uma estrutura aberta multiplataforma que você pode usar para trocar conteúdo de interface do usuário com base em cartão de uma maneira comum e consistente entre dispositivos e plataformas.

Para obter mais informações, consulte os [docs dos Cartões Adaptáveis](https://docs.microsoft.com/adaptive-cards/).
