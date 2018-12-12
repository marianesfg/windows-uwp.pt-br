---
title: Joystick de arcade
description: Use as APIs de joystick de arcade Windows.Gaming.Input para detectar e ler joysticks de arcade.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, joystick de arcade, entrada
ms.localizationpriority: medium
ms.openlocfilehash: 6f9e3ff29dfb17b6e2a07df52153013b5266206e
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933811"
---
# <a name="arcade-stick"></a>Joystick de arcade

Esta página descreve os conceitos básicos de programação para joysticks de arcade do Xbox One usando [Windows.Gaming.Input.ArcadeStick][arcadestick] e APIs relacionadas para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:

* obter uma lista de joysticks de arcade conectados e seus usuários
* detectar se um joystick de arcade foi adicionado ou removido
* ler a entrada de um ou mais joysticks de arcade
* como os joysticks de arcade se comportam como dispositivos de navegação da interface do usuário

## <a name="arcade-stick-overview"></a>Visão geral do joystick de arcade

Os joysticks de arcade são dispositivos de entrada valorizados por reproduzirem a sensação de máquinas de arcade de fliperama e por seus controles digitais de alta precisão. Os joysticks de arcade são o dispositivo de entrada perfeito para luta "mano a mano" e outros jogos estilo arcade e são adequados para qualquer jogo que funcione bem com controles inteiramente digitais. Os joysticks de arcade têm suporte em aplicativos UWP do Windows 10 e do Xbox One pelo namespace [Windows.Gaming.Input][].

Os joysticks de arcade Xbox One são equipados com um joystick digital de 8 vias, seis botões de **ação** (representados como A1 A6 na imagem abaixo) e dois botões **especiais** (representados como S1 e S2); eles são dispositivos de entrada inteiramente digitais que não dão suporte a controles analógicos ou vibração. Os joysticks de arcade Xbox One também são equipados com botões **Exibir** e **Menu** usado para dar suporte à navegação da interface do usuário, mas eles não estiverem aceitam comandos e não podem ser prontamente acessados como botões de joystick.

