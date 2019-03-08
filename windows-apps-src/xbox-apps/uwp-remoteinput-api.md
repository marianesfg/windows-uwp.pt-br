---
title: Referência de API de entrada remota do Portal de Dispositivos
description: Saiba como enviar remotamente a entrada de mouse, teclado e controle em um Xbox.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 882e84c5126e4f67e246dd479008133c979c06b1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595951"
---
# <a name="remote-input-api-reference"></a>Referência de API de entrada remota   
Você pode enviar a entrada de mouse, teclado e controle em tempo real remotamente por meio dessa API.

**Solicitação**

Método      | URI da solicitação
:------     | :-----
Websocket | /ext/remoteinput
<br />
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Solicitação**

O websocket, uma série de mensagens de matriz de bytes. Para cada mensagem, o formato é o seguinte.

O primeiro byte indica o tipo de entrada. Os tipos de entrada a seguir são suportados:

| Tipo de entrada        | Valor de byte |
|------------|-------------|
Códigos de tecla do teclado | 0x01
ScanCodes de teclado | 0x02
Mouse | 0x03
Desmarcar Tudo | 0x04

Para KeyboardKeyCodes e KeyboardScanCodes, a segundo byte é o valor do keycode ou scancode; o terceiro byte é 0x01 ao pressionar para baixo e 0x00 para soltar.

Para uma mensagem de Mouse, o próximo valor é UINT16 na ordem de rede (2 bytes) indicando o tipo de evento de mouse:

| Tipo de ação        | Valor UINT16 |
|------------|-------------|
Mover | 0x0001
LeftDown | 0x0002
LeftUp | 0x0004
RightDown | 0x0008
RightUp | 0x0010
MiddleDown | 0x0020
MiddleUp | 0x0040
X1Down | 0x0080
X1Up | 0x0100
X2Down | 0x0200
X2Up | 0x0400
VerticalWheelMoved | 0x0800
HorizontalWheelMoved | 0x1000

Esse byte é seguido por dois valores UINT32 na ordem de rede e um UINT32 opcional para ações de rolagem. Os dois primeiros valores são as coordenadas X e Y e o terceiro é o delta de roda de rolagem. As coordenadas X e Y devem ser um valor entre 0 e 65535, indicando a posição relativa do mouse nos planos horizontais e verticais.

ClearAll indica que qualquer tecla pressionada atualmente deve ser solta. Não são esperados outros bytes.

Para enviar a entrada de Gamepad, os valores de keycode que representam os pressionamentos de botão do Gamepad podem ser usados com KeyboardKeyCodes. Essas valores são:

| Tipo de Gamepad        | Valor de byte |
|------------|-------------|
VK_GAMEPAD_A                       |  0xC3
VK_GAMEPAD_B                       |  0xC4
VK_GAMEPAD_X                       |  0xC5
VK_GAMEPAD_Y                       |  0xC6
VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7
VK_GAMEPAD_LEFT_SHOULDER           |  0xC8
VK_GAMEPAD_LEFT_TRIGGER            |  0xC9
VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA
VK_GAMEPAD_DPAD_UP                 |  0xCB
VK_GAMEPAD_DPAD_DOWN               |  0xCC
VK_GAMEPAD_DPAD_LEFT               |  0xCD
VK_GAMEPAD_DPAD_RIGHT              |  0xCE
VK_GAMEPAD_MENU                    |  0xCF
VK_GAMEPAD_VIEW                    |  0xD0
VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1
VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2
VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3
VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4
VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5
VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6
VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7
VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8
VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9
VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA


**Resposta**   

- Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox