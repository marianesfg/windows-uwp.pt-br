---
author: mijacobs
Description: TODO
title: "Interações de Gamepad e controle remoto"
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b1ee5409cfca253b3bb084b95365d22526c66920
ms.lasthandoff: 02/07/2017

---

# <a name="gamepad-and-remote-control-interactions"></a>Interações de Gamepad e controle remoto

Os aplicativos UWP (Plataforma Universal do Windows) agora dão suporte à entrada de gamepad e de controle remoto. Gamepads e controles remotos são os dispositivos de entrada principais para Xbox e experiências com TV. Os aplicativos UWP devem ser otimizados para esses tipos de dispositivo de entrada, da mesma forma como são otimizados para entrada de mouse e de teclado em um computador e entrada por toque em um telefone ou tablet. Verificar se seu aplicativo funciona bem com esses dispositivos de entrada é a etapa mais importante ao otimizar para Xbox e os programas de TV.
Agora você pode conectar e usar o gamepad com aplicativos UWP no computador o que facilita a validação do trabalho.

Para garantir uma experiência de usuário bem-sucedida e agradável para seu aplicativo UWP ao usar um gamepad ou controle remoto, você deve considerar o seguinte:

* [Botões de hardware](designing-for-tv.md#hardware-buttons) -
O gamepad e o controle remoto fornecem configurações e botões muito diferentes.

* [Interação e navegação de foco do plano XY](designing-for-tv.md#xy-focus-navigation-and-interaction)
 -
A navegação de foco do plano XY permite que o usuário navegue na interface do usuário do aplicativo.

* [Modo de mouse](designing-for-tv.md#mouse-mode) -
O modo de mouse permite que seu aplicativo emule uma experiência de mouse quando a navegação de foco do plano XY não for suficiente.

