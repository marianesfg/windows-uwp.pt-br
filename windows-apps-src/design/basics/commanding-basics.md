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
ms.openlocfilehash: 8b33fe420e93c9ce78c625ad365ec8dc10e343ad
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867451"
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
![imagem do botão](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>Botões</b>

Os <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Botões</a> disparam uma ação imediata. Dentre os exemplos estão: enviar um email, enviar dados de formulários ou confirmar uma ação em uma caixa de diálogo.
:::row-end:::

:::row:::
    :::column:::
![imagem da lista](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>Listas</b>

As <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Listas</a> apresentam itens em uma grade ou lista interativa. Geralmente usado para muitas opções ou itens de exibição. Dentre os exemplos estão: lista suspensa, caixa de listagem, exibição de lista e exibição de grade.
:::row-end:::

:::row:::
    :::column:::
![imagem do controle de seleção](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>Controles de seleção</b>

Permite que os usuários escolham entre algumas opções, por exemplo, ao concluir uma pesquisa ou definir as configurações do aplicativo. Dentre os exemplos estão: <a href="../controls-and-patterns/checkbox.md">caixa de seleção</a>, <a href="../controls-and-patterns/radio-button.md">botão de opção</a> e <a href="../controls-and-patterns/toggles.md">botão de alternância</a>.
:::row-end:::

:::row:::
    :::column:::
![Imagem do calendário](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>Seletores de calendário, data e hora</b>

Os <a href="../controls-and-patterns/date-and-time.md">Seletores de calendário, data e hora</a> permitem que os usuários exibam e modifiquem informações de data e hora, por exemplo, ao criar um evento ou definir um alarme. Dentre os exemplos estão: seletor de data do calendário, exibição de calendário, seletor de data e seletor de hora.
:::row-end:::

:::row:::
    :::column:::
![Imagem da entrada de texto preditiva](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>Entrada de texto preditiva</b>

Fornece sugestões à medida que os usuários digitam, por exemplo, ao inserir dados ou realizar consultas. Dentre os exemplos estão: <a href="../controls-and-patterns/auto-suggest-box.md">caixa de sugestão automática</a>.<br>
:::row-end:::

Para obter uma lista completa, consulte [Controles e elementos de interface do usuário](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>Colocar comandos na superfície certa

É possível colocar elementos de comando em várias superfícies no aplicativo, inclusive na tela do aplicativo ou em contêineres de comando especial, como uma barra de comando, o submenu de uma barra de comando, a barra de menus e as caixas de diálogo.

Sempre tente permitir que os usuários manipulem o conteúdo diretamente em vez de por meio de comandos que atuam no conteúdo, como arrastar e soltar para reorganizar itens de lista em vez de botões de comando para cima e para baixo. 

No entanto, talvez isso não seja possível com determinados dispositivos de entrada ou ao acomodar as preferências e as habilidades de um usuário específico. Nesses casos, oferecer o maior número possível de funcionalidades e colocar esses elementos de comando em uma superfície de comando no aplicativo.

Veja uma lista das superfícies de comando mais comuns.

:::row:::
    :::column:::
![imagem da tela do aplicativo](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>Tela do aplicativo (área de conteúdo)</b>

Se um comando for constantemente necessário para que os usuários concluam os principais cenários, coloque-o na tela. Como você pode colocar comandos próximos dos objetos que eles afetam (ou sobre eles), colocar os comandos na tela torna seu uso fácil e evidente. No entanto, escolha com atenção os comandos que você colocará na tela. O excesso de comandos na tela do aplicativo ocupa espaço valioso e pode sobrecarregar o usuário. Se o comando não for usado com frequência, considere a possibilidade de colocá-lo em outra superfície de comando.
:::row-end:::

:::row:::
    :::column:::
![imagem da barra de comandos](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>Barras de comandos e barras de menus</b>

As <a href="../controls-and-patterns/app-bars.md">Barras de comandos</a> ajudam a organizar os comandos e facilitam o acesso a eles. As barras de comandos podem ser colocadas na parte superior da tela, na parte inferior da tela ou em ambas ao mesmo tempo (uma <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> também pode ser usada quando a funcionalidade do seu aplicativo é complexa demais para uma barra de comandos).
:::row-end:::

:::row:::
    :::column:::
![imagem do menu de contexto](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>Menus e menus de contexto</b>

<p>Menus e menus de contexto economizam espaço organizando os comandos e ocultando-os até que o usuário precise deles. Normalmente, os usuários acessam um menu ou um menu de contexto clicando em um botão ou clicando com o botão direito do mouse em um controle.</p> 

<p>O <a href="../controls-and-patterns/command-bar-flyout.md">submenu da barra de comandos</a> é um tipo de menu contextual que combina os benefícios de uma barra de comandos e de um menu de contexto em um único controle. Eles podem fornecer atalhos para ações usadas com frequência e fornecer acesso a comandos secundários que são relevantes somente em determinados contextos, como na área de transferência ou em comandos personalizados.</p>

<p>A UWP também fornece um conjunto de menus tradicionais e menus de contexto; para obter mais informações, consulte a <a href="../controls-and-patterns/menus.md">visão geral de menus e menus de contexto</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Fazer comentários sobre o comando 

Os comentários sobre o comando comunicam aos usuários que uma interação ou comando foi detectado, como o comando foi interpretado e manipulado e se o comando foi bem-sucedido ou não. Isso ajuda os usuários a entender o que fizeram e o que pode ser feito. Idealmente, os comentários devem ser integrados naturalmente em sua interface do usuário para que os usuários não precisem ser interrompidos ou executar nenhuma ação adicional, a menos que absolutamente necessário.

> [!NOTE]
> Faça comentários somente quando necessário e se não estiver disponível em outro local. Mantenha a interface do usuário do aplicativo limpa e organizada, a menos que você esteja agregando valor.

Aqui estão algumas maneiras de fazer comentários em seu aplicativo.

:::row:::
    :::column:::
![imagem da área de conteúdo da barra de comandos](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>Barra de comandos</b>

A área de conteúdo da <a href="../controls-and-patterns/app-bars.md">barra de comandos</a> é um lugar intuitivo para comunicar o status aos usuários se eles quiserem ver os comentários.
:::row-end:::

:::row:::
    :::column:::
![Imagem do submenu](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>Submenus</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Submenus</a> são pop-ups contextuais leves que podem ser ignorados tocando ou clicando em algum lugar fora deles.
:::row-end:::

:::row:::
    :::column:::
![Imagem da caixa de diálogo](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>Controles de caixa de diálogo</b>

Os <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Controles de caixa de diálogo</a> são sobreposições de interface do usuário modais que fornecem informações contextuais de aplicativo. Na maioria dos casos, as caixas de diálogo bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignoradas e muitas vezes solicitam algum tipo de ação do usuário. As caixas de diálogo podem causar interrupções e só devem ser usadas em determinadas situações. Para obter mais informações, consulte a seção [Quando confirmar ou desfazer ações](#when-to-confirm-or-undo-actions).
    :::column-end:::
:::row-end:::

> [!TIP]
> Tome cuidado com a quantidade de caixas de diálogo de confirmação que seu aplicativo usa; elas podem ser muito úteis quando o usuário comete um erro, mas são um obstáculo sempre que ele está tentando executar intencionalmente uma ação.

### <a name="when-to-confirm-or-undo-actions"></a>Quando confirmar ou desfazer ações

Não importa se a interface do usuário do aplicativo foi bem projetada, todos os usuários podem executar as ações indesejadas. O aplicativo pode ajudar nessas situações ao exigir que a confirmação de uma ação ou ao fornecer uma forma de desfazer ações recentes.

:::row:::
    :::column:::
![imagem do fazer](images/do.svg)

Para ações que não podem ser desfeitas e têm consequências importantes, recomendamos o uso de uma caixa de diálogo de confirmação. Exemplos de tais ações incluem:
-   Substituir um arquivo
-   Não salvar um arquivo antes de fechar
-   Confirmar a exclusão permanente de um arquivo ou dados
-   Fazer uma compra (a menos que o usuário opte por não exigir uma confirmação)
-   Enviar um formulário, por exemplo, inscrever-se para algo
    :::column-end:::
    :::column:::
![imagem do fazer](images/do.svg)

Para ações que podem ser desfeitas, oferecer um comando Desfazer simples geralmente é suficiente. Exemplos de tais ações incluem:
-   Excluir um arquivo
-   Excluir um email (não permanentemente)
-   Modificar o conteúdo ou editar texto
-   Renomear um arquivo
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>Otimizar para tipos específicos de entrada

Consulte a [Cartilha de interação](../input/index.md) para obter mais detalhes sobre a otimização de experiências do usuário em relação a um tipo de entrada ou um dispositivo específico.

