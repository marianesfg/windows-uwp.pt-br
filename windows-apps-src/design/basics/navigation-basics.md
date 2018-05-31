---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Noções básicas de navegação para aplicativos UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3edb7dc28561b5d316a908df951e3052eafc995b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654265"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Noções básicas de design de navegação para aplicativos UWP

Se você considera um aplicativo uma coleção de páginas, *navegação* descreve o ato de movimentação entre páginas e em uma página. É o ponto de partida da experiência do usuário, e é como os usuários encontram o conteúdo e recursos em que estão interessados. É muito importante, e pode ser difícil acertar.

É difícil pois, como designers de aplicativos, temos que fazer muitas escolhas. Nós podemos exigir que o usuário passe por uma série de páginas em ordem. Ou poderíamos fornecer um menu que permite aos usuários ir diretamente para qualquer página. Ou poderíamos colocar tudo em uma única página e fornecer mecanismos de filtragem para exibição do conteúdo.
 
Embora não haja nenhum design de navegação simples que funcione para todos os aplicativos, há princípios e diretrizes para ajudá-lo a decidir o design ideal para seu aplicativo. 

<figure class="wdg-figure">
  <img alt="Navigation diagram for an app" src="images/navigation_diagram.png" />
  <figcaption>Diagrama de navegação de um aplicativo.</figcaption>
</figure> 

## <a name="principles-of-good-navigation"></a>Princípios de boa navegação
Vamos começar com os princípios básicos de bom design de navegação: 

* Consistência: atenda às expectativas do usuário.
* Simplicidade: não faça mais que o necessário.
* Clareza: Forneça caminhos claros e opções.

