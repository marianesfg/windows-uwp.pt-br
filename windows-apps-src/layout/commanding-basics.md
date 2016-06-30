---
author: mijacobs
Description: "Em um aplicativo da Plataforma Universal do Windows (UWP), elementos de comando são os elementos interativos da interface do usuário que permitem ao usuário executar ações, como enviar um email, excluir um item ou enviar um formulário."
title: "Noções básicas de design de comando para aplicativos da Plataforma Universal do Windows (UWP)"
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: be7ff187df9800a8c2c47c4315f3f9b021e265f8

---

#  Noções básicas de design de comandos para aplicativos UWP

Em um aplicativo da Plataforma Universal do Windows (UWP), *elementos de comando* são os elementos interativos da interface do usuário que permitem ao usuário executar ações, como enviar um email, excluir um item ou enviar um formulário. Este artigo descreve os elementos de comando, como botões e caixas de seleção, as interações a que eles dão suporte e as superfícies de comando (por exemplo, barras de comandos e menus de contexto) para hospedá-los.

## <span id="Provide_the_right_type_of_interactions"></span><span id="provide_the_right_type_of_interactions"></span><span id="PROVIDE_THE_RIGHT_TYPE_OF_INTERACTIONS"></span>Forneça o tipo correto de interações


Ao projetar uma interface de comando, a decisão mais importante é escolher quais ações os usuários podem executar. Por exemplo, se você estiver criando um aplicativo de fotos, o usuário precisará de ferramentas para editar fotos. No entanto, se você estiver criando um aplicativo de mídia social que exiba fotos, a edição de imagens talvez não seja uma prioridade e, portanto, as ferramentas de edição podem ser omitidas para economizar espaço. Decida quais ações os usuários podem executar e forneça as ferramentas para ajudá-los a fazê-lo.

Para obter recomendações sobre como planejar as interações certas para o aplicativo, consulte [Planeje seu aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465427.aspx).

## <span id="Use_the_right_command_element_for_the_interaction"></span><span id="use_the_right_command_element_for_the_interaction"></span><span id="USE_THE_RIGHT_COMMAND_ELEMENT_FOR_THE_INTERACTION"></span>Use o elemento de comando correto para a interação


Usar os elementos certos para as interações certas pode significar a diferença entre um aplicativo que parece intuitivo de usar e um que parece difícil ou confuso. A plataforma universal do Windows (UWP) fornece um grande conjunto de elementos de comando, na forma de controles, que você pode usar em seu aplicativo. Aqui está uma lista de alguns dos controles mais comuns e um resumo das interações que eles habilitam.

