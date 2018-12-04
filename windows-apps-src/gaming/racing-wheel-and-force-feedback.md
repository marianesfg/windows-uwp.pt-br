---
title: Volante de corrida e force feedback
description: Use as APIs do volante de corrida Windows.Gaming.Input para detectar, determinar funcionalidades, ler e enviar comandos de force feedback.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10, uwp, jogos, volante de corrida, force feedback
ms.localizationpriority: medium
ms.openlocfilehash: 90d12caca103648824ceb36a4ca4968754beb7f2
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8471594"
---
# <a name="racing-wheel-and-force-feedback"></a>Volante de corrida e force feedback

Esta página descreve os conceitos básicos de programação para volantes de corrida do Xbox One que usam as APIs relacionadas [Windows.Gaming.Input.RacingWheel][racingwheel] para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:

* obter uma lista de volantes de corrida conectados e os usuários
* detectar se um volante de corrida foi adicionado ou removido
* ler entradas de um ou mais volantes de corrida
* enviar comandos de force feedback
* como volantes de corrida se comportam como um dispositivo de navegação da interface do usuário

## <a name="racing-wheel-overview"></a>Visão geral do volante de corrida

Volantes de corrida são dispositivos de entrada que reproduzem a sensação do cockpit de um carro de corrida real. Volantes de corrida são o dispositivo de entrada perfeito para jogos de corrida em estilos arcade e simulação com carros ou caminhões. Os volantes de corrida são compatíveis em aplicativos UWP do Windows 10 e do Xbox One pelo namespace [Windows.Gaming.Input][].

Os volantes de corrida do Xbox One são oferecidos em uma grande variedade de faixas de preço com entrada e funcionalidades de force feedback melhores à medida que as faixas de preços sobem. Todos os volantes de corrida estão equipados com um volante analógico, controles de aceleração e freio analógicos e alguns botões no volante. Além disso, alguns volantes de corrida estão equipados com controles de embreagem e freio de mão analógicos, borboletas de câmbio e funcionalidades de force feedback. Nem todos os volantes de corrida estão equipados com os mesmos conjuntos de recursos e também podem variar no suporte para determinados recursos, por exemplo, os volantes podem suportar graus de esterçamento diferentes e as borboletas de câmbio podem suportar números de marchas diferentes.

### <a name="device-capabilities"></a>Funcionalidades de dispositivo

Os volantes de corrida do Xbox One oferecem conjuntos de funcionalidades de dispositivo opcionais diferentes e níveis variados de suporte para essas funcionalidades; esse nível de variação entre um único tipo de dispositivo de entrada é exclusivo dentre os dispositivos compatíveis com as APIs [Windows.Gaming.Input][]. Além disso, a maioria dos dispositivos que você encontrará dará suporte a pelo menos alguma funcionalidade opcional ou outras variações. Por isso, é importante determinar as funcionalidades de cada volante de corrida conectado individualmente e dar suporte a toda a variação de funcionalidades justificável para o jogo.

