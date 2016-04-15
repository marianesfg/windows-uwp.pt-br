---
Description: Fornece navegação de nível superior ao mesmo tempo que economiza o espaço da tela.
title: Diretrizes para painéis de navegação
ms.assetid: 8FB52F5E-8E72-4604-9222-0B0EC6A97541
label: Nav pane
template: detail.hbs
---

Painéis de navegação
=============================================================================================
Um painel de navegação é um padrão que permite muitos itens de navegação de nível superior sem ocupar o espaço da tela. O painel de navegação é bastante usado em aplicativos móveis, mas também funciona bem em telas maiores. Quando usado como uma sobreposição, o painel permanece recolhido e oculto até o usuário pressionar o botão, o que é útil em telas menores. Quando usado no modo encaixado, o painel permanece aberto, o que garante maior utilidade se não houver espaço suficiente na tela.

![Exemplo de um painel de navegação](images/NAV_PANE_EXAMPLE.png)

<span class="sidebar_heading" style="font-weight: bold;">APIs Importantes</span>

-   [**Classe SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objeto SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

<<<<<<< HEAD

=======

>>>>>>> origem

<span id="Is_this_the_right_pattern_"></span><span id="is_this_the_right_pattern_"></span><span id="IS_THIS_THE_RIGHT_PATTERN_"></span>Esse é o padrão correto?
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

O painel de navegação funciona bem em:

-   Aplicativos com muitos itens de navegação de primeiro nível na mesma classe, como um aplicativos de esportes com categorias como Futebol americano, Beisebol, Basquete, Futebol etc.
-   Fornecer uma experiência de navegação consistente em todos os aplicativos, desde que apenas os elementos de navegação sejam colocados dentro do painel.
-   Um número de médio a alto (entre 5 e 10, ou mais) de categorias de navegação de nível superior.
-   Preservar o estado real da tela (como uma sobreposição).
-   Itens de navegação que são acessados com pouca frequência. (como uma sobreposição).
-   Cenários de arrastar e soltar (quando encaixado).

<span id="Building_a_nav_pane"></span><span id="building_a_nav_pane"></span><span id="BUILDING_A_NAV_PANE"></span>Criando um painel de navegação
-------------------------------------------------------------------------------------------------------------------------------------

O padrão do painel de navegação consiste em um botão, um painel para categorias de navegação e uma área de conteúdo. A maneira mais fácil de criar um painel de navegação é com um [controle de modo de exibição dividido](split-view.md), que vem com um painel vazio e uma área de conteúdo que é sempre visível. O painel pode estar visível ou oculto e pode apresentar-se do lado esquerdo ou do lado direito de uma janela do aplicativo.

Para criar um painel de navegação sem o controle de modo divisão, serão necessários três componentes principais: um botão, um painel e uma área de conteúdo. O botão permite ao usuário abrir e fechar o painel. O painel é um contêiner para os elementos de navegação. A área de conteúdo exibe as informações do item de navegação selecionado. O painel de navegação também pode existir em um modo encaixado, em que o painel é sempre mostrado, portanto um botão não é necessário nesse caso.

### <span id="Button"></span><span id="button"></span><span id="BUTTON"></span>Botão

O botão do painel de navegação é visualizado, por padrão, como três linhas horizontais empilhadas e é conhecido como o botão "hambúrguer". O botão permite ao usuário abrir e fechar o painel quando necessário e não se move com o painel. Recomendamos colocar o botão no canto superior esquerdo do seu aplicativo. O botão não se move com o painel.

![Exemplo de botão do painel de navegação](images/navpane_buttononly.png)

O botão geralmente está associado a uma cadeia de caracteres de texto. No nível superior do aplicativo, o título do aplicativo pode ser exibido ao lado do botão. Em níveis inferiores do aplicativo, a cadeia de caracteres de texto pode ser associada à página em que o usuário está no momento.

### <span id="Pane"></span><span id="pane"></span><span id="PANE"></span>Painel

Cabeçalhos para categorias de navegação entram no painel. Pontos de entrada para as configurações do aplicativo e o gerenciamento de contas, se aplicáveis, também entram no painel. Os cabeçalhos de navegação podem ser de nível superior ou ficar aninhados no nível superior/segundo nível.

![Exemplo de painel do painel de navegação](images/navpane_pane.png)

### <span id="Content_area"></span><span id="content_area"></span><span id="CONTENT_AREA"></span>Área de conteúdo

A área de conteúdo é onde as informações do local de navegação selecionado são exibidas. Ela pode conter elementos individuais ou outra navegação de nível inferior.

<span id="Nav_pane_variations"></span><span id="nav_pane_variations"></span><span id="NAV_PANE_VARIATIONS"></span>Variações do painel de navegação
-------------------------------------------------------------------------------------------------------------------------------------

O painel de navegação tem duas variações principais: de sobreposição e encaixado. Uma sobreposição é recolhida ou expandida conforme necessário. Um painel encaixado permanece aberto por padrão.

### <span id="Overlay"></span><span id="overlay"></span><span id="OVERLAY"></span>Sobreposição

-   Uma sobreposição pode ser usada em qualquer tamanho de tela e na orientação retrato ou paisagem. Em seu estado padrão (recolhido), a sobreposição não ocupa estado real, com apenas com o botão mostrado.
-   Fornece navegação sob demanda que preserva o estado real da tela. Ideal para aplicativos em telefones e phablets.
-   O painel fica oculto por padrão, somente com o botão visível.
-   Pressionar o botão do painel de navegação abre e fecha a sobreposição.
-   O estado expandido é transitório, sendo ignorado quando uma seleção é feita, quando o botão Voltar é usado ou quando o usuário toca fora do painel.
-   A sobreposição fica sobre o conteúdo e não reorganiza o conteúdo.

### <span id="Docked"></span><span id="docked"></span><span id="DOCKED"></span>Encaixado

-   O painel de navegação permanece aberto. Esse modo é mais adequado para telas maiores, geralmente de tablets e superiores.
-   Na orientação de paisagem, a largura de tela mínima que pode tirar proveito do estado encaixado é de 720 epx. Nesse tamanho, o estado encaixado pode exigir atenção especial ao dimensionamento do conteúdo.
-   Dá suporte a cenários de arrastar e soltar de e para o painel.
-   O botão do painel de navegação não é necessário para esse estado. Se o botão for usado, a área de conteúdo será enviada por push e o conteúdo dessa área será reorganizado.
-   A seleção deve ser mostrada nos itens da lista para realçar onde o usuário está na árvore de navegação.
-   Quando um dispositivo é muito estreito na orientação retrato para exibir o painel encaixado, o seguinte comportamento é recomendado quando o dispositivo for girado:
    -   Paisagem para retrato. O painel recolhe para o estado de sobreposição ou estado minimizado.
    -   Retrato para paisagem. O painel reaparece.

<span id="related_topics"></span>Tópicos relacionados
-----------------------------------------------

* [Controle de modo de exibição dividido](split-view.md)
* [Mestre/detalhes](master-details.md)
* [Noções básicas de navegação](https://msdn.microsoft.com/library/windows/apps/dn958438)
 

 


<!--HONumber=Mar16_HO4-->


