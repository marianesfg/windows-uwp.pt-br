---
author: eliotcowley
title: Controlador de navegação da interface do usuário
description: Use as APIs do controlador de navegação da interface do usuário Windows.Gaming.Input para detectar e ler diferentes tipos de dispositivos de entrada para a navegação da interface do usuário.
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, interface do usuário, navegação
ms.localizationpriority: medium
ms.openlocfilehash: a0ec2790f6dddf93959a8c826602c0ac622a7d23
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7292166"
---
# <a name="ui-navigation-controller"></a>Controlador de navegação da interface do usuário

Esta página descreve os conceitos básicos de programação para dispositivos de navegação da interface do usuário usando [Windows.Gaming.Input.UINavigationController][uinavigationcontroller] e APIs relacionadas para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:
* Obter uma lista de dispositivos de navegação da interface conectados e seus usuários
* Detectar se um dispositivo de navegação foi adicionado ou removido
* Ler entradas de um ou mais dispositivos de navegação da interface do usuário
* Os gamepads e joysticks de arcade se comportam como dispositivos de navegação

## <a name="ui-navigation-controller-overview"></a>Visão geral do controlador de navegação da interface do usuário

Quase todos os jogos têm pelo menos uma interface do usuário que é separada do jogo, mesmo que sejam apenas menus pré-jogo ou caixas de diálogo no jogo. Os jogadores precisam ser capaz de navegar nessa interface do usuário usando o dispositivo de entrada que escolherem, mas isso força os desenvolvedores a adicionar suporte específico para cada tipo de dispositivo de entrada e também pode introduzir inconsistências entre os jogos e os dispositivos de entrada que podem confundir os jogadores. Por esses motivos, a API [UINavigationController][] foi criada.

Controladores de navegação da interface do usuário são os dispositivos de entrada _lógicos_ que existem para fornecer um vocabulário de comandos comuns de navegação da interface do usuário que podem ser aceitos por uma variedade de dispositivos de entrada _físicos_. Um _controlador de navegação da interface do usuário_ é apenas uma maneira diferente de olhar para um dispositivo de entrada físico; usamos _dispositivo de navegação_ para fazer referência a qualquer dispositivo de entrada físico encarado como um controlador de navegação. Ao fazer a programação de acordo com um dispositivo de navegação em vez de dispositivos de entrada específicos, os desenvolvedores evitam a sobrecarga do suporte para diferentes dispositivos de entrada e obtêm uma consistência por padrão.

Como o número e a variedade de controles aceitos por cada tipo de dispositivo de entrada podem ser bem diferentes e certos dispositivos de entrada podem permitir um conjunto maior de comandos de navegação, a interface do controlador de navegação divide o vocabulário de comandos em um _conjunto obrigatório_, que contém os comandos mais comuns e essenciais, e um _conjunto opcional_, que contém comandos convenientes, não são essenciais. Todos os dispositivos de navegação aceitam todos os comandos do _conjunto obrigatório_ e podem dar suporte a todos, alguns ou nenhum dos comandos no _conjunto opcional_.

### <a name="required-set"></a>Conjunto obrigatório

Os dispositivos de navegação devem permitir todos os comandos de navegação do _conjunto obrigatório_; esses são os comandos direcionais (para cima, para baixo, esquerda e direita), exibir, menu, aceitar e cancelar.

Os comandos direcionais destinam-se à [navegação principal de foco do plano XY](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction) entre elementos de interface do usuário únicos. Os comandos exibir e menu servem para exibir informações do jogo (geralmente momentâneas, às vezes, restritas) e para alternar entre os contextos de jogo e menu, respectivamente. Os comandos aceitar e cancelar servem como respostas afirmativas (sim) e negativas (não), respectivamente.

A tabela a seguir resume esses comandos e o uso pretendido, com exemplos.
| Comando | Uso pretendido
| -------:| ---------------
|      Para cima | Navegação de foco do plano XY para cima
|    Para baixo | Navegação de foco do plano XY para baixo
|    Esquerda | Navegação de foco do plano XY para a esquerda
|   Direita | Navegação de foco do plano XY para a direita
|    Exibir | Exibir informações do jogo _(placar, estatísticas do jogo, objetivos, mapa do mundo ou da região)_
|    Menu | Menu principal / Pausar _(configurações, status, equipamento, inventário, pausa)_
|  Aceitar | Resposta afirmativa _(aceitar, avançar, confirmar, iniciar, sim)_
|  Cancelar | Resposta negativa _(rejeitar, inverter, recusar, parar, não)_


### <a name="optional-set"></a>Conjunto opcional

Os dispositivos de navegação também podem aceitar todos, alguns ou nenhum dos comandos de navegação do _conjunto opcional_; esses são comandos de paginação (para cima, para baixo, esquerda e direita), de rolagem (para cima, para baixo, esquerda e direita) e contextuais (contexto 1 a 4).

Os comandos contextuais são explicitamente projetados para comandos e atalhos de navegação específicos do aplicativo. Os comandos de paginação e de rolagem servem para navegação rápida entre páginas ou grupos de elementos de interface do usuário e navegação refinada nos elementos de interface do usuário, respectivamente.

A tabela a seguir resume esses comandos e o uso pretendido.
|     Comando | Uso pretendido
| -----------:| ------------
|      PageUp | Ir para cima (para a página ou o grupo vertical superior/anterior)
|    PageDown | Ir para baixo (para a página ou o grupo vertical inferior/seguinte)
|    PageLeft | Ir para a esquerda (para a página ou o grupo horizontal à esquerda/anterior)
|   PageRight | Ir para a direita (para a página ou o grupo horizontal à direita/seguinte)
|    ScrollUp | Rolar para cima (no elemento de interface do usuário ou no grupo rolável em foco)
|  ScrollDown | Rolar para baixo (no elemento de interface do usuário ou no grupo rolável em foco)
|  ScrollLeft | Rolar para a esquerda (no elemento de interface do usuário ou no grupo rolável em foco)
| ScrollRight | Rolar para a direita (no elemento de interface do usuário ou no grupo rolável em foco)
|    Context1 | Ação de contexto principal
|    Context2 | Ação de contexto secundária
|    Context3 | Terceira ação de contexto
|    Context4 | Quarta ação de contexto

> **Observação** Embora um jogo seja livre para responder a qualquer comando com uma função real diferente do uso pretendido, o comportamento surpresa deve ser evitado. Particularmente, não altere a função real de um comando se precisar do uso pretendido dele, tente atribuir funções novas ao comando que faça mais sentido e atribua funções equivalentes aos comandos equivalentes, como PageUp/PageDown. Por fim, considere quais comandos são compatíveis com cada tipo de dispositivo de entrada e para quais controles eles são mapeados, certificando-se de que os comandos essenciais sejam acessíveis em todos os dispositivos.

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>Navegação com gamepad, joystick de arcade e roda de corrida

Todos os dispositivos de entrada com suporte do namespace Windows.Gaming.Input são dispositivos de navegação da interface do usuário.

A tabela a seguir resume como o _conjunto obrigatório_ de comandos de navegação é mapeado para vários dispositivos de entrada.

| Comando de navegação | Entrada do gamepad                       | Entrada do joystick de arcade | Entrada do volante de corrida |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 Para cima | Botão esquerdo para cima/Direcional para cima       | Joystick para cima           | Direcional para cima           |
|               Para baixo | Botão esquerdo para baixo/Direcional para baixo   | Joystick para baixo         | Direcional para baixo         |
|               Esquerda | Botão esquerdo para a esquerda/Direcional para a esquerda   | Joystick para a esquerda         | Direcional para a esquerda         |
|              Direita | Botão esquerdo para a direita/Direcional para a direita | Joystick para a direita        | Direcional para a direita        |
|               Exibir | Botão Exibir                         | Botão Exibir        | Botão Exibir        |
|               Menu | Botão Menu                         | Botão Menu        | Botão Menu        |
|             Aceitar | Botão A                            | Botão de ação 1    | Botão A           |
|             Cancelar | Botão B                            | Botão de ação 2    | Botão B           |

A tabela a seguir resume como o _conjunto opcional_ de comandos de navegação é mapeado para vários dispositivos de entrada.

| Comando de navegação | Entrada do gamepad          | Entrada do joystick de arcade | Entrada do volante de corrida    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             PageUp | Gatilho esquerdo           | _sem suporte_    | _varia_              |
|           PageDown | Gatilho direito          | _sem suporte_    | _varia_              |
|           PageLeft | Botão superior esquerdo            | _sem suporte_    | _varia_              |
|          PageRight | Botão superior direito           | _sem suporte_    | _varia_              |
|           ScrollUp | Botão direito para cima    | _sem suporte_    | _varia_              |
|         ScrollDown | Botão direito para baixo  | _sem suporte_    | _varia_              |
|         ScrollLeft | Botão direito para a esquerda  | _sem suporte_    | _varia_              |
|        ScrollRight | Botão direito para a direita | _sem suporte_    | _varia_              |
|           Context1 | Botão X               | _sem suporte_    | Botão X (_comumente_) |
|           Context2 | Botão Y               | _sem suporte_    | Botão Y (_comumente_) |
|           Context3 | Pressionar o botão esquerdo  | _sem suporte_    | _varia_              |
|           Context4 | Pressionar o botão direito | _sem suporte_    | _varia_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>Detectar e rastrear controladores de navegação da interface do usuário

Embora os controladores de navegação da interface do usuário sejam dispositivos de entrada lógicos, eles são uma representação de um dispositivo físico e gerenciados pelo sistema da mesma forma. Você não precisa criá-los ou inicializá-los; o sistema fornece uma lista de controladores de navegação da interface do usuário e eventos conectados para avisar você quando um controlador de navegação da interface do usuário é adicionado ou removido.

### <a name="the-ui-navigation-controllers-list"></a>A lista de controladores de navegação da interface do usuário

A classe [UINavigationController][] fornece uma propriedade estática, [UINavigationControllers][], que é uma lista somente leitura de dispositivos de navegação da interface do usuário conectados no momento. Como você pode estar interessado apenas em alguns dos dispositivos de navegação conectados, é recomendável manter sua própria coleção em vez de acessá-los por meio da propriedade `UINavigationControllers`.

O exemplo a seguir copia todos os controladores de navegação da interface do usuário conectados para uma nova coleção.
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>Adicionando e removendo controladores de navegação da interface do usuário

Quando um controlador de navegação da interface do usuário é adicionado ou removido, os eventos [UINavigationControllerAdded][] e [UINavigationControllerRemoved][] são gerados. Você pode registrar um manipulador de eventos para esses eventos para rastrear os dispositivos de navegação que estão conectados no momento.

O exemplo a seguir inicia o rastreamento de um dispositivo de navegação da interface do Usuário que foi adicionado.
```cpp
UINavigationController::UINavigationControllerAdded += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    // This code assumes that you're interested in all new navigation controllers.
    myNavigationControllers->Append(args);
}
```

O exemplo a seguir interrompe o rastreamento de um joystick de arcade que foi removido.
```cpp
UINavigationController::UINavigationControllerRemoved += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    unsigned int indexRemoved;

    if(myNavigationControllers->IndexOf(args, &indexRemoved))
    {
        myNavigationControllers->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>Usuários e headsets

Cada dispositivo de navegação pode ser associado a uma conta de usuário para vincular a identidade do usuário à entrada dele. Também pode ter um headset anexado para facilitar os recursos de navegação por voz ou chat. Para saber mais sobre como trabalhar com usuários e headsets, consulte [Rastreando os usuários e seus dispositivos](input-practices-for-games.md#tracking-users-and-their-devices) e [Headset](headset.md).

## <a name="reading-the-ui-navigation-controller"></a>Lendo o controlador de navegação da interface do usuário

Depois de identificar o dispositivo de navegação da interface do usuário de seu interesse, você está pronto para coletar as informações dele. No entanto, ao contrário de outros tipos de entrada com os quais você possa estar acostumado, os dispositivos de navegação não comunicam a alteração de estado acionando eventos. Você faz leituras regulares do estado atual deles fazendo uma _sondagem_.

### <a name="polling-the-ui-navigation-controller"></a>Fazendo a sondagem do controlador de navegação da interface do usuário

A sondagem captura um instantâneo do dispositivo de navegação em um momento preciso. Essa abordagem de coleta de entrada é ótima para a maioria dos jogos, pois sua lógica normalmente é executada em um loop determinante em vez de ser orientada por evento; também é normalmente mais simples interpretar os comandos de jogos da entrada coletada de uma vez do que de várias entradas individuais coletadas ao longo do tempo.

Você pode sondar um dispositivo de navegação chamando [UINavigationController.GetCurrentReading][getcurrentreading]; essa função retorna um [UINavigationReading][] que contém o estado do dispositivo de navegação.

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>Lendo os botões

Cada um dos botões de navegação da interface do usuário fornecem uma leitura booliana que corresponde a se ele está pressionado (para baixo) ou liberado (para acima). Para garantir a eficiência, as leituras dos botões não são representadas como valores booleanos individuais; elas são agrupadas em um dos dois campos de bits representados pelas enumerações [RequiredUINavigationButtons][] e [OptionalUINavigationButtons][].

Os botões pertencentes ao _conjunto obrigatório_ são lidas pela propriedade `RequiredButtons` da estruturar [UINavigationReading][]; os botões pertencentes ao _conjunto opcional_ são lidos pela propriedade `OptionalButtons`. Como essas propriedades são campos de bits, o mascaramento bit a bit é usado para isolar o valor do botão de seu interesse. O botão está pressionado (para baixo) quando o bit correspondente está definido; caso contrário, ele está liberado (para acima).

O exemplo a seguir determina se o botão Aceitar do _conjunto obrigatório_ está pressionado.
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

O exemplo a seguir determina se o botão Aceitar do _conjunto obrigatório_ está liberado.
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

Certifique-se de usar a propriedade `OptionalButtons` e a enumeração `OptionalUINavigationButtons` ao ler os botões do _conjunto opcional_.

O exemplo a seguir determina se o botão Contexto 1 do _conjunto opcional_ está pressionado.
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

Às vezes, convém determinar quando um botão passa de pressionado para liberado ou vice-versa, se vários botões foram pressionados ou liberados ou se um conjunto de botões está organizado de determinada maneira – alguns pressionados, outros, não. Para obter informações sobre como detectar essas condições, consulte [Detectando transições do botão](input-practices-for-games.md#detecting-button-transitions) e [Detectando organizações complexas de botão](input-practices-for-games.md#detecting-complex-button-arrangements).


## <a name="run-the-ui-navigation-controller-sample"></a>Executar o exemplo de controlador de navegação da interface do usuário

O [exemplo InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/InputInterfacingUWP) demonstra como os diferentes dispositivos de entrada se comportam como controladores de navegação da interface do usuário.

## <a name="see-also"></a>Consulte também
[Windows.Gaming.Input.Gamepad][]
[Windows.Gaming.Input.ArcadeStick][]
[Windows.Gaming.Input.RacingWheel][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Windows.Gaming.Input.Arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[Windows.Gaming.Input.Racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[uinavigationcontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[uinavigationcontrollers]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollers.aspx
[uinavigationcontrolleradded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrolleradded.aspx
[uinavigationcontrollerremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollerremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.getcurrentreading.aspx
[uinavigationreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationreading.aspx
[requireduinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.requireduinavigationbuttons.aspx
[optionaluinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.optionaluinavigationbuttons.aspx
