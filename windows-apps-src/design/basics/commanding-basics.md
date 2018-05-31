---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Noções básicas de design de comando para apps da Plataforma Universal do Windows (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07b9ce7b5a57f6dc1ba202ed57e8b2d4d93e583f
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654385"
---
#  <a name="command-design-basics-for-uwp-apps"></a>Noções básicas de design de comandos para apps UWP

Em um aplicativo da Plataforma Universal do Windows (UWP), *elementos de comando* são os elementos interativos da interface do usuário que permitem aos usuários executar ações, como enviar um email, excluir um item ou enviar um formulário. 

Este artigo descreve os elementos de comando comuns, as interações a que eles dão suporte e as superfícies de comando para hospedá-los.

![elementos de comando no aplicativo Mapas](images/maps.png)

Acima, veja exemplos de elementos de comando no aplicativo Mapas.

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

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Categoria</th>
<th align="left">Elementos</th>
<th align="left">Interação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Botões</b><br/><br/>
    <img src="../controls-and-patterns/images/controls/button.png" alt="button" /></td>
<td align="left"><a href="../controls-and-patterns/buttons.md">Botão</a></td>
<td align="left">Aciona uma ação imediata. Dentre alguns exemplos estão: enviar um email, enviar dados de formulários ou confirmar uma ação em uma caixa de diálogo.</td>
</tr>
<tr class="even">
<td align="left">Listas<br/><br/>
    <img src="../controls-and-patterns/images/controls/combo-box-open.png" alt="drop down list" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">lista suspensa, caixa de listagem, modo de exibição de lista e modo de exibição de grade</a></td>
<td align="left">Apresenta itens em uma lista interativa ou em uma grade. Geralmente usado para muitas opções ou itens de exibição.</td>
</tr>
<tr class="odd">
<td align="left">Controles de seleção<br/><br/>
    <img src="../controls-and-patterns/images/controls/radio-button.png" alt="radio button" /></td>
<td align="left"><a href="../controls-and-patterns/checkbox.md">caixa de seleção</a>, <a href="../controls-and-patterns/radio-button.md">botão de opção</a>, <a href="../controls-and-patterns/toggles.md">botão de alternância</a></td>
<td align="left">Permite que os usuários escolham entre algumas opções, por exemplo, quando concluir uma pesquisa ou definir as configurações do aplicativo.</td>
</tr>
<tr class="even">
<td align="left">Seletores de data e hora<br/><br/>
    <img src="../controls-and-patterns/images/controls/calendar-date-picker-open.png" alt="date picker" /></td>
<td align="left"><a href="../controls-and-patterns/date-and-time.md">seletor de data do calendário, modo de exibição de calendário, seletor de data, seletor de hora</a></td>
<td align="left">Permite que os usuários exibam e modifique informações de data e hora, como quando criar um evento ou definir um alarme.</td>
</tr>
<tr class="odd">
<td align="left">Entrada de texto previsto<br/><br/>
    <img src="../controls-and-patterns/images/controls/auto-suggest-box.png" alt="autosuggest box" /></td>
<td align="left"><a href="../controls-and-patterns/auto-suggest-box.md">Caixa de sugestão automática</a></td>
<td align="left">Fornece sugestões à medida que os usuários digitam, tais como ao inserirem dados ou realizar consultas.</td>
</tr>
</tbody>
</table>
</div>

Para obter uma lista completa, consulte [Controles e elementos de interface do usuário](https://dev.windows.com/design/controls-patterns)

##  <a name="place-commands-on-the-right-surface"></a>Colocar comandos na superfície certa
Você pode colocar elementos de comando em várias superfícies no aplicativo, inclusive na tela do aplicativo ou em elementos de comando especial, como barras de comando, menus, caixas de diálogo e submenus.

Observe que, sempre que possível, você deve permitir que os usuários manipulem diretamente o conteúdo em vez de usar comandos que atuam no conteúdo. Por exemplo, permita que os usuários reorganizem listas arrastando e soltando itens da lista, em vez de usar botões de comando para cima e para baixo.
  
Caso contrário, se os usuários não puderem manipular o conteúdo diretamente, coloque os elementos de comando em uma superfície de comando no seu aplicativo:

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">Superfície</th>
<th align="left">Descrição</th>
<th align="left">Exemplo</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top">Tela do aplicativo (área de conteúdo)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>

<td align="left" style="vertical-align: top;">Se um comando for constantemente necessário para que os usuários concluam os principais cenários, coloque-o na tela. Como você pode colocar comandos próximos dos objetos que eles afetam (ou sobre eles), colocar os comandos na tela torna seu uso fácil e evidente.
<p>No entanto, escolha com atenção os comandos que você colocará na tela. O excesso de comandos na tela do aplicativo ocupa espaço valioso e pode sobrecarregar o usuário. Se o comando não for usado com frequência, considere a possibilidade de colocá-lo em outra superfície de comando.</p> 
</td><td>
Uma caixa de sugestão automática na tela do aplicativo Mapas.
<br></br>
  <img src="images/maps-canvas.png" alt="autosuggest box on Maps app canvas"/>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/app-bars.md">Barra de comandos</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p></td>
<td align="left" style="vertical-align: top;"> Barras de comando ajudam a organizar comandos e facilitar o acesso a eles. As barras de comandos podem ser posicionadas na parte superior, na parte inferior ou na parte superior e inferior da tela. 
</td>
<td>
Uma barra de comando na parte superior do aplicativo Mapas.
<br></br>
<img src="images/maps-commandbar.png" alt="command bar in Maps app"/>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/menus.md">Menus e menus de contexto</a>
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left" style="vertical-align: top;">Às vezes, é mais eficiente agrupar vários comandos em um menu de comando para economizar espaço. Menus e menus de contexto exibem uma lista de comandos ou opções quando o usuário os solicita.
<p>Os menus de contexto podem fornecer atalhos para ações comumente usadas e fornecer acesso aos comandos secundários que são relevantes somente em determinados contextos, como área de transferência ou comandos personalizados. Menus de contexto geralmente são acionados quando o usuário clica o botão direito do mouse.</p>
</td><td>
Um menu de contexto é exibido quando os usuários clicam com o botão direito do mouse no aplicativo Mapas.
<br></br>
  <img src="images/maps-contextmenu.png" alt="context menu in Maps app"/>
</td>
</tr>
</table>
</div>

## <a name="provide-feedback-for-interactions"></a>Envie comentários sobre as interações

Os comentários transmitem os resultados dos comandos e permite que os usuários entendam o que fizeram e o que eles podem fazer em seguida. Idealmente, os comentários devem ser integrados naturalmente em sua interface de usuário para que os usuários não precisem ser interrompidos ou executar nenhuma ação adicional, a menos que absolutamente necessário. 

Aqui estão algumas maneiras de fornecer comentários em seu aplicativo.

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">Superfície</th>
<th align="left">Description</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"> <a href="../controls-and-patterns/app-bars.md">Barra de comandos</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p>
</td>
<td align="left" style="vertical-align: top;"> A área de conteúdo da barra de comandos é um lugar intuitivo para comunicar o status aos usuários se eles quiserem ver os comentários.
<p>
  <img src="images/commandbar_anatomy.png" alt="Command bar content area for feedback"/>
  </p>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">Submenu</a>
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left" style="vertical-align: top;">
Um popup contextual leve que pode ser ignorado tocando ou clicando em algum lugar fora do submenu.
<p>
  <img src="../controls-and-patterns/images/controls/flyout.png" alt="Flyout above button"/>
  </p>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">Controles de caixa de diálogo</a>
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left" style="vertical-align: top;">Caixas de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do aplicativo. Na maioria dos casos, as caixas de diálogo bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignoradas e muitas vezes solicitam algum tipo de ação do usuário.
<p>As caixas de diálogo podem causar interrupções e só devem ser usadas em determinadas situações. Para obter mais informações, consulte a seção [Quando confirmar ou desfazer ações](#when-to-confirm-or-undo-actions).</p>
<p>
  <img src="../controls-and-patterns/images/dialogs/dialog_RS2_delete_file.png" alt="dialog delete file"/></p>
</td>
</tr>

</table>
</div>

> [!TIP]
> Tome cuidado com a quantidade de caixas de diálogo de confirmação que seu aplicativo usa; elas podem ser muito úteis quando o usuário comete um erro, mas são um obstáculo sempre que ele está tentando executar intencionalmente uma ação.

### <a name="when-to-confirm-or-undo-actions"></a>Quando confirmar ou desfazer ações

Não importa se a interface do usuário foi bem projetada e se o usuário é cuidadoso; em algum momento, todos os usuários executarão uma ação que desejariam não ter feito. Seu aplicativo pode ajudar nessas situações ao exigir que o usuário confirme uma ação, ou ao fornecer uma forma de desfazer ações recentes.

-   Para ações que não podem ser desfeitas e têm consequências importantes, recomendamos o uso de uma caixa de diálogo de confirmação. Exemplos de tais ações incluem:
    -   Substituir um arquivo
    -   Não salvar um arquivo antes de fechar
    -   Confirmar a exclusão permanente de um arquivo ou dados
    -   Fazer uma compra (a menos que o usuário opte por não exigir uma confirmação)
    -   Enviar um formulário, por exemplo, inscrever-se para algo
-   Para ações que podem ser desfeitas, oferecer um comando Desfazer simples geralmente é suficiente. Exemplos de tais ações incluem:
    -   Excluir um arquivo
    -   Excluir um email (não permanentemente)
    -   Modificar o conteúdo ou editar texto
    -   Renomear um arquivo

##  <a name="optimize-for-specific-input-types"></a>Otimizar para tipos específicos de entrada

Consulte a [Cartilha de interação](../input/index.md) para obter mais detalhes sobre a otimização de experiências do usuário em relação a um tipo de entrada ou um dispositivo específico.