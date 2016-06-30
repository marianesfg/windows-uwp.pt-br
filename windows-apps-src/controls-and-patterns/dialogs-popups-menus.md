---
author: Jwmsft
Description: "Um submenu é um pop-up leve que é usado para mostrar temporariamente a interface do usuário relacionada ao que o usuário está fazendo no momento."
title: "Menus de contexto e caixas de diálogo"
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: e268a5facebbdb80d7cc5cdd52c1a6f944ef7d00

---
# Menus, caixas de diálogo, submenus e pop-ups

Menus, caixas de diálogo, menus suspensos e pop-ups exibem elementos transitórios da interface do usuário que aparecem quando o usuário os solicita ou quando acontece algo que requer notificação ou aprovação.

<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [Classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
-   [Classe Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)
-   [Classe ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)

Um menu de contexto fornece ações instantâneas ao usuário. Ele pode ser preenchido com comandos de texto. Os menus de contexto podem ser ignorados com o toque ou clique em algum lugar fora do menu.

Caixas de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do aplicativo. As caixas de diálogo bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignoradas. Elas muitas vezes solicitam algum tipo de ação do usuário.

Um submenu é um pop-up contextual leve que exibe a interface do usuário relacionada ao que o usuário está fazendo. Ele inclui a lógica de colocação e dimensionamento, e pode ser usado para exibir um controle oculto, mostrar mais detalhes de um item ou pedir que o usuário confirme uma ação. Os submenus podem ser ignorados tocando ou clicando em algum lugar fora do popup.


## Este é o controle correto?

Menus de contexto podem ser usados para:

-   Ações contextuais.
-   Comandos para um objeto no qual se deve atuar, mas que não pode ser selecionado.

As caixas de diálogo podem ser usadas para:

- Expressar informações importantes que o usuário deve ler e confirmar antes de prosseguir.
- Solicitar uma ação clara do usuário ou comunicar a mensagem importante que o usuário deve confirmar. Os exemplos incluem:
  - Quando a segurança do usuário pode ser comprometida
  - Quando o usuário está prestes a alterar um ativo valioso de forma permanente
  - Quando o usuário está prestes a excluir um ativo valioso
  - Para confirmar uma compra no aplicativo
- Mensagens de erro que se aplicam ao contexto geral do aplicativo, como um erro de conectividade.
- Perguntas, quando o aplicativo precisar fazer uma pergunta de bloqueio ao usuário, por exemplo, quando o aplicativo não puder escolher em nome do usuário. Uma pergunta de bloqueio não pode ser ignorada ou adiada e deve oferecer ao usuário opções bem-definidas.

Os submenus podem ser usados para:

-   Interface do usuário contextual e transitória.
-   Avisos e confirmações, incluindo aqueles relacionados às ações potencialmente destrutivas.
-   Exibindo mais informações, como detalhes ou descrições mais longas de um item na página.


## Exemplos

Aqui está um menu de contexto de painel único típico com uma lista curta de comandos simples. Ela pode rolar, se necessário. Use separadores conforme necessário para agrupar comandos semelhantes.

![Exemplo de um menu de contexto típico](images/controls_contextmenu_singlepane.png)

Um menu de contexto em cascata pode ser usado para uma coleção mais abrangente de comandos. Ele possui vários níveis de submenu e pode rolar. Use separadores conforme necessário para agrupar comandos semelhantes.

![Exemplo de um menu de contexto em cascata](images/controls_contextmenu_cascading.png)

Este é um exemplo de uma caixa de diálogo de confirmação de tela inteira, com um único botão. Com esse tipo de caixa de diálogo, é apresentada ao usuário uma quantidade razoável de informações que ele deve ler antes de pressionar o botão para continuar.

![Exemplo de uma caixa de diálogo de página inteira, com um único botão](images/controls_dialog_singlebutton.png)

Aqui está um exemplo de uma caixa de diálogo de dois botões que apresenta ao usuário uma opção A/B. Em geral, a quantidade de informações apresentadas nesta caixa de diálogo é pequena.

![Exemplo de uma caixa de diálogo botão inteira](images/controls_dialog_twobutton.png)

## Modal vs light dismiss

As caixas de diálogo são modais, o que significa que elas bloqueiam toda a interação com o aplicativo até que o usuário selecione um botão de caixa de diálogo. Para reforçar visualmente seu comportamento modal, as caixas de diálogo desenham uma camada de sobreposição que obscurece parcialmente a interface de usuário do aplicativo inacessível temporariamente.

**Observação** Quando Cancelar é uma das opções disponíveis da caixa de diálogo, os aplicativos podem optar por permitir que os usuários ignorem a caixa de diálogo, pressionando a tecla Escape. Esse comportamento não é incorporado ao controle, mas é um atalho comumente implementado.

Submenus e menus de contexto são controles light dismiss, o que significa que os usuários podem escolher entre uma variedade de ações para ignorar rapidamente as interfaces do usuário transitórias. Essas interações destinam-se a ser leves e sem bloqueio. As ações light dismiss incluem
- Clicar ou tocar fora da interface de usuário transitória
- Pressionar a tecla Escape
- Pressionar o botão Voltar
- Redimensionar a janela do aplicativo
- Alterar a orientação do dispositivo


## Diretrizes de utilização de caixa de diálogo

-   Identifique claramente o problema ou o objetivo do usuário na primeira linha do texto da caixa de diálogo.
-   O título da caixa de diálogo é a principal instrução e é opcional.
    -   Use um título curto para explicar o que as pessoas precisam fazer na caixa de diálogo. Títulos longos não terão quebra de linha e serão truncados.
    -   Se você está usando a caixa de diálogo para passar uma mensagem simples, de erro ou fazer uma pergunta, pode omitir o título. Utilize o texto do conteúdo para passar as informações principais.
    -   O título deve estar diretamente relacionado às opções de botão.
-   O conteúdo da caixa de diálogo contém o texto descritivo e é obrigatório.
    -   Apresente a mensagem, o erro ou a pergunta de bloqueio da maneira mais simples possível.
    -   Se um título da caixa de diálogo for usado, use a área de conteúdo para fornecer mais detalhes ou definir a terminologia. Não repita o título com palavras ligeiramente diferentes.
-   Pelo menos um botão da caixa de diálogo deve aparecer.
    -   Botões são o único mecanismo para que os usuários ignorem a caixa de diálogo.
    -   Use botões com texto que identifiquem respostas específicas para a instrução ou o conteúdo principal. Um exemplo é "Você permite que NomeDoAplicativo acesse o sua localização?", seguido pelos botões "Permitir" e "Bloquear". Respostas específicas podem ser entendidas mais rapidamente, resultando na tomada de decisão eficiente.
-   Caixas de diálogo de erro exibem a mensagem de erro na caixa de diálogo, juntamente com quaisquer informações pertinentes. O único botão usado em uma caixa de diálogo de erro deve ser "Fechar" ou uma ação semelhante.
-   Não use caixas de diálogo para erros que são contextuais a um local específico da página, como erros de validação (em campos de senha, por exemplo). Use a própria tela do aplicativo para mostrar erros embutidos.

## Menus de contexto e submenus

Submenus e menus de contexto são controles relacionados que compartilham comportamentos de interação. A principal diferença entre esses controles é o tipo de conteúdo que eles aceitam.

### MenuFlyout
Um menu de contexto, implementado com a classe MenuFlyout, pode conter [**MenuFlyoutItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutitem.aspx), [**ToggleMenuFlyoutItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.togglemenuflyoutitem.aspx), [**MenuFlyoutSubItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutsubitem.aspx) e [**MenuFlyoutSeparator**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutseparator.aspx). Para mostrar qualquer outro tipo de interface do usuário, use Flyout.

- **Diretrizes de utilização**
  - Use um separador entre grupos de comandos em um menu de contexto para:
    - Diferenciar grupos de comandos relacionados.
    - Agrupar conjuntos de comandos.
    - Dividir um conjunto previsível de comandos, como comandos da área de transferência (Recortar/Copiar/Colar), de comandos específicos do aplicativo ou do modo de exibição.
  -   Em laptops e desktops, os menus de contexto e as dicas de ferramentas não se limitam à janela do aplicativo e podem aparecer parcialmente fora dela. Se o aplicativo tentar renderizar um menu de contexto completamente fora da janela, será gerada uma exceção.

- **O que fazer e o que não fazer**
  -   Mantenha os comandos do menu de contexto curtos. Comandos mais longos acabam truncados.
  -   Use a primeira letra maiúscula em cada nome de comando.
  -   Em qualquer menu de contexto, mostre o menor número possível de comandos.
  -   Se a manipulação direta de um elemento de interface do usuário for possível, evite colocar esse comando em um menu de contexto. Um menu de contexto deve ser reservado para os comandos contextuais que não são detectáveis na tela.

### Submenu

Um submenu é um contêiner aberto que pode mostrar uma interface do usuário arbitrária como conteúdo.  Os submenus não têm partes visuais por si próprios, eles são simplesmente um controle de conteúdo. Os submenus têm uma margem e barras de rolagem opcionais adicionadas em torno de seu conteúdo. Para estilizar um submenu, modifique o `FlyoutPresenterStyle`.

O código a seguir mostra um parágrafo de quebra de texto e torna o bloco de texto acessível para um leitor de tela.

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

### Invocação e posicionamento

Submenus e menus de contexto são anexados a controles específicos. Quando visíveis, eles devem ser ancorados ao objeto de chamada e especificar sua posição relativa preferencial para o objeto: parte superior, esquerda, inferior ou direita. O submenu também tem um modo de posicionamento completo que tenta ampliá-lo e centralizá-lo dentro da janela do aplicativo.

A [classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) inclui uma propriedade `Flyout` que permite que você especifique a interface do usuário transitória que se abrirá quando o usuário clicar ou tocar no botão.

````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="Yay!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Para abrir um menu de contexto, os usuários podem realizar uma das seguintes ações:
- Clique com o botão direito com o mouse
- Pressione e segure com toque
- Digite Shift + F10
- Pressione a tecla de menu do teclado
- Pressione o botão de menu do gamepad

Para abrir facilmente um menu de contexto ou submenu em resposta a qualquer uma das ações acima, os aplicativos podem aproveitar a nova propriedade [`ContextFlyout`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx), a classe base para a maioria dos controles.

````xaml
<Rectangle Height="100" Width="100" Fill="Red">
  <Rectangle.ContextFlyout>
     <MenuFlyout>
        <MenuFlyoutItem Text="Close"/>
     </MenuFlyout>
  </Rectangle.Flyout>
</Rectangle>
````

## Artigos relacionados

**Para desenvolvedores**
- [**Classe MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Classe Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**Classe ContentDialog**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Jun16_HO4-->


