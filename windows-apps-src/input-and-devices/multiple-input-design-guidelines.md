---
author: Karl-Bridge-Microsoft
Description: Assim como as pessoas usam uma combinação de voz e gestos ao se comunicar uns com os outros, vários tipos e modos de entrada também podem ser úteis ao interagir com um aplicativo.
title: Diretrizes para design de várias entradas
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
---

# Várias entradas

Assim como as pessoas usam uma combinação de voz e gestos ao se comunicar uns com os outros, vários tipos e modos de entrada também podem ser úteis ao interagir com um aplicativo.


Para acomodar o máximo possível de usuários e dispositivos, recomendamos que você desenvolva seus aplicativos para funcionar com o máximo possível de tipos de entrada (gesto, controle por voz, toque, touchpad, mouse e teclado). Fazer isso maximizará a flexibilidade, a usabilidade e a acessibilidade.

Para começar, considere os vários cenários em que seu aplicativo manipulará a entrada. Tente ser consistente em todo o aplicativo, e lembre-se de que os controles da plataforma fornecem suporte interno para vários tipos de entrada.

-   Os usuários podem interagir com o aplicativo por meio de vários dispositivos de entrada?
-   Todos os métodos de entrada têm suporte em todos os momentos? Com determinados controles? Em circunstâncias ou momentos específicos?
-   Algum método de entrada tem prioridade?

## <span id="Single__or_exclusive_-mode_interactions_"></span><span id="single__or_exclusive_-mode_interactions_"></span><span id="SINGLE__OR_EXCLUSIVE_-MODE_INTERACTIONS_"></span>Interações de modo único (ou exclusivo)


Com interações de modo único, há suporte para vários tipos de entrada, mas apenas um pode ser usado por ação. Por exemplo, o reconhecimento de fala para comandos e gestos para navegação; ou, entrada de texto usando touch ou gestos, dependendo da proximidade.

## <span id="Multimodal_interactions"></span><span id="multimodal_interactions"></span><span id="MULTIMODAL_INTERACTIONS"></span>Interações multimodais


Com interações multimodais, vários métodos de entrada em sequência são usados para concluir uma única ação.

<span id="Speech___gesture"></span><span id="speech___gesture"></span><span id="SPEECH___GESTURE"></span>Fala + gesto  
O usuário aponta para um produto e diz "Adicionar ao carrinho".

<span id="Speech___touch"></span><span id="speech___touch"></span><span id="SPEECH___TOUCH"></span>Fala + toque  
O usuário seleciona uma foto usando pressionar e segurar e, em seguida, diz "Enviar foto".





<!--HONumber=May16_HO2-->