![Joystick com 4 direcional joystick, de Arcade 6 botões de ação (A1-A6) e 2 botões especiais (S1 e S2)](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>Navegação da interface do usuário

Para aliviar a sobrecarga do suporte para muitos dispositivos de entrada diferentes para a navegação de interface do usuário e para incentivar a consistência entre dispositivos e jogos, os dispositivos de entrada mais _físicos_ atuam simultaneamente como um dispositivo de entrada _lógico_ separado chamado de [controlador de navegação da interface do usuário](ui-navigation-controller.md). O controlador de navegação da interface do usuário fornece um vocabulário comum para comandos de navegação da interface do usuário em dispositivos de entrada.

Como um controlador de navegação da interface do usuário, os joysticks de arcade mapeiam o [conjunto necessário](ui-navigation-controller.md#required-set) de comandos de navegação para o joystick e os botões de **modo de exibição**, **Menu**, **ação 1**e **2 de ação** .

| Comando de navegação | Entrada do joystick de arcade  |
| ------------------:| ------------------- |
|                 Para cima | Joystick para cima            |
|               Para baixo | Joystick para baixo          |
|               Esquerda | Joystick para a esquerda          |
|              Direita | Joystick para a direita         |
|               Exibir | Botão Exibir         |
|               Menu | Botão Menu         |
|             Aceitar | Botão de ação 1     |
|             Cancelar | Botão de ação 2     |

Os joysticks de arcade não mapeiam qualquer um dos comandos de navegação do [conjunto opcional](ui-navigation-controller.md#optional-set).

## <a name="detect-and-track-arcade-sticks"></a>Detectar e rastrear joysticks de arcade

Detectando e controle de Arcade funciona exatamente da mesma maneira como faz para gamepads, exceto com a classe [ArcadeStick][] em vez da classe [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) . Consulte [Gamepad e vibração](gamepad-and-vibration.md) para obter mais informações.

<!-- Arcade sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected arcades sticks and events to notify you when an arcade stick is added or removed.

### The arcade sticks list

The [ArcadeStick][] class provides a static property, [ArcadeSticks][], which is a read-only list of arcade sticks that are currently connected. Because you might only be interested in some of the connected arcade sticks, it's recommended that you maintain your own collection instead of accessing them through the `ArcadeSticks` property.

The following example copies all connected arcade sticks into a new collection. Note that because other threads in the background will be accessing this collection (in the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events), you need to place a lock around any code that reads or updates the collection.

```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();
critical_section myLock{};

for (auto arcadeStick : ArcadeStick::ArcadeSticks)
{
    // Check if the arcade stick is already in myArcadeSticks; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myArcadeSticks), end(myArcadeSticks), arcadeStick);

    if (it == end(myArcadeSticks))
    {
        // This code assumes that you're interested in all arcade sticks.
        myArcadeSticks->Append(arcadeStick);
    }
}
```

### Adding and removing arcade sticks

When an arcade stick is added or removed the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events are raised. You can register handlers for these events to keep track of the arcade sticks that are currently connected.

The following example starts tracking an arcade stick that's been added.

```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // Check if the just-added arcade stick is already in myArcadeSticks; if it
    // isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

The following example stops tracking an arcade stick that's been removed.

```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each arcade stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-arcade-stick"></a>Lendo o joystick de arcade

Depois de identificar o joystick de arcade de seu interesse, você está pronto para coletar as informações dele. No entanto, ao contrário de outros tipos de entrada com os quais você possa estar acostumado, os joysticks de arcade não comunicam a alteração de estado acionando eventos. Você faz leituras regulares do estado atual deles fazendo uma _sondagem_.

### <a name="polling-the-arcade-stick"></a>Fazendo a sondagem do joystick de arcade

A sondagem captura um instantâneo do joystick de arcade em um momento preciso. Essa abordagem de coleta de entrada é ótima para a maioria dos jogos, pois sua lógica normalmente é executada em um loop determinante em vez de ser orientada por evento; também é normalmente mais simples interpretar os comandos de jogos da entrada coletada de uma vez do que de várias entradas individuais coletadas ao longo do tempo.

Para sondar um joystick de arcade, é preciso chamar [GetCurrentReading][]. Essa função retorna um [ArcadeStickReading][] que contém o estado do joystick de arcade.

O exemplo a seguir sonda o estado atual de um joystick de arcade.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

Além do estado do joystick de arcade, cada leitura inclui um carimbo de data e hora que indica precisamente quando o estado foi recuperado. O carimbo de data e hora é útil para estabelecer a relação entre o tempo das leituras anteriores e o tempo da simulação do jogo.

### <a name="reading-the-buttons"></a>Lendo os botões

Cada um dos botões de joystick de arcade&mdash;as quatro direções do joystick, seis botões de **ação** e dois botões **especiais** &mdash;fornece uma leitura digital que indica se ele foi pressionado (para baixo) ou liberado (para cima). Para garantir a eficiência, as leituras dos botões não são representadas como valores booleanos individuais; em vez disso, elas são reunidas em um único campo de bits que é representado pela enumeração [ArcadeStickButtons][] .

> [!NOTE]
> Os joysticks de Arcade são equipados com botões adicionais usados para navegação na interface do usuário, como os botões de **modo de exibição** e **Menu** . Esses botões não fazem parte da enumeração `ArcadeStickButtons` e só podem ser lidos acessando o joystick de arcade como um dispositivo de navegação da interface do usuário. Para obter mais informações, consulte [Dispositivo de navegação da interface do usuário](ui-navigation-controller.md).

Os valores dos botões são lidos na propriedade `Buttons` da estrutura [ArcadeStickReading][]. Como essa propriedade é um campo de bits, o mascaramento bit a bit é usado para isolar o valor do botão de interesse. O botão está pressionado (para baixo) quando o bit correspondente está definido; caso contrário, ele está liberado (para acima).

O exemplo a seguir determina se o botão **ação 1** está pressionado.

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

O exemplo a seguir determina se o botão **ação 1** está liberado.

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

Às vezes, convém determinar quando um botão passa de pressionado para liberado ou vice-versa, se vários botões foram pressionados ou liberados ou se um conjunto de botões está organizado de determinada maneira &mdash; alguns pressionados, outros, não. Para obter informações sobre como detectar essas condições, consulte [Detectando transições do botão](input-practices-for-games.md#detecting-button-transitions) e [Detectando organizações complexas de botão](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Executar a amostra InputInterfacing

A [amostra InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstra como usar joysticks de arcade e diferentes tipos de dispositivos de entrada em conjunto e como esses dispositivos de entrada se comportam como controladores de navegação da interface do usuário.

## <a name="see-also"></a>Consulte também

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Práticas de entrada para jogos](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[arcadesticks]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadesticks.aspx
[arcadestickadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickadded.aspx
[arcadestickremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.getcurrentreading.aspx
[arcadestickreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickreading.aspx
[arcadestickbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickbuttons.aspx
