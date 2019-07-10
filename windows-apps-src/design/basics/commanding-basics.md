---
Description: Em um aplicativo da Plataforma Universal do Windows (UWP), elementos de comando são os elementos interativos da interface do usuário que permitem ao usuário executar ações, como enviar um email, excluir um item ou enviar um formulário.
title: Noções básicas de design de comando para aplicativos da Plataforma Universal do Windows (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ac2bd55d1cea25359c3c609148c7098532d76c46
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63796479"
---
# <a name="command-design-basics-for-uwp-apps"></a>Noções básicas de design de comandos para aplicativos UWP

Em um aplicativo UWP (Plataforma Universal do Windows), *elementos de comando* são os elementos interativos da interface do usuário que permitem aos usuários executar ações como enviar um email, excluir um item ou enviar um formulário. As *interfaces de comando* são formadas por elementos de comando comuns, as superfícies de comando que as hospedam, as interações compatíveis com elas e as experiências que elas oferecem.

## <a name="provide-the-best-command-experience"></a>Proporcionar a melhor experiência de comando

O aspecto mais importante de uma interface de comando é o que você está tentando permitir que o usuário realize. Ao planejar a funcionalidade de um aplicativo, considere seguir as etapas necessárias para realizar essas tarefas e as experiências de usuário que você quer oferecer. Após concluir um rascunho inicial dessas experiências, tome decisões sobre as ferramentas e as interações para implementá-las.

Veja algumas experiências de comando comuns:

- Enviar informações
- Selecionar configurações e opções
- Conteúdo de filtro e pesquisa
- Abrir, salvar e excluir arquivos
- Editar ou criar conteúdo

Seja criativo ao projetar experiências de comando. Escolha quais dispositivos de entrada serão compatíveis e como o aplicativo responderá a cada um deles. Ao oferecer compatibilidade com a mais ampla variedade de funcionalidades e preferências, o aplicativo fica o mais utilizável, móvel e acessível o possível. Confira [Noções básicas de design de comandos para aplicativos da UWP (Plataforma Universal do Windows)](../controls-and-patterns/commanding.md) para saber mais detalhes.



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>Escolher os elementos de comando certos

Usar os elementos certos em uma interface de comando pode fazer a diferença entre um aplicativo que parece intuitivo e fácil de usar e um que parece difícil ou confuso. Um conjunto abrangente de elementos de comando está disponível na UWP (Plataforma Universal do Windows). Veja uma lista de alguns dos elementos de comando mais comuns da UWP.

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

É possível colocar elementos de comando em várias superfícies no aplicativo, inclusive na tela do aplicativo ou em contêineres de comando especial, como uma barra de comando, o submenu de uma barra de comando, a barra de menus e as caixas de diálogo.

Sempre tente permitir que os usuários manipulem o conteúdo diretamente em vez de por meio de comandos que atuam no conteúdo, como arrastar e soltar para reorganizar itens de lista em vez de botões de comando para cima e para baixo. 

No entanto, talvez isso não seja possível com determinados dispositivos de entrada ou ao acomodar as preferências e as habilidades de um usuário específico. Nesses casos, oferecer o maior número possível de funcionalidades e colocar esses elementos de comando em uma superfície de comando no aplicativo.

Veja uma lista das superfícies de comando mais comuns.

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

## <a name="provide-command-feedback"></a>Fazer comentários sobre o comando 

Os comentários sobre o comando comunicam aos usuários que uma interação ou comando foi detectado, como o comando foi interpretado e manipulado e se o comando foi bem-sucedido ou não. Isso ajuda os usuários a entender o que fizeram e o que pode ser feito. Idealmente, os comentários devem ser integrados naturalmente em sua interface do usuário para que os usuários não precisem ser interrompidos ou executar nenhuma ação adicional, a menos que absolutamente necessário.

> [!NOTE]
> Faça comentários somente quando necessário e se não estiver disponível em outro local. Mantenha a interface do usuário do aplicativo limpa e organizada, a menos que você esteja agregando valor.

Aqui estão algumas maneiras de fazer comentários em seu aplicativo.

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

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Submenus</a> são pop-ups contextuais leves que podem ser ignorados tocando ou clicando em algum lugar fora deles.
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

Não importa se a interface do usuário do aplicativo foi bem projetada, todos os usuários podem executar as ações indesejadas. O aplicativo pode ajudar nessas situações ao exigir que a confirmação de uma ação ou ao fornecer uma forma de desfazer ações recentes.

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

