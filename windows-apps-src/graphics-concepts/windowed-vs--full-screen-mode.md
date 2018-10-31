---
title: Modo de janela versus modo de tela inteira
description: 'Os aplicativos Direct3D podem ser executados em um destes modos: janela ou tela inteira.'
ms.assetid: EE8B9F87-822B-4576-A446-CA603E786862
keywords:
- Modo de janela versus modo de tela inteira
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b2f8c52835801f6cabccad3419bef9ef510522dc
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5866114"
---
# <a name="span-iddirect3dconceptswindowedvsfull-screenmodespanwindowed-vs-full-screen-mode"></a><span id="direct3dconcepts.windowed_vs__full-screen_mode"></span>Modo de janela versus modo de tela inteira


Os aplicativos Direct3D podem ser executados em um destes modos: janela ou tela inteira. No *modo de janela*, o aplicativo compartilha o espaço disponível na tela da área de trabalho com todos os aplicativos em execução. No *modo de tela inteira*, a janela em que o aplicativo é executado abrange a área de trabalho inteira, ocultando todos os aplicativos em execução (incluindo o ambiente de desenvolvimento). Jogos normalmente usam o modo de tela inteira por padrão para imergir o usuário no jogo, ocultando todos os aplicativos em execução.

As diferenças de código entre o modo de tela inteira e modo de janela são muito pequenas.

Como um aplicativo em execução no modo de tela inteira assume a tela, depurar o aplicativo requer um monitor separado ou o uso de um depurador remoto. Uma vantagem de um aplicativo de modo de janela é que você pode percorrer o código em um depurador sem vários monitores ou um depurador remoto.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Dispositivos](devices.md)

 

 




