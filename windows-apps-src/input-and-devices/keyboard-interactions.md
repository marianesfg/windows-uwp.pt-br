---
author: Karl-Bridge-Microsoft
Description: "Aprenda como projetar e otimizar seus aplicativos UWP para que eles ofereçam a melhor experiência possível tanto para usuários avançados como para usuários com deficiências e outras necessidades de acessibilidade."
title: "Interações por teclado"
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: "teclado, acessibilidade, navegação, foco, texto, entrada, interações de usuário, gamepad, controle remoto"
ms.author: kbridge
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 22a95cfb77740d7521c62f0b7153130ccdc1b552
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2017
---
# <a name="keyboard-interactions"></a>Interações por teclado
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

![imagem do teclado](images/keyboard/keyboard-hero.jpg)

Aprenda como projetar e otimizar seus aplicativos UWP para que eles ofereçam a melhor experiência possível tanto para usuários avançados como para usuários com deficiências e outras necessidades de acessibilidade.

Em qualquer dispositivo, as entradas por teclado são uma parte importante da experiência interativa geral da Plataforma Universal do Windows (UWP). Uma experiência de teclado bem projetada permite que os usuários naveguem de forma eficiente pela interface de usuário do seu aplicativo e acessem todas as suas funcionalidades sem precisar tirar as mãos do teclado.

![imagem de teclado e gamepad](images/keyboard/keyboard-gamepad.jpg)

***Padrões de interação comuns são compartilhados entre teclado e gamepad***

