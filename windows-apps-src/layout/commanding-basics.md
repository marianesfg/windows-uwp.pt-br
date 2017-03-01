---
author: mijacobs
Description: "Em um app da Plataforma Universal do Windows (UWP), elementos de comando são os elementos interativos da interface do usuário que permitem ao usuário executar ações, como enviar um email, excluir um item ou enviar um formulário."
title: "Noções básicas de design de comando para apps da Plataforma Universal do Windows (UWP)"
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ec6e7879faf3c33edc97b26b2d0721808a02dfdc
ms.lasthandoff: 02/07/2017

---

#  <a name="command-design-basics-for-uwp-apps"></a>Noções básicas de design de comandos para apps UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Em um app da Plataforma Universal do Windows (UWP), *elementos de comando* são os elementos interativos da interface do usuário que permitem ao usuário executar ações, como enviar um email, excluir um item ou enviar um formulário. Este artigo descreve os elementos de comando, como botões e caixas de seleção, as interações a que eles dão suporte e as superfícies de comando (por exemplo, barras de comandos e menus de contexto) para hospedá-los.

## <a name="provide-the-right-type-of-interactions"></a>Forneça o tipo correto de interações


Ao projetar uma interface de comando, a decisão mais importante é escolher quais ações os usuários podem executar. Por exemplo, se você estiver criando um app de fotos, o usuário precisará de ferramentas para editar fotos. No entanto, se você estiver criando um app de mídia social que exiba fotos, a edição de imagens talvez não seja uma prioridade e, portanto, as ferramentas de edição podem ser omitidas para economizar espaço. Decida quais ações os usuários podem executar e forneça as ferramentas para ajudá-los a fazê-lo.

Para obter recomendações sobre como planejar as interações certas para o app, consulte [Planeje seu app](https://msdn.microsoft.com/library/windows/apps/hh465427.aspx).

## <a name="use-the-right-command-element-for-the-interaction"></a>Use o elemento de comando correto para a interação


Usar os elementos certos para as interações certas pode significar a diferença entre um app que parece intuitivo de usar e um que parece difícil ou confuso. A plataforma universal do Windows (UWP) fornece um grande conjunto de elementos de comando, na forma de controles, que você pode usar em seu app. Aqui está uma lista de alguns dos controles mais comuns e um resumo das interações que eles habilitam.

| Categoria              | Elementos                                                                                                                                                                                                            | Interação                                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Botões               | [Botão](https://msdn.microsoft.com/library/windows/apps/hh465470)                                                                                                                                                     | Aciona uma ação imediata, como enviar um email, confirmar uma ação em uma caixa de diálogo, enviar dados de formulário.                                    |
| Seletores de data e hora | [seletor de data do calendário, modo de exibição de calendário, seletor de data, seletor de hora](https://msdn.microsoft.com/library/windows/apps/hh465466)                                                                                                                 | Permite que o usuário exiba e modifique informações de data e hora, como quando inserir uma data de validade de cartão de crédito ou definir um alarme.                   |
| Listas                 | [lista suspensa, caixa de listagem, modo de exibição de lista e modo de exibição de grade](https://msdn.microsoft.com/library/windows/apps/mt186889)                                                                                                                                              | Apresenta itens em uma lista interativa ou em uma grade. Use esses elementos para permitir que os usuários selecionem um filme em uma lista de lançamentos ou gerenciem um estoque. |
| Entrada de texto previsto | [Caixa de sugestão automática](https://msdn.microsoft.com/library/windows/apps/dn997762)                                                                                                                                                                    | Economiza tempo dos usuários quando eles inserem dados ou realizam consultas ao fornecer sugestões conforme eles digitam.                                                   |
| Controles de seleção    | [caixa de seleção](https://msdn.microsoft.com/library/windows/apps/hh700393), [botão de opção](https://msdn.microsoft.com/library/windows/apps/hh700395), [botão de alternância](https://msdn.microsoft.com/library/windows/apps/hh465475) | Permite que o usuário escolha entre diferentes opções, por exemplo, quando concluir uma pesquisa ou definir as configurações do app.                                      |

 

Para obter uma lista completa, consulte [Controles e elementos de interface do usuário](https://dev.windows.com/design/controls-patterns)

##  <a name="place-commands-on-the-right-surface"></a>Colocar comandos na superfície certa


Você pode colocar elementos de comando em várias superfícies no app, inclusive na tela do app (a área de conteúdo do app) ou em elementos de comando especial que podem atuar como contêineres de comando, como barras de comandos, menus, caixas de diálogo e submenus. Aqui estão algumas recomendações gerais para colocação de comandos:

-   Sempre que possível, permita que os usuários manipulem diretamente o conteúdo na tela do app, em vez de adicionar comandos que atuam no conteúdo. Por exemplo, no app de viagens, permita que os usuários reorganizem seu itinerário arrastando e soltando atividades em uma lista na tela, em vez de selecionar a atividade e usar botões de comando Para cima ou Para baixo.
-   Caso contrário, coloque os comandos em uma destas superfícies de IU se os usuários não puderem tratar diretamente o conteúdo:

    -   Na [barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302): você deve colocar a maioria dos comandos na barra de comandos, o que ajuda a organizá-los e facilitar seu acesso.
    -   Na tela do app: se o usuário estiver em uma página ou modo de exibição que tenha uma única finalidade, você pode oferecer comandos para essa finalidade diretamente na tela. Deve haver muito pouco desses comandos presentes.
    -   Em um [menu de contexto](https://msdn.microsoft.com/library/windows/apps/hh465308): você pode usar menus de contexto para ações da área de transferência (como recortar, copiar e colar) ou para comandos que se aplicam ao conteúdo que não pode ser selecionado (como a adição de uma tachinha a um local no mapa).

Aqui está uma lista das superfícies de comando que o Windows fornece e recomendações para quando usá-las.

<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">Superfície</th>
<th align="left">Descrição</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top">Tela do app (área de conteúdo)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>

<td align="left" style="vertical-align: top;">Se um comando for essencial e constantemente necessário para que o usuário complete os principais cenários, coloque-o na tela (a área de conteúdo do app). Como você pode colocar comandos próximos dos objetos que eles afetam (ou sobre eles), colocar os comandos na tela torna seu uso fácil e evidente.
<p>No entanto, escolha com atenção os comandos que você colocará na tela. O excesso de comandos na tela do app ocupa espaço valioso e pode sobrecarregar o usuário. Se o comando não for usado com frequência, considere a possibilidade de colocá-lo em outra superfície de comando, como no menu ou na área &quot;Mais&quot; da barra de comandos.</p></td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;">[Barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302)
<p><img src="images/controls-appbar-icons-200.png" alt="Example of a command bar with icons" /></p></td>
<td align="left" style="vertical-align: top;">As barras de comandos fornecem aos usuários acesso fácil a ações. Você pode usar uma barra de comandos para mostrar comandos ou opções específicos do contexto do usuário, como o modo de desenho ou seleção de fotos.
<p>As barras de comandos podem ser posicionadas na parte superior, na parte inferior ou na parte superior e inferior da tela. Esse design de uma app de edição de fotos mostra a área de conteúdo e a barra de comandos:</p>
<p><img src="images/commands-appcanvas-example.png" alt="A photo app" /></p>
<p>Para saber mais sobre barras de comandos, consulte o artigo [Diretrizes para barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302).</p></td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;">[Menus e menus de contexto](../controls-and-patterns/menus.md)
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left" style="vertical-align: top;">Às vezes, é mais eficiente agrupar vários comandos em um menu de comando. Os menus permitem apresentar mais opções em menos espaço. Eles podem incluir controles interativos.
<p>Os menus de contexto podem fornecer atalhos para ações comumente usadas e fornecer acesso aos comandos secundários que são relevantes somente em determinados contextos.</p>
<p>Menus de contexto servem para os seguintes tipos de comandos e cenários de comando:</p>
<ul>
<li>Ações contextuais em seleções de texto, como Copiar, Recortar, Colar, Verificar Ortografia e assim por diante.</li>
<li>Os comandos de um objeto com o qual se deve atuar, mas que não pode ser selecionado ou indicado de outra maneira.</li>
<li>Mostrando comandos da área de transferência.</li>
<li>Comandos personalizados.</li>
</ul>
<p>Este exemplo mostra o design de um app de metrô que usa um menu de contexto para modificar a rota, marcar uma rota ou selecionar outro trem.</p>
<p><img src="images/subway/uap-subway-ak-8in-dashboard-200.png" alt="A context menu in an subway app" /></p>
<p>Para saber mais sobre menus de contexto, consulte o artigo [Diretrizes para menu de contexto](https://msdn.microsoft.com/library/windows/apps/hh465308).</p></td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;">[Controles de caixa de diálogo](../controls-and-patterns/dialogs.md)
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left" style="vertical-align: top;">Caixas de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do app. Na maioria dos casos, as caixas de diálogo bloqueiam interações com a janela do app até que sejam explicitamente ignoradas e muitas vezes solicitam algum tipo de ação do usuário.
<p>As caixas de diálogo podem causar interrupções e só devem ser usadas em determinadas situações. Para obter mais informações, consulte a seção [Quando confirmar ou desfazer ações](#whentoconfirm).</p></td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;">[Submenu](../controls-and-patterns/dialogs.md)
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left" style="vertical-align: top;">Um pop-up leve e contextual que exibe a interface do usuário relacionada ao que o usuário está fazendo. Use um submenu para:
<p></p>
<ul>
<li>Mostrar um menu.</li>
<li>Mostrar mais detalhes sobre um item.</li>
<li>Solicitar que o usuário confirme uma ação sem bloquear a interação com o app.</li>
</ul>
<p>Os submenus podem ser ignorados tocando ou clicando em algum lugar fora do submenu. Para saber mais sobre controles do submenu, consulte o artigo [Caixas de diálogo e submenus](../controls-and-patterns/dialogs.md).</p></td>
</tr>
</table>

 

## <a name="when-to-confirm-or-undo-actions"></a>Quando confirmar ou desfazer ações


Não importa se a interface do usuário foi bem projetada e se o usuário é cuidadoso; em algum momento, todos os usuários executarão uma ação que desejariam não ter feito. Seu app pode ajudar nessas situações ao exigir que o usuário confirme uma ação, ou ao fornecer uma forma de desfazer ações recentes.

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

> [!TIP]
> Tome cuidado com a quantidade de caixas de diálogo de confirmação que seu app usa; elas podem ser muito úteis quando o usuário comete um erro, mas são um obstáculo sempre que ele está tentando executar intencionalmente uma ação.

 

##  <a name="optimize-for-specific-input-types"></a>Otimizar para tipos específicos de entrada


Consulte a [Cartilha de interação](../input-and-devices/input-primer.md) para obter mais detalhes sobre a otimização de experiências do usuário em relação a um tipo de entrada ou um dispositivo específico.




 

 





