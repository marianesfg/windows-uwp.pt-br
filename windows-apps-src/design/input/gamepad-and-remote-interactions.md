---
author: mijacobs
Description: TODO
title: Interações de gamepad e controle remoto
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a3e87fea7dfce66b16ccdf04ef13950c092660c8
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5766708"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interações de gamepad e controle remoto

![Controle remoto e direcional](images/dpad-remote/dpad-remote.png)

Agora, os aplicativos da Plataforma Universal do Windows (UWP) oferecem suporte à entrada por gamepad e controle remoto, que são os dispositivos de entrada principais para experiências de Xbox e TV.

Os aplicativos UWP devem ser otimizados para esses tipos de dispositivo de entrada, da mesma forma como são otimizados para entrada de mouse e de teclado em um computador e entrada por toque em um telefone ou tablet.

Certifique-se de que o aplicativo funciona bem com esses dispositivos de entrada. Essa é a etapa mais importante ao otimizar para Xbox e os programas de TV.

> [!NOTE] 
> Agora você pode conectar e usar o gamepad com aplicativos UWP no computador o que facilita a validação do trabalho.

Para garantir uma experiência de usuário bem-sucedida e agradável para seu aplicativo UWP ao usar um gamepad ou controle remoto, você deve considerar o seguinte:

* [Botões de hardware](../devices/designing-for-tv.md#hardware-buttons) - O gamepad e o controle remoto fornecem configurações e botões muito diferentes.

* [Interação e navegação de foco do plano XY](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction) - A navegação de foco do plano XY permite que o usuário navegue na interface do usuário do aplicativo.

* [Modo de mouse](../devices/designing-for-tv.md#mouse-mode) - O modo de mouse permite que seu aplicativo emule uma experiência de mouse quando a navegação de foco do plano XY não for suficiente.
