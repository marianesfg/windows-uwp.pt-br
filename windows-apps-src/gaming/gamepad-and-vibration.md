---
author: eliotcowley
title: Gamepad e vibração
description: Use as APIs de gamepad Windows.Gaming.Input para detectar, ler e enviar comandos de impulso e vibração para gamepads.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.author: wdg-dev-content
ms.date: 09/06/2018
ms.topic: article
keywords: windows 10, uwp, jogos, gamepad, vibração
ms.localizationpriority: medium
ms.openlocfilehash: 4ea8afb0a9e66ccb4ea603bd78dc5030ca18babe
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6182602"
---
# <a name="gamepad-and-vibration"></a>Gamepad e vibração

Esta página descreve os conceitos básicos de programação para gamepads do Xbox One usando [Windows.Gaming.Input.Gamepad][gamepad] e APIs relacionadas para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:

* obter uma lista de gamepads conectados e seus usuários
* detectar se um gamepad foi adicionado ou removido
* ler entradas de um ou mais gamepads
* enviar comandos de vibração e impulso
* os gamepads se comportam como dispositivos de navegação da interface do usuário

## <a name="gamepad-overview"></a>Visão geral do gamepad

Gamepads como o Controle sem Fio Xbox e o Controle sem Fio Xbox S são dispositivos de entrada de jogos de finalidade geral. Eles são o dispositivo de entrada padrão no Xbox One e uma opção comum para jogadores do Windows quando eles não gostam de usar teclado e mouse. Gamepads têm suporte em aplicativos UWP do Windows 10 e Xbox pelo namespace [Windows.Gaming.Input][].

