---
author: mithom
title: "Práticas de entrada para jogos"
description: "Aprenda padrões e técnicas para usar dispositivos de entrada de maneira efetiva."
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
translationtype: Human Translation
ms.sourcegitcommit: 858bf6a0862d6459b2bac22d8ab9431b51332fef
ms.openlocfilehash: 15e5096c00334bf200fa0076d227e42fc463e25d

---

# <a name="input-practices-for-games"></a>Práticas de entrada para jogos

Esta página descreve padrões e técnicas para usar dispositivos de entrada de maneira efetiva em jogos da Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:
* acompanhar jogadores e quais dispositivos de entrada e navegação eles estão usando atualmente
* detectar transições de botão (de pressionado para liberado, de liberado para pressionado)
* detectar disposições de botão complexas com um único teste

## <a name="tracking-users-and-their-devices"></a>Como acompanhar usuários e dispositivos

Todos os dispositivos de entrada estão associados a um [Usuário][Windows.System.User], de maneira que a identidade possa ser vinculada ao jogo, às conquistas, às alterações de configurações e a outras atividades. Os usuários podem entrar ou sair quando quiserem, e é comum um usuário diferente fazer logon no dispositivo de entrada que continua conectado ao sistema depois que o usuário anterior saiu. Quando um usuário entra ou sai, o evento [IGameController.UserChanged][] é acionado. É possível registrar um manipulador de eventos para esse evento a fim de acompanhar jogadores e os dispositivos que eles estão usando.

Identidade do usuário também é a maneira como um dispositivo de entrada está associado ao controlador de navegação de interface do usuário correspondente.

Por esses motivos, a entrada do jogador deve ser acompanhada e correlacionada usando-se a propriedade [Usuário][igamecontroller.user] da classe de dispositivo (herdada da interface [IGameController][]).

O exemplo [UserGamepadPairingUWP _(github)_](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) demonstra como é possível acompanhar usuários e os dispositivos que eles estão usando.

## <a name="detecting-button-transitions"></a>Como detectar transições de botão

Às vezes, você deseja saber quando um botão foi pressionado ou liberado pela primeira vez; ou seja, exatamente quando o estado do botão passa de liberado para pressionado ou de pressionado para liberado. Para determinar isso, você precisa lembrar a leitura do dispositivo anterior e comparar a leitura atual com ela para saber o que mudou.

O exemplo a seguir demonstra uma abordagem básica para lembrar a leitura anterior; os gamepads são mostrados aqui, mas os princípios são os mesmos para direcional analógico de arcade, volante de corrida e botões de navegação da interface do usuário

```cpp
GamepadReading newReading();
GamepadReading oldReading();

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.CurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased
}
```

Antes de fazer algo, `Game::Loop` move o valor existente de `newReading` (a leitura do gamepad da iteração de loop anterior) para `oldReading` e, em seguida, preenche `newReading` com uma leitura de gamepad atualizada para a iteração atual. Isso fornece as informações de que você precisa para detectar transições de botão.

O exemplo a seguir demonstra uma abordagem básica para detectar transições de botão.

```cpp
bool buttonJustPressed(const gamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool buttonJustReleased(gamepadButtons selection)
{
    bool newSelectionReleased = (gamepadButtons.None == (newReading.Buttons & selection));
    bool oldSelectionReleased = (gamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Essas duas funções derivam primeiramente o estado booliano da seleção do botão de `newReading` e `oldReading` e, em seguida, realizam uma lógica booliana para determinar se a transição de destino ocorreu. Essas funções só retornarão _true_ se a leitura nova contiver o estado de destino (pressionado ou liberado, respectivamente) *e* a leitura anterior também não contiver o estado de destino; do contrário, elas retornarão _false_.


## <a name="detecting-complex-button-arrangements"></a>Como detectar disposições de botão complexas

Cada botão de um dispositivo de entrada fornece uma leitura digital que indica se ele está pressionado (para baixo) ou liberado (para cima). Tendo em vista a eficiência, as leituras de botão não são representadas como valores boolianos individuais; em vez disso, elas são todas empacotadas em campos de bits representados por enumerações específicas, como [GamepadButtons][]. Para ler botões específicos, o mascaramento bit a bit é usado para isolar os valores nos quais você tem interesse. Os botões estão pressionados (para baixo) quando o bit correspondente está definido; do contrário, ele está liberado (para acima).

Lembre-se de como se determina se botões únicos estão pressionados ou liberados; os gamepads são mostrados aqui, mas os princípios são os mesmos para direcional analógico de arcade, volante de corrida e botões de navegação da interface do usuário.

```cpp
// determines whether gamepad button A is pressed
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}

// determines whether gamepad button A is released
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released (not pressed)
}
```

Como é possível ver, determinar o estado de um botão único é simples, mas, às vezes, convém determinar se vários botões estão pressionados ou liberados, ou se um conjunto de botões está organizado de uma determinada maneira – alguns pressionados, outros não. Testar vários botões é mais complexo do que testar botões únicos – especialmente com o potencial do estado de botão misto –, mas existe uma fórmula simples para esses testes que se aplica a testes de botões únicos e múltiplos.

O exemplo a seguir determina se os botões de gamepad A e B estão ambos pressionados.

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are pressed
}
```

O exemplo a seguir determina se os botões de gamepad A e B estão ambos liberados.

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are released (not pressed)
}
```

O exemplo a seguir determina se o botão de gamepad A está pressionado e o botão B está liberado.

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

A fórmula que todos esses cinco exemplos têm em comum é que a disposição dos botões a ser testada é especificada pela expressão no lado esquerdo do operador de igualdade e os botões a serem considerados são selecionados pela expressão de mascaramento no lado direito.

O exemplo a seguir demonstra essa fórmula mais claramente reescrevendo o exemplo anterior.

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

Esta fórmula pode ser aplicada para testar qualquer número de botões em qualquer disposição dos estados.



[Windows.System.User]: https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.user]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.user.aspx
[igamecontroller.userchanged]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.userchanged.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx



<!--HONumber=Dec16_HO3-->


