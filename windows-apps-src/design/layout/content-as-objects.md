---
description: ''
title: Conteúdo como objetos
template: detail.hbs
ms.localizationpriority: medium
ms.date: 04/18/2019
ms.openlocfilehash: b73e595454121fa34b536f3672c75a5c554ee0f3
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714025"
---
# <a name="content-as-objects"></a>Conteúdo como objetos

 

Você pode manipular a profundidade ou ordem z dos elementos para criar uma hierarquia visual que ajuda a tornar o aplicativo mais fácil de usar.  

> Observação: Este artigo é um esboço inicial para um novo recurso do Windows 10 RS2. Os nomes, a terminologia e a funcionalidade dos recursos não são finais. 

## <a name="why-visual-hierarchy-is-important"></a>Por que a hierarquia visual é importante

Os usuários estão sendo continuamente bombardeados com solicitações por sua atenção. Cada elemento na tela implora para ser examinado, cada sequência de texto quer ser lida, e cada botão clicado. À medida que o ambiente visual cresce de forma bagunçada e caótica, ele precisa de mais esforço para analisar a cena e descobrir o que está acontecendo.  

É por isso que é tão importante selecionar cuidadosamente os elementos da interface do usuário, e por isso que é importante criar um layout que estabelece uma hierarquia visual limpa entre seus elementos de interface do usuário. <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

Uma hierarquia visual limpa informa quais elementos são os mais importantes aos usuários e cria relações entre os elementos. Com uma boa hierarquia visual, os usuários entendem o layout da página em uma olhada e podem se concentrar no que estão tentando realizar. 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>Portanto, como criar uma hierarquia visual limpa? Com versões anteriores do Windows 10, você pode usar o espaço em branco, a posição e a tipografia para definir uma hierarquia visual. </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/flat-layout.png">Um layout simples</a>
    
  </div>
</div>
</div>

Com o Windows 10 RS2, nós literalmente adicionamos outra dimensão: profundidade. 

<a href="images/content-as-objects/depth-in-layout2.png">Profundidade no layout</a>


## <a name="use-depth-to-establish-a-hierarchy"></a>Use profundidade para estabelecer uma hierarquia 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>Você pode usar profundidade (ordem z), juntamente com suas outras ferramentas de design (espaços em branco, posição, tipografia) para estabelecer uma hierarquia. Eleve seus elementos mais importantes para a camada superior; use as camadas inferiores para exibir interface do usuário menos crítica. 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Profundidade no layout</a> 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>Como isso funciona?
> TODO: Breve descrição de como você pode controlar a ordem z dos elementos. Para você codificar explicitamente a ordem Z, ou há um sistema de classificação semântico? Como os itens se movem de uma camada para outra? O que o sistema faz automaticamente, e com o que os designers/desenvolvedores precisam se preocupar? 

## <a name="the-four-layers-of-a-typical-app-layers"></a>As quatro camadas de uma aplicativo de camadas típico

<p>Um aplicativo típico tem 4 camadas.</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Além do plano de fundo</b> essa camada reside atrás de aplicativo.  Ao mover elementos para essa camada, é recomendável torná-los não interativos. Elementos nesta camada têm o paralaxe mais lento e estão recortados para a janela do aplicativo. TODO: Essa camada é dimensionado? 

<p>Elementos de plano de fundo de exemplo incluem a imagem do conteúdo, TODO: Exemplo, TODO: Exemplo.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Camada de um aplicativo de plano de fundo do além</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Camada passiva</b> isso é a camada base do aplicativo, onde os elementos de interface do usuário ao vivo por padrão.  Elementos movem em tempo real nessa camada (nenhum paralaxe), são cortados para a janela do aplicativo e renderizados em escala de 100%. 

<p>Elementos de exemplo: O aplicativo em segundo plano, texto, secundária da interface do usuário, como navegação de aplicativo da interface do usuário.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">A camada passiva de um aplicativo</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Planos de ação</b> essa camada destina-se itens interativos que priorizar acima dos elementos de camada passiva. Elementos nesta camada apresentam paralaxe médio e estão recortados para a janela do aplicativo. TODO: Fazer os elementos nessa escala de camada ou tem uma sombra?

<p>Elementos de exemplo: listas, grades, comandos primários (TODO: Such as...).</p> 
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">A camada de um aplicativo de chamada de ação</a>
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Camada de Hero</b> essa camada destina-se o elemento de prioridade mais alto na tela no momento.  Elementos nessa camada podem quebrar os limites da janela do aplicativo, eles podem ser redimensionados e receber uma sombra automaticamente.

<p>Elementos de exemplo: elementos fotográficos, o item atualmente selecionado.</p>  
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">A camada de herói de um aplicativo</a>
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>Exemplo: A ser definido
> TODO: Mostre como adaptar um padrão comum de interface do usuário para usar a ordenação z. Nós devemos mostrar ilustrações e código. 

## <a name="download-the-code-samples"></a>Baixar as amostras de código
>TODO: Link para exemplos que demonstram esse recurso. 


## <a name="related-articles"></a>Artigos relacionados
* [Noções básicas de conteúdo](../basics/content-basics.md)
