---
title: Práticas de entrada para jogos
description: Aprenda padrões e técnicas para usar dispositivos de entrada de maneira efetiva.
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: windows 10, uwp, jogos, entrada
ms.localizationpriority: medium
ms.openlocfilehash: 73e0ba3e563b57c2e392809097567b7e6739c90d
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2018
ms.locfileid: "8351464"
---
# <a name="input-practices-for-games"></a>Práticas de entrada para jogos

Esta página descreve padrões e técnicas para usar dispositivos de entrada de maneira efetiva em jogos da Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:

* acompanhar jogadores e quais dispositivos de entrada e navegação eles estão usando atualmente
* detectar transições de botão (de pressionado para liberado, de liberado para pressionado)
* detectar disposições de botão complexas com um único teste

## <a name="choosing-an-input-device-class"></a>Como escolher uma classe de dispositivo de entrada

Existem muitos tipos diferentes de APIs de entrada disponíveis para você, como [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) e [Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). Como decidir qual API usar para seu jogo?

Você deve escolher qualquer API que oferecer a você a entrada mais apropriada para seu jogo. Por exemplo, se você estiver fazendo um jogo da plataforma 2D, provavelmente poderá usar apenas a classe **Gamepad** e não se preocupará com a funcionalidade extra disponível por meio de outras classes. Isso seria restringir o jogo para dar suporte apenas a gamepads e fornecer uma interface consistente que funcionará em muitos gamepads diferentes sem a necessidade de código adicional.

Por outro lado, para simulações complexas de corrida e de voo, talvez você queira enumerar todos os objetos [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) como uma linha de base para garantir que eles deem suporte a qualquer dispositivo de nicho que os jogadores entusiastas possa ter, incluindo os dispositivos como pedais ou aceleradores separados que ainda sejam usados por um único jogador. 

A partir daí, você poderá usar um método **FromGameController** da classe de entrada, como [Gamepad.FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller), para ver se cada dispositivo tem um modo de exibição mais organizado. Por exemplo, se o dispositivo também for um **Gamepad**, então talvez você queira ajustar a interface do usuário de mapeamento de botões para refletir isso e fornecer algumas opções sensatas de mapeamentos de botão padrão. (Isso é o oposto de exigir que o jogador configure manualmente as entradas de gamepad, se você estiver usando apenas **RawGameController**). 

Como alternativa, você pode examinar a ID do fornecedor (VID) e a ID do produto (PID) de um **RawGameController** (usando [HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) e [HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId), respectivamente) e fornecer mapeamentos de botões sugeridos para dispositivos populares enquanto eles ainda forem compatíveis com dispositivos desconhecidos que aparecerem no futuro por meio de mapeamentos manuais feitos pelo jogador.

## <a name="keeping-track-of-connected-controllers"></a>Mantendo o controle de controladores conectados

Embora cada tipo de controlador inclua uma lista de controladores conectados (como [Gamepad.Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)), é uma boa ideia manter sua própria lista de controladores. Consulte [A lista de gamepads](gamepad-and-vibration.md#the-gamepads-list) para obter mais informações (cada tipo de controlador tem uma seção de nome similar em seu próprio tópico).

No entanto, o que acontece quando o jogador desconecta seu controlador ou conecta-se em um novo? Você precisa manipular esses eventos e atualizar sua lista adequadamente. Consulte [Adicionando e removendo gamepads](gamepad-and-vibration.md#adding-and-removing-gamepads) para obter mais informações (novamente, cada tipo de controlador tem uma seção de nome similar em seu próprio tópico).

Como os eventos adicionados e removidos são gerados de forma assíncrona, você pode obter resultados incorretos ao lidar com a sua lista de controladores. Portanto, sempre que acessar sua lista de controladores, você deve bloqueá-la para que apenas um thread possa acessá-la de cada vez. Isso pode ser feito com o [Tempo de Execução de Simultaneidade](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime), especialmente a [classe critical_section](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class), em **&lt;ppl.h&gt;**.

Outra coisa que deve ser considerada é que a lista de controladores conectados estará inicialmente vazia e levará um segundo ou dois para ser populada. Portanto, se você apenas atribuir o gamepad atual no método inicial, ele será **null**!

Para retificar isso, você deve ter um método que "atualiza" o gamepad principal (em um jogo de um jogador; os jogos para vários jogadores precisarão de soluções mais sofisticadas). Você deve chamar esse método nos manipuladores de eventos do controlador adicionado e do controlador removido ou no método atualizado.

O seguinte método simplesmente retorna o primeiro gamepad na lista (ou **nullptr** se a lista estiver vazia). Então você só precisa se lembrar de verificar **nullptr** sempre que você fizer algo com o controlador. Cabe a você decidir se bloqueia o jogo quando não há um controlador conectado (por exemplo, pausando o jogo) ou simplesmente dá continuidade ao jogo, ignorando a entrada.

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

Juntando tudo, veja um exemplo de como manipular a entrada de um gamepad:

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>Como acompanhar usuários e dispositivos

Todos os dispositivos de entrada estão associados a um [Usuário](https://docs.microsoft.com/uwp/api/windows.system.user), de maneira que a identidade possa ser vinculada ao jogo, às conquistas, às alterações de configurações e a outras atividades. Os usuários podem entrar ou sair quando quiserem, e é comum um usuário diferente se conectar a um dispositivo de entrada que continue conectado ao sistema depois que o usuário anterior saiu. Quando um usuário entra ou sai, o evento [IGameController.UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged) é gerado. É possível registrar um manipulador de eventos para esse evento a fim de acompanhar jogadores e os dispositivos que eles estão usando.

Identidade do usuário também é a maneira como um dispositivo de entrada está associado ao [controlador de navegação de interface do usuário](ui-navigation-controller.md) correspondente.

Por esses motivos, a entrada do jogador deve ser acompanhada e correlacionada com a propriedade [Usuário](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) da classe de dispositivo (herdada da interface [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)).

O exemplo [UserGamepadPairingUWP (GitHub)](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) demonstra como é possível acompanhar usuários e os dispositivos que eles estão usando.

## <a name="detecting-button-transitions"></a>Como detectar transições de botão

Às vezes, você deseja saber quando um botão foi pressionado ou liberado pela primeira vez; ou seja, exatamente quando o estado do botão passa de liberado para pressionado ou de pressionado para liberado. Para determinar isso, você precisa lembrar a leitura do dispositivo anterior e comparar a leitura atual com ela para saber o que mudou.

O exemplo a seguir demonstra uma abordagem básica para lembrar a leitura anterior; os gamepads são mostrados aqui, mas os princípios são os mesmos para joystick de arcade, volante de corrida e os outros tipos de dispositivo de entrada.

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

Antes de fazer algo, `Game::Loop` move o valor existente de `newReading` (a leitura do gamepad da iteração de loop anterior) para `oldReading` e, em seguida, preenche `newReading` com uma leitura de gamepad atualizada para a iteração atual. Isso fornece as informações de que você precisa para detectar transições de botão.

O exemplo a seguir demonstra uma abordagem básica para detectar transições de botão:

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Essas duas funções derivam primeiramente o estado Booliano da seleção do botão de `newReading` e `oldReading` e, em seguida, realizam uma lógica Booliana para determinar se a transição de destino ocorreu. Essas funções só retornarão **true** se a leitura nova contiver o estado de destino (pressionado ou liberado, respectivamente) *e* a leitura anterior também não contiver o estado de destino; do contrário, elas retornarão **false**.

## <a name="detecting-complex-button-arrangements"></a>Como detectar disposições de botão complexas

Cada botão de um dispositivo de entrada fornece uma leitura digital que indica se ele está pressionado (para baixo) ou liberado (para cima). Tendo em vista a eficiência, as leituras de botão não são representadas como valores boolianos individuais; em vez disso, elas são todas empacotadas em campos de bits representados por enumerações específicas, como [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons). Para ler botões específicos, o mascaramento bit a bit é usado para isolar os valores nos quais você tem interesse. Um botão está pressionado (para baixo) quando seu bit correspondente está definido; caso contrário, ele está liberado (para acima).

Lembre-se de como se determina se botões únicos estão pressionados ou liberados; os gamepads são mostrados aqui, mas os princípios são os mesmos para joystick de arcade, volante de corrida e os outros tipos de dispositivo de entrada.

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

Como é possível ver, determinar o estado de um botão único é simples, mas, às vezes, convém determinar se vários botões estão pressionados ou liberados, ou se um conjunto de botões está organizado de uma determinada maneira &mdash; alguns pressionados, outros não. Testar vários botões é mais complexo do que testar botões únicos &mdash; especialmente com o potencial do estado de botão misto &mdash; mas existe uma fórmula simples para esses testes que se aplica a testes de botões únicos e múltiplos.

O exemplo a seguir determina se os botões de gamepad A e B estão ambos pressionados:

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

O exemplo a seguir determina se os botões de gamepad A e B estão ambos liberados:

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

O exemplo a seguir determina se o botão de gamepad A está pressionado e o botão B está liberado:

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

A fórmula que todos esses cinco exemplos têm em comum é que a disposição dos botões a ser testada é especificada pela expressão no lado esquerdo do operador de igualdade e os botões a serem considerados são selecionados pela expressão de mascaramento no lado direito.

O exemplo a seguir demonstra essa fórmula mais claramente reescrevendo o exemplo anterior:

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Esta fórmula pode ser aplicada para testar qualquer número de botões em qualquer disposição dos estados.

## <a name="get-the-state-of-the-battery"></a>Obter o estado da bateria

Em qualquer controlador de jogo que implementa a interface [IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo), você pode chamar o [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) na instância do controlador para obter um objeto [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) que fornece informações sobre a bateria no controlador. Você pode obter propriedades, como a taxa a que a bateria está carregando ([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), a capacidade de energia estimada de uma nova bateria ([DesignCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) e a capacidade de energia totalmente carregada da bateria atual ([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)).

No caso dos controladores de jogo que dão suporte a relatórios detalhados sobre a bateria, você pode obter esses relatórios e mais informações sobre a bateria, conforme detalhado em [Obter informações sobre a bateria](../devices-sensors/get-battery-info.md). No entanto, a maior parte dos controladores de jogo não dão suporte a esse nível de relatório de bateria e, em vez disso, usam hardware de baixo custo. No caso desses controladores, você precisará considerar o seguinte:

* **ChargeRateInMilliwatts** e **DesignCapacityInMilliwattHours** sempre serão **NULL**.

* Você pode obter a porcentagem de bateria calculando [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours) / **FullChargeCapacityInMilliwattHours**. Você deve ignorar os valores dessas propriedades e lidar apenas com a porcentagem calculada.

* A porcentagem do ponto de marcador anterior sempre será:

    * 100% (completo)
    * 70% (médio)
    * 40% (baixo)
    * 10% (crítico)

Se seu código realizar alguma ação (como desenhar a interface do usuário) com base na porcentagem de duração de bateria restante, certifique-se de que ele está de acordo com os valores acima. Por exemplo, se quiser avisar o player quando a bateria do controlador estiver baixa, faça-o quando ela chegar a 10%.

## <a name="see-also"></a>Veja também

* [Classe Windows.System.User](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Interface Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Enumeração Windows.Gaming.Input.GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
