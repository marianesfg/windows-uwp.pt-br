---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Noções básicas de design de comando para apps da Plataforma Universal do Windows (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09f775ad0ba596379b6d3ddf158285849520111f
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842563"
---
#  <a name="command-design-basics-for-uwp-apps"></a>Noções básicas de design de comandos para apps UWP

Em um aplicativo da Plataforma Universal do Windows (UWP), *elementos de comando* são os elementos interativos da interface do usuário que permitem aos usuários executar ações, como enviar um email, excluir um item ou enviar um formulário. Este artigo descreve os elementos de comando comuns, as interações a que eles dão suporte e as superfícies de comando para hospedá-los.

## <a name="provide-the-right-type-of-interactions"></a>Forneça o tipo correto de interações

Ao projetar uma interface de comando, a decisão mais importante é escolher quais ações os usuários podem executar. Para planejar o tipo correto de interações, concentre-se em seu aplicativo - considere as experiências do usuário que você deseja habilitar e o que os usuários precisarão fazer. Quando decidir o que você quer que os usuários realizem, então você pode fornecer ferramentas para que eles possam fazê-lo.

Algumas interações que talvez seja bom fornecer a seu aplicativo incluem:

- Enviar informações 
- Selecionar configurações e opções
- Conteúdo de filtro e pesquisa
- Abrir, salvar e excluir arquivos
- Edição ou a criação de conteúdo

## <a name="use-the-right-command-element-for-the-interaction"></a>Use o elemento de comando correto para a interação

Usar os elementos certos para permitir as interações de comando pode fazer a diferença entre um aplicativo que parece intuitivo e fácil de usar e um que parece difícil ou confuso. A Plataforma Universal do Windows (UWP) fornece um grande conjunto de elementos de comando que você pode usar em seu aplicativo. Aqui está uma lista de alguns dos controles mais comuns e um resumo das interações que eles podem habilitar.

:::linha::: :::coluna::: ![imagem do botão](images/commanding/thumbnail-button.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Botões</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::fim da linha:::

:::linha::: :::coluna::: ![imagem da lista](images/commanding/thumbnail-list.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Listas</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::fim da linha:::

:::linha::: :::coluna::: ![imagem do controle de seleção](images/commanding/thumbnail-selection.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Controles de seleção</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::fim da linha:::

:::linha::: :::coluna::: ![Imagem de calendário](images/commanding/thumbnail-calendar.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Seletores de calendário, data e hora</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::fim da linha:::

:::linha::: :::coluna::: ![Imagem de entrada de texto com previsão](images/commanding/thumbnail-autosuggest.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Entrada de texto com previsão</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::fim da linha:::

Para obter uma lista completa, consulte [Controles e elementos de interface do usuário](../controls-and-patterns/index.md)

##  <a name="place-commands-on-the-right-surface"></a>Colocar comandos na superfície certa
Você pode colocar elementos de comando em várias superfícies no aplicativo, inclusive na tela do aplicativo ou em elementos de comando especial, como barras de comando, menus, caixas de diálogo e submenus.

Observe que, sempre que possível, você deve permitir que os usuários manipulem diretamente o conteúdo em vez de usar comandos que atuam no conteúdo. Por exemplo, permita que os usuários reorganizem listas arrastando e soltando itens da lista, em vez de usar botões de comando para cima e para baixo.

Caso contrário, se os usuários não puderem manipular o conteúdo diretamente, coloque os elementos de comando em uma superfície de comando no seu aplicativo. Veja uma lista de algumas das superfícies de comando mais comuns.

:::linha::: :::coluna::: ![imagem da tela do aplicativo](images/commanding/thumbnail-canvas.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Tela do aplicativo (área de conteúdo)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::fim da linha:::

:::linha::: :::coluna::: ![imagem da barra de comando](images/commanding/thumbnail-commandbar.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Barras de comando</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen.
:::fim da linha:::

:::linha::: :::coluna::: ![imagem do menu de contexto](images/commanding/thumbnail-contextmenu.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Menus e menus de contexto</b>

        Sometimes it is more efficient to group multiple commands into a command menu to save space. <a href="../controls-and-patterns/menus.md">Menus and context menus</a> display a list of commands or options when the user requests them. Context menus can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands. Context menus are usually prompted by a user right-clicking.
:::fim da linha:::

## <a name="provide-feedback-for-interactions"></a>Envie comentários sobre as interações

Os comentários transmitem os resultados dos comandos e permite que os usuários entendam o que fizeram e o que eles podem fazer em seguida. Idealmente, os comentários devem ser integrados naturalmente em sua interface de usuário para que os usuários não precisem ser interrompidos ou executar nenhuma ação adicional, a menos que absolutamente necessário. 

Aqui estão algumas maneiras de fornecer comentários em seu aplicativo.

:::linha::: :::coluna::: ![imagem da área de conteúdo da barra de comando](images/commanding/thumbnail-commandbar2.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Barra de comando</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::fim da linha:::

:::linha::: :::coluna::: ![imagem de submenu](images/commanding/thumbnail-flyout.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Submenus</b>

       <a href="../controls-and-patterns/dialogs.md">Flyouts</a> are lightweight contextual popups that can be dismissed by tapping or clicking somewhere outside the flyout.
:::fim da linha:::

:::linha::: :::coluna::: ![imagem da caixa de diálogo](images/commanding/thumbnail-dialog.svg) :::fim da coluna::: :::extensão da coluna="2"::: <b>Controles de caixa de diálogo</b>

        <a href="../controls-and-patterns/dialogs.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::fim da linha:::

> [!TIP]
> Tome cuidado com a quantidade de caixas de diálogo de confirmação que seu aplicativo usa; elas podem ser muito úteis quando o usuário comete um erro, mas são um obstáculo sempre que ele está tentando executar intencionalmente uma ação.

### <a name="when-to-confirm-or-undo-actions"></a>Quando confirmar ou desfazer ações

Não importa se a interface do usuário foi bem projetada e se o usuário é cuidadoso; em algum momento, todos os usuários executarão uma ação que desejariam não ter feito. Seu aplicativo pode ajudar nessas situações ao exigir que o usuário confirme uma ação, ou ao fornecer uma forma de desfazer ações recentes.

::: linha:::::: coluna::: ![imagem do que fazer](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::fim da linha:::

##  <a name="optimize-for-specific-input-types"></a>Otimizar para tipos específicos de entrada

Consulte a [Cartilha de interação](../input/index.md) para obter mais detalhes sobre a otimização de experiências do usuário em relação a um tipo de entrada ou um dispositivo específico.

