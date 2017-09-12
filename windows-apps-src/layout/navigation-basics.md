---
author: mijacobs
Description: "A navegação em aplicativos da Plataforma Universal do Windows (UWP) é baseada em um modelo flexível de estruturas de navegação, elementos de navegação e recursos no nível do sistema."
title: "Noções básicas de navegação para aplicativos UWP"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a944e02ea116c0e054918397d9d46d366d43622a
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Noções básicas de design de navegação para apps UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Se você considera um aplicativo uma coleção de páginas, o termo *navegação* descreve o ato de movimentação entre páginas e na página. É o ponto de partida da experiência do usuário. É como os usuários encontram o conteúdo e os recursos que interessam a ele. É muito importante, e pode ser difícil acertar. 

> **APIs importantes**: [Quadro](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame), [Classe Pivot](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Pivot), [Classe NavigationView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

Em parte, um dos motivos pelo qual é difícil acertar é que, como designers de aplicativos, temos que fazer muitas escolhas. Se fôssemos criar um livro, nossas escolhas seriam simples: a ordem em que os capítulos seriam organizados. Com um aplicativo, podemos criar uma experiência de navegação que imita um livro, exigindo que o usuário percorra uma série de páginas ordenadas. Ou poderíamos fornecer um menu que permitisse ao usuário acessar diretamente qualquer página desejada, mas se houvesse muitas páginas, poderíamos confundir o usuário com tantas opções. Ou poderíamos colocar tudo em uma única página e fornecer mecanismos de filtragem para exibição do conteúdo. 

Embora não haja nenhum design de navegação simples que funcione para todos os aplicativos, há um conjunto de princípios e diretrizes que você pode seguir para chegar ao design ideal para seu aplicativo. 

## <a name="principles-of-good-design"></a>Princípios de um bom design 
Vamos começar com os princípios básicos que a pesquisa já comprovou ser a base para um bom design de navegação: 

* Seja coerente: atenda às expectativas do usuário.
* Simplifique: não faça mais que o necessário.
* Seja objetivo: não permita que os recursos de navegação entrem no caminho do usuário.

### <a name="be-consistent"></a>Seja coerente 
A navegação deve ser coerente com as expectativas dos usuários, baseada em convenções padrão para ícones, localização e estilo. 

Por exemplo, na ilustração a seguir, você pode ver os pontos em que os usuários normalmente esperam encontrar funcionalidades, como o painel de navegação e a barra de comandos. Diferentes famílias de dispositivos têm suas próprias convenções para elementos de navegação. Por exemplo, o painel de navegação normalmente aparece no lado esquerdo da tela em tablets, mas na parte superior no caso dos dispositivos móveis.

<figure class="wdg-figure">
  ![Local preferencial para elementos de navegação](images/nav/nav-component-layout.png)
  <figcaption>Os usuários esperam encontrar determinados elementos da interface do usuário em locais padrão.</figcaption>
</figure> 

### <a name="keep-it-simple"></a>Simplifique
Outro fator importante no design de navegação é a lei de Hick-Hyman, geralmente citada em referência às opções de navegação. Essa lei nos incentiva a adicionar menos opções ao menu. Quanto mais opções houver, menos o usuário interagirá com elas, principalmente quando ele estiver explorando um novo aplicativo. 

<figure class="wdg-figure">
  ![Menu simples x menu complexo](images/nav/nav-simple-menus.png)
  <figcaption> À esquerda, observe que há menos opções para o usuário selecionar, enquanto à direita, há várias. A lei de Hick-Hyman indica que o menu à esquerda será mais fácil para os usuários compreenderem e utilizarem.
</figcaption>
</figure> 

### <a name="keep-it-clean"></a>Seja objetivo
A principal característica final da navegação é uma interação limpa, que se refere à maneira física como os usuários interagem com a navegação em vários contextos. Essa é uma área em que colocar-se no lugar dos usuários trará informações para o seu design. Tente entender o usuário e seus comportamentos. Por exemplo, se você estiver criando um aplicativo de culinária, é recomendável facilitar o acesso a uma lista de compras e um cronômetro. 

## <a name="three-general-rules"></a>Três regras gerais
Agora vamos usar nossos princípios de design, ou seja, consistência, simplicidade e interação limpa, para criar algumas regras gerais. Como acontece com qualquer regra geral, use-as como ponto de partida e ajuste-as quando necessário. 

1. Evite aprofundar as hierarquias de navegação. Quantos níveis de navegação são mais adequados para os usuários? Uma navegação de nível superior e um nível abaixo dela geralmente é suficiente. Se você ultrapassar três níveis de navegação, violará o princípio da simplicidade. E pior ainda, você corre o risco de deixar o usuário empacado em uma hierarquia profunda da qual ele terá dificuldade de sair.

2. Evite muitas opções de navegação. Três a seis elementos de navegação por nível são mais comuns. Se a navegação precisar de mais do que isso, principalmente no nível superior da hierarquia, é recomendável dividir o aplicativo em vários, pois você pode estar tentando executar muitas tarefas em um único lugar. O excesso de elementos de navegação em um aplicativo geralmente resulta em objetivos inconsistentes e não relacionados.

3. Evite o "pula-pula". O "pula-pula" ocorre quando há conteúdo relacionado, mas a navegação até ele requer que o usuário suba um nível e desça novamente. O "pula-pula" viola o princípio da interação limpa, exigindo cliques ou interações desnecessários para atingir uma meta óbvia, por exemplo, acessar o conteúdo relacionado em uma série. (A exceção a essa regra está na pesquisa e na procura, em que o "pula-pula" pode ser a única maneira de oferecer a diversidade e a profundidade necessárias.)
<figure class="wdg-figure">
  ![Um exemplo de "pula-pula"](images/nav/nav-pogo-sticking-1.png)
  <figcaption> Subir e descer um nível para navegar em um aplicativo; o usuário precisa voltar (seta para trás verde) à página principal para navegar até a guia "Projetos".
</figcaption>
</figure> 
<figure class="wdg-figure">
  ![A navegação lateral através do gesto de passar o dedo resolve o problema do "sobe e desce"](images/nav/nav-pogo-sticking-2.png)
  <figcaption>Você pode resolver alguns problemas de "sobe e desce" usando um ícone (observe o gesto de passar o dedo, destacado em verde).
</figcaption>
</figure> 


## <a name="use-the-right-structure"></a>Use a estrutura correta 
Agora que você está familiarizado com os princípios e as regras gerais de navegação, é hora de tomar a decisão de navegação mais importante de todas: como estruturar o aplicativo? Existem duas estruturas gerais: simples e hierárquica. 

### <a name="flatlateral"></a>Simples/lateral
Em uma estrutura simples/lateral, as páginas são dispostas lado a lado. Você pode ir de uma página para outra em qualquer ordem. 
<figure class="wdg-figure">
  <img src="images/nav/nav-pages-peer.png" alt="Pages arranged in a flat structure" />
<figcaption>Páginas dispostas em uma estrutura simples</figcaption>
</figure> 

As estruturas simples oferecem muitos benefícios: elas são simples e fáceis de entender, e permitem que o usuário vá diretamente para uma página específica sem precisar percorrer páginas intermediárias.  Em geral, as estruturas simples são ótimas, mas elas não funcionam para todos os aplicativos. Se o aplicativo tiver muitas páginas, uma lista simples pode complicar as coisas. Se as páginas precisarem ser exibidas em uma ordem específica, uma estrutura simples não funcionará. 

Recomendamos o uso de uma estrutura simples quando: 
<ul>
<li>As páginas podem ser exibidas em qualquer ordem.</li>
<li>As páginas são claramente distintas umas das outros e não têm uma relação pai/filho óbvia.</li>
<li>Há menos de 8 páginas no grupo.<br/>
Quando há mais de 7 páginas no grupo, talvez seja difícil para os usuários compreender como as páginas são únicas ou entender sua localização atual dentro do grupo. Se você não achar que isso é um problema para seu aplicativo, vá em frente e torne as páginas pares. Caso contrário, considere a possibilidade de usar uma estrutura hierárquica para quebrar as páginas em dois ou mais grupos menores. (Um controle hub pode ajudá-lo a agrupar as páginas em categorias).</li>
</ul>


### <a name="hierarchical"></a>Hierárquica

Em uma estrutura hierárquica, as páginas são organizadas em uma estrutura de árvore. Cada página filho tem apenas um pai, mas um pai pode ter uma ou mais páginas filho. Para acessar uma página filho, você passa pela página pai.

<figure class="wdg-figure">
  <img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" />
<figcaption>Páginas dispostas em uma hierarquia</figcaption>
</figure>

Estruturas hierárquicas são ideais para organizar um conteúdo complexo que abrange várias páginas ou quando as páginas precisam ser exibidas em uma ordem específica. A desvantagem é que as páginas hierárquicas sobrecarregam a navegação: quanto mais profunda for a estrutura, mais cliques serão necessários para os usuários passarem de uma página para outra. 

Recomendamos uma estrutura hierárquica quando: 
<ul>
<li>Você espera que o usuário percorra as páginas em uma ordem específica. Organize a hierarquia para impor essa ordem.</li>
<li>Há uma relação de pai-filho clara entre uma das páginas e as outras páginas do grupo.</li>
<li>Existem mais de sete páginas no grupo.<br/>
Quando há mais de sete páginas no grupo, talvez seja difícil para os usuários compreender como as páginas são únicas ou entender sua localização atual dentro do grupo. Se você não achar que isso é um problema para seu aplicativo, vá em frente e torne as páginas pares. Caso contrário, considere a possibilidade de usar uma estrutura hierárquica para quebrar as páginas em dois ou mais grupos menores. (Um controle hub pode ajudá-lo a agrupar as páginas em categorias).</li>
</ul>

### <a name="combining-structures"></a>Combinando estruturas
Você não tem que escolher uma ou outra estrutura; muitos aplicativos com design satisfatório usam tanto estruturas simples quanto hierárquicas:

![um aplicativo com uma estrutura híbrida](images/nav/nav-hybridstructure.png.png)

Esses aplicativos usam estruturas simples em páginas de nível superior que podem ser exibidas em qualquer ordem e estruturas hierárquicas em páginas que têm relações mais complexas. 

Se sua estrutura de navegação tiver vários níveis, recomendamos que elementos de navegação ponto a ponto sejam vinculados apenas aos pares em sua subárvore atual. Considere a ilustração a seguir, que mostra uma estrutura de navegação que tem três níveis:

![um aplicativo com duas subárvores](images/nav/nav-subtrees.png)
-   Para o nível 1, o elemento de navegação ponto a ponto deve fornecer acesso às páginas A, B, C e D.
-   No nível 2, os elementos de navegação ponto a ponto para as páginas A2 só devem vincular a outras páginas A2. Eles não devem vincular a páginas do nível 2 páginas na subárvore C.

![um aplicativo com duas subárvores](images/nav/nav-subtrees2.png)
 

## <a name="use-the-right-controls"></a>Use os controles corretos

Após decidir-se por uma estrutura de página, você precisará decidir como os usuários navegarão por essas páginas. A UWP oferece vários controles de navegação que podem ajudar você. Como esses controles estão disponíveis para todos os aplicativos UWP, utilizá-los ajuda a garantir uma experiência de navegação consistente e confiável. 


<table>
<tr>
    <th>Controle</th>
    <th>Descrição</th>
</tr>
<tr>
    <td>[Quadro](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame)</td>
    <td>Com algumas exceções, qualquer aplicativo com várias páginas usa o quadro. Em uma configuração típica, o aplicativo tem uma página principal que contém o quadro e um elemento de navegação principal, como um controle de modo de exibição de navegação. Quando o usuário seleciona uma página, o quadro é carregado e o exibe.</td>
</tr>
<tr>
    <td>[Guias e pivôs](../controls-and-patterns/tabs-pivot.md)<br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>Exibe uma lista de links para páginas no mesmo nível.
<p>Use guias/pivôs quando:</p>
<ul>
<li><p>Existem de 2 a 5 páginas.</p>
<p>(Você pode usar guias/pivôs quando houver mais de cinco páginas, mas pode ser difícil ajustar todas as guias/pivôs na tela.)</p></li>
<li>Você esperar que os usuários alternem entre as páginas com frequência.</li>
</ul></td>
</tr>
<tr>
    <td>[Modo de exibição de navegação](../controls-and-patterns/navigationview.md)<br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>Exibe uma lista de links para páginas de nível superior.
<p>Use um painel de navegação quando:</p>
<ul>
<li>Você não espera que os usuários alternem entre as páginas com frequência.</li>
<li>Você quer economizar espaço à custa de diminuir a velocidade das operações de navegação.</li>
<li>As páginas estiverem no nível superior.</li>
</ul></td>
</tr>
<tr>
<td>[Mestre/detalhes](../controls-and-patterns/master-details.md)<br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>Exibe uma lista (exibição mestre) de resumos de item. Selecionar um item exibe a página de itens correspondente na seção de detalhes.
<p>Use o elemento mestre/detalhes quando:</p>
<ul>
<li>Você espera que os usuários alternem entre os itens filho com frequência.</li>
<li>Você quer permitir que o usuário execute operações de alto nível, como excluir ou classificar, em itens individuais ou grupos de itens, e também deseja permitir que o usuário exiba ou atualize os detalhes de cada item.</li>
</ul>
<p>Elementos mestre/de detalhes são adequados para caixas de entrada de email, listas de contatos e entrada de dados.</p>
</td>
</tr>
<tr>
<td s>[Voltar](navigation-history-and-backwards-navigation.md)</td>
<td style="vertical-align:top;">Façamos o usuário percorrer o histórico de navegação dentro de um aplicativo e, dependendo do dispositivo, de aplicativo para aplicativo. Para obter mais informações, consulte o artigo [Histórico de navegação e navegação retroativa](navigation-history-and-backwards-navigation.md).</td>
</tr>
<tr class="odd">
<td>Hiperlinks e botões</td>
<td>Elementos de navegação de conteúdo inserido aparecem no conteúdo de uma página. Diferente de outros elementos de navegação, que devem ser consistentes no grupo ou na subárvore da página, os elementos de navegação de conteúdo inserido são exclusivos de uma página para outra.</td>
</tr>
</table>

## <a name="next-add-navigation-code-to-your-app"></a>Próximo: Adicionar código de navegação ao aplicativo
O próximo artigo, [Implementar a navegação básica](navigate-between-two-pages.md), mostra o XAML e o código necessários para usar um controle Frame que habilitará a navegação básica no aplicativo. 


<!--
## History and the back button
UWP provides a back button and a system for traversing the user's navigation hsitory within an app. This system does most of the work for you, but there are a few APIs you need to call so that it works properly. For more info and instructions, see [History and backwards navigation](navigation-history-and-backwards-navigation.md). 
-->





