---
author: eliotcowley
title: Controlador de jogo bruto
description: Use as APIs de controlador de jogo Windows.Gaming.Input para ler entradas de praticamente qualquer tipo de controlador de jogo.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.author: wdg-dev-content
ms.date: 03/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, entrada, controlador de jogo bruto
ms.localizationpriority: medium
ms.openlocfilehash: c57db3f9604e20d0dc83d6c3cf2ced87b1f5dcc1
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861280"
---
# <a name="raw-game-controller"></a>Controlador de jogo bruto

Esta página descreve os conceitos básicos de programação para praticamente qualquer tipo de controlador de jogo usando [Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) e APIs relacionadas para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:

* como obter uma lista de controladores de jogo brutos e seus usuários
* como detectar se um controlador de jogo bruto foi adicionado ou removido
* como obter os recursos de um controlador de jogo bruto
* como ler a entrada de um controlador de jogo bruto

## <a name="overview"></a>Visão geral

Um controlador de jogo bruto é uma representação genérica de um controlador de jogo, com entradas encontradas em vários tipos diferentes de controladores de jogos comuns. Essas entradas são expostas como matrizes simples de botões, comutadores e eixos sem nome. Usando um controlador de jogo bruto, você pode permitir que os clientes criem mapeamentos de entrada personalizados, não importando o tipo de controlador que eles estejam usando.

Na verdade, a classe [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) foi criada para cenários em que as outras classes de entrada ([ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) e assim por diante) não atendem às suas necessidades &mdash; se quiser algo mais genérico, prevendo que os clientes usarão muitos tipos diferentes de controladores de jogos, então essa classe é para você.

## <a name="detect-and-track-raw-game-controllers"></a>Detectar e acompanhar controladores de jogos brutos

A detecção e o acompanhamento de controladores de jogos brutos funcionam exatamente da mesma forma nos gamepads, exceto na classe [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) em vez da classe [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad). Consulte [Gamepad e vibração](gamepad-and-vibration.md) para obter mais informações.

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>Obter os recursos de um controlador de jogo bruto