### <a name="consistency"></a>Consistência
A navegação deve ser consistente com as expectativas dos usuários. Usar [controles padrão](#use-the-right-controls) com os quais os usuários estão familiarizados e seguir convenções padrão para ícones, localização, e estilo tornarão a navegação previsível e intuitiva para os usuários.

<figure class="wdg-figure">
<img src="images/nav/nav-component-layout.png" alt="Preferred location for navigation elements"/>
  <figcaption>Os usuários esperam encontrar determinados elementos da interface do usuário em locais padrão.</figcaption>
</figure> 

### <a name="simplicity"></a>Simplicidade
Menos itens de navegação simplificam a tomada de decisão para os usuários. Fornecer acesso fácil a destinos importantes e ocultar itens menos importantes ajudarão os usuários a chegarem onde desejam mais rapidamente.

<figure class="wdg-figure">
<img src="images/nav/nav-simple-menus.png" alt="A simple versus a complex menu"/>
  <figcaption> O menu à esquerda será mais fácil para os usuários compreenderem e utilizarem pois há menos itens.
</figcaption>
</figure> 

### <a name="clarity"></a>Clareza
Caminhos claros permitem navegação lógica para os usuários. Tornar as opções de navegação óbvias e esclarecer as relações entre as páginas deve impedir que os usuários se percam.

<figure class="wdg-figure">
<img src="images/nav/nav-pages.png" alt="Clear paths and destinations"/>
  <figcaption> Os destinos têm rótulos claros para que os usuários saibam onde eles estão.
</figcaption>
</figure> 

## <a name="general-recommendations"></a>Recomendações gerais
Agora, vamos usar nossos princípios de design, ou seja, consistência, simplicidade e clareza, para criar algumas recomendações gerais.

1. Pense em seus usuários. Trace os caminhos típicos que eles podem tomar por meio de seu aplicativo e, para cada página, pense no motivo de o usuário estar lá e onde eles podem querer ir. 

2. Evite aprofundar as hierarquias de navegação. Se você for além de três níveis de navegação, você corre o risco de deixar o usuário empacado em uma hierarquia profunda da qual ele terá dificuldade de sair.

3. Evite o "pula-pula". O "pula-pula" ocorre quando há conteúdo relacionado, mas a navegação até ele requer que o usuário suba um nível e desça novamente.

## <a name="use-the-right-structure"></a>Use a estrutura correta 
Agora que você está familiarizado com os princípios gerais de navegação, como você deve estruturar seu aplicativo? Existem duas estruturas gerais: simples e hierárquica. 

### <a name="flatlateral"></a>Simples/lateral
![Páginas dispostas em uma estrutura simples](images/nav/nav-pages-peer.png)

Em uma estrutura simples/lateral, as páginas são dispostas lado a lado. Você pode ir de uma página para outra em qualquer ordem. 

Recomendamos o uso de uma estrutura simples quando: 
<ul>
<li>As páginas podem ser exibidas em qualquer ordem.</li>
<li>As páginas são claramente distintas umas das outros e não têm uma relação pai/filho óbvia.</li>
<li>Há menos de 8 páginas no grupo.<br/>
(Quando há mais páginas, talvez seja difícil para os usuários compreender como as páginas são únicas ou entender sua localização atual dentro do grupo. Se você não achar que isso é um problema para seu aplicativo, vá em frente e torne as páginas pares. Caso contrário, considere a possibilidade de usar uma estrutura hierárquica para quebrar as páginas em dois ou mais grupos menores.)</li>
</ul>

### <a name="hierarchical"></a>Hierárquica
![Páginas dispostas em uma hierarquia](images/nav/nav-pages-hiearchy.png)

Em uma estrutura hierárquica, as páginas são organizadas em uma estrutura de árvore. Cada página filho tem um pai, mas um pai pode ter uma ou mais páginas filho. Para acessar uma página filho, você passa pela página pai.

Estruturas hierárquicas são adequadas para organizar conteúdo complexo que abrange várias páginas. A desvantagem é um pouco de sobrecarga de navegação: quanto mais profunda for a estrutura, mais cliques serão necessários para os usuários passarem de uma página para outra. 

Recomendamos uma estrutura hierárquica quando: 
<ul>
<li>As páginas devem ser percorridas em uma ordem específica.</li>
<li>Há uma relação de pai-filho clara entre as páginas.</li>
<li>Existem mais de sete páginas no grupo.</li>
</ul>

### <a name="combining-structures"></a>Combinando estruturas
![um aplicativo com uma estrutura híbrida](images/nav/nav-hybridstructure.png.png)

Você não tem que escolher uma ou outra estrutura; muitos aplicativos com design satisfatório usam ambas. Um aplicativo pode usar estruturas simples em páginas de nível superior que podem ser exibidas em qualquer ordem e estruturas hierárquicas em páginas que têm relações mais complexas. 

Se sua estrutura de navegação tiver vários níveis, recomendamos que elementos de navegação ponto a ponto sejam vinculados apenas aos pares em sua subárvore atual. Considere a ilustração a seguir, que mostra uma estrutura de navegação que tem três níveis:

![um aplicativo com duas subárvores](images/nav/nav-subtrees.png)
- No nível 1, o elemento de navegação ponto a ponto deve fornecer acesso às páginas A, B, C e D.
- No nível 2, os elementos de navegação ponto a ponto para as páginas A2 só devem vincular a outras páginas A2. Eles não devem vincular a páginas do nível 2 páginas na subárvore C.

![um aplicativo com duas subárvores](images/nav/nav-subtrees2.png)

## <a name="use-the-right-controls"></a>Use os controles corretos
Após decidir-se por uma estrutura de página, você precisará decidir como os usuários navegarão por essas páginas. A UWP oferece uma variedade de controles de navegação para ajudar a garantir uma experiência de navegação consistente e confiável em seu aplicativo. 

É recomendável selecionar um controle de navegação com base no número de elementos de navegação em seu aplicativo. Se você tiver cinco ou menos itens de navegação, utilize navegação de nível superior, como [guias e pivô](../controls-and-patterns/tabs-pivot.md). Se você tem seis ou mais itens de navegação, use a navegação à esquerda, como [modo de exibição navegação](../controls-and-patterns/navigationview.md) ou [mestre/detalhes](../controls-and-patterns/master-details.md).

<div class="mx-responsive-img">

<table>
<tr>
    <th>Controle</th>
    <th>Descrição</th>
</tr>
<tr>
    <td><a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame">Quadro</a><br/><br/>
    <img src="images/frame.png" alt="Frame" /></td>
    <td>Exibe páginas. <p>Com algumas exceções, qualquer aplicativo com várias páginas usa um quadro. Geralmente, um aplicativo tem uma página principal que contém o quadro e um elemento de navegação principal, como um controle de modo de exibição de navegação. Quando o usuário seleciona uma página, o quadro é carregado e o exibe.</p></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/tabs-pivot.md">Guias e pivôs</a><br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>Exibe uma lista horizontal de links para páginas no mesmo nível.
<p>Use quando:</p>
<ul>
<li>Existem de 2 a 5 páginas. (Você pode usar guias/pivôs quando houver mais de cinco páginas, mas pode ser difícil ajustar todas as guias/pivôs na tela.)</li>
<li>Você esperar que os usuários alternem entre as páginas com frequência.</li>
</ul></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/navigationview.md">Modo de exibição de navegação</a><br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>Exibe uma lista vertical de links para páginas de nível superior.
<p>Use quando:</p>
<ul>
<li>As páginas estiverem no nível superior.</li>
<li>Há muitos itens de navegação (mais de 5).</li>
<li>Você não espera que os usuários alternem entre as páginas com frequência.</li>

</ul></td>
</tr>
<tr>
<td><a href="../controls-and-patterns/master-details.md">Mestre/detalhes</a><br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>Exibe uma lista (exibição mestre) de itens. Selecionar um item exibe a página correspondente na seção de detalhes.
<p>Use quando:</p>
<ul>
<li>Você espera que os usuários alternem entre os itens filho com frequência.</li>
<li>Você quer permitir que o usuário execute operações de alto nível, como excluir ou classificar, em itens individuais ou grupos de itens, e também deseja permitir que o usuário exiba ou atualize os detalhes de cada item.</li>
</ul>
<p>Mestre/detalhes é adequado para caixas de entrada de email, listas de contatos e entrada de dados.</p>
</td>
<tr>
<td><a href="../controls-and-patterns/hub.md">Hub</a><br/><br/>
<img src="images/hub.png" alt="Hub" /></td>
<td> Exibe as seções de itens ordenados em grades. 
<p>Use quando:</p>
<ul>
<li>Você deseja criar navegação visual com imagens de celebridades.</li>
</ul>
<p>Hubs são bem adequados para navegação, visualização ou experiências de compra.</p>
</td>
</tr>
<tr>
<td><a href="../controls-and-patterns/hyperlinks.md">Hiperlinks</a> e <a href="../controls-and-patterns/buttons.md">botões</a></td>
<td> Elementos de navegação de conteúdo inserido podem aparecer no conteúdo de uma página. Diferente de outros elementos de navegação, que devem ser consistentes em todas as páginas, os elementos de navegação de conteúdo inserido são exclusivos de uma página para outra.</td>
</tr>
</table>
</div>

## <a name="next-add-navigation-code-to-your-app"></a>Próximo: Adicionar código de navegação ao aplicativo
O próximo artigo, [Implementar a navegação básica](navigate-between-two-pages.md), mostra o código necessário para usar um controle de Quadro que habilitará a navegação básica entre duas páginas no aplicativo. 