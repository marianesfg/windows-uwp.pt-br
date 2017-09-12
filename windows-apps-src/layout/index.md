---
description: "Saiba como projetar e codificar um aplicativo UWP que seja fácil de navegar e tenha um visual espetacular em uma variedade de dispositivos e tamanhos de tela."
title: "Design de layout do aplicativo UWP – Desenvolvimento de aplicativos do Windows"
author: mijacobs
keywords: layout do aplicativo UWP, Plataforma Universal do Windows, design do aplicativo, interface
label: Layout
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 1aa12606-8a99-4db3-8311-90e02fde9cf1
ms.openlocfilehash: 4c1b4617b3b58cb613bcca8d5df456621af730fa
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2017
---
# <a name="layout-for-uwp-apps"></a>Layout para aplicativos UWP
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

A estrutura, o layout de página e a navegação são a base da experiência do usuário do seu aplicativo. Os artigos nesta seção usam o Sistema de Design Fluent para ajudar você a criar um aplicativo que seja fácil de navegar e tenha ótima aparência em diversos dispositivos e tamanhos de tela.

## <a name="intro"></a>Introdução

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p><b>[Introdução ao design da interface do usuário de aplicativos](design-and-ui-intro.md)</b><br />
Ao desenvolver um aplicativo UWP, você cria uma interface do usuário adequada a vários dispositivos com tamanhos de tela diferentes. Este artigo fornece uma introdução ao Sistema de Design Fluent, uma visão geral dos recursos relacionados à interface do usuário e dos benefícios dos aplicativos UWP e algumas dicas e truques para projetar uma interface do usuário responsiva. </p>
  </div>
  <div class="side-by-side-content-right">
    ![Um aplicativo executado em vários dispositivos](images/rspd-reposition-type1-sm.png)
  </div>
</div>
</div>

## <a name="app-layout-and-structure"></a>Estrutura e layout do aplicativo
Confira essas recomendações para estruturar seu aplicativo e usar os três tipos de elementos de interface do usuário: navegação, comandos e conteúdo.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p>
<b>[Noções básicas de navegação](navigation-basics.md)</b><br/>
A navegação em aplicativos UWP se baseia em um modelo flexível de estruturas de navegação, elementos de navegação e recursos de nível do sistema. Este artigo apresenta esses componentes e mostra como usá-los juntos para proporcionar uma boa experiência de navegação.
</p>
<p>
<b>[Noções básicas de conteúdo](content-basics.md)</b><br/>
O objetivo principal de qualquer aplicativo é fornecer acesso ao conteúdo: em um aplicativo de edição de fotos, a foto é o conteúdo; em um aplicativo de viagens, mapas e informações sobre destinos de viagem são o conteúdo; e assim por diante. Este artigo fornece recomendações de design de conteúdo para os três cenários de conteúdo: consumo, criação e interação.
</p> 
  </div>
  <div class="side-by-side-content-right">
<p><b>[Noções básicas de comandos](commanding-basics.md)</b> <br />
Elementos de comando são os elementos interativos da interface do usuário que permitem ao usuário executar ações, como enviar um email, excluir um item ou enviar um formulário. Este artigo descreve os elementos de comando, como botões e caixas de seleção, as interações a que eles dão suporte e as superfícies de comando (por exemplo, barras de comandos e menus de contexto) para hospedá-los.</p>
  </div>
</div>
</div>

## <a name="page-layout"></a>Layout de página 
Estes artigos ajudarão você a criar uma interface do usuário flexível que tenha um visual espetacular em diferentes tamanhos de tela, tamanhos de janela, resoluções e orientações. 

<div style="column-count: 2; column-gap: 40px; margin-top: 40px;">

<div style="-webkit-column-break-inside: avoid; page-break-inside: avoid; break-inside: avoid;">
<p style="margin-top: 0px; padding-top: 0px;"><b>[Tamanhos de tela e pontos de interrupção](screen-sizes-and-breakpoints-for-responsive-design.md)</b><br/>
O número de destinos de dispositivo e tamanhos de tela do ecossistema do Windows 10 é muito grande para você se preocupar com a otimização de sua interface do usuário em cada um deles. Em vez disso, recomendamos projetar para algumas larguras principais (também chamadas de "pontos de interrupção"): 360, 640, 1024 e 1366 epx.</p>
</div>

<div style="-webkit-column-break-inside: avoid; page-break-inside: avoid; break-inside: avoid;">
  <p><b>[Definir layouts com XAML](layouts-with-xaml.md)</b> <br/>
Como usar painéis de layout e propriedades XAML para deixar seu aplicativo dinâmico e adaptável.</p>
</div>
<div style="-webkit-column-break-inside: avoid; page-break-inside: avoid; break-inside: avoid;">
   <p><b>[Painéis de layout](layout-panels.md)</b> <br />
Saiba mais sobre cada tipo de layout de cada painel e veja como usá-los para criar o layout de elementos de interface do usuário em XAML.</p> 
</div>
<div style="-webkit-column-break-inside: avoid; page-break-inside: avoid; break-inside: avoid;">
 <p><b>[Alinhamento, margens e preenchimento](alignment-margin-padding.md)</b> <br />
Além das propriedades de dimensão (largura, altura e restrições), os elementos também podem ter as propriedades de alinhamento, margem e preenchimento que influenciam o comportamento do layout quando um elemento passa por um cálculo de layout e é renderizado em uma interface do usuário.</p> 
</div>
<div style="-webkit-column-break-inside: avoid; page-break-inside: avoid; break-inside: avoid;">
 <p><b>[Criar layouts com Grid e StackPanel](grid-tutorial.md)</b> <br />
Use XAML para criar o layout de um app de clima simples usando os elementos Grid e StackPanel. </p> 
</div>

</div>