Depois de identificar o controlador de jogo bruto em que você está interessado, será possível coletar informações sobre os recursos do controlador. Você pode obter o número de botões no controlador de jogo bruto com [RawGameController.ButtonCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount), o número de eixos analógicos com [RawGameController.AxisCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount) e o número de opções com [RawGameController.SwitchCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount). Além disso, você pode obter o tipo de um comutador usando [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

O exemplo a seguir obtém as contagens de entrada de um controlador de jogo bruto:

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

O exemplo a seguir determina o tipo de cada comutador:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>Leitura do controlador de jogo bruto

Depois que você souber o número de entradas em um controlador de jogo bruto, estará pronto para coletar informações dele. No entanto, ao contrário de outros tipos de entrada com os quais você possa estar acostumado, um controlador de jogo bruto não comunica a alteração de estado acionando eventos. Você faz leituras regulares do estado atual deles fazendo uma _sondagem_.

### <a name="polling-the-raw-game-controller"></a>Sondagem do controlador de jogo bruto

A sondagem registra um instantâneo do controlador de jogo bruto em um momento preciso. Essa abordagem para a coleta de entrada é uma boa ideia para a maioria dos jogos porque sua lógica normalmente é executada em um loop determinístico em vez de ser controlada por eventos. Também é geralmente mais simples interpretar os comandos de jogos da entrada coletados todos de uma vez do que de muitas entradas únicas coletadas ao longo do tempo.

Você sonda um controlador de jogo bruto ao chamar [RawGameController.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___). Essa função preenche matrizes de botões, comutadores e eixos que contêm o estado do controlador de jogo bruto.

O exemplo a seguir sonda o estado atual de um controlador de jogo bruto:

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

Não há nenhuma garantia de qual posição em cada matriz manterá qual valor de entrada entre diferentes tipos de controladores, portanto você precisará verificar as entradas usando os métodos [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) e [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

**GetButtonLabel** informará o texto ou o símbolo impresso no botão físico em vez da função do botão &mdash; portanto, ele será melhor utilizado como um auxílio para a interface do usuário nos casos em que você deseja dar ao jogador dicas de quais botões realizam quais funções. **GetSwitchKind** informará o tipo de comutador (ou seja, quantas posições ele tem), mas não o nome.

Não há nenhuma maneira padronizada para obter o rótulo de um eixo ou de um computador e, portanto, você precisará testá-los por conta própria para determinar as entradas.

Se você tiver um controlador específico ao qual deseja dar suporte, obtenha [RawGameController.HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) e [RawGameController.HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) e verifique se elas correspondem a esse controlador. A posição de cada entrada em cada matriz é a mesmo para todos os controladores com as mesmas **HardwareProductId** e **HardwareVendorId** e, portanto, você não precisa se preocupar se sua lógica estiver sendo potencialmente inconsistente entre os diferentes controladores do mesmo tipo.

Além do estado do controlador de jogo bruto, cada leitura retorna um carimbo de data/hora que indica precisamente quando o estado foi recuperado. O carimbo de data e hora é útil para estabelecer a relação entre o tempo das leituras anteriores e o tempo da simulação do jogo.

### <a name="reading-the-buttons-and-switches"></a>Leitura dos botões e comutadores

Cada um dos botões do controlador de jogo bruto fornece uma leitura digital que indica se ele está pressionado (para baixo) ou liberado (para cima). As leituras de botão são representadas como valores Booleanos individuais em uma única matriz. O rótulo de cada botão pode ser encontrado usando [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) com o índice do valor Booliano na matriz. Cada valor é representado como um [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel).

O exemplo a seguir determina se o botão **XboxA** está pressionado:

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

Às vezes, convém determinar quando um botão passa de pressionado para liberado ou vice-versa, se vários botões foram pressionados ou liberados ou se um conjunto de botões está organizado de determinada maneira &mdash; alguns pressionados, outros, não. Para obter informações sobre como detectar cada uma dessas condições, consulte [Detectando transições do botão](input-practices-for-games.md#detecting-button-transitions) e [Detectando organizações complexas de botão](input-practices-for-games.md#detecting-complex-button-arrangements).

Os valores do comutador são fornecidos como uma matriz de [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition). Como essa propriedade é um campo de bits, o mascaramento bit a bit é usado para isolar a direção do comutador.

O exemplo a seguir determina se cada comutador está na posição vertical:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>Leitura das entradas analógicas (joysticks, gatilhos, aceleradores e assim por diante)

Um eixo analógico fornece uma leitura entre 0,0 e 1,0. Isso inclui cada dimensão em um joystick, como X e Y para joysticks padrão, ou até mesmo os eixos X, Y e Z (rolagem, rotação e guinada, respectivamente) para joysticks para simulador de voo.

Os valores podem representar gatilhos e aceleradores analógicos ou qualquer outro tipo de entrada analógica. Esses valores não são fornecidos com rótulos, por isso, sugerimos que seu código seja testado com uma variedade de dispositivos de entrada para garantir que eles correspondam corretamente às suas suposições.

Nos dois eixos, o valor é de aproximadamente 0,5 quando o joystick está na posição central, mas é normal que o valor exato varie, até mesmo entre as leituras subsequentes; as estratégias para atenuar essa variação são abordadas mais adiante nesta seção.

O exemplo a seguir mostra como ler os valores analógicos de um controlador do Xbox:

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

Na leitura dos valores dos joysticks, você observará que eles não produzem uma leitura neutra confiável de 0,5 quando estão em repouso na posição central; eles produzirão valores diferentes próximos de 0,5 cada vez que o joystick for movido e retornado para a posição central. Para atenuar essas variações, você pode implementar uma pequena _zona morta_, que é um intervalo de valores próximos à posição central ideal que são ignorados.

Uma maneira de implementar uma zona morta é determinar a que distância do centro o joystick foi movido e ignorar as leituras mais próximas em vez da distância que você escolher. Você pode calcular a distância aproximadamente &mdash; ela não é exata porque as leituras dos botões são essencialmente valores polares, não planares &mdash; usando apenas o Teorema de Pitágoras. Isso produz uma zona morta radial.

O exemplo a seguir demonstra uma zona morta radial básica usando o Teorema de Pitágoras:

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>Veja também

* [Entrada para jogos](input-for-games.md)
* [Práticas de entrada para jogos](input-practices-for-games.md)
* [Namespace Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Classe Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)
* [Interface Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Interface Windows.Gaming.Input.IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)