---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Noções básicas de design de comando para apps da Plataforma Universal do Windows (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 10/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 028cc9586180f2d94337282c3ed0cd58317b539b
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6653851"
---
# <a name="command-design-basics-for-uwp-apps"></a>Noções básicas de design de comandos para apps UWP

Em um aplicativo da plataforma Universal do Windows (UWP), *elementos de comando* são os elementos de interface do usuário interativos que permitem aos usuários executar ações como enviar um email, excluir um item ou enviar um formulário. *Interfaces de comando* são compostos de elementos de comando comuns, as superfícies de comando que hospedá-los, as interações que eles dão suporte e as experiências que eles fornecem.

## <a name="provide-the-best-command-experience"></a>Fornecer a melhor experiência de comando

O aspecto de uma interface de comando mais importante é que sua tentativa de permitir que o usuário realizar. À medida que você pretende a funcionalidade do seu aplicativo, considere as etapas necessárias para executar essas tarefas e as experiências do usuário que você deseja habilitar. Depois de concluir um rascunho inicial dessas experiências, em seguida, você pode tomar decisões sobre as ferramentas e interações implementá-los.

Aqui estão algumas experiências de aplicativo comuns:

- Enviar informações
- Selecionar configurações e opções
- Conteúdo de filtro e pesquisa
- Abrir, salvar e excluir arquivos
- Edição ou a criação de conteúdo

Seja criativo com o design de suas experiências de comando. Escolha quais dispositivos de entrada de seu aplicativo dá suporte, e como seu aplicativo responde a cada dispositivo. Oferecendo suporte a mais ampla variedade de recursos e preferências tornar seu aplicativo como acessível possível, portátil e utilizável.



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>Escolha os elementos de comando correto

Usar os elementos certos em uma interface de comando pode fazer a diferença entre um aplicativo intuitivo e fácil de usar e um aplicativo difícil, confuso. Um conjunto abrangente de elementos de comando estão disponíveis na plataforma Universal do Windows (UWP). Aqui está uma lista de alguns dos elementos de comando mais comuns do UWP.

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

Para obter uma lista completa, consulte [Controles e elementos de interface do usuário](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>Colocar comandos na superfície certa

Você pode colocar elementos de comando em várias superfícies em seu aplicativo, incluindo a tela do aplicativo ou contêineres de comando especial, como uma barra de comandos, submenu da barra de comandos, barra de menu ou caixa de diálogo.

Sempre tente permitir que os usuários manipulem diretamente o conteúdo em vez de por meio de comandos que act no conteúdo, como arrastar e soltar para reorganizar os itens de lista em vez de down botões de comando. 

No entanto, isso talvez não seja possível com alguns dispositivos de entrada, ou ao acomodar preferências e recursos de usuário específico. Nesses casos, forneça funcionalidades de comandos máximo possível e coloque esses elementos de comando em uma superfície de comando no seu aplicativo.

Veja uma lista de algumas das superfícies de comando mais comuns.

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Fornecer comentários de comando 

Comentários do comando se comunica com os usuários que uma interação ou um comando foi detectado, como ele foi interpretado e manipulado e se ele foi bem-sucedida ou não. Isso ajuda os usuários a entender o que fizeram e o que eles podem fazer em seguida. Idealmente, os comentários devem ser integrados naturalmente em sua interface de usuário para que os usuários não precisem ser interrompidos ou executar nenhuma ação adicional, a menos que absolutamente necessário.

> [!NOTE]
> Não fornece comentários, a menos que seja absolutamente necessário e os comentários não está disponível em outro lugar. Mantenha seu aplicativo da interface do usuário clara e organizada, a menos que você esteja agregando valor.

Aqui estão algumas maneiras de fornecer comentários em seu aplicativo.

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Submenus</a> são popups contextuais leves que podem ser ignorados tocando ou clicando em algum lugar fora do submenu.
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> Tome cuidado com a quantidade de caixas de diálogo de confirmação que seu aplicativo usa; elas podem ser muito úteis quando o usuário comete um erro, mas são um obstáculo sempre que ele está tentando executar intencionalmente uma ação.

### <a name="when-to-confirm-or-undo-actions"></a>Quando confirmar ou desfazer ações

Não importa como bem projetada interface do usuário do seu aplicativo é, todos os usuários executem uma ação que desejariam. Seu aplicativo pode ajudar nessas situações ao exigir a confirmação de uma ação, ou fornecendo uma forma de desfazer ações recentes.

:::row:::
    :::column:::
        ![do image](images/do.svg)

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
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>Otimizar para tipos específicos de entrada

Consulte a [Cartilha de interação](../input/index.md) para obter mais detalhes sobre a otimização de experiências do usuário em relação a um tipo de entrada ou um dispositivo específico.