| Categoria              | Elementos                                                                                                                                                                                                            | Interação                                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Botões               | [Botão](https://msdn.microsoft.com/library/windows/apps/hh465470)                                                                                                                                                     | Aciona uma ação imediata, como enviar um email, confirmar uma ação em uma caixa de diálogo, enviar dados de formulário.                                    |
| Seletores de data e hora | [seletor de data do calendário, modo de exibição de calendário, seletor de data, seletor de hora](https://msdn.microsoft.com/library/windows/apps/hh465466)                                                                                                                 | Permite que o usuário exiba e modifique informações de data e hora, como quando inserir uma data de validade de cartão de crédito ou definir um alarme.                   |
| Listas                 | [lista suspensa, caixa de listagem, modo de exibição de lista e modo de exibição de grade](https://msdn.microsoft.com/library/windows/apps/mt186889)                                                                                                                                              | Apresenta itens em uma lista interativa ou em uma grade. Use esses elementos para permitir que os usuários selecionem um filme em uma lista de lançamentos ou gerenciem um estoque. |
| Entrada de texto previsto | [Caixa de sugestão automática](https://msdn.microsoft.com/library/windows/apps/dn997762)                                                                                                                                                                    | Economiza tempo dos usuários quando eles inserem dados ou realizam consultas ao fornecer sugestões conforme eles digitam.                                                   |
| Controles de seleção    | [caixa de seleção](https://msdn.microsoft.com/library/windows/apps/hh700393), [botão de opção](https://msdn.microsoft.com/library/windows/apps/hh700395), [botão de alternância](https://msdn.microsoft.com/library/windows/apps/hh465475) | Permite que o usuário escolha entre diferentes opções, por exemplo, quando concluir uma pesquisa ou definir as configurações do aplicativo.                                      |

 

Para obter uma lista completa, consulte [Controles e elementos de interface do usuário](https://dev.windows.com/design/controls-patterns)

## <span id="_________Place_commands_on_the_right_surface_______"></span><span id="_________place_commands_on_the_right_surface_______"></span><span id="_________PLACE_COMMANDS_ON_THE_RIGHT_SURFACE_______"></span> Colocar comandos na superfície certa


Você pode colocar elementos de comando em várias superfícies no aplicativo, inclusive na tela do aplicativo (a área de conteúdo do aplicativo) ou em elementos de comando especial que podem atuar como contêineres de comando, como barras de comandos, menus, caixas de diálogo e submenus. Aqui estão algumas recomendações gerais para colocação de comandos:

-   Sempre que possível, permita que os usuários manipulem diretamente o conteúdo na tela do aplicativo, em vez de adicionar comandos que atuam no conteúdo. Por exemplo, no aplicativo de viagens, permita que os usuários reorganizem seu itinerário arrastando e soltando atividades em uma lista na tela, em vez de selecionar a atividade e usar botões de comando Para cima ou Para baixo.
-   Caso contrário, coloque os comandos em uma destas superfícies de IU se os usuários não puderem manipular diretamente o conteúdo:

    -   Na [barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302): você deve colocar a maioria dos comandos na barra de comandos, o que ajuda a organizá-los e facilitar seu acesso.
    -   Na tela do aplicativo: se o usuário estiver em uma página ou modo de exibição que tenha uma única finalidade, você pode oferecer comandos para essa finalidade diretamente na tela. Deve haver muito pouco desses comandos presentes.
    -   Em um [menu de contexto](https://msdn.microsoft.com/library/windows/apps/hh465308): você pode usar menus de contexto para ações da área de transferência (como recortar, copiar e colar) ou para comandos que se aplicam ao conteúdo que não pode ser selecionado (como a adição de uma tachinha a um local no mapa).

Aqui está uma lista das superfícies de comando que o Windows fornece e recomendações para quando usá-las.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Superfície</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Tela do aplicativo (área de conteúdo)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>
<td align="left"><p>Se um comando for essencial e constantemente necessário para que o usuário complete os principais cenários, coloque-o na tela (a área de conteúdo do aplicativo). Como você pode colocar comandos próximos dos objetos que eles afetam (ou sobre eles), colocar os comandos na tela torna seu uso fácil e evidente.</p>
<p>No entanto, escolha com atenção os comandos que você colocará na tela. O excesso de comandos na tela do aplicativo ocupa espaço valioso e pode sobrecarregar o usuário. Se o comando não for usado com frequência, considere a possibilidade de colocá-lo em outra superfície de comando, como no menu ou na área &quot;Mais&quot; da barra de comandos.</p></td>
</tr>
<tr class="even">
<td align="left">[Barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302)
<p><img src="images/controls-appbar-icons-200.png" alt="Example of a command bar with icons" /></p></td>
<td align="left"><p>As barras de comandos fornecem aos usuários acesso fácil a ações. Você pode usar uma barra de comandos para mostrar comandos ou opções específicos do contexto do usuário, como o modo de desenho ou seleção de fotos.</p>
<p>As barras de comandos podem ser posicionadas na parte superior, na parte inferior ou na parte superior e inferior da tela. Esse design de uma aplicativo de edição de fotos mostra a área de conteúdo e a barra de comandos:</p>
<p><img src="images/commands-appcanvas-example.png" alt="A photo app" /></p>
<p>Para saber mais sobre barras de comandos, consulte o artigo [Diretrizes para barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302).</p></td>
</tr>
<tr class="odd">
<td align="left">[Menus e menus de contexto](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left"><p>Às vezes, é mais eficiente agrupar vários comandos em um menu de comando. Os menus permitem apresentar mais opções em menos espaço. Eles podem incluir controles interativos.</p>
<p>Os menus de contexto podem fornecer atalhos para ações comumente usadas e fornecer acesso aos comandos secundários que são relevantes somente em determinados contextos.</p>
<p>Menus de contexto servem para os seguintes tipos de comandos e cenários de comando:</p>
<ul>
<li>Ações contextuais em seleções de texto, como Copiar, Recortar, Colar, Verificar Ortografia e assim por diante.</li>
<li>Os comandos de um objeto com o qual se deve atuar, mas que não pode ser selecionado ou indicado de outra maneira.</li>
<li>Mostrando comandos da área de transferência.</li>
<li>Comandos personalizados.</li>
</ul>
<p>Este exemplo mostra o design de um aplicativo de metrô que usa um menu de contexto para modificar a rota, marcar uma rota ou selecionar outro trem.</p>
<p><img src="images/subway/uap-subway-ak-8in-dashboard-200.png" alt="A context menu in an subway app" /></p>
<p>Para saber mais sobre menus de contexto, consulte o artigo [Diretrizes para menu de contexto](https://msdn.microsoft.com/library/windows/apps/hh465308).</p></td>
</tr>
<tr class="even">
<td align="left">[Controles de caixa de diálogo](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left"><p>Caixas de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do aplicativo. Na maioria dos casos, as caixas de diálogo bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignoradas e muitas vezes solicitam algum tipo de ação do usuário.</p>
<p>As caixas de diálogo podem causar interrupções e só devem ser usadas em determinadas situações. Para obter mais informações, consulte a seção [Quando confirmar ou desfazer ações](#whentoconfirm).</p></td>
</tr>
<tr class="odd">
<td align="left">[Submenu](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left"><p>Um pop-up leve e contextual que exibe a interface do usuário relacionada ao que o usuário está fazendo. Use um submenu para:</p>
<p></p>
<ul>
<li>Mostrar um menu.</li>
<li>Mostrar mais detalhes sobre um item.</li>
<li>Solicitar que o usuário confirme uma ação sem bloquear a interação com o aplicativo.</li>
</ul>
<p>Os submenus podem ser ignorados tocando ou clicando em algum lugar fora do submenu. Para saber mais sobre controles do submenu, consulte o artigo [Caixas de diálogo, menus e pop-ups](../controls-and-patterns/dialogs-popups-menus.md).</p></td>
</tr>
</tbody>
</table>

 

## <span id="whentoconfirm"></span><span id="WHENTOCONFIRM"></span>Quando confirmar ou desfazer ações


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

**Dica**  Tome cuidado com a quantidade de caixas de diálogo de confirmação que seu aplicativo usa; elas podem ser muito úteis quando o usuário comete um erro, mas são um obstáculo sempre que ele está tentando executar intencionalmente uma ação.

 

## <span id="_________Optimize_for_specific_input_types_______"></span><span id="_________optimize_for_specific_input_types_______"></span><span id="_________OPTIMIZE_FOR_SPECIFIC_INPUT_TYPES_______"></span> Otimizar para tipos específicos de entrada


Consulte a [Cartilha de interação](../input-and-devices/input-primer.md) para obter mais detalhes sobre a otimização de experiências do usuário em relação a um tipo de entrada ou um dispositivo específico.




 

 







<!--HONumber=Jun16_HO4-->


