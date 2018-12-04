---
Description: Just as people use a combination of voice and gesture when communicating with each other, multiple types and modes of input can also be useful when interacting with an app.
title: Diretrizes para design de várias entradas
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c67430680854e7940d12af15ecd3c07dcd976802
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8467590"
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



