---
author: Karl-Bridge-Microsoft
Description: "Assim como as pessoas usam uma combinação de voz e gestos ao se comunicar uns com os outros, vários tipos e modos de entrada também podem ser úteis ao interagir com um app."
title: "Diretrizes para design de várias entradas"
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4311ab1c817beaac5a57b12a4426f26ac8710fdf
ms.lasthandoff: 02/07/2017

---

# <a name="multiple-inputs"></a>Várias entradas
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Assim como as pessoas usam uma combinação de voz e gestos ao se comunicar uns com os outros, vários tipos e modos de entrada também podem ser úteis ao interagir com um app.


Para acomodar o máximo possível de usuários e dispositivos, recomendamos que você desenvolva seus apps para funcionar com o máximo possível de tipos de entrada (gesto, controle por voz, toque, touchpad, mouse e teclado). Fazer isso maximizará a flexibilidade, a usabilidade e a acessibilidade.

Para começar, considere os vários cenários em que seu app manipulará a entrada. Tente ser consistente em todo o app, e lembre-se de que os controles da plataforma fornecem suporte interno para vários tipos de entrada.

-   Os usuários podem interagir com o app por meio de vários dispositivos de entrada?
-   Todos os métodos de entrada têm suporte em todos os momentos? Com determinados controles? Em circunstâncias ou momentos específicos?
-   Algum método de entrada tem prioridade?

## <a name="single-or-exclusive-mode-interactions"></a>Interações de modo único (ou exclusivo)


Com interações de modo único, há suporte para vários tipos de entrada, mas apenas um pode ser usado por ação. Por exemplo, o reconhecimento de fala para comandos e gestos para navegação; ou, entrada de texto usando touch ou gestos, dependendo da proximidade.

## <a name="multimodal-interactions"></a>Interações multimodais


Com interações multimodais, vários métodos de entrada em sequência são usados para concluir uma única ação.

Fala + gesto  
O usuário aponta para um produto e diz "Adicionar ao carrinho".

Fala + toque  
O usuário seleciona uma foto usando pressionar e segurar e, em seguida, diz "Enviar foto".




