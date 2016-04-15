---
Description: Um submenu é um pop-up leve que é usado para mostrar temporariamente a interface do usuário relacionada ao que o usuário está fazendo no momento.
title: Menus de contexto e caixas de diálogo
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
---
# Menus, caixas de diálogo e pop-ups

Menus, caixas de diálogo e pop-ups exibem elementos transitórios da interface do usuário que aparecem quando o usuário os solicita ou quando acontece algo que requer notificação ou aprovação. 

<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [Classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
-   [Classe Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)

Um menu de contexto fornece ações instantâneas ao usuário. Ele pode ser preenchido com comandos personalizados. Os menus de contexto podem ser ignorados com o toque ou clique em algum lugar fora do menu.

Caixas de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do aplicativo. Na maioria dos casos, as caixas de diálogo bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignoradas e muitas vezes solicitam algum tipo de ação do usuário.

Um pop-up, também conhecido como submenu, é um pop-up contextual leve que exibe a interface do usuário relacionada ao que o usuário está fazendo. Ele inclui a lógica de colocação e dimensionamento, e pode ser usado para mostrar um menu, exibir um controle oculto, mostrar mais detalhes de um item ou pedir que o usuário confirme uma ação. Os pop-ups podem ser ignorados tocando ou clicando em algum lugar fora do pop-up.


## Este é o controle correto?

Menus de contexto podem ser usados para:

-   Ações contextuais em seleções de texto, como Copiar, Recortar, Colar, Verificar Ortografia e assim por diante.
-   Comandos da área de transferência.
-   Comandos personalizados.
-   Comandos para um objeto no qual se deve atuar, mas que não pode ser selecionado ou indicado de outra maneira.

As caixas de diálogo podem ser usadas para:

- Expressar informações importantes que o usuário deve ler e confirmar antes de prosseguir.
- Solicitar uma ação clara do usuário ou comunicar a mensagem importante que o usuário deve confirmar. Os exemplos incluem:
  - Quando a segurança do usuário pode ser comprometida
  - Quando o usuário está prestes a alterar um ativo valioso de forma permanente
  - Quando o usuário está prestes a excluir um ativo valioso
  - Para confirmar uma compra no aplicativo
- Mensagens de erro que se aplicam ao contexto geral do aplicativo, como um erro de conectividade.
- Perguntas, quando o aplicativo precisar fazer uma pergunta de bloqueio ao usuário, por exemplo, quando o aplicativo não puder escolher em nome do usuário. Uma pergunta de bloqueio não pode ser ignorada ou adiada e deve oferecer ao usuário opções bem-definidas.

Os pop-ups (submenus) podem ser usados para:

-   Interface do usuário contextual e transitória.
-   Avisos e confirmações, incluindo aqueles relacionados às ações potencialmente destrutivas.
-   Exibir mais informações, como detalhes sobre um item na tela.
-   Menus de segundo nível.
-   Comandos personalizados.


## Exemplos

Aqui está um menu de contexto de painel único típico. Ele seria usado para obter uma lista mais curta de comandos simples. Use separadores conforme necessário para agrupar comandos semelhantes.

![Exemplo de um menu de contexto típico](images/controls_contextmenu_singlepane.png)

Um menu de contexto em cascata seria usado para uma coleção mais abrangente de comandos. Ele possui um nível de submenu e pode rolar. Use separadores conforme necessário para agrupar comandos semelhantes.

![Exemplo de um menu de contexto em cascata](images/controls_contextmenu_cascading.png)

Este é um exemplo de uma caixa de diálogo de confirmação de tela inteira, com um único botão. Com esse tipo de caixa de diálogo, é apresentada ao usuário uma quantidade razoável de informações que ele deve ler antes de pressionar o botão para continuar.

![Exemplo de uma caixa de diálogo de página inteira, com um único botão](images/controls_dialog_singlebutton.png)

Aqui está um exemplo de uma caixa de diálogo de dois botões que apresenta ao usuário uma opção A/B. Em geral, a quantidade de informações apresentadas nesta caixa de diálogo é pequena.

![Exemplo de uma caixa de diálogo botão inteira](images/controls_dialog_twobutton.png)


## Diretriz de uso

**Menus de contexto**
- Use um separador entre grupos de comandos em um menu de contexto para:
  - Diferenciar grupos de comandos relacionados.
  - Agrupar conjuntos de comandos.
  - Dividir um conjunto previsível de comandos, como comandos da área de transferência (Recortar/Copiar/Colar), de comandos específicos do aplicativo ou do modo de exibição.
-   Em laptops e desktops, os menus de contexto e as dicas de ferramentas não se limitam à janela do aplicativo e podem aparecer fora dela. Se o aplicativo tentar renderizar um menu de contexto completamente fora da janela, será gerada uma exceção.

**Caixas de diálogo**
-   Identifique claramente o problema ou o objetivo do usuário na primeira linha do texto da caixa de diálogo.
-   O título da caixa de diálogo é a principal instrução e é opcional.
    -   Use um título curto para explicar o que as pessoas precisam fazer na caixa de diálogo. Títulos longos não terão quebra de linha e serão truncados.
    -   Se você está usando a caixa de diálogo para passar uma mensagem simples, de erro ou fazer uma pergunta, pode omitir o título. Utilize o texto do conteúdo para passar as informações principais.
    -   O título deve estar diretamente relacionado às opções de botão.
-   O conteúdo da caixa de diálogo contém o texto descritivo e é obrigatório.
    -   Apresente a mensagem, o erro ou a pergunta de bloqueio da maneira mais simples possível.
    -   Se um título da caixa de diálogo for usado, use a área de conteúdo para fornecer mais detalhes ou definir a terminologia. Não repita o título com palavras ligeiramente diferentes.
-   Pelo menos um botão da caixa de diálogo deve aparecer.
    -   Use botões com texto que identifiquem respostas específicas para a instrução ou o conteúdo principal. Um exemplo é "Você permite que NomeDoAplicativo acesse o sua localização?", seguido pelos botões "Permitir" e "Bloquear". Respostas específicas podem ser entendidas mais rapidamente, resultando na tomada de decisão eficiente.
-   Caixas de diálogo de erro exibem a mensagem de erro na caixa de diálogo, juntamente com quaisquer informações pertinentes. O único botão usado em uma caixa de diálogo de erro deve ser "Fechar" ou uma ação semelhante.
-   Não use caixas de diálogo para erros que são contextuais a um local específico da página, como erros de validação (em campos de senha, por exemplo). Use a própria tela do aplicativo para mostrar erros embutidos.

**Pop-ups**

-   Como acontece com outras interfaces do usuário contextuais, coloque um pop-up ao lado do ponto em que é chamado.
    -   Especifique o objeto no qual você deseja ancorar o pop-up e o lado do objeto no qual o pop-up será exibido.
    -   Tente posicionar o pop-up de modo que ele não bloqueie nenhuma interface do usuário importante.
-   O pop-up deve ser ignorado assim que algo nele for selecionado.
-   Pop-ups funcionam melhor com apenas um nível. Vários níveis de menu de pop-up são difíceis de navegar e proporcionam uma experiência ruim para o usuário.

## O que fazer e o que não fazer

-   Mantenha os comandos do menu de contexto curtos. Comandos mais longos acabam truncados.
-   Use a primeira letra maiúscula em cada nome de comando.
-   Em qualquer menu de contexto, mostre o menor número possível de comandos.
-   Se a manipulação direta de um elemento de interface do usuário for possível, evite colocar esse comando em um menu de contexto. Um menu de contexto deve ser reservado para os comandos contextuais que não são detectáveis na tela.



## Artigos relacionados

**Para desenvolvedores**
- [**Classe MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Classe Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)


<!--HONumber=Mar16_HO4-->


