---
author: mithom
title: Entrada para jogos
description: Esta seção demonstra como trabalhar com gamepads e outros dispositivos de entrada para jogos da Plataforma Universal do Windows (UWP).
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, entrada
ms.localizationpriority: medium
ms.openlocfilehash: 26510cac6e4d42ee5eb36a7ae09c9c1518b47089
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "300172"
---
# <a name="input-for-games"></a>Entrada para jogos

Esta seção descreve os diferentes tipos de dispositivos de entrada que podem ser usados em jogos da Plataforma Universal do Windows (UWP) no Windows 10 e no Xbox One, demonstra o uso básico e recomenda padrões e técnicas para programação de entrada efetiva em jogos.

> **Observação**    Existem outros tipos de dispositivos de entrada e que estão disponíveis para serem usados em jogos UWP, como dispositivos de entrada personalizados que podem ser específicos de gênero ou do jogo. Esses dispositivos e a programação não são abordados nesta seção. Para obter informações sobre as interfaces usadas para facilitar dispositivos de entrada personalizados, consulte o namespace [Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom).

## <a name="gaming-input-devices"></a>Dispositivos de entrada para jogos

Os dispositivos de entrada para jogos são compatíveis em jogos e aplicativos UWP para Windows 10 e Xbox One pelo namespace [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input).

### <a name="gamepads"></a>Gamepads

Os gamepads são o dispositivo de entrada padrão no Xbox One e uma opção comum para jogadores do Windows quando eles não gostam de usar teclado e mouse. Eles oferecem uma grande variedade de controles digitais e analógicos, o que os indica para praticamente qualquer tipo de jogo e também dá uma resposta física por meio de motores de vibração inseridos.

Para obter informações sobre como usar gamepads no jogo UWP, consulte [Gamepad e vibração](gamepad-and-vibration.md).

### <a name="arcade-sticks"></a>Direcionais analógicos de arcade

Direcionais analógicos de arcade são dispositivos de entrada totalmente digital valorizados por reproduzir a sensação das máquinas de arcade verticais para lutas mano a mano ou outros jogos em estilo arcade.

Para obter informações sobre como usar direcionais analógicos de arcade no jogo UWP, consulte [Direcional analógico de arcade](arcade-stick.md).

### <a name="racing-wheels"></a>Volantes de corrida

Volantes de corrida são dispositivos de entrada que reproduzem a sensação do cockpit de um carro de corrida real e são o dispositivo de entrada perfeito para qualquer jogo de corrida que tenha carros ou caminhões. Muitos volantes de corrida são equipados com um force feedback real, ou seja, eles podem aplicar forças reais em um eixo de controle como no volante, e não uma simples vibração.

Para obter informações sobre como usar volantes de corrida no jogo UWP, consulte [Volante de corrida e force feedback](racing-wheel-and-force-feedback.md).

### <a name="flight-sticks"></a>Cartões de aéreas

Cartões de aéreas são dispositivos de entrada de jogos que reproduza a sensação de cartões aéreas que seria encontrado em um avião ou cabine do espaçonave. Eles são o dispositivo de entrada perfeito para um controle de voo rápido e preciso.

Para obter mais informações sobre como usar cartões aéreas no jogo UWP, consulte [pente aéreas](flight-stick.md).

### <a name="raw-game-controllers"></a>Controladores de jogo brutos

Um controlador de jogo bruto é uma representação genérica de um controlador de jogo, com entradas encontradas em vários tipos diferentes de controladores de jogos comuns. Essas entradas são expostas como matrizes simples de botões, comutadores e eixos sem nome. Usando um controlador de jogo bruto, você pode permitir que os clientes criem mapeamentos de entrada personalizados, não importando o tipo de controlador que eles estejam usando.

Para obter mais informações sobre como usar os controladores de jogo brutos no jogo UWP, consulte [bruto controlador de jogo](raw-game-controller.md).

### <a name="ui-navigation-controllers"></a>Controladores de navegação da interface do usuário

Controladores de navegação da interface do usuário são dispositivos de entrada lógica que existem para fornecer um vocabulário em comum para comandos de navegação da interface do usuário que promove uma experiência do usuário consistente em jogos e dispositivos de entrada física diferentes. A interface do usuário de um jogo deve usar as interfaces UINavigationController, em vez de interfaces específicas do dispositivo.

Para obter informações sobre como usar controladores de navegação da interface do Usuário no jogo UWP, consulte [Controlador de navegação da interface do usuário](ui-navigation-controller.md).

### <a name="headsets"></a>Fones de ouvido

Fones de ouvido são dispositivos de captura e reprodução de áudio associados a um usuário específico quando conectado por meio do dispositivo de entrada. Eles costumam ser usados por jogos online para chat de voz, mas também podem ser usados para aprimorar a imersão ou fornecer recursos de jogo em jogos online e offline.

Para obter informações sobre como usar fones de ouvido no jogo UWP, consulte [Fone de ouvido](headset.md)

### <a name="users"></a>Usuários

Cada dispositivo de entrada e o fone de ouvido conectado podem ser associados a um usuário específico para vincular a identidade ao jogo. A identidade do usuário também é o meio pelo qual um dispositivo de entrada física é correlacionado à entrada pelo controlador de navegação da interface do usuário lógico.

Para obter informações sobre como gerenciar usuários e os dispositivos de entrada, consulte [Como acompanhar usuários e dispositivos](input-practices-for-games.md#tracking-users-and-their-devices).

## <a name="see-also"></a>Consulte também

* [Práticas de entrada para jogos](input-practices-for-games.md)
* [Namespace Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Namespace Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)