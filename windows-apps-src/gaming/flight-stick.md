---
author: eliotcowley
title: Joystick para simulador de voo
description: Use as APIs de joystick para simulador de voo Windows.Gaming.Input para ler a entrada de joysticks para simulador de voo.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.author: wdg-dev-content
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp, jogos, entrada, joystick para simulador de voo
ms.localizationpriority: medium
ms.openlocfilehash: ebe7695b3f16271f3adedae658c0d62d38d7c078
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995399"
---
# <a name="flight-stick"></a>Joystick de voo

Esta página descreve os conceitos básicos de programação para joysticks para simulador de voo do Xbox One usando [Windows.Gaming.Input.FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) e APIs relacionadas para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:

* obter uma lista de joysticks para simulador de voo conectados e seus usuários
* detectar se um joystick para simulador de voo foi adicionado ou removido
* ler a entrada de um ou mais joysticks para simulador de voo
* como os joysticks para simulador de voo se comportam como dispositivos de navegação da interface do usuário

## <a name="overview"></a>Visão geral

Os joysticks para simulador de voo são dispositivos de entrada de jogos valorizados pela reprodução da sensação de manches que seriam encontrados na cabine de um avião ou de uma espaçonave. Eles são o dispositivo de entrada perfeito para um controle de voo rápido e preciso. Os joysticks para simulador de voo têm suporte em aplicativos UWP do Windows 10 e do Xbox One pelo namespace [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input).

Os joysticks para simulador de voo do Xbox One são equipados com os seguintes controles:

* Um joystick analógico que pode ser girado compatível com rolagem, inclinação e orientação
* Um acelerador analógico
* Dois botões de tiro
* Um botão com oito direções
* Botões **Exibir** e **Menu**

> [!NOTE]
> Os botões **View** e **Menu** são usados para dar suporte à navegação da interface do usuário, e não para comandos de jogo e, portanto, não podem ser prontamente acessados como botões de joystick.

### <a name="ui-navigation"></a>Navegação da interface do usuário

Para aliviar a sobrecarga do suporte para diferentes dispositivos de entrada para a navegação de interface do usuário e para incentivar a consistência entre dispositivos e jogos, os dispositivos de entrada mais _físicos_ atuam simultaneamente como dispositivos de entrada _lógicos_ separados chamado de [controladores de navegação da interface do usuário](ui-navigation-controller.md). O controlador de navegação da interface do usuário fornece um vocabulário comum para comandos de navegação da interface do usuário em dispositivos de entrada.

