---
description: "Saiba como projetar e codificar um app UWP que seja fácil de navegar e tenha um visual espetacular em uma variedade de dispositivos e tamanhos de tela."
title: "Design de layout do app UWP – Desenvolvimento de aplicativos do Windows"
author: mijacobs
keywords: layout do app UWP, Plataforma Universal do Windows, design do app, interface
label: Layout
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 1aa12606-8a99-4db3-8311-90e02fde9cf1
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ce668d5450f9b47e49e9f2535c420bc9c07d2a30
ms.lasthandoff: 02/08/2017

---
# <a name="layout-for-uwp-apps"></a>Layout para apps UWP
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


A estrutura, o layout de página e a navegação do app são a base da experiência do usuário de seu app. Os artigos nesta seção ajudarão você a criar um app que seja fácil de navegar e tenha um visual espetacular em uma variedade de dispositivos e tamanhos de tela.

## <a name="intro"></a>Introdução

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p><b>[Introdução ao design da interface do usuário de apps](design-and-ui-intro.md)</b><br />
Ao desenvolver um app UWP, você cria uma interface do usuário adequada a vários dispositivos com tamanhos de tela diferentes. Este artigo fornece uma visão geral dos recursos relacionados à interface do usuário e dos benefícios dos apps UWP e algumas dicas e truques para projetar uma interface do usuário responsiva. </p>
  </div>
  <div class="side-by-side-content-right">
    ![Um app executado em vários dispositivos](images/rspd-reposition-type1-sm.png)
  </div>
</div>
</div>

## <a name="app-layout-and-structure"></a>Estrutura e layout do app
Confira essas recomendações para estruturar seu app e usar os três tipos de elementos de interface do usuário: navegação, comandos e conteúdo.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p>
<b>[Noções básicas de navegação](navigation-basics.md)</b><br/>
A navegação em apps UWP se baseia em um modelo flexível de estruturas de navegação, elementos de navegação e recursos de nível do sistema. Este artigo apresenta esses componentes e mostra como usá-los juntos para proporcionar uma boa experiência de navegação.
</p>
<p>
<b>[Noções básicas de conteúdo](content-basics.md)</b><br/>
O objetivo principal de qualquer app é fornecer acesso ao conteúdo: em um app de edição de fotos, a foto é o conteúdo; em um app de viagens, mapas e informações sobre destinos de viagem são o conteúdo; e assim por diante. Este artigo fornece recomendações de design de conteúdo para os três cenários de conteúdo: consumo, criação e interação.
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


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Tamanhos de tela e pontos de interrupção](screen-sizes-and-breakpoints-for-responsive-design.md)</b><br/>
O número de destinos de dispositivo e tamanhos de tela do ecossistema do Windows 10 é muito grande para você se preocupar com a otimização de sua interface do usuário em cada um deles. Em vez disso, recomendamos projetar para algumas larguras principais (também chamadas de "pontos de interrupção"): 360, 640, 1024 e 1366 epx.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Definir layouts com XAML](layouts-with-xaml.md)</b> <br/>
Como usar painéis de layout e propriedades XAML para deixar seu app dinâmico e adaptável.</p>
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Painéis de layout](layout-panels.md)</b> <br />
Saiba mais sobre cada tipo de layout de cada painel e veja como usá-los para criar o layout de elementos de interface do usuário em XAML.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Alinhamento, margens e preenchimento](alignment-margin-padding.md)</b> <br />
Além das propriedades de dimensão (largura, altura e restrições), os elementos também podem ter as propriedades de alinhamento, margem e preenchimento que influenciam o comportamento do layout quando um elemento passa por um cálculo de layout e é renderizado em uma interface do usuário.</p> 
  </div>
</div>
</div>



