---
description: Veja diretrizes de design e instruções de codificação para adicionar controles &amp; padrões ao aplicativo do Windows. Encontre mais de 45 controles poderosos para uso com o seu aplicativo.
title: Controles e padrões do Windows – Desenvolvimento de aplicativos do Windows
keywords: controles da UWP, interface do usuário, controles de aplicativo, controles do Windows
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 82e6d91b5a4147237cd04b46dc5b69b8151fece9
ms.sourcegitcommit: 6dd6d61c912daab2cc4defe5ba0cf717339f7765
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978378"
---
# <a name="controls-for-windows-apps"></a>Controles para aplicativos do Windows

![Controles](../images/controls-2x.png)

No desenvolvimento de aplicativos do Windows, um <i>controle</i> é um elemento de interface do usuário que exibe conteúdo ou permite interação. Os controles são os blocos de construção da interface do usuário. Um <i>padrão</i> é a receita para combinar vários controles para criar algo novo.

Fornecemos mais de 45 controles para você usar, desde botões simples a controles de dados avançados, como o modo de exibição de grade.  Esses controles fazem parte do Sistema de Design Fluent e podem ajudá-lo a criar uma interface do usuário ousada e escalonável com uma ótima aparência em todos os dispositivos e tamanhos de tela.

Os artigos desta seção fornecem orientações de design e instruções de codificação para adicionar controles e padrões ao seu aplicativo do Windows.

## <a name="intro"></a>Introdução

Instruções gerais e exemplos de código para adicionar e estilizar controles em XAML e C#.

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">Adicionar controles e manipular eventos</a></b> <br/>
Há 3 etapas principais para adicionar controles ao seu aplicativo: Adicione um controle ao seu aplicativo da interface do usuário, defina as propriedades no controle e adicione código aos manipuladores de eventos do controle para que ele faça algo.</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">Aplicando estilos a controles</a></b> <br/>
É possível personalizar a aparência de seus aplicativos de muitas formas usando a estrutura XAML. Os estilos permitem definir propriedades de controle e reutilizar essas configurações para criar uma aparência consistente em vários controles.</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>Obter a biblioteca de interface do usuário do Windows

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | Alguns controles estão disponíveis somente na WinUI (Biblioteca de interface do usuário do Windows), um pacote NuGet que contém novos controles e recursos de interface do usuário. Para obtê-la, veja [Visão geral e instruções de instalação da Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/).<br/>Do WinUI 2.2 em diante, o estilo padrão para muitos controles foi atualizado para usar cantos arredondados. Para obter mais informações, confira [Raio de canto](/windows/uwp/design/style/rounded-corner). |

## <a name="alphabetical-index"></a>Índice alfabético

Informações detalhadas sobre controles e padrões específicos. (Para obter uma lista classificada por função, consulte [índice dos controles por função](controls-by-function.md).)

:::row:::
    :::column:::

- Player visual animado (confira [Lottie](/windows/communitytoolkit/animations/lottie)) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Caixa de sugestão automática](auto-suggest-box.md)
- [Botão](buttons.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Exibição de calendário](calendar-view.md)
- [Caixa de seleção](checkbox.md)
- [Seletor de cor](color-picker.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Caixa de combinação](combo-box.md)
- [Barra de comandos](app-bars.md)
- [Submenu da barra de comandos](command-bar-flyout.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Cartão de visita](contact-card.md)
- [Caixa de diálogo de conteúdo](dialogs-and-flyouts/dialogs.md)
- [Link de conteúdo](content-links.md)
- [Menu de contexto](menus.md)
- [Seletor de data](date-picker.md)
- [Caixas de diálogo e submenus](dialogs-and-flyouts/index.md)
- [Botão suspenso](buttons.md#create-a-drop-down-button) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Exibição de inversão](flipview.md)
- [Submenu](dialogs-and-flyouts/flyouts.md)
- [Formulários](forms.md) (padrão)
- [Exibição de grade](listview-and-gridview.md)
- [Hiperlink](hyperlinks.md)
- [Botão de hiperlink](hyperlinks.md#create-a-hyperlinkbutton)
- [Imagens e pincéis de imagem](images-imagebrushes.md)
- [Controles de escrita à tinta](inking-controls.md)
- [Modo de exibição de lista](listview-and-gridview.md)
- [Controle de mapeamento](../../maps-and-location/controls-map.md)
- [Mestre/detalhes](master-details.md) (padrão)
- [Reprodução de mídia](media-playback.md)
- [Barra de menus](menus.md#create-a-menu-bar) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Submenu de menu](menus.md)
- [Exibição de navegação](navigationview.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)

    :::column-end:::
    :::column:::

- [Caixa de número](number-box.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Exibição de paralaxe](..\motion\parallax.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Caixa de senha](password-box.md)
- [Imagem da pessoa](person-picture.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Pivô](pivot.md)
- [Barra de progresso](progress-controls.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Anel de progresso](progress-controls.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Botão de opção](radio-button.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Controle de classificação](rating.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Botão de repetição](buttons.md#create-a-repeat-button)
- [Caixa de edição avançada](rich-edit-box.md)
- [Bloco rich text](rich-text-block.md)
- [Visualizador de rolagem](scroll-controls.md)
- [Pesquisar](search.md) (padrão)
- [Zoom semântico](semantic-zoom.md)
- [Formas](shapes.md)
- [Controle deslizante](slider.md)
- [Botão de divisão](buttons.md#create-a-split-button) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Modo divisão](split-view.md)
- [Controle de passar o dedo](swipe.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Exibição de guia](tab-view.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Dica informativa](dialogs-and-flyouts/teaching-tip.md) ![logotipo do WinUI](images/winui-logo-16x16.png)
- [Bloco de texto](text-block.md)
- [Caixa de texto](text-box.md)
- [Seletor de hora](time-picker.md)
- [Botão de alternância](toggles.md)
- [Botão de alternância](buttons.md)
- [Botão de alternância de divisão](buttons.md#create-a-toggle-split-button)
- [Dicas de ferramentas](tooltips.md)
- [Modo de exibição de árvore](tree-view.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Exibição de dois painéis](two-pane-view.md) ![Logotipo do WinUI](images/winui-logo-16x16.png)
- [Modo de exibição da Web](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>XAML Controls Gallery

Obtenha o _XAML Controls Gallery_ aplicativo da Microsoft Store para ver esses controles e o sistema de Design Fluente em ação. O aplicativo é um complemento interativo para este site. Quando você tiver instalado, você pode usar os links em páginas de controle individuais para iniciar o aplicativo e ver o controle em ação.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>Controles adicionais

Controles adicionais para o desenvolvimento no Windows são disponibilizados por empresas como <a href="https://www.telerik.com/">Telerik</a>, <a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>, <a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>, <a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>, <a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> e <a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a>. Esses controles fornecem suporte adicional para empresas e desenvolvedores .NET aumentando os controles padrão do sistema com os serviços e controles personalizados.