Neste tópico, nos focamos especificamente no design de aplicativo UWP para entradas por teclado em computadores. No entanto, uma experiência de teclado bem projetada é importante para dar suporte às ferramentas de acessibilidade como o Narrador do Windows, usando teclados de software como o [teclado virtual](#touch-keyboard) e o [Teclado Virtual (OSK)](#osk), e para o tratamento de outros tipos de dispositivos de entrada, como o gamepad do Xbox e o controle remoto.

Muitas das diretrizes e recomendações discutidas aqui, incluindo os [elementos visuais de foco](#focus-visual), as [teclas de acesso](#access-keys), e a [navegação da interface de usuário](#navigation), também se aplicam a outros cenários.

**OBSERVAÇÃO**  Enquanto ambos os teclados de hardware e software são usados para entrada de texto, o foco deste tópico é a navegação e a interação.

## <a name="built-in-support"></a>Suporte interno

Ao lado do mouse, o teclado é o periférico mais usado em computadores e, como tal, é parte fundamental da experiência de uso de um computador. Os usuários de computadores esperam uma experiência abrangente e consistente, tanto do sistema como dos aplicativos individuais, da resposta às entradas por teclado.

Todos os controles UWP incluem suporte interno para experiências de teclado avançadas e interações de usuário, enquanto a plataforma em si oferece uma base extensa para a criação de experiências de teclado que você considere mais apropriadas tanto para os controles personalizados como para o aplicativo.

![imagem de teclado com telefone](images/keyboard/keyboard-phone.jpg)

***A UWP suporta teclado para qualquer dispositivo***

## <a name="basic-experiences"></a>Experiências básicas
![Dispositivo baseados no foco](images/keyboard/focus-based-devices.jpg)

Como foi mencionado anteriormente, dispositivos de entrada como o gamepad do Xbox e o controle remoto, e ferramentas de acessibilidade como o Narrador, compartilham boa parte da experiência de entrada por teclado para a navegação e comandos. Essa experiência mútua entre tipos e ferramentas de entrada minimizam o seu trabalho extra e contribuem para o objetivo "crie uma vez, execute em qualquer lugar" da Plataforma Universal do Windows.

Onde for necessário, iremos identificar diferenças essências que você deve conhecer e descrever qualquer mitigação que você deva considerar.

Aqui estão os dispositivos e ferramentas discutidos neste tópico:

| Dispositivo/ferramenta                       | Descrição     |
|-----------------------------------|-----------------|
|Teclado (hardware e software)   |Além do teclado de hardware padrão, os aplicativos UWP suportam dois teclados de software: o de [toque (ou teclado de software)](#touch-keyboard) e o [Teclado Virtual](#osk).|
|Gamepad e controle remoto         |O gamepad do Xbox e o controle remoto são dispositivos de entrada fundamentais para a [experiência de 3 metros](designing-for-tv.md).
Para obter detalhes específicos a respeito do suporte da UWP para o gamepad e o controle remoto, consulte [Interações por gamepad e controle remoto](gamepad-and-remote-interactions.md).|
|Leitores de tela (Narrador)          |O Narrador é um leitor de tela interno para Windows que oferece experiências de interação e funcionalidade exclusivas, mas que ainda depende da navegação e da entrada básica por teclado.
Para obter detalhes sobre o Narrador, consulte [Introdução ao Narrador](https://support.microsoft.com/help/22798/windows-10-narrator-get-started).|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Experiências personalizadas e uso eficiente do teclado
Conforme mencionado, o suporte ao teclado é essencial para garantir que os aplicativos funcionem bem para usuários com habilidades e expectativas diferentes. Recomendamos que você priorize o seguinte.
- Ofereça suporte à interação e à navegação por teclado
    - Certifique-se de que itens acionáveis sejam identificados como paradas de tabulação (e de que itens não acionáveis não o sejam) e a ordem de navegação seja lógica e previsível (consulte [Paradas de tabulação](#tab-stops))
    - Defina o elemento mais lógico como o foco inicial (consulte [Foco inicial](#initial-focus))
    - Forneça navegação por setas para "navegações internas" (consulte [Navegação](#navigation))
- Ofereça suporte a atalhos de teclado
    - Forneça teclas de aceleração para ações rápidas (consulte [Aceleradores](#accelerators))
    - Forneça teclas de acesso para navegar pela interface do usuário do aplicativo (consulte [Teclas de acesso](access-keys.md))

### <a name="focus-visuals-a-namefocus-visual"></a>Elementos visuais de foco <a name="focus-visual">

A UWP oferece suporte a um design visual de foco único que funciona bem para todos os tipos e experiências de entrada.
![Visual de foco](images/keyboard/focus-visual.png)

Um visual de foco:
-   É mostrado quando elemento da interface usuário recebe o foco a partir de um teclado e/ou gamepad/controle remoto
-   É renderizado como uma borda realçada em torno do elemento da interface de usuário para indicar que uma ação pode ser executada
-   Ajuda o usuário a navegar pela interface de usuário do aplicativo sem se perder
-   Pode ser personalizado para o seu aplicativo (Consulte [Visuais do foco de alta visibilidade](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**OBSERVAÇÃO** O foco visual UWP não é o mesmo que o foco retangular do Narrador.

### <a name="tab-stops-a-nametab-stops"></a>Paradas de tabulação <a name="tab-stops">

Para usar um controle (incluindo os elementos de navegação) com o teclado, o controle precisa ter foco. Uma maneira de um controle receber o foco do teclado é torná-lo acessível via navegação de guia, identificando-o como uma parada de guia na ordem de guias do seu aplicativo.

Para um controle ser incluído na ordem de tabulação, a propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) deve ser definida como **verdadeiro** e a propriedade [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) deve ser definida como **verdadeiro.**

Para excluir especificamente um controle da ordem de tabulação, defina a propriedade [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) para **falso.**

Por padrão, a ordem de tabulação reflete a ordem na qual elementos da interface de usuário são criados. Por exemplo, se um `StackPanel` contém um `Button`, uma `Checkbox`, e uma `TextBox`, a ordem de tabulação é `Button`, `Checkbox`, e `TextBox`.

Você pode substituir a ordem de tabulação padrão definindo a propriedade [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461).

#### <a name="tab-order-should-be-logical-and-predictable"></a>A Ordem de tabulação deve ser previsível e lógica

Um modelo de navegação por teclado bem projetado, usando uma ordem de tabulação previsível e lógica, torna seu aplicativo mais intuitivo e ajuda os usuários a explorar, descobrir e acessar as funcionalidades de forma mais eficiente e efetiva.

Todos os controles interativos devem ter paradas de tabulação (a menos que estejam em um [grupo](#control-group)), enquanto controles não interativos, como o `labels`, não devem.

Evite a ordem de tabulação que faz com que o foco fique saltando em seu aplicativo. Por exemplo, uma lista de controles em uma interface de usuário do tipo formulário deve ter uma ordem de tabulação que flui de cima para baixo e da esquerda para a direita.

Consulte a página [Acessibilidade do teclado](../accessibility/keyboard-accessibility.md) para obter mais detalhes sobre como personalizar paradas de tabulação.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Tente coordenar a ordem de tabulação e a ordem visual

Coordenar a ordem de tabulação e a ordem visual (também chamado de ordem de leitura ou ordem de exibição) ajuda a reduzir a confusão de usuários conforme eles navegam pela interface de usuário do aplicativo.

Experimente classificar e apresentar os comandos, controles e conteúdos mais importantes primeiro na ordem de tabulação e na ordem visual. No entanto, a posição de exibição real pode depender do recipiente do layout pai e de certas propriedades dos elementos filho que influenciam o layout. Especificamente, layouts que usam uma metáfora de grade ou uma metáfora de tabela podem ter uma ordem visual bem diferente da ordem de tabulação.

**OBSERVAÇÃO** A ordem visual também depende do local e do idioma.

### <a name="initial-focus-a-nameinitial-focus"></a>Foco inicial <a name="initial-focus">

O foco inicial especifica o elemento da interface de usuário que recebe o foco quando um aplicativo ou uma página é iniciada ou ativada pela primeira vez. Ao usar um teclado, é a partir desse elemento que um usuário inicia a interação com a interface de usuário do aplicativo.

Para aplicativos UWP, o foco inicial é definido para o elemento com o maior [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) que pode receber foco. Elementos filho de controles do recipiente são ignorados. No case de empate, o primeiro elemento na árvore visual recebe o foco.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Definir o foco inicial para o elemento mais lógico

Defina o foco inicial para o elemento da interface de usuário com a primeira ação, ou ação primária, que os usuários provavelmente executarão ao iniciar o aplicativo ou navegar pela página. Veja a seguir alguns exemplos:
-   Um aplicativo de fotos, onde o foco é definido como o primeiro item em uma galeria
-   Um aplicativo de música, onde o foco é definido como o botão Reproduzir

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Não defina o foco inicial em um elemento que exponha um resultado potencialmente negativo, ou até mesmo desastroso.

Esse nível de funcionalidade deve ser uma escolha do usuário. Definir o foco inicial para um elemento com um resultado significativo pode resultar em perda não intencional de dados ou acesso ao sistema. Por exemplo, não defina o foco para o botão Excluir ao navegar até um e-mail.

Consulte a página [Acessibilidade do teclado](../accessibility/keyboard-accessibility.md) para obter mais detalhes sobre como substituir a ordem de tabulação.

### <a name="navigation-a-namenavigation"></a>Navegação <a name="navigation">

A navegação por teclado é geralmente suportada através da tecla Tab e das teclas de seta.

![tecla tab e teclas de seta](images/keyboard/tab-and-arrow.png)

Por padrão, os controles UWP seguem esses comportamentos de teclado básicos:
-   **Tecla Tab** navegar entre controles acionáveis/ativos na ordem de tabulação.
-   **Shift + Tab** navegar entre controles em ordem de tabulação reversa. Se o usuário navegou dentro do controle usando a tecla de seta, o foco é definido para o último valor conhecido dentro do controle.
-   **Teclas de seta** expor a "navegação interna" com controles específicos quando o usuário inicia a "navegação interna", teclas de seta não permitem a navegação fora de um controle. Veja a seguir alguns exemplos:
    -   Teclas Para cima/baixo movem o foco para dentro de `ListView` e `MenuFlyout`
    -   Modificar os valores atualmente selecionados para `Slider` e `RatingsControl`
    -   Mover o cursor para dentro `TextBox`
    -   Expandir/recolher itens dentro de `TreeView`

Use esses comportamentos padrão para otimizar a navegação por teclado do seu aplicativo.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Usar a "navegação interna" com conjuntos de controles relacionados

Fornecer a navegação pelas teclas de seta em um conjunto de controles relacionados reforça sua relação dentro da organização geral da interface de usuário do seu aplicativo.

Por exemplo, o controle `ContentDialog` mostrado aqui fornece navegação interna por padrão para uma linha horizontal de botões (para controles personalizados, consulte a seção [Grupo de Controles](#control-group)).

![exemplo de diálogo](images/keyboard/dialog.png)

***A interação com um conjunto de botões relacionados se torna mais fácil com a navegação pelas teclas de seta***

Se os itens são exibidos em uma única coluna, as setas Para cima/Para baixo navegam pelos itens. Se os itens são exibidos em uma única linha, as setas Direita/Esquerda navegam pelos itens. Se os itens são várias colunas, todas as 4 teclas de seta fazem a navegação.

#### <a name="make-a-set-of-related-controls-a-single-tab-stop"></a>Faça um conjunto de controles relacionados como uma única parada de tabulação

Ao tornar um conjunto de controles relacionados, ou complementares, uma única parada de tabulação, é possível minimizar o número total de paradas de tabulação em seu aplicativo.

Por exemplo, as imagens a seguir mostram dois controles `ListView` empilhados. A imagem à esquerda mostra a navegação por teclas de seta, usada com uma parada de tabulação para navegar entre controles `ListView`, enquanto a imagem da direita mostra como a navegação entre elementos filho pode ser mais fácil e mais eficiente através da eliminação da necessidade de percorrer os controles pai com a tecla tab.


<table>
  <td>![seta e tab](images/keyboard/arrow-and-tab.png)</td>
  <td>![somente seta](images/keyboard/arrow-only.png)</td>
</table>

***A interação com dois controles ListView empilhados pode ser mais fácil e eficiente através da eliminação da parada de tabulação, navegando apenas através das teclas de seta.***

Visite a seção [Grupo de Controles](#control-group) para aprender como aplicar os exemplos de otimização na interface de usuário do seu aplicativo.

### <a name="interaction-and-commanding"></a>Interação e comandos

Uma vez que um controle tem o foco, um usuário pode interagir com ele e invocar qualquer funcionalidade associada utilizando entradas por teclado específicas.

#### <a name="text-entry"></a>Entrada de texto

Para os controles especificamente projetados para entrada de texto, como o `TextBox` e o `RichEditBox`, toda entrada por teclado é usada para inserir texto ou navegar por ele, garantindo prioridade sobre outros comandos do teclado. Por exemplo, o menu suspenso para um controle `AutoSuggestBox` não reconhece a tecla **Espaço** como um comando de seleção.

![entrada de texto](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Tecla de espaço

Quando não estiver em modo de entrada de texto, a tecla **Espaço** invoca a ação ou comando associado ao controle em foco (da mesma forma que um toque no touch ou um clique com um mouse).

![tecla de espaço](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Tecla Enter

A tecla **Enter** pode executar uma variedade de interações comuns do usuário, dependendo do controle com foco:
-   Ativa controles de comando, como um `Button` ou um `Hyperlink`. Para evitar a confusão do usuário final, a tecla **Enter** também ativa controles que se parecem com controles de comando, como o `ToggleButton` ou o `AppBarToggleButton`.
-   Exibe a interface de seleção para controles como o `ComboBox` e o `DatePicker`. A tecla **Enter** também confirma e fecha a interface de seleção.
-   Ativa controles de lista como o `ListView`, o `GridView` e o `ComboBox`.
    -   A tecla **Enter** executa a ação de seleção da mesma forma que a tecla **Espaço** para itens de lista e grade, a menos que exista uma ação adicional associada a esses itens (abrir uma nova janela).
    -   Se uma ação adicional está associada ao controle, a tecla **Enter** executa a ação adicional e a tecla **Espaço** realiza a ação de seleção.

**OBSERVAÇÃO** As teclas **Enter** e **Espaço** nem sempre executam a mesma ação, mas geralmente sim.

![tecla Enter](images/keyboard/enter-key.png)

A tecla Esc permite ao usuário cancelar a interface de usuário temporária (juntamente com todas as ações em andamento nessa interface).

Exemplos dessa experiência incluem:
-   O usuário abre um `ComboBox` com um valor selecionado e usa as teclas de seta para mover a seleção de foco para um novo valor. Pressionar o tecla Esc fecha o `ComboBox` e redefine o valor selecionado de volta para o valor original.
-   O usuário invoca uma ação de exclusão permanente para um e-mail e recebe o aviso para confirmar a ação através de um `ContentDialog`. O usuário decide que essa não é a ação desejada e pressiona a tecla **Esc** para fechar a caixa de diálogo. Como a tecla **Esc** está associada ao botão **Cancelar**, a caixa de diálogo é fechada e a ação é cancelada. A tecla **Esc** afeta somente a interface de usuário temporária, ela não fecha, ou retorna a navegação, na interface do aplicativo.

![Tecla Esc](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Teclas Home e End

As teclas **Home** e **End** permitem que um usuário vá rapidamente até o início ou final de uma região da interface de usuário.

Exemplos dessa experiência incluem:
-   Para os controles `ListView` e `GridView`, a tecla **Home** move o foco para o primeiro elemento e o coloca em exibição, enquanto a tecla **End** move o foco para o último elemento, também colocando-o em exibição.
-   Para um controle `ScrollView`, a tecla **Home** rola até a parte superior da região, enquanto a tecla **End** rola até a parte inferior (o foco não é alterado).

![teclas home e end](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Teclas Page up e Page down

As teclas **Page** permitem ao usuário rolar pela região da interface de usuário através de incrementos discretos.

Por exemplo, para os controles `ListView` e `GridView`, a tecla **Page up** sobe a visualização da região em uma "página" (normalmente a altura do visor) e move o foco para a parte superior da região. Alternativamente, a tecla **Page down** abaixa a visualização da região em uma página e move o foco para a parte inferior da região.

![teclas page up e page down](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Atalhos de teclado

Atalhos de teclado podem tornar seu aplicativo mais fácil e mais eficiente de usar.

Além de implementar a navegação e ativação por teclado em seu aplicativo, é recomendável implementar atalhos para as funcionalidades do seu aplicativo. A navegação por tabulação fornece um bom nível básico de suporte ao teclado, mas com formulários complexos, você poderia desejar adicionar suporte às teclas de atalho também. Isso pode tornar o seu aplicativo mais eficiente para usar, mesmo para as pessoas que usam dispositivos de teclado e de ponteiro.

Um atalho é uma combinação de teclas que aumenta a produtividade, oferecendo uma maneira eficiente para o usuário acessar as funcionalidades do aplicativo. Existem dois tipos de atalho:
-   Uma [tecla aceleradora](#accelerators) é um atalho para um comando do aplicativo. Seu aplicativo pode ter uma interface do usuário que corresponde exatamente ao comando. As teclas aceleradoras consistem na tecla Ctrl, mais uma tecla de letra.
-   Uma [tecla de acesso](#access-keys) é um atalho para um elemento da interface de usuário no seu aplicativo. As teclas de acesso consistem na tecla Alt, mais uma tecla de letra.

Visite esta página para a listagem completa de [atalhos de teclado para o Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts), assim como os [atalhos de teclado específicos do aplicativo](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps) usado por aplicativos desenvolvidos pela Microsoft.

#### <a name="accelerators-a-nameaccelerators"></a>Aceleradoras <a name="accelerators">

As aceleradoras ajudam os usuários a realizar ações comuns existentes no aplicativo rapidamente. Oferecer teclas aceleradoras consistentes, as quais usuários consigam lembrar facilmente e usar em diversos aplicativos que ofereçam tarefas similares, é muito importante para tornar a aceleradora útil e poderosa.

Exemplos de aceleradoras:
-   Pressionar as teclas Ctrl + N em qualquer lugar no aplicativo Mail cria um novo item de e-mail.
-   Pressionar as teclas Ctrl + E em qualquer lugar nos aplicativos Edge e Loja permite que o usuário insira rapidamente texto em uma caixa de pesquisa.

As aceleradoras possuem as seguintes características:
-   Elas usam principalmente as sequências de teclas Ctrl e Função (as teclas de atalho do sistema Windows também usam Alt+teclas não alfanuméricas e o logotipo do Windows).
-   Elas são principalmente para eficiência de usuários avançados.
-   Elas são atribuídas apenas a comandos usados mais comumente.
-   Elas devem ser memorizadas e estão documentadas apenas em menus, dicas de ferramentas e na Ajuda.
-   Elas têm efeito em todo o programa, mas não têm nenhum efeito caso não se apliquem.
-   Elas devem ser atribuídas consistentemente, uma vez que são memorizadas e não diretamente documentadas.

#### <a name="access-keys-a-nameaccess-keys"></a>Teclas de acesso <a name="access-keys">

As teclas de acesso oferecem, tanto para usuários com necessidades de acessibilidade como para usuários avançados, um modo eficiente e efetivo de navegação na interface de usuário do aplicativo.

Consulte a página [Teclas de acesso](access-keys.md) para obter informações mais detalhadas sobre o suporte às teclas de acesso na UWP.

As teclas de acesso ajudam os usuários com deficiências motoras e aqueles capazes de pressionar apenas uma tecla por vez para agir sobre um item específico da interface de usuário. Além disso, as teclas de acesso podem ser usadas para comunicar teclas de atalho adicionais, ajudando usuários avançados a executar ações mais rapidamente.

As teclas de acesso possuem as seguintes características:
-   Elas usam a tecla Alt, mais uma tecla alfanumérica.
-   Elas são principalmente para acessibilidade.
-   Elas estão documentadas diretamente na interface de usuário adjacente ao controle através do uso de [Dicas de Tecla](access-keys.md).
-   Elas afetam apenas a janela atual, navegando ao menu de itens ou controle correspondente.
-   Elas não são atribuídas consistentemente porque nem sempre podem. No entanto, as teclas de acesso devem ser atribuídas consistentemente a comandos usados comumente, em especial os botões de confirmação.
-   Elas são localizadas.

#### <a name="common-keyboard-shortcuts"></a>Atalhos de teclado comuns

A tabela a seguir é uma pequena amostra de comandos de teclado usados com frequência. Para obter uma lista completa dos comandos de teclado, consulte [Teclas de atalho de teclado do Windows](https://support.microsoft.com/kb/126449).

| Ação                               | Comando de tecla                                      |
|--------------------------------------|--------------------------------------------------|
| Selecionar tudo                           | Ctrl+A                                           |
| Selecionar continuamente                  | Shift+Tecla de seta                                  |
| Salvar                                 | Ctrl+S                                           |
| Localizar                                 | Ctrl+F                                           |
| Impressão                                | Ctrl+P                                           |
| Copiar                                 | Ctrl+C                                           |
| Recortar                                  | Ctrl+X                                           |
| Colar                                | Ctrl+V                                           |
| Desfazer                                 | Ctrl+Z                                           |
| Próxima guia                             | Ctrl+Tab                                         |
| Fechar a guia                            | Ctrl+F4 ou Ctrl+W                                |
| Zoom semântico                        | Ctrl++ ou Ctrl+-                                 |

## <a name="advanced-experiences"></a>Experiências avançadas

Nesta seção, vamos discutir algumas das experiências de interação por teclado mais complexas compatíveis com aplicativos UWP, junto com alguns comportamentos que você deve conhecer para quando o aplicativo é usado em dispositivos diferentes e com ferramentas diferentes.

### <a name="control-group-a-namecontrol-group"></a>Grupo de controles <a name="control-group">

Você pode agrupar um conjunto de controles relacionados, ou complementares, em um "grupo de controles" (ou área direcional), o que permite a "navegação interna" usando as teclas de seta. O grupo de controles pode ser uma única parada de tabulação, ou você pode especificar várias paradas de tabulação dentro do grupo de controles.

#### <a name="arrow-key-navigation"></a>Navegação por teclas de seta

Os usuários esperam suporte para navegação com teclas de seta quando existe um grupo de controles semelhantes, relacionados, em uma região da interface de usuário:
-   `AppBarButtons` em uma `CommandBar`
-   `ListItems` ou `GridItems` dentro da `ListView` ou `GridView`
-   `Buttons` dentro do `ContentDialog`

Os controles UWP suportem a navegação pelas setas por padrão. Para layouts personalizados e grupos de controles, use `XYFocusKeyboardNavigation="Enabled"` para fornecer um comportamento semelhante.

Considere a adição de suporte para a navegação por setas quando você tiver os seguintes controles:

<table>
  <tr>
    <td>
      <p>![caixa de diálogo](images/keyboard/dialog.png)</p>
      <p>**Botões**</p>
      <p>![radiobutton](images/keyboard/radiobutton.png)</p>
      <p>**RadioButtons**</p>     
    </td>
    <td>
      <p>![appbar](images/keyboard/appbar.png)</p>
      <p>**AppBarButtons**</p>
      <p>![itens de lista e grade](images/keyboard/list-and-grid-items.png)</p>
      <p>**ListItems e GridItems**</p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Paradas de tabulação

Dependendo da funcionalidade e do layout do seu aplicativo, a melhor opção de navegação para um grupo de controles pode ser uma única parada de tabulação com navegação por setas para os elementos filho, várias paradas de tabulação ou alguma combinação.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Usar várias paradas de tabulação e as teclas de seta para botões

Os usuários de acessibilidade dependem de regras de navegação por teclado bem estabelecidas, que normalmente não usa as teclas de seta para navegar por uma coleção de botões. No entanto, os usuários sem deficiências visuais podem achar que o comportamento natural.

Um exemplo de comportamento UWP padrão neste caso é o `ContentDialog`. Enquanto as teclas de seta podem ser usadas para navegar entre os botões, cada botão também é uma parada de tabulação.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Atribuir parada de tabulação única para padrões da interface de usuário familiares

Em casos onde o layout segue um padrão de interface de usuário conhecido para grupos de controles, atribuir uma única parada de tabulação ao grupo pode melhorar a eficiência da navegação para os usuários.

Os exemplos incluem:
-   `RadioButtons`
-   Várias `ListViews` que se parecem e se comportam como uma única `ListView`
-   Qualquer interface de usuário feita para se parecer e se comportar como grade de blocos (como os blocos do menu Iniciar)

#### <a name="specifying-control-group-behavior"></a>Especificando o comportamento do grupo de controles

Use os seguintes APIs para dar suporte ao comportamento personalizado do grupo de controles (todos serão abordados com mais detalhes neste tópico mais adiante):

-   [XYFocusKeyboardNavigation](custom-keyboard-interactions.md#xyfocuskeyboardnavigation) habilita a navegação por setas entre os controles
-   [TabFocusNavigation](custom-keyboard-interactions.md#tab-navigation) indica se existem várias paradas de tabulação ou apenas uma
-   [FindFirstFocusableElement e FindLastFocusableElement](managing-focus-navigation.md#findfirstfocusableelement) definem o foco sobre o primeiro item através da tecla **Home** e sobre o último item através da tecla **End**, respectivamente

A imagem a seguir mostra um comportamento de navegação por teclado intuitivo para um grupo de controles de botões de rádio associados. Nesse caso, recomendamos uma única parada de tabulação para o grupo de controles, navegação interna entre os botões de rádio através das teclas de seta, tecla **Home** associada ao primeiro botão, e tecla **End** associada ao último botão.

![reunindo tudo](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Teclado e o Narrador

O Narrador é uma ferramenta de acessibilidade da interface de usuário voltada para os usuários do teclado (também há suporte para outros tipos de entrada). No entanto, as funcionalidades do Narrador excedem as interações por teclado compatíveis com aplicativos UWP, sendo necessário o cuidado extra ao projetar seu aplicativo UWP para o Narrador. (A [página Noções básicas do Narrador](https://support.microsoft.com/help/22808/windows-10-narrator-learning-basics) o orientará durante a experiência de usuário do Narrador.)

Algumas das diferenças entre os comportamentos de teclado UWP e aqueles suportados pelo Narrador incluem:
-   Combinações extras de teclas para a navegação, para elementos da interface de usuário que não são expostos através da navegação por teclado padrão, como o Caps lock + teclas de seta para leitura dos rótulos dos controles.
-   Navegação para itens desativados. Por padrão, os itens desativados não são expostos através da navegação por teclado padrão.
    -   Controle "visualizações" para navegação mais rápida com base na granularidade da interface de usuário. Os usuários podem navegar para itens, caracteres, palavras, linhas, parágrafos, links, títulos, tabelas, marcos e sugestões. A navegação por teclado padrão expõe esses objetos como uma lista simples, o que pode tornar a navegação complicada, a menos que você ofereça teclas de atalho.

#### <a name="case-study--autosuggestbox-control"></a>Estudo de caso – controle AutoSuggestBox

O botão de pesquisa para o `AutoSuggestBox` não está acessível através da navegação por teclado padrão usando a tecla tab e as setas, pois o usuário pode pressionar a tecla **Enter** para enviar a consulta de pesquisa. No entanto, ele está acessível através do Narrador, quando o usuário pressiona Caps Lock + uma tecla de seta.

![sugestão automática de foco do teclado](images/keyboard/auto-suggest-keyboard.png)

***Com o teclado***, *os usuários usam* a tecla ***Enter*** *para enviar a consulta de pesquisa*

<table>
  <tr>
    <td>
      <p>![sugestão automática de foco do Narrador](images/keyboard/auto-suggest-narrator-1.png)</p>
      <p>**Com o Narrador,** *os usuários podem usar a tecla Enter para enviar a consulta de pesquisa*</P>
    </td>
    <td>
      <p>![sugestão automática de foco do Narrador na pesquisa](images/keyboard/auto-suggest-narrator-2.png)</p>
      <p>*O usuário também é capaz de acessar o botão de pesquisa através do Caps Lock + seta Direita, pressionando a tecla Espaço em seguida*</p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>O teclado e o gamepad do Xbox e controle remoto

Os gamepads do Xbox e os controles remotos suportam diversas experiências e comportamentos de teclado UWP. No entanto, devido à falta das várias opções de teclas disponíveis em um teclado, o gamepad e o controle remoto carecem de muitas otimizações do teclado (o controle remoto é ainda mais limitado que o gamepad).

Consulte [projetando para Xbox e TV](designing-for-tv.md#gamepad-and-remote-control) para obter mais detalhes sobre o suporte UWP para entradas de gamepad e controle remoto.

Veja a seguir algum mapeamentos de teclas entre teclado, gamepad e controle remoto.

| **Teclado**  | **Gamepad**                         | **Controle remoto**  |
|---------------|-------------------------------------|---------------------|
| Espaço         | Botão A                            | Botão de seleção       |
| Enter         | Botão A                            | Botão de seleção       |
| Esc        | Botão B                            | Botão Voltar         |
| Home/End      | N/D                                 | N/D                 |
| Page Up/Down  | RT e LT para rolagem vertical, RB e LB para rolagem horizontal   | N/D                 |

Algumas diferenças importantes que você deve conhecer ao projetar seu aplicativo UWP para uso com gamepad e controle remoto incluem:
-   Entrada de texto requer que o usuário pressione A para ativar um controle de texto.
-   Navegação por foco não está limitada aos grupos de controles, os usuários podem navegar livremente para qualquer elemento focalizável da interface de usuário no aplicativo.

    **OBSERVAÇÃO** O foco pode se mover para qualquer elemento da interface na direção desejada a menos que esteja em uma interface de usuário de sobreposição ou que o [envolvimento do foco](designing-for-tv.md#focus-engagement) esteja especificado, o que impede que o foco entre/saia da região até que o botão A seja ativado/desativado. Para obter mais informações, consulte a seção [navegação direcional](#directional-navigation).
-   O D-pad e os botões do joystick esquerdo são usados para mover o foco entre controles e para navegação interna.

    **OBSERVAÇÃO** O gamepad e o controle remoto navegam apenas para itens que estão na mesma ordem visual que a tecla direcional pressionada. A navegação é desabilitada naquela direção quando não existe um elemento subsequente que possa receber o foco. Dependendo da situação, os usuários de teclado nem sempre têm essa restrição. Consulte a seção [Otimização de teclado integrada](#built-in-keyboard-optimization) para obter mais informações.

#### <a name="directional-navigation-a-namedirectional-navigation"></a>Navegação direcional <a name="directional-navigation">

A navegação direcional é gerenciada pela classe auxiliar [Gerenciador de Foco](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager), que analisa a tecla direcional pressionada (tecla de seta, D-pad) e tenta mover o foco na direção visual correspondente.

Ao contrário do teclado, quando um aplicativo abandona o [Modo de Mouse](designing-for-tv.md#mouse-mode), a navegação direcional é aplicada em todo o aplicativo para o gamepad e o controle remoto. Visite o [artigo sobre interação e navegação de foco no plano XY](designing-for-tv.md#xy-focus-navigation-and-interaction) para obter mais detalhes sobre as otimizações da navegação direcional para gamepad e controle remoto.

**OBSERVAÇÃO** A navegação usando a tecla Tab do teclado não é considerada navegação direcional. Para obter mais informações, consulte a seção [Paradas de tabulação](#tab-stops).

<table>
  <tr>
    <td>
      <p>![navegação direcional](images/keyboard/directional-navigation.png)</p>
      <p>***Navegação direcional suportada*** </br>*Usando as teclas direcionais (setas do teclado, gamepad e controle remoto D-pad), o usuário pode navegar entre controles diferentes.*</p>
    </td>
    <td>
      <p>![sem navegação direcional](images/keyboard/no-directional-navigation.png)</p>
      <p>***Navegação direcional não suportada*** </br>*O usuário não pode navegar entre diferentes controles usando as teclas direcionais. Outros métodos de navegação entre controles (tecla tab) não são afetados.*</p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization-a-namebuilt-in-keyboard-optimization"></a>Otimização de teclado integrada <a name="built-in-keyboard-optimization">

Dependendo do layout e controles usados, aplicativos UWP podem ser otimizados especificamente para entrada por teclado.

O exemplo a seguir mostra um grupo de itens de lista, itens de grade e itens de menu que foram atribuídos a uma única parada de tabulação (consulte a seção [Paradas de tabulação](#tab-stops)). Quando o grupo tem o foco, a navegação interna é realizada com as teclas direcionais de seta na ordem visual correspondente (consulte a seção [Navegação](#navigation)).

![navegação por setas em coluna única](images/keyboard/single-column-arrow.png)

***Navegação por Setas em Coluna Única***

![navegação por setas em linha única](images/keyboard/single-row-arrow.png)

***Navegação por Setas em Linha Única***

![navegação por setas em colunas e linhas múltiplas](images/keyboard/multiple-column-and-row-navigation.png)

***Navegação por Setas em Colunas/Linhas Múltiplas***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Envolvendo Itens Homogêneos de Exibição de Lista e Grade

A navegação direcional nem sempre é a maneira mais eficiente para navegar entre os itens List e GridView de várias linhas e colunas.

**OBSERVAÇÃO** Itens de menu são, normalmente, listas de coluna única, mas regras especiais de foco podem se aplicar em alguns casos (consulte [interface de usuário do pop-up](#popup-ui)).

Objetos de lista e grade podem ser criados com várias linhas e colunas. Eles normalmente estão nas ordens linha-principal (onde os itens preenchem primeiro toda uma linha antes de mover para a próxima) ou coluna-principal (onde os itens preenchem toda uma coluna antes de mover para a próxima coluna). As ordens de linha ou coluna principal dependem da direção de rolagem e você deve garantir que a ordem dos itens não entre em conflito com essa direção.

Na ordem linha-principal (onde os itens preenchem da esquerda para direita, de cima para baixo), quando o foco está no último item de uma linha e a seta Direita é pressionada, o foco é movido para o primeiro item da próxima linha. Esse mesmo comportamento ocorre na ordem inversa: quando o foco for definido para o primeiro item em uma linha e a seta Esquerda for pressionada, o foco é movido para o último item da linha anterior.

Na ordem coluna-principal (onde os itens preenchem de cima para baixo, da esquerda para a direita), quando o foco está no último item de uma coluna e o usuário pressiona a seta Para baixo, o foco é movido para o primeiro item da próxima coluna. Esse mesmo comportamento ocorre na ordem inversa: quando o foco for definido para o primeiro item em uma coluna e a seta Para cima for pressionada, o foco é movido para o último item da coluna anterior.

<table>
  <tr>
    <td>
      <p>![navegação por teclado linha principal](images/keyboard/row-major-keyboard.png)</p>
      <p>***Navegação por teclado na ordem linha principal***</p>
    </td>
    <td>
      <p>![navegação por teclado coluna principal](images/keyboard/column-major-keyboard.png)</p>
      <p>***Navegação por teclado na ordem coluna principal***</p>
    </td>
  </tr>
</table>

#### <a name="popup-ui-a-namepopup-ui"></a>Interface de usuário pop-up <a name="popup-ui">

Conforme mencionado, você deve tentar garantir que a navegação direcional corresponda à ordem visual dos controles na interface de usuário do seu aplicativo.

Alguns controles, como o `ContextMenu`, o `AppBarOverflowMenu` e o `AutoSuggest`, incluem um menu pop-up exibido em uma localização e uma direção relativa ao controle primário (com base no espaço de tela disponível). Por exemplo, quando não há espaço suficiente para que o menu seja aberto em baixo (a direção padrão), ele é aberto em cima. Não há nenhuma garantia de que o menu será aberto sempre na mesma direção.

<table>
  <td>![barra de comandos abre para baixo com a seta para baixo](images/keyboard/command-bar-open-down.png)</td>
  <td>![barra de comandos abre para cima com a seta para baixo](images/keyboard/command-bar-open-up.png)</td>
</table>

Para esses controles, quando o menu for aberto pela primeira vez (e nenhum item foi selecionado pelo usuário), a seta Para baixo sempre define o foco no primeiro item e a seta Para cima sempre define o foco no último item do menu. Da mesma forma, quando o último item é selecionado e a seta Para baixo é pressionada, o foco é movido para o primeiro item do menu e quando o primeiro item é selecionado e a seta Para cima é pressionada, o foco é movido para o último item do menu.

Você deve tentar imitar esses mesmos comportamentos em seus controles personalizados. Amostra de código sobre como implementar esse comportamento pode ser encontrada na documentação [Gerenciar a navegação por foco](managing-focus-navigation.md#popup-ui-code-sample).

## <a name="test-your-app"></a>Testar seu aplicativo

Teste seu aplicativo com todos os dispositivos de entrada suportados para garantir que a navegação pelos elementos da interface de usuário aconteça de maneira coerente e intuitiva para que nenhum elemento não esperado interfira na ordem de tabulação desejada.

## <a name="related-articles"></a>Artigos relacionados
* [Eventos de teclado](keyboard-events.md)
* [Identificar dispositivos de entrada](identify-input-devices.md)
* [Responder à presença do teclado virtual](respond-to-the-presence-of-the-touch-keyboard.md)
* [Amostra de elementos visuais do foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

## <a name="appendix"></a>Apêndice

### <a name="software-keyboard-a-nametouch-keyboard"></a>Teclado de software <a name="touch-keyboard">

O teclado de software é um teclado exibido na tela, que o usuário pode utilizar no lugar do teclado físico para digitar e inserir dados através do toque, caneta/stylus ou outro dispositivo apontador (não é necessária uma tela sensível ao toque). Na tela sensível ao toque, esses teclados também podem ser tocados diretamente para inserir texto. Em dispositivos Xbox One, teclas individuais precisam ser selecionadas, movendo o visual do foco ou utilizando teclas de atalho no gamepad ou controle remoto.

![Teclado virtual do Windows 10](images/keyboard/kbdpcdefault.png)

***Teclado Virtual do Windows 10***

![Teclado virtual de celular do Windows 10](images/keyboard/kbdwpdefault.png)

***Teclado Virtual de Windows Phone 10***

![Teclado virtual do Xbox one](images/keyboard/xbox-onscreen-keyboard.png)

***Teclado Virtual do Xbox One***

Dependendo do dispositivo, o teclado de software aparece quando um campo de texto ou outro controle de texto editável é focalizado, ou quando o usuário o ativa manualmente através do **Centro de Notificações**:

![ícone de teclado virtual no centro de notificações](images/keyboard/touch-keyboard-notificationcenter.png)

Se o seu aplicativo define o foco de forma programática para um controle de entrada de texto, o teclado virtual não é invocado. Isso elimina comportamentos inesperados não instigados diretamente pelo usuário. No entanto, o teclado é ocultado automaticamente quando o foco é movido por meio de programação para um controle de entrada que não é de texto.

Normalmente, o teclado virtual permanece visível enquanto o usuário navega entre controles em um formulário. Esse comportamento pode variar com base nos outros tipos de controle no formulário.

A seguir há uma lista de controles que não são de edição que podem receber o foco durante uma sessão de entrada de texto por meio do teclado virtual sem descartar o teclado. Ao invés de mover desnecessariamente a interface do usuário e possivelmente desorientar o usuário, o teclado virtual permanece à vista, pois pode ser que o usuário alterne entre esses controles e a entrada de texto com o teclado virtual.

-   Caixa de seleção
-   Caixa de combinação
-   Botão de opção
-   Barra de rolagem
-   Árvore
-   Item de árvore
-   Menu
-   Barra de menu
-   Item de menu
-   Barra de ferramentas
-   Lista
-   Item de lista

Veja aqui exemplos de modos diferentes do teclado virtual. A primeira imagem ilustra o layout padrão, a segunda é o layout em miniatura (que pode não estar disponível em todos os idiomas).

![o teclado virtual no modo de layout padrão](images/keyboard/touchkeyboard-standard.png)

***O teclado virtual no modo de layout padrão***

![o teclado virtual no modo de layout expandido](images/keyboard/touchkeyboard-expanded.png)

***O teclado virtual no modo de layout expandido***

![o teclado virtual no modo de layout em miniatura](images/keyboard/touchkeyboard-thumb.png)

***O teclado virtual no modo de layout em miniatura***

![o teclado virtual no modo de layout numérico em miniatura](images/keyboard/touchkeyboard-numeric-thumb.png)

***O teclado virtual no modo de layout numérico em miniatura***

Interações de teclado bem-sucedidas permitem que os usuários utilizem cenários básicos de aplicativos apenas com o teclado, ou seja, os usuários podem acessar todos os elementos interativos da interface do usuário e ativar a funcionalidade padrão. Diversos fatores podem afetar o grau de sucesso, incluindo a navegação por teclado, as teclas de acesso para acessibilidade e as teclas de aceleração (atalho) para usuários avançados.

**OBSERVAÇÃO**  O teclado virtual não oferece suporte à alternância e à muitos comandos do sistema.

#### <a name="on-screen-keyboard-a-nameosk"></a>Teclado Virtual <a name="osk">
Como o teclado de software, o Teclado Virtual é um teclado de software, visual, que pode ser usado no lugar do teclado físico para digitar e inserir dados através do toque, mouse, caneta/stylus ou outro dispositivo apontador (não é necessária uma tela sensível ao toque). O teclado virtual é fornecido para sistemas que não têm um teclado físico ou para usuários cujos problemas de mobilidade impedem o uso de dispositivos de entrada físicos tradicionais. O teclado virtual emula a maior parte, se não toda a funcionalidades de um teclado de hardware.

O Teclado Virtual pode ser ativado pela página Teclado em Configurações &gt; Facilidade de acesso.

**OBSERVAÇÃO** O Teclado Virtual possui prioridade sobre o teclado virtual, que não será exibido caso o Teclado Virtual esteja presente.

![o teclado virtual](images/keyboard/osk.png)

***Teclado Virtual***

Visite [a página Teclado Virtual](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard) para obter mais detalhes sobre o Teclado Virtual.