Para obter mais informações, consulte [Determinar funcionalidades de volante de corrida](#determining-racing-wheel-capabilities).

### <a name="force-feedback"></a>Force feedback

Alguns volantes de corrida do Xbox One oferecem o force feedback real, ou seja, eles podem aplicar forças reais em um eixo de controle como no volante, e não uma simples vibração. Os jogos usam essa funcionalidade para criar uma sensação maior de imersão (_dano por batida simulado_, "sensação da estrada") e aumentar o desafio de guiar bem.

Para obter mais informações, consulte [Visão geral do force feedback](#force-feedback-overview).

### <a name="ui-navigation"></a>Navegação da interface do usuário

Para aliviar a sobrecarga do suporte para diferentes dispositivos de entrada para a navegação de interface do usuário e para incentivar a consistência entre dispositivos e jogos, os dispositivos de entrada mais _físicos_ atuam simultaneamente como um dispositivo de entrada _lógico_ separado chamado de [controlador de navegação da interface do usuário](ui-navigation-controller.md). O controlador de navegação da interface do usuário fornece um vocabulário comum para comandos de navegação da interface do usuário em dispositivos de entrada.

Por causa do foco exclusivo sobre controles analógicos e o grau de variação entre volantes de corrida diferentes, eles normalmente estão equipados com um direcional digital, botões **Exibir**, **Menu**, **A**, **B**, **X** e **Y** que se assemelham aos de um [gamepad](gamepad-and-vibration.md); esses botões não devem dar suporte a comandos de jogo e não podem ser prontamente acessados como botões de volante de corrida.

Como um controlador de navegação da interface do usuário, os volantes de corrida mapeiam o [conjunto obrigatório](ui-navigation-controller.md#required-set) de comandos de navegação para o botão esquerdo, direcional, botões **Exibir**, **Menu**, **A** e **B**.

| Comando de navegação | Entrada do volante de corrida |
| ------------------:| ------------------ |
|                 Para cima | Direcional para cima           |
|               Para baixo | Direcional para baixo         |
|               Para esquerda | Direcional para esquerda         |
|              Direita | Direcional para direita        |
|               Exibir | Botão Exibir        |
|               Menu | Botão Menu        |
|             Aceitar | Botão A           |
|             Cancelar | Botão B           |

Além disso, alguns volantes de corrida podem mapear alguns dos comandos de navegação do [conjunto opcional](ui-navigation-controller.md#optional-set) para outras entradas compatíveis, mas os mapeamentos de comando podem variar de um dispositivo para outro. Também leve em consideração o suporte a esses comandos, mas certifique-se de que esses comandos não sejam essenciais para navegar na interface do jogo.

| Comando de navegação | Entrada do volante de corrida    |
| ------------------:| --------------------- |
|            Página acima | _varia_              |
|          Página abaixo | _varia_              |
|          Página à esquerda | _varia_              |
|         Página à direita | _varia_              |
|          Rolar para cima | _varia_              |
|        Rolar para baixo | _varia_              |
|        Rolar para esquerda | _varia_              |
|       Rolar para direita | _varia_              |
|          Contexto 1 | Botão X (_normalmente_) |
|          Contexto 2 | Botão Y (_normalmente_) |
|          Contexto 3 | _varia_              |
|          Contexto 4 | _varia_              |

## <a name="detect-and-track-racing-wheels"></a>Detectar e acompanhar volantes de corrida

A detecção e o acompanhamento de joysticks para simulador de voo funcionam exatamente da mesma forma nos gamepads, exceto na classe [RacingWheel][] em vez da classe [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad). Consulte [Gamepad e vibração](gamepad-and-vibration.md) para obter mais informações.

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel][] class provides a static property, [RacingWheels][], which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded][] and [RacingWheelRemoved][] events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>Como ler o volante de corrida

Depois de identificar os volantes de corrida de interesse, você estará pronto para coletar informações dele. No entanto, diferentemente de outros tipos de entrada com os quais você possa estar acostumado, os volantes de corrida não informam a alteração de estado acionando eventos. Você faz leituras regulares do estado atual deles fazendo uma _sondagem_.

### <a name="polling-the-racing-wheel"></a>Como sondar o volante de corrida

A sondagem registra um instantâneo do volante de corrida em um momento preciso. Essa abordagem de coleta de entrada é ótima para a maioria dos jogos, pois sua lógica normalmente é executada em um loop determinante em vez de ser orientada por evento; também é normalmente mais simples interpretar os comandos de jogos da entrada coletada de uma vez do que de várias entradas individuais coletadas ao longo do tempo.

Você sonda um volante de corrida chamando [GetCurrentReading][]; essa função retorna um [RacingWheelReading][] que contém o estado do volante de corrida.

O exemplo a seguir faz a sondagem de um volante de corrida para saber o estado atual.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

Além do estado do volante de corrida, cada leitura inclui um carimbo de data e hora que indica precisamente quando o estado foi recuperado. O carimbo de data e hora é útil para estabelecer a relação entre o tempo das leituras anteriores e o tempo da simulação do jogo.

### <a name="determining-racing-wheel-capabilities"></a>Como determinar funcionalidades do volante de corrida

Muitos dos controles de volante de corrida são opcionais ou dão suporte a variações diferentes mesmo nos controles obrigatórios. Portanto, você precisa determinar as funcionalidades de cada volante de corrida individualmente para poder processar a entrada coletada em cada leitura do volante de corrida.

Os controles opcionais são o freio de mão, a embreagem e a borboleta de câmbio; você pode determinar se um volante de corrida conectado dá suporte a esses controles lendo as propriedades [HasHandbrake][], [HasClutch][] e [HasPatternShifter][] do volante de corrida, respectivamente. O controle será compatível se o valor da propriedade for **true**; do contrário, ele não será compatível.

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

Além disso, os controles que podem variar são o volante de corrida e a borboleta de câmbio. O volante de corrida pode variar de acordo com o grau de esterçamento físico que o volante real pode suportar, e a borboleta de cambio pode variar de acordo com o número de marchas à frente que ele suporta. Você pode determinar o maior ângulo de esterçamento a que o volante real dá suporte lendo a propriedade `MaxWheelAngle` do volante de corrida; o valor é o ângulo físico suportado máximo em graus no sentido horário (positivo), suportado da mesma forma no sentido anti-horário (graus negativos). Você pode determinar o maior marcha à frente a que a borboleta de câmbio dá suporte lendo a propriedade `MaxPatternShifterGear` do volante de corrida; o valor é a maior marcha à frente suportada, inclusive&mdash;ou seja, se o valor for 4, a borboleta de câmbio dará suporte a ré, neutro, primeira, segunda, terceira e quarta marchas.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

Por fim, alguns volantes de corrida são suporte a force feedback por meio do volante de corrida. Você pode determinar se um volante de corrida conectado dá suporte a force feedback lendo a propriedade [WheelMotor][] do volante de corrida. Force feedback será compatível se `WheelMotor` não for **null**; do contrário, ele não será compatível.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

Para obter informações sobre como usar a funcionalidade de force feedback dos volantes de corrida compatíveis, consulte [Visão geral do force feedback](#force-feedback-overview).

### <a name="reading-the-buttons"></a>Lendo os botões

Cada um dos botões do volante de corrida&mdash;as quatro direções do direcional, os botões **Marcha anterior** e **Marcha posterior** e os 16 botões adicionais&mdash;oferecem uma leitura digital que indica se ele está pressionado (para baixo) ou liberado (para cima). Para garantir a eficiência, as leituras dos botões não são representadas como valores boolianos individuais; elas são empacotadas em um único campo de bits representado pela enumeração [RacingWheelButtons][].

> [!NOTE]
> Os volantes de corrida são equipados com botões adicionais usados para navegação na interface do usuário, como os botões **Exibir** e **Menu**. Esses botões não fazem parte da enumeração `RacingWheelButtons` e só podem ser lidos acessando-se o volante de corrida como um dispositivo de navegação da interface do usuário. Para obter mais informações, consulte [Dispositivo de navegação da interface do usuário](ui-navigation-controller.md).

Os valores dos botões são lidos pela propriedade `Buttons` da estrutura [RacingWheelReading][]. Como essa propriedade é um campo de bits, o mascaramento bit a bit é usado para isolar o valor do botão de interesse. O botão está pressionado (para baixo) quando o bit correspondente está definido; caso contrário, ele está liberado (para acima).

O exemplo a seguir determina se o botão **Marcha posterior** é pressionado.

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

O exemplo a seguir determina se o botão Marcha posterior é liberado.

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

Às vezes, convém determinar quando um botão passa de pressionado para liberado ou vice-versa, se vários botões foram pressionados ou liberados ou se um conjunto de botões está organizado de determinada maneira &mdash; alguns pressionados, outros, não. Para obter informações sobre como detectar essas condições, consulte [Detectando transições do botão](input-practices-for-games.md#detecting-button-transitions) e [Detectando organizações complexas de botão](input-practices-for-games.md#detecting-complex-button-arrangements).

### <a name="reading-the-wheel"></a>Como ler o volante

O volante é um controle obrigatório que fornece uma leitura analógica entre -1,0 e +1,0. Um valor -1,0 corresponde à posição mais à esquerda do volante; um valor + 1,0 corresponde à posição mais à direita. O valor do volante é lido com base na propriedade `Wheel` da estrutura [RacingWheelReading][].

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

Embora as leituras do volante correspondam a graus diferentes de esterçamento físico no volante real, dependendo da faixa de esterçamento compatível com o volante de corrida físico, você normalmente não quer dimensionar as leituras do volante; volantes compatíveis com graus de esterçamento maiores acabam proporcionando mais precisão.

### <a name="reading-the-throttle-and-brake"></a>Como ler o acelerador e o freio

O acelerador e o freio são controles obrigatórios que fornecem leituras analógicas entre 0,0 (totalmente liberados) e 1,0 (totalmente pressionados) representadas como valores de ponto flutuante. O valor de controle do acelerador é lido com base na propriedade `Throttle` do struct [RacingWheelReading][]; o valor de controle do freio é lido com base na propriedade `Brake`.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>Como ler o freio de mão e a embreagem

O freio de mão e a embreagem são controles opcionais que fornecem leituras analógicas entre 0,0 (totalmente liberados) e 1,0 (totalmente acionados) representados como valores de ponto flutuante. O valor de controle do freio de mão é lido com base na propriedade `Handbrake` do struct [RacingWheelReading][]; o valor de controle da embreagem é lido com base na propriedade `Clutch`.

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>Como ler a borboleta de câmbio

A borboleta de câmbio é um controle opcional que fornece uma leitura digital entre -1 e [MaxPatternShifterGear][] representada como um valor de inteiro assinado. Um valor -1 ou 0 corresponde às marchas _a ré_ e _neutra_ marcha, respectivamente; valores cada vez mais positivos correspondem a marchas à frente mais altas até [MaxPatternShifterGear][], inclusive. O valor da borboleta de câmbio é lido com base na propriedade `PatternShifterGear` do struct [RacingWheelReading][].

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> A borboleta de câmbio, quando compatível, fica ao lado dos botões de **Marcha anterior** e **Posterior** obrigatórios, o que também afeta a marcha atual do carro do jogador. Uma estratégia simple para unificar essas entradas nas quais ambos estejam presentes é ignorar a borboleta de câmbio (e a embreagem) quando um jogador escolhe uma transmissão automática para o carro e só ignorar os botões de **Marcha anterior** e **posterior** quando um jogador escolhe uma transmissão manual para o carro caso o volante de corrida esteja equipado com uma borboleta de câmbio. Será possível implementar uma estratégia de unificação diferente se ela não for indicada para o jogo.

## <a name="run-the-inputinterfacing-sample"></a>Executar o exemplo InputInterfacing

O [exemplo InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstra como usar volantes de corrida e diferentes tipos de dispositivos de entrada em conjunto, além de como esses dispositivos de entrada se comportam como controladores de navegação da interface do usuário.

## <a name="force-feedback-overview"></a>Visão geral do force feedback

Muitos volantes de corrida têm a funcionalidade de force feedback para oferecer uma experiência de direção mais imersiva e desafiadora. Os volantes de corrida compatíveis com force feedback normalmente são equipados com um único motor que aplica força ao volante ao longo de um único eixo, o eixo de esterçamento do volante. O force feedback é compatível em aplicativos UWP do Windows 10 e do Xbox One pelo namespace [Windows.Gaming.Input.ForceFeedback][].

> [!NOTE]
> As APIs de force feedback são capazes de dar suporte a diversos eixos de força, mas nenhum volante de corrida do Xbox One atualmente dá suporte a qualquer eixo de feedback que não seja de esterçamento do volante.

## <a name="using-force-feedback"></a>Como usar force feedback

Estas seções descrevem as noções básicas de programação dos efeitos de force feedback para volantes de corrida do Xbox One. O feedback é aplicado usando-se efeitos, que são carregados primeiramente sobre o dispositivo de force feedback e, em seguida, podem ser iniciados, pausados, retomados e interrompidos de maneira semelhante a efeitos sonoros. No entanto, você deve primeiro determinar as funcionalidades de feedback do volante de corrida.

### <a name="determining-force-feedback-capabilities"></a>Como determinar funcionalidades de force feedback

Você pode determinar se um volante de corrida conectado dá suporte a force feedback lendo a propriedade [WheelMotor][] do volante de corrida. O force feedback não será compatível se `WheelMotor` for **null**. Do contrário, force feedback será compatível, e será possível continuar para determinar as funcionalidades de feedback específicas do motor, como os eixos que ele pode afetar.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>Como carregar efeitos de force feedback

Os efeitos de force feedback são carregados no dispositivo de feedback no qual são "executados" de maneira autônoma no comando do jogo. Vários efeitos básicos sãos fornecidos; efeitos personalizados podem ser criados por meio de uma classe que implementa a interface [IForceFeedbackEffect][].

| Classe de efeito         | Descrição de efeito                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | Um efeito que aplica força variável em resposta ao sensor atual dentro do dispositivo. |
| ConstantForceEffect  | Um efeito que aplica força constante ao longo de um vetor.                                  |
| PeriodicForceEffect  | Um efeito que aplica força variável definida por uma forma de onda ao longo de um vetor.           |
| RampForceEffect      | Um efeito que aplica uma força que aumenta/diminui de maneira linear ao longo de um vetor.          |

```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>Como usar efeitos de force feedback

Depois de carregados, os efeitos podem ser todos iniciados, pausados, retomados e interrompidos de maneira síncrona chamando funções na propriedade `WheelMotor` do volante de corrida ou individual chamando funções sobre o efeito de feedback propriamente dito. Normalmente, você deve carregar todos os efeitos que deseja usar no dispositivo de feedback antes do jogo começar e, em seguida, usar as respectivas funções `SetParameters` para atualizar os efeitos à medida que o jogo avança.

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

Por fim, é possível habilitar, desabilitar ou redefinir de maneira assíncrona todo o sistema de force feedback em um determinado volante sempre que precisar.

## <a name="see-also"></a>Consulte também

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Práticas de entrada para jogos](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[racingwheels]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheels.aspx
[racingwheeladded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheeladded.aspx
[racingwheelremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheelremoved.aspx
[haspatternshifter]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.haspatternshifter.aspx
[hashandbrake]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hashandbrake.aspx
[hasclutch]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hasclutch.aspx
[maxpatternshiftergear]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxpatternshiftergear.aspx
[maxwheelangle]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxwheelangle.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.getcurrentreading.aspx
[wheelmotor]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.wheelmotor.aspx
[racingwheelreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelreading.aspx
[racingwheelbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelbuttons.aspx