Como um controlador de navegação da interface do usuário, um joystick para simulador de voo mapeia o [conjunto necessário](ui-navigation-controller.md#required-set) de comandos de navegação para o joystick e os botões **Exibir**, **Menu**, **FirePrimary** e **FireSecondary**.

| Comando de navegação | Entrada de joystick para simulador de voo                  |
| ------------------:| ----------------------------------- |
|                 Para cima | Joystick para cima                         |
|               Para baixo | Joystick para baixo                       |
|               Para a esquerda | Joystick para a esquerda                       |
|              Para a direita | Joystick para a direita                      |
|               Exibir | Botão **Exibir**                     |
|               Menu | Botão **Menu**                     |
|             Aceitar | Botão **FirePrimary**              |
|             Cancelar | Botão **FireSecondary**            |

Os joysticks para simulador de voo não mapeiam qualquer um dos comandos de navegação do [conjunto opcional](ui-navigation-controller.md#optional-set).

## <a name="detect-and-track-flight-sticks"></a>Detectar e rastrear joysticks para simulador de voo

A detecção e o acompanhamento de joysticks para simulador de voo funcionam exatamente da mesma forma nos gamepads, exceto na classe [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) em vez da classe [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad). Consulte [Gamepad e vibração](gamepad-and-vibration.md) para obter mais informações.

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>Leitura do joystick para simulador de voo

Depois de identificar o joystick para simulador de voo de seu interesse, você estará pronto para coletar as informações dele. No entanto, ao contrário de outros tipos de entrada com os quais você possa estar acostumado, os joysticks para simulador de voo não comunicam a alteração de estado acionando eventos. Você faz leituras regulares do estado atual deles fazendo uma _sondagem_.

### <a name="polling-the-flight-stick"></a>Sondagem do joystick para simulador de voo

A sondagem captura um instantâneo do joystick para simulador de voo em um momento preciso. Essa abordagem para a coleta de entrada é uma boa ideia para a maioria dos jogos porque sua lógica normalmente é executada em um loop determinístico em vez de ser controlada por eventos. Também é geralmente mais simples interpretar os comandos de jogos da entrada coletados todos de uma vez do que de muitas entradas únicas coletadas ao longo do tempo.

Você sonda um joystick para simulador de voo ao chamar [FlightStick.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick.GetCurrentReading). Essa função retorna uma [FlightStickReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading), que contém o estado do joystick para simulador de voo.

O exemplo a seguir sonda o estado atual de um joystick para simulador de voo:

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

Além do estado do joystick para simulador de voo, cada leitura inclui um carimbo de data e hora que indica precisamente quando o estado foi recuperado. O carimbo de data e hora é útil para estabelecer a relação entre o tempo das leituras anteriores e o tempo da simulação do jogo.

### <a name="reading-the-joystick-and-throttle-input"></a>Leitura da saída do joystick e do acelerador

O joystick fornece uma leitura analógica entre -1,0 e 1,0 nos eixos X, Y e Z (rolagem, inclinação e orientação, respectivamente). Para uma rolagem, um valor -1,0 corresponde à posição máxima à esquerda do joystick, enquanto um valor 1,0 corresponde à posição máxima à direita. Para a inclinação, um valor -1,0 corresponde à posição máxima inferior do joystick, enquanto um valor 1,0 corresponde à posição máxima superior. Para a orientação, um valor -1,0 corresponde à posição torcida máxima no sentido anti-horário,, enquanto um valor 1.0 corresponde à posição máxima no sentido horário.

Em todos os eixos, o valor é de aproximadamente 0,0 quando o joystick está na posição central, mas é normal que o valor exato varie, até mesmo entre as leituras subsequentes. As estratégias para atenuar essa variação são discutidas posteriormente nesta seção.

O valor de rolagem do joystick é lido da propriedade [FlightStickReading.Roll](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Roll), o valor da rotação é lido da propriedade [FlightStickReading.Pitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Pitch) e o valor da rotação é lido da propriedade [FlightStickReading.Yaw](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Yaw):

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

Na leitura dos valores do joystick, você observará que eles não produzem uma leitura neutra confiável de 0,0 quando o joystick está em repouso na posição central; eles produzirão valores diferentes próximos de 0,0 cada vez que o joystick for movido e retornado para a posição central. Para atenuar essas variações, você pode implementar uma pequena _zona morta_, que é um intervalo de valores próximos à posição central ideal que são ignorados.

Uma maneira de implementar uma zona morta é determinar a que distância do centro o joystick foi movido e ignorar as leituras mais próximas em vez da distância que você escolher. Você pode calcular a distância aproximadamente &mdash; ela não é exata porque as leituras do joystick são essencialmente valores polares, não planares &mdash; usando apenas o Teorema de Pitágoras. Isso produz uma zona morta radial.

O exemplo a seguir demonstra uma zona morta radial básica usando o Teorema de Pitágoras:

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>Leitura dos botões e do botão de ângulo de visão

Cada um dos dois botões de tiro do joystick para simulador de voo fornece uma leitura digital que indica se ele foi pressionado (para baixo) ou liberado (para cima). Para garantir a eficiência, as leituras do botão não são representadas como valores boolianos individuais; elas são reunidas em um único campo de bits que é representado pela enumeração [FlightStickButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickbuttons). Além disso, o botão de oito direções fornece uma direção incluída em um único campo de bits que é representado pela enumeração [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition).

> [!NOTE]
> Os joysticks para simulador de voo são equipados com botões adicionais usados para a navegação da interface do usuário, como os botões **Exibir** e **Menu**. Esses botões não fazem parte da enumeração `FlightStickButtons` e só podem ser lidos acessando o joystick para simulador de voo como um dispositivo de navegação da interface do usuário. Para saber mais, veja [Controlador de navegação da interface do usuário](ui-navigation-controller.md).

Os valores de botão são lidos da propriedade [FlightStickReading.Buttons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Buttons). Como essa propriedade é um campo de bits, o mascaramento bit a bit é usado para isolar o valor do botão de seu interesse. O botão está pressionado (para baixo) quando o bit correspondente está definido; caso contrário, ele está liberado (para acima).

O exemplo a seguir determina se o botão **FirePrimary** é pressionado:

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

O exemplo a seguir determina se o botão **FirePrimary** é liberado.

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

Às vezes, convém determinar quando um botão passa de pressionado para liberado ou vice-versa, se vários botões foram pressionados ou liberados ou se um conjunto de botões está organizado de determinada maneira &mdash; alguns pressionados, outros, não. Para obter informações sobre como detectar cada uma dessas condições, consulte [Detectando transições do botão](input-practices-for-games.md#detecting-button-transitions) e [Detectando organizações complexas de botão](input-practices-for-games.md#detecting-complex-button-arrangements).

O valor do botão de ângulo de visão é lido da propriedade [FlightStickReading.HatSwitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.HatSwitch). Como essa propriedade também é um campo de bits, o mascaramento bit a bit é usado novamente para isolar a posição do botão de ângulo de visão.

O exemplo a seguir determina se cada botão de ângulo de visão está na posição vertical:

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

O exemplo a seguir determina se o botão de ângulo de visão está na posição central:

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>Veja também

* [Classe Windows.Gaming.Input.UINavigationController](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Interface Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Práticas de entrada para jogos](input-practices-for-games.md)