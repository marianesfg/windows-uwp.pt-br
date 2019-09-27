---
Description: Adicione interfaces de usuário modernas do XAML, crie pacotes MSIX e incorpore outros componentes modernos em seu aplicativo da área de trabalho.
title: Modernize seus aplicativos da área de trabalho para Windows
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: b8ad9726397671bcb2b641d6769f014721a27a72
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317093"
---
# <a name="modernize-your-desktop-apps"></a>Modernize seus aplicativos da área de trabalho

O Windows 10 e a UWP (Plataforma Universal do Windows) oferecem muitos recursos que você pode usar para fornecer uma experiência moderna em seus aplicativos da área de trabalho. A maioria desses recursos está disponível como componentes modulares que você pode adotar em seus aplicativos da área de trabalho em seu próprio ritmo, sem reescrever seu aplicativo para uma plataforma diferente. Você pode aprimorar seus aplicativos da área de trabalho existentes, escolhendo quais partes do Windows 10 e da UWP adotar.

Este artigo descreve os recursos do Windows 10 e da UWP que você pode usar em seus aplicativos da área de trabalho atualmente. Para ver um tutorial que demonstra como modernizar um aplicativo existente a fim de usar muitos dos recursos descritos neste artigo, consulte o tutorial [Modernizar um aplicativo WPF](modernize-wpf-tutorial.md).

> [!NOTE]
> Você precisa de assistência para migrar os aplicativos da área de trabalho para o Windows 10? O serviço [Garantia de Aplicativo da Área de Trabalho](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure) fornece suporte direto e sem custos para os desenvolvedores que estão compatibilizando seus aplicativos com o Windows 10. Esse programa está disponível para todos os ISVs e empresas qualificadas. Para obter mais detalhes sobre qualificação e sobre o programa propriamente dito, visite [https://aka.ms/DesktopAppAssure](https://aka.ms/DesktopAppAssure). Para começar agora mesmo, [envie sua solicitação](https://aka.ms/DesktopAppAssureRequest).

## <a name="msix-packages"></a>Pacotes de MSIX

MSIX é um formato de pacote do aplicativo do Windows moderno, que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows, incluindo aplicativos UWP, WPF, Windows Forms e Win32. O MSIX reúne os melhores aspectos das tecnologias de instalação MSI, .appx, App-V e ClickOnce e é criado para ser seguro, protegido e confiável.

Empacotar seus aplicativos da área de trabalho do Windows em pacotes MSIX fornece a você acesso a uma experiência robusta de instalação e atualização, um modelo de segurança gerenciado com um sistema de capacidade flexível, suporte à Microsoft Store, gerenciamento empresarial e muitos modelos de distribuição personalizados.

Para obter mais informações, consulte [empacotar aplicativos da área de trabalho](/windows/msix/desktop/desktop-to-uwp-root) na documentação do MSIX.

## <a name="net-core-3"></a>.NET Core 3

O .NET Core 3 é a versão principal mais recente do .NET Core. O destaque dessa versão é o suporte para aplicativos da área de trabalho do Windows, incluindo aplicativos Windows Forms e WPF. Você pode executar aplicativos da área de trabalho do Windows novos e existentes no .NET Core 3 e aproveitar todos os benefícios que o .NET Core tem a oferecer. Controles da UWP que são hospedados nas [ilhas de XAML](xaml-islands.md) também podem ser usados em aplicativos do Windows Forms e WPF destinados ao .NET Core 3.

Para obter mais informações, consulte [Novidades do .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).

## <a name="uwp-apis"></a>APIs UWP

Você pode chamar várias APIs UWP diretamente no seu aplicativo da área de trabalho Win32 WPF, Windows Forms ou C++ para integrar experiências modernas interessantes para usuários do Windows 10. Por exemplo, você pode chamar APIs UWP para adicionar notificações do sistema ao seu aplicativo da área de trabalho.

Para obter mais informações, consulte [Usar APIs UWP em aplicativos da área de trabalho](desktop-to-uwp-enhance.md).

## <a name="host-uwp-controls-xaml-islands"></a>Controles de host UWP (ilhas de XAML)

Começando com o Windows 10 versão 1903, você pode adicionar [controles XAML UWP](/windows/uwp/design/controls-and-patterns/controls-by-function) diretamente para qualquer elemento de interface do usuário em um aplicativo Win32, WPF, Windows Forms ou C++ que esteja associado com um identificador de janela (HWND). Isso significa que você pode integrar totalmente os recursos mais recentes da UWP, tais como o [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) e os controles que dão suporte ao [Sistema de Design Fluente](/windows/uwp/design/fluent-design-system/index), com janelas e outras superfícies de exibição em seus aplicativos da área de trabalho. Às vezes, esse cenário de desenvolvedor é chamado de *ilhas de XAML*.

Para obter mais informações, consulte [Controles UWP em aplicativos da área de trabalho](xaml-islands.md)

## <a name="use-the-visual-layer-in-desktop-apps"></a>Usar a camada Visual em aplicativos da área de trabalho

Agora você pode usar as APIs UWP em aplicativos de área de trabalho não UWP para aprimorar a aparência, a funcionalidade de seus aplicativos Win32, WPF, Windows Forms e C++, além de tirar proveito dos recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio da UWP. Isso é útil quando você precisa criar experiências personalizadas que vão além dos controles internos da UWP que você pode hospedar usando ilhas de XAML.

Para obter mais informações, consulte [Modernize seu aplicativo da área de trabalho usando a camada Visual](visual-layer-in-desktop-apps.md).

## <a name="additional-features-available-to-packaged-apps"></a>Recursos adicionais disponíveis para aplicativos empacotados

Algumas experiências modernas do Windows 10 estão disponíveis apenas em aplicativos da área de trabalho que são empacotados em um [pacote MSIX](/windows/msix/desktop/desktop-to-uwp-root). Se empacotar seu aplicativo da área de trabalho em um pacote MSIX, você poderá usar APIs UWP que exigem a identidade do pacote, extensões do pacote e componentes da UWP em seu aplicativo empacotado.

Para obter mais informações, consulte [recursos que exigem a identidade do pacote](modernize-packaged-apps.md).

<a id="desktop-uwp-controls"/>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>Controles UWP otimizados para aplicativos de área de trabalho

Esteja você criando um aplicativo UWP destinado exclusivamente à família de dispositivos de desktop ou desejando usar controles da UWP em um aplicativo da área de trabalho Win32 WPF, Windows Forms, ou C++, os seguintes controles da UWP novos e atualizados são projetados para oferecer experiências de otimização de área de trabalho com o [Sistema de Design Fluente](/windows/uwp/design/fluent-design-system/index). Esses controles foram introduzidos no Windows 10, versão 1809 (atualização de outubro de 2018 ou versão 10.0.17763).

| Controle |  Descrição |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | Fornece uma maneira rápida e simples de expor um conjunto de comandos para aplicativos que podem precisar de mais agrupamento ou organização do que uma **CommandBar** permite. |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | Mostra uma divisa como um indicador visual de que ele tem um submenu anexado que contém mais opções.  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | Fornece que um botão com duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão padrão e invoca uma ação imediata. A outra parte invoca um submenu que contém opções adicionais dentre as quais o usuário pode escolher.|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | Fornece que um botão com duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão de alternância que pode ser ativado ou desativado. A outra parte invoca um submenu que contém opções adicionais dentre as quais o usuário pode escolher. |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  Permite que você mostre tarefas de usuário comuns no contexto de um item na tela da interface do usuário. |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | Agora você pode tornar uma caixa de combinação editável para que o usuário possa inserir valores que não estão listados no controle.  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | Agora você pode configurar um modo de exibição de árvore para habilitar associação de dados, modelos de item e funcionalidade de arrastar e soltar.  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   Oferece uma maneira flexível de exibir um conjunto de dados em linhas e colunas. Esse controle está disponível no [Kit de Ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).  |

## <a name="windows-ui-library"></a>Biblioteca de Interface do Usuário do Windows

A biblioteca de interface do usuário do Windows é um conjunto de pacotes NuGet que fornecem novos controles e outros elementos de interface do usuário para aplicativos UWP. As APIs da biblioteca de interface do usuário do Windows funcionam em versões anteriores do Windows 10, de modo que seu aplicativo funcione mesmo se os usuários não estiverem executando a versão mais recente do Windows 10. Isso permite a você adotar novos controles conforme eles são lançados na biblioteca de interface do usuário do Windows sem precisar se preocupar sobre incluir verificações de versão ou XAML condicional.

Veja a [Biblioteca de Interface do Usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="other-technologies-for-modern-desktop-apps"></a>Outras tecnologias para aplicativos modernos de área de trabalho

### <a name="microsoft-graph"></a>Microsoft Graph

O Microsoft Graph é uma coleção de APIs que você pode usar para criar aplicativos para empresas e consumidores que interagem com os dados de milhões de usuários. O Microsoft Graph expõe APIs REST e bibliotecas de cliente para acessar dados no seguinte:
* Azure Active Directory
* Serviços do Office 365: SharePoint, OneDrive, Outlook/Exchange, Microsoft Teams, OneNote, Planner e Excel
* Serviços de segurança e mobilidade corporativa: Identity Manager, Intune, Advanced Threat Analytics e Proteção Avançada Contra Ameaças.
* Serviços do Windows 10: atividades e dispositivos

Para obter mais informações, consulte os [docs do Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/overview).

### <a name="adaptive-cards"></a>Cartões Adaptáveis

Os Cartões Adaptáveis são uma estrutura aberta multiplataforma que você pode usar para trocar conteúdo de interface do usuário com base em cartão de uma maneira comum e consistente entre dispositivos e plataformas.

Para obter mais informações, consulte os [docs dos Cartões Adaptáveis](https://docs.microsoft.com/adaptive-cards/).
