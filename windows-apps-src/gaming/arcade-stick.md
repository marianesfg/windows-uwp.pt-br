---
author: mithom
title: Joystick de arcade
description: Use as APIs de joystick de arcade Windows.Gaming.Input para detectar e ler joysticks de arcade.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, joystick de arcade, entrada
ms.openlocfilehash: b0411dcf1fd75ec7dc31d29a39e95f5c26073953
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="arcade-stick"></a>Joystick de arcade

Esta página descreve os conceitos básicos de programação para joysticks de arcade do Xbox One usando [Windows.Gaming.Input.ArcadeStick][arcadestick] e APIs relacionadas para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:
* obter uma lista de joysticks de arcade conectados e seus usuários
* detectar se um joystick de arcade foi adicionado ou removido
* ler a entrada de um ou mais joysticks de arcade
* os joysticks de arcade se comportam como um dispositivo de navegação


## <a name="arcade-stick-overview"></a>Visão geral do joystick de arcade

Os joysticks de arcade são dispositivos de entrada valorizados por reproduzirem a sensação de máquinas de arcade de fliperama e por seus controles digitais de alta precisão. Os joysticks de arcade são o dispositivo de entrada perfeito para luta "mano a mano" e outros jogos estilo arcade e são adequados para qualquer jogo que funcione bem com controles inteiramente digitais. Os joysticks de arcade têm suporte em aplicativos UWP do Windows 10 e do Xbox One pelo namespace [Windows.Gaming.Input][].

Os joysticks de arcade do Xbox One são equipados com um joystick digital de 8 vias, seis botões de **ação** e dois botões **especiais**; eles são dispositivos de entrada inteiramente digitais que não dão suporte a controles analógicos ou vibração. Os joysticks de arcade do Xbox One também são equipados com botões **exibir** e **menu** usados para dar suporte à navegação da interface do usuário, mas eles não aceitam comandos de jogo e não podem ser prontamente acessados como botões de joystick.

### <a name="ui-navigation"></a>Navegação da interface do usuário

Para aliviar a sobrecarga do suporte para muitos dispositivos de entrada diferentes para a navegação de interface do usuário e para incentivar a consistência entre dispositivos e jogos, os dispositivos de entrada mais _físicos_ atuam simultaneamente como um dispositivo de entrada _lógico_ separado chamado de [controlador de navegação da interface do usuário](ui-navigation-controller.md). O controlador de navegação da interface do usuário fornece um vocabulário comum para comandos de navegação da interface do usuário em dispositivos de entrada.

Como controlador de navegação da interface do usuário, o joystick de arcade mapeia o [conjunto obrigatório](ui-navigation-controller.md#required-set) de comandos de navegação para o joystick e os botões **exibir**, **menu**, **ação 1** e **ação 2**.

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

Os joysticks de arcade são gerenciados pelo sistema. Portanto, você não precisa criá-los ou inicializá-los. O sistema fornece uma lista de joysticks de arcade e eventos conectados para notificar você quando um joystick de arcade é adicionado ou removido.

### <a name="the-arcade-sticks-list"></a>A lista de joysticks de arcade

A classe [ArcadeStick][] fornece uma propriedade estática, [ArcadeSticks][], que é uma lista somente leitura de joysticks de arcade conectados no momento. Como você pode estar interessado apenas em alguns dos joysticks de arcade conectados, é recomendável manter sua própria coleção em vez de acessá-los por meio da propriedade `ArcadeSticks`.

O exemplo a seguir copia todos os joysticks de arcade conectados para uma nova coleção.
```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();

for (auto arcadestick : ArcadeStick::ArcadeSticks)
{
    // This code assumes that you're interested in all arcade sticks.
    myArcadeSticks->Append(arcadestick);
}
```

### <a name="adding-and-removing-arcade-sticks"></a>Adicionando e removendo joysticks de arcade

Quando um joystick de arcade é adicionado ou removido, os eventos [ArcadeStickAdded][] e [ArcadeStickRemoved][] são gerados. Você pode registrar manipuladores para esses eventos para rastrear os joysticks de arcade que estão conectados no momento.

O exemplo a seguir inicia o rastreamento de um joystick de arcade que foi adicionado.
```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

O exemplo a seguir interrompe o rastreamento de um joystick de arcade que foi removido.
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

### <a name="users-and-headsets"></a>Usuários e headsets

Cada joystick de arcade pode ser associado a uma conta de usuário para vincular a identidade dele aos jogos e pode ter um headset conectado para facilitar o chat de voz ou recursos no jogo. Para saber mais sobre como trabalhar com usuários e headsets, consulte [Rastreando os usuários e seus dispositivos](input-practices-for-games.md#tracking-users-and-their-devices) e [Headset](headset.md).


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

Cada um dos botões do joystick de arcade – as quatro direções do joystick, seis botões de **ação** e dois botões **especiais** – fornece uma leitura digital que indica se ele está pressionado (para baixo) ou liberado (para cima). Para garantir a eficiência, as leituras dos botões não são representadas como valores booleanos individuais; elas são reunidas em um único campo de bits que é representado pela enumeração [ArcadeStickButtons][].

> **Observação**    Os joysticks de arcade são equipados com botões adicionais usados para navegação da interface do usuário, como os botões **exibir** e **menu**. Esses botões não fazem parte da enumeração `ArcadeStickButtons` e só podem ser lidos acessando o joystick de arcade como um dispositivo de navegação da interface do usuário. Para obter mais informações, consulte [Dispositivo de navegação da interface do usuário](ui-navigation-controller.md).

Os valores dos botões são lidos na propriedade `Buttons` da estrutura [ArcadeStickReading][]. Como essa propriedade é um campo de bits, o mascaramento bit a bit é usado para isolar o valor do botão de seu interesse. O botão está pressionado (para baixo) quando o bit correspondente está definido; caso contrário, ele está liberado (para acima).

O exemplo a seguir determina se o botão Ação 1 está pressionado.
```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

O exemplo a seguir determina se o botão Ação 1 está liberado.
```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

Às vezes, convém determinar quando um botão passa de pressionado para liberado ou vice-versa, se vários botões foram pressionados ou liberados ou se um conjunto de botões está organizado de determinada maneira – alguns pressionados, outros, não. Para obter informações sobre como detectar essas condições, consulte [Detectando transições do botão](input-practices-for-games.md#detecting-button-transitions) e [Detectando organizações complexas de botão](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Executar a amostra InputInterfacing

A [amostra InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstra como usar joysticks de arcade e diferentes tipos de dispositivos de entrada em conjunto e como esses dispositivos de entrada se comportam como controladores de navegação da interface do usuário.


## <a name="see-also"></a>Consulte também
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]

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
