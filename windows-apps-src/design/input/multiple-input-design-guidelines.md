---
author: Karl-Bridge-Microsoft
Description: Just as people use a combination of voice and gesture when communicating with each other, multiple types and modes of input can also be useful when interacting with an app.
title: Diretrizes para design de várias entradas
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 670caf5c519fcf1b7ff57b47781541d98358aa50
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6990755"
---
# <a name="multiple-inputs"></a>Várias entradas


Assim como as pessoas usam uma combinação de voz e gestos ao se comunicar uns com os outros, vários tipos e modos de entrada também podem ser úteis ao interagir com um aplicativo.


Para acomodar o máximo possível de usuários e dispositivos, recomendamos que você desenvolva seus aplicativos para funcionar com o máximo possível de tipos de entrada (gesto, controle por voz, toque, touchpad, mouse e teclado). Fazer isso maximizará a flexibilidade, a usabilidade e a acessibilidade.

Para começar, considere os vários cenários em que seu aplicativo manipulará a entrada. Tente ser consistente em todo o aplicativo, e lembre-se de que os controles da plataforma fornecem suporte interno para vários tipos de entrada.

-   Os usuários podem interagir com o aplicativo por meio de vários dispositivos de entrada?
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