Gamepads do Xbox One são equipados com um teclado direcional (ou D-pad); **A**, **B**, **X**, **Y**, **modo de exibição**e botões de **Menu** ; thumbsticks esquerdo e direito, botões superiores e gatilhos; e um total de quatro motores de vibração. Os dois botões direcionais fornecem leituras duplamente analógicas nos eixos X e Y e também funcionam como um botão comum quando pressionados para dentro. Cada gatilho fornece uma leitura analógica que representa a distância é extraída novamente.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` também dá suporte a gamepads do Xbox 360, que têm o mesmo layout de controle padrão gamepads do Xbox One.

### <a name="vibration-and-impulse-triggers"></a>Gatilhos de vibração e impulso

Os gamepads do Xbox One fornecem dois motores independentes para vibração forte e sutil do gamepad, bem como dois motores dedicados para fornecer uma vibração acentuada a cada gatilho (esse recurso exclusivo é o motivo pelo qual os gatilhos do gamepad do Xbox One são chamados de _gatilhos de impulso_).

> [!NOTE]
> Gamepads do Xbox 360 não são equipados com _gatilhos de impulso_.

Para obter mais informações, consulte [Visão geral dos gatilhos de vibração e impulso](#vibration-and-impulse-triggers-overview).

### <a name="thumbstick-deadzones"></a>Zonas mortas dos botões

O ideal é que um botão em repouso na posição central produza a mesma leitura neutra nos eixos X e Y sempre. Entretanto, devido à força mecânica e sensibilidade do botão, as leituras reais na posição central apenas se aproximam do valor neutro ideal e podem variar entre as leituras subsequentes. Por esse motivo, você sempre deve usar um pequeno _zona morta_&mdash;um intervalo de valores próximos à posição ideal central que são ignorados&mdash;para compensar diferenças de fabricação, o desgaste mecânico ou outros problemas do gamepad.

Zonas mortas maiores oferecem uma estratégia simples para separar a entrada intencional da entrada não intencional.

Para obter mais informações, consulte [Lendo os botões de controle](#reading-the-thumbsticks).

### <a name="ui-navigation"></a>Navegação da interface do usuário

Para aliviar a sobrecarga do suporte para diferentes dispositivos de entrada para a navegação de interface do usuário e para incentivar a consistência entre dispositivos e jogos, os dispositivos de entrada mais _físicos_ atuam simultaneamente como um dispositivo de entrada _lógico_ separado chamado de [controlador de navegação da interface do usuário](ui-navigation-controller.md). O controlador de navegação da interface do usuário fornece um vocabulário comum para comandos de navegação da interface do usuário em dispositivos de entrada.

Como um controlador de navegação da interface do usuário, o Gamepad mapeia o [conjunto obrigatório](ui-navigation-controller.md#required-set) de comandos de navegação para o botão esquerdo, direcional, **Exibir**, **Menu**, **A**e **B** botões.

| Comando de navegação | Entrada do gamepad                       |
| ------------------:| ----------------------------------- |
|                 Para cima | Botão esquerdo para cima/Direcional para cima       |
|               Para baixo | Botão esquerdo para baixo/Direcional para baixo   |
|               Esquerda | Botão esquerdo para a esquerda/Direcional para a esquerda   |
|              Direita | Botão esquerdo para a direita/Direcional para a direita |
|               Exibir | Botão Exibir                         |
|               Menu | Botão Menu                         |
|             Aceitar | Botão A                            |
|             Cancelar | Botão B                            |

Além disso, o gamepad mapeia todo o [conjunto opcional](ui-navigation-controller.md#optional-set) de comandos de navegação para as entradas restantes.

| Comando de navegação | Entrada do gamepad          |
| ------------------:| ---------------------- |
|            Página acima | Gatilho esquerdo           |
|          Página abaixo | Gatilho direito          |
|          Página à esquerda | Botão superior esquerdo            |
|         Página à direita | Botão superior direito           |
|          Role para cima | Botão direito para cima    |
|        Rolar para baixo | Botão direito para baixo  |
|        Rolar para a esquerda | Botão direito para a esquerda  |
|       Rolar para a direita | Botão direito para a direita |
|          Contexto 1 | Botão X               |
|          Contexto 2 | Botão Y               |
|          Contexto 3 | Pressionar o botão esquerdo  |
|          Contexto 4 | Pressionar o botão direito |

## <a name="detect-and-track-gamepads"></a>Detectar e rastrear gamepads

Os gamepads são gerenciados pelo sistema. Portanto, você não precisa criá-los ou inicializá-los. O sistema fornece uma lista de gamepads e eventos conectados para notificar você quando um gamepad é adicionado ou removido.

### <a name="the-gamepads-list"></a>A lista de gamepads

A classe [Gamepad][] fornece uma propriedade estática, [Gamepads][], que é uma lista somente leitura de gamepads que estão conectados no momento. Como você pode estar interessado apenas em alguns dos gamepads conectados, é recomendável manter sua própria coleção em vez de acessá-los por meio do `Gamepads` propriedade.

O exemplo a seguir copia todos os gamepads conectados para uma nova coleção. Observe que, como outros threads em segundo plano acessarão essa coleção (nos eventos [GamepadAdded][] e [GamepadRemoved][] ), você precisa colocar um bloqueio em torno de qualquer código que lê ou atualiza a coleção.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>Adicionando e removendo gamepads

Quando um gamepad é adicionado ou removido, os eventos [GamepadAdded][] e [GamepadRemoved][] são gerados. Você pode registrar manipuladores para esses eventos para rastrear os gamepads que estão conectados no momento.

O exemplo a seguir começa a rastrear um gamepad que foi adicionado.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

O exemplo a seguir para o acompanhamento de um gamepad que foi removido. Você também precisará manipular o que acontece com os gamepads que você está controlando quando forem removidas; Por exemplo, esse código apenas rastreia entrada de um gamepad e simplesmente define-a como `nullptr` quando ele é removido. Você precisará verificar cada quadro, caso o gamepad está ativo e quais gamepad você estiver coleta de entrada do quando controladores são conectados e desconectados de atualização.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

Consulte [práticas de entrada para jogos](input-practices-for-games.md) para obter mais informações.

### <a name="users-and-headsets"></a>Usuários e headsets

Cada gamepad pode ser associado a uma conta de usuário para vincular a identidade dele aos jogos e pode ter um headset conectado para facilitar o chat de voz ou recursos no jogo. Para saber mais sobre como trabalhar com usuários e headsets, consulte [Rastreando os usuários e seus dispositivos](input-practices-for-games.md#tracking-users-and-their-devices) e [Headset](headset.md).

## <a name="reading-the-gamepad"></a>Lendo o gamepad

Depois de identificar o gamepad de seu interesse, você está pronto para coletar as informações dele. No entanto, ao contrário de outros tipos de entrada com os quais você possa estar acostumado, os gamepads não comunicam a alteração de estado acionando eventos. Você faz leituras regulares do estado atual deles fazendo uma _sondagem_.

### <a name="polling-the-gamepad"></a>Fazendo a sondagem do gamepad

A sondagem captura um instantâneo do dispositivo de navegação em um momento preciso. Essa abordagem de coleta de entrada é ótima para a maioria dos jogos, pois sua lógica normalmente é executada em um loop determinante em vez de ser orientada por evento; também é normalmente mais simples interpretar os comandos de jogos da entrada coletada de uma vez do que de várias entradas individuais coletadas ao longo do tempo.

Para sondar um gamepad, é preciso chamar [GetCurrentReading][]. Essa função retorna um [GamepadReading][] que contém o estado do gamepad.

O exemplo a seguir sonda o estado atual de um gamepad.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

Além do estado do gamepad, cada leitura inclui um carimbo de data e hora que indica precisamente quando o estado foi recuperado. O carimbo de data e hora é útil para estabelecer a relação entre o tempo das leituras anteriores e o tempo da simulação do jogo.

### <a name="reading-the-thumbsticks"></a>Lendo os botões de controle

Cada botão fornece uma leitura analógica entre -1,0 e + 1,0 nos eixos X e Y. No eixo X, um valor de -1,0 corresponde à posição mais à esquerda do botão; um valor de +1,0 corresponde à posição mais à direita. No eixo Y, um valor de -1,0 corresponde à posição mais inferior do botão; um valor de +1,0 corresponde à posição mais superior. Nos dois eixos, o valor é de aproximadamente 0,0 quando o joystick está na posição central, mas é normal que o valor exato varie, até mesmo entre as leituras subsequentes; estratégias para atenuar essa variação são discutidas posteriormente nesta seção.

O valor do eixo X do botão esquerdo é lido na propriedade `LeftThumbstickX` da estrutura [GamepadReading][]; o valor do eixo Y é lido na propriedade `LeftThumbstickY`. O valor do eixo X do botão direito é lido na propriedade `RightThumbstickX`; o valor do eixo Y é lido na propriedade `RightThumbstickY`.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

Na leitura dos valores dos botões, você observará que eles não produzem uma leitura neutra confiável de 0,0 quando o botão está em repouso na posição central; eles produzirão valores diferentes próximos de 0,0 cada vez que o botão for movido e retornado para a posição central. Para atenuar essas variações, você pode implementar uma pequena _zona morta_, que é um intervalo de valores próximos à posição central ideal que são ignorados. Uma maneira de implementar uma zona morta é determinar a que distância do centro o botão foi movido e ignorar as leituras mais próximas em vez da distância que você escolher. Você pode calcular a distância aproximadamente&mdash;não é exata porque as leituras dos botões são essencialmente valores polares, não planares&mdash;usando apenas o Teorema de Pitágoras. Isso produz uma zona morta radial.

O exemplo a seguir demonstra uma zona morta radial básica usando o Teorema de Pitágoras.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

Cada botão de controle também atua como um botão comum quando pressionado para dentro. Para obter mais informações sobre a leitura dessa entrada, consulte [Lendo os botões](#reading-the-buttons).

### <a name="reading-the-triggers"></a>Lendo os gatilhos

Os gatilhos são representados como valores de ponto flutuante entre 0,0 (totalmente liberado) e 1,0 (totalmente pressionado). O valor do gatilho esquerdo é lido na propriedade `LeftTrigger` da estrutura [GamepadReading][]; o valor do gatilho direito é lido na propriedade `RightTrigger`.

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>Lendo os botões

Cada um dos botões de gamepad&mdash;as quatro direções do direcional, botões superiores esquerdos e direito, pressionar o botão esquerdo e direito, **A**, **B**, **X**, **Y**, **modo de exibição**e **Menu**&mdash;fornece um digital ler Indica se ele foi pressionado (para baixo) ou liberado (para cima). Para garantir a eficiência, as leituras dos botões não são representadas como valores booleanos individuais; em vez disso, elas são reunidas em um único campo de bits que é representado pela enumeração [GamepadButtons][] .

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

Os valores dos botões são lidos na propriedade `Buttons` da estrutura [GamepadReading][]. Como essa propriedade é um campo de bits, o mascaramento bit a bit é usado para isolar o valor do botão de seu interesse. O botão está pressionado (para baixo) quando o bit correspondente está definido; caso contrário, ele está liberado (para acima).

O exemplo a seguir determina se o botão A está pressionado.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

O exemplo a seguir determina se o botão A está liberado.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

Às vezes, convém determinar quando um botão passa de pressionado para liberado ou vice-versa, se vários botões foram pressionados ou liberados, ou se um conjunto de botões está organizado de determinada maneira&mdash;alguns pressionados, outros não. Para obter informações sobre como detectar cada uma dessas condições, consulte [Detectando transições do botão](input-practices-for-games.md#detecting-button-transitions) e [Detectando organizações complexas de botão](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-gamepad-input-sample"></a>Executar a amostra de entrada de gamepad

A [amostra GamepadUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) demonstra como se conectar a um gamepad e ler o estado dele.

## <a name="vibration-and-impulse-triggers-overview"></a>Visão geral de gatilhos de vibração e impulso

Os motores de vibração dentro de um gamepad fornecem feedback tátil ao usuário. Os jogos usam essa capacidade para criar uma noção maior de imersão, para ajudar a comunicar informações de status (como sofrendo danos), para sinalizar a proximidade a objetos importantes ou para outros usos criativos.

Os gamepads do Xbox One são equipados com um total de quatro motores de vibração independentes. Dois são motores grandes localizados no corpo do gamepad; o motor esquerdo fornece vibração bruta de alta amplitude, enquanto o motor direito fornece uma vibração mais sutil e suave. Os outros dois são motores pequenos, dentro de cada gatilho, que fornecem picos acentuados de vibração diretamente para os dedos de gatilho do usuário; essa habilidade única do gamepad do Xbox One é o motivo pelo qual seus gatilhos são chamados de _gatilhos de impulso_. Ao orquestrar esses motores juntos, uma ampla variedade de sensações táteis pode ser produzida.

## <a name="using-vibration-and-impulse"></a>Usando vibração e impulso

A vibração do gamepad é controlada por meio da propriedade [Vibration][] da classe [Gamepad][]. `Vibration` é uma instância da estrutura [GamepadVibration][] que é composta de quatro valores de ponto flutuante; cada valor representa a intensidade de um dos motores.

Embora os membros do `Gamepad.Vibration` propriedade pode ser modificada diretamente, é recomendável que você inicializar um separado `GamepadVibration` instância para os valores que você deseja e, em seguida, copie-o para o `Gamepad.Vibration` propriedade para alterar as intensidades reais do motoras de uma vez.

O exemplo a seguir demonstra como alterar as intensidades do motor de uma vez.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

### <a name="using-the-vibration-motors"></a>Usando os motores de vibração

Os motores de vibração esquerdo e direito usam valores de ponto flutuante entre 0,0 (nenhuma vibração) e 1,0 (vibração mais intensa). A intensidade do motor esquerdo é definida pela propriedade `LeftMotor` da estrutura [GamepadVibration][]; a intensidade do motor direito é definida pela propriedade `RightMotor`.

O exemplo a seguir define a intensidade dos dois motores de vibração e ativa a vibração gamepad.

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

Lembre-se de que esses dois motores não são idênticos. Portanto, definir essas propriedades com o mesmo valor não produz a mesma vibração em um motor como no outro. Para qualquer valor, o motor esquerdo produz uma vibração mais forte com uma frequência menor que o direito de r motor que&mdash;para o mesmo valor&mdash;produz uma vibração mais suave e com maior frequência. Mesmo no valor máximo, o motor esquerdo não consegue produzir as frequências altas do motor direito, nem o motor direito consegue produzir as forças altas do motor esquerdo. Ainda assim, como os motores são rigidamente conectados ao corpo do gamepad, os jogadores não sentem as vibrações totalmente de forma independente mesmo os motores tendo características diferentes e podendo vibrar com intensidades diferentes. Esse esquema permite produzir uma variedade mais ampla e mais expressiva de sensações do que se os motores fossem idênticos.

### <a name="using-the-impulse-triggers"></a>Usando os gatilhos de impulso

Cada motor de gatilho de impulso usa um valor de ponto flutuante entre 0,0 (nenhuma vibração) e 1,0 (vibração mais intensa). A intensidade do motor de gatilho esquerdo é definida pela propriedade `LeftTrigger` da estrutura [GamepadVibration][]; a intensidade do gatilho direito é definida pela propriedade `RightTrigger`.

O exemplo a seguir define a intensidade dos dois gatilhos de impulso e os ativa.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

Ao contrário dos outros, os dois motores de vibração dentro dos gatilhos são idênticos. Portanto, eles produzem a mesma vibração em qualquer um dos motores para o mesmo valor. No entanto, como esses motores não estão conectados rigidamente de forma alguma, os jogadores sentirão as vibrações de forma independente. Esse esquema permite que sensações totalmente independentes sejam direcionadas aos dois gatilhos simultaneamente e os ajuda a transmitir informações mais específicas do que os motores no corpo do gamepad.

## <a name="run-the-gamepad-vibration-sample"></a>Executar a amostra de vibração do gamepad

A [amostra GamepadVibrationUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) demonstra como os motores de vibração e os gatilhos de impulso do gamepad são usados para produzir uma variedade de efeitos.

## <a name="see-also"></a>Consulte também

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Práticas de entrada para jogos](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[gamepads]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[vibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx
