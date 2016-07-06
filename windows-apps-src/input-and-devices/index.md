---
description: "Personalize seu aplicativo UWP para tipos específicos de entrada e dispositivos."
title: "Design de entrada e dispositivo – Desenvolvimento de aplicativos do Windows"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: fa1567d3ff80dc9c9376736c7d25c2bb06e79cc9
ms.openlocfilehash: f2055318fe67a0af2bcc009c6f9782029f9032f1

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# Entrada e dispositivos

Os aplicativos UWP processam automaticamente uma ampla variedade de entradas e funcionam em uma variedade de dispositivos — você não precisa de mais nada para habilitar a entrada por toque ou fazer seu aplicativo funcionar em um telefone, por exemplo. 

Mas há momentos em que você pode querer otimizar seu aplicativo para certos tipos de entrada ou dispositivos. Por exemplo, se você estiver criando um aplicativo de pintura, convém personalizar a maneira como a entrada de caneta é processada. 

As instruções de design e codificação nesta seção ajudam você a personalizar seu aplicativo UWP para tipos específicos de entradas e dispositivos. 

## Entradas e interações

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Cartilha de entrada](input-primer.md)</b><br/> Familiarize-se com cada tipo de dispositivo de entrada e seus comportamentos, recursos e limitações quando combinados com determinados fatores forma.   
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Cortana](cortana-interactions.md) </b><br/> Amplie a funcionalidade básica da Cortana com comandos de voz que iniciam e executam uma única ação em um aplicativo externo.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>[Gamepad e controle remoto](gamepad-and-remote-interactions.md)</b><br/>Os aplicativos UWP agora dão suporte a entrada por gamepad e controle remoto. Gamepads e controles remotos são os dispositivos de entrada principais para Xbox e experiências com TV.  
  </div>
  <div class="side-by-side-content-right">
<b>[Teclado](keyboard-interactions.md)</b><br/>A entrada por teclado é uma parte importante da experiência geral da interação do usuário com aplicativos. O teclado é indispensável para pessoas portadoras de determinadas deficiências ou usuários que simplesmente o consideram um método mais eficiente de interagir com um aplicativo.  
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Mouse](mouse-interactions.md)</b><br/>A entrada via mouse é a mais adequada às interações que exigem precisão do usuário para apontar e clicar. Essa precisão inerente é naturalmente aceita pela interface do usuário do Windows, que foi otimizado para a natureza imprecisa do toque.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Caneta](pen-and-stylus-interactions.md)</b><br/>Otimize seu aplicativo UWP para entrada por caneta para fornecer a funcionalidade padrão de dispositivo apontador e a melhor experiência com o Windows Ink para seus usuários.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Controle por voz](speech-interactions.md)</b><br/>Integre reconhecimento de fala e conversão de texto em fala (também conhecida como TTS ou sintetização de voz) diretamente à experiência do usuário do seu aplicativo.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Touch](touch-interactions.md)</b><br/>A UWP inclui diversos mecanismos para manipulação de entrada por toque, permitindo criar uma experiência imersiva que os usuários podem explorar com confiança.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Touchpad](touchpad-interactions.md)  </b><br/>Projete seu aplicativo de forma que os usuários possam interagir com ele por meio de um touchpad. Um touchpad combina a entrada multi-touch indireta com a entrada de precisão de um dispositivo apontador, como um mouse. Essa combinação torna o touchpad adequado para uma interface do usuário otimizada para touch e destinos menores de aplicativos de produtividade.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Várias entradas](multiple-input-design-guidelines.md)  </b><br/>Para acomodar o máximo possível de usuários e dispositivos, recomendamos que você desenvolva seus aplicativos para funcionar com o máximo possível de tipos de entrada (gesto, controle por voz, toque, touchpad, mouse e teclado). Fazer isso maximizará a flexibilidade, a usabilidade e a acessibilidade.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Zoom óptico e redimensionamento](guidelines-for-optical-zoom.md)</b><br/>Este artigo descreve o zoom e o redimensionamento de elementos do Windows e fornece as diretrizes da experiência do usuário para o uso desses mecanismos de interação em seus aplicativos.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Movimento panorâmico](guidelines-for-panning.md)</b><br/>O movimento panorâmico ou rolagem permite aos usuários navegar dentro de uma única exibição, para ver o conteúdo da exibição que não se encaixa no visor.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Rotação](guidelines-for-rotation.md)</b><br/> Este artigo descreve a nova IU do Windows para rotação. Também fornece diretrizes para a experiência do usuário que devem ser consideradas ao usar esse novo mecanismo de interação no seu aplicativo UWP.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Selecionando texto e imagens](guidelines-for-textselection.md)</b><br/>Este artigo descreve a seleção e a manipulação de texto, imagens e controles e fornece diretrizes da experiência do usuário que devem ser consideradas ao usar esses mecanismos em seus aplicativos.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Seleção por área touch](guidelines-for-targeting.md)</b><br/>A seleção por área touch no Windows usa a área de contato total de cada dedo detectado por um digitalizador de toque. O conjunto maior e mais complexo de dados de entrada relatados pelo digitalizador é usado para aumentar a precisão ao determinar o destino desejado (ou mais provável) pelo usuário.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Comentários visuais](guidelines-for-visualfeedback.md)</b><br/>Use os comentários visuais para mostrar aos usuários quando suas interações são detectadas, interpretadas e manipuladas. O feedback visual poderá ajudar os usuários incentivando a interação. Ele indica o sucesso da interação, o que oferece ao usuário uma sensação de controle. Além de retransmitir o status do sistema, também reduz os erros.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Identificar dispositivos de entrada](identify-input-devices.md)</b><br/>Identifique os dispositivos de entrada conectados a um dispositivo UWP (Plataforma Universal do Windows) e seus recursos e atributos. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Entrada de texto personalizado](custom-text-input.md)</b><br/>As APIs de texto básicas no namespace Windows.UI.Text.Core permitem que um aplicativo UWP receba a entrada de texto de qualquer serviço de texto compatível em dispositivos Windows. Isso permite que o aplicativo receba texto em qualquer idioma e de qualquer tipo de entrada, como teclado, fala ou caneta.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Identificar entrada do ponteiro](handle-pointer-input.md)</b><br/>Receber, processar e gerenciar dados de entrada de dispositivos apontadores, como toque, mouse, caneta e touchpad, em aplicativos UWP (Plataforma Universal do Windows).
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b><br/>   
</p>
  </div>
</div>
</div>


## Dispositivos

Conhecer os dispositivos que dão suporte a aplicativos UWP ajudará você a oferecer a melhor experiência de usuário para cada fator forma. Ao projetar para um dispositivo específico, as principais considerações incluem como o aplicativo aparecerá no dispositivo, onde, quando e como o aplicativo será usado nesse dispositivo, e como o usuário vai interagir com esse dispositivo.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Cartilha de dispositivos](device-primer.md)</b><br/>Conhecer os dispositivos que dão suporte a aplicativos UWP ajudará você a oferecer a melhor experiência de usuário para cada fator forma. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Projetando para TV e Xbox](designing-for-tv.md)</b><br/>Projete seu aplicativo UWP (Plataforma Universal do Windows) para que ele tenha uma boa aparência e funcione bem no Xbox One e em telas de televisão.
</p>
  </div>
</div>
</div>




<!--HONumber=Jul16_HO1-->


