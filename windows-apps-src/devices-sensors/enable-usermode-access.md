---
author: JordanRh1
title: Habilitar o acesso de modo do usuário para GPIO, I2C, SPI
description: Este tutorial descreve como habilitar o acesso de modo do usuário para GPIO, I2C, SPI e UART no Windows 10.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, acpi, gpio, i2c, spi, uefi
ms.assetid: 2fbdfc78-3a43-4828-ae55-fd3789da7b34
ms.localizationpriority: medium
ms.openlocfilehash: 09957c19414f586a49a1a2cb9186aa027dc1de07
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5756305"
---
# <a name="enable-usermode-access-to-gpio-i2c-and-spi"></a>Habilitar o acesso de modo do usuário para GPIO, I2C, SPI



O Windows 10 contém novas APIs para acessar GPIO, I2C, SPI e UART diretamente no modo do usuário. Placas de desenvolvimento como Raspberry Pi 2 expõem uma sub-rede dessas conexões, o que permite que os usuários estendam um módulo de computação base com circuitos personalizados para endereçar um aplicativo específico. Esses barramentos de nível inferior geralmente são compartilhados com outras funções onboard críticas, com apenas um subconjunto de pinos e barramentos GPIO expostos em cabeçalhos. Para preservar a estabilidade do sistema, é necessário especificar quais pinos e barramentos são seguros para modificação por aplicativos de modo do usuário. 

Este documento descreve como especificar essa configuração na ACPI e fornece ferramentas para validar se a configuração foi especificada corretamente. 

> [!IMPORTANT]
> O público-alvo deste documento são os desenvolvedores de UEFI e ACPI. A familiaridade com a criação de ACPI, ASL e SpbCx/GpioClx é presumida.

O acesso do modo do usuário aos barramentos de nível inferior no Windows é inserido por meio das estruturas `GpioClx` e `SpbCx` existentes. Um novo driver chamado *RhProxy*, disponível no Windows IoT Core e no Windows Enterprise, expõe os recursos `GpioClx` e `SpbCx` ao modo do usuário. Para habilitar as APIs, um nó de dispositivo para rhproxy deve ser declarado em suas tabelas ACPI com cada um dos recursos GPIO e SPB que devem ser expostos ao modo do usuário. Este documento discorre sobre a criação e a verificação de ASL. 


## <a name="asl-by-example"></a>ASL por exemplo

Vamos examinar a declaração do nó de dispositivo rhproxy Raspberry Pi 2. Primeiro, crie a declaração de dispositivo ACPI no escopo \\_SB.  

```cpp
Device(RHPX) 
{ 
    Name(_HID, "MSFT8000") 
    Name(_CID, "MSFT8000") 
    Name(_UID, 1) 
    
```

* _HID – ID de Hardware. Defina isso como uma ID de hardware específica do fornecedor. 
* _CID – ID compatível. Deve ser "MSFT8000".  
* _UID – ID exclusiva. Defina como 1.  

Em seguida, declaramos cada um dos recursos GPIO e SPB que devem ser expostos ao modo do usuário. A ordem em que os recursos são declarados é importante porque os índices de recurso são usados para associar propriedades a recursos. Se houver vários barramentos I2C ou SPI expostos, o primeiro barramento declarado será considerado o barramento 'padrão' para esse tipo, e será a instância retornada pelos métodos `GetDefaultAsync()` [Windows.Devices.I2c.I2cController](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2ccontroller.aspx) e [Windows.Devices.Spi.SpiController](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spicontroller.aspx). 

### <a name="spi"></a>SPI 

Raspberry Pi têm dois barramentos SPI expostos. SPI0 tem duas linhas de seleção de chip de hardware e SPI1 tem uma linha de seleção de chip de hardware. Uma declaração de recurso SPISerialBus() é necessária para cada linha de seleção de chip de cada barramento. As duas declarações de recursos SPISerialBus a seguir são para as duas linhas de seleção de chip em SPI0. O campo DeviceSelection contém um valor exclusivo que o driver interpreta como um identificador de linha de seleção de chip de hardware. O valor exato que você coloca no campo DeviceSelection depende de como seu driver interpreta esse campo do descritor de conexão de ACPI.  

```cpp
// Index 0 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE0  - GPIO 8  - Pin 24 
    0,                     // Device selection (CE0) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

// Index 1 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE1  - GPIO 7  - Pin 26 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

Como o software sabe que esses dois recursos devem ser associados ao mesmo barramento? O mapeamento entre o nome amigável do barramento e o índice de recurso é especificado no DSD:  

```cpp
Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }}, 
```

Isso cria um barramento chamado "SPI0" com duas linhas de seleção de chip – índices de recurso 0 e 1. São necessárias outras várias propriedades para a declaração das funcionalidades do barramento SPI.  

```cpp
Package(2) { "SPI0-MinClockInHz", 7629 }, 
Package(2) { "SPI0-MaxClockInHz", 125000000 },
```

As propriedades **MinClockInHz** e **MaxClockInHz** especificam as velocidades de clock máximas e mínimas aceitas pelo controlador. A API impedirá que os usuários especifiquem valores fora desse intervalo. A velocidade de clock é passada para o driver SPB no campo _SPE do descritor de conexão (ACPI seção 6.4.3.8.2.2).  

```cpp
Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }}, 
```

A propriedade **SupportedDataBitLengths** lista os tamanhos de bit de dados aceitos pelo controlador. Vários valores podem ser especificados em uma lista separada por vírgulas. A API impedirá que os usuários especifiquem valores fora dessa lista. O tamanho de bit de dados é passado para seu driver SPB no campo _LEN do descritor de conexão (ACPI seção 6.4.3.8.2.2).  

Você pode considerar essas declarações de recursos como "modelos". Alguns dos campos são corrigidos na inicialização do sistema, enquanto outros são especificados dinamicamente no tempo de execução. Os campos a seguir do descritor de SPISerialBus são fixos: 

* DeviceSelection 
* DeviceSelectionPolarity 
* WireMode 
* SlaveMode 
* ResourceSource 

Os campos a seguir são espaços reservados para os valores especificados pelo usuário no tempo de execução: 

* DataBitLength 
* ConnectionSpeed 
* ClockPolarity 
* ClockPhase 

Como o SPI1 contém somente uma linha de seleção de chip, um único recurso `SPISerialBus()` é declarado: 

```cpp
// Index 2 

SPISerialBus(              // SCKL - GPIO 21 - Pin 40 
                           // MOSI - GPIO 20 - Pin 38 
                           // MISO - GPIO 19 - Pin 35 
                           // CE1  - GPIO 17 - Pin 11 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

A declaração de nome amigável correspondente – que é necessária – é especificada no DSD e se refere ao índice dessa declaração de recurso. 

```cpp
Package(2) { "bus-SPI-SPI1", Package() { 2 }}, 
```

Isso cria um barramento chamado "SPI1" e o associa ao índice de recurso 2.  

#### <a name="spi-driver-requirements"></a>Requisitos de driver SPI 

* Deve usar `SpbCx` ou ser compatível com SpbCx 
* Deve ter sido aprovado nos [Testes SPI MITT](https://msdn.microsoft.com/library/windows/hardware/dn919873.aspx)
* Deve aceitar a velocidade de clock de 4Mhz 
* Deve aceitar o tamanho de dados de 8 bits 
* Deve aceitar todos os modos de SPI: 0, 1, 2, 3 

### <a name="i2c"></a>I2C 

Em seguida, declaramos os recursos de I2C. Raspberry Pi expõe um único barramento I2C nos pinos 3 e 5. 

```cpp
// Index 3 
I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1) 
    0xFFFF,                // SlaveAddress: placeholder 
    ,                      // SlaveMode: default to ControllerInitiated 
    0,                     // ConnectionSpeed: placeholder 
    ,                      // Addressing Mode: placeholder 
    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name 
    , 
    , 
    )                      // VendorData 

```

A declaração de nome amigável correspondente – que é necessária – é especificada no DSD: 

```cpp
Package(2) { "bus-I2C-I2C1", Package() { 3 }}, 
```

Isso declara um barramento I2C com o nome amigável "I2C1" que se refere ao índice de recurso 3, que é o índice do recurso I2CSerialBus() que declaramos acima. 

Os campos a seguir do descritor de I2CSerialBus() são fixos: 

* SlaveMode 
* ResourceSource 

Os campos a seguir são espaços reservados para os valores especificados pelo usuário no tempo de execução. 

* SlaveAddress 
* ConnectionSpeed 
* AddressingMode 

#### <a name="i2c-driver-requirements"></a>Requisitos de driver I2C 

* Deve usar SpbCx ou ser compatível com SpbCx 
* Deve ter sido aprovado nos [Testes I2C MITT](https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx) 
* Deve aceitar endereçamento de 7 bits 
* Deve aceitar velocidade de clock de 100kHz 
* Deve aceitar velocidade de clock de 400kHz 

### <a name="gpio"></a>GPIO 

Em seguida, declaramos todos os pinos GPIO que são expostos no modo do usuário. Oferecemos a orientação a seguir ao decidir quais pinos expor: 

* Declare todos os pinos em cabeçalhos expostos. 
* Declare pinos que estão conectados a funções onboard úteis como botões e LEDs. 
* Não declare pinos que são reservados para funções de sistema ou não estão conectados a nada. 

O bloco de ASL seguir declara dois pinos – GPIO4 e GPIO5. Os outros pinos não são mostrados aqui por questão de brevidade. O Apêndice C contém um exemplo de script de powershell que pode ser usado para gerar os recursos GPIO. 

```cpp
// Index 4 – GPIO 4 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 4 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 4 } 

// Index 6 – GPIO 5 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 5 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 5 } 
```

Os requisitos a seguir devem ser observados ao se declarar pinos GPIO: 

* Só há suporte para controladores GPIO mapeados na memória. Não há suporte para controladores GPIO com interface sobre I2C/SPI. O driver do controlador é um controlador de memória mapeado se ele definir o sinalizador [MemoryMappedController](https://msdn.microsoft.com/library/windows/hardware/hh439449.aspx) na estrutura [CLIENT_CONTROLLER_BASIC_INFORMATION](https://msdn.microsoft.com/library/windows/hardware/hh439358.aspx) em resposta ao retorno de chamada [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx). 
* Cada pino requer um recurso GpioIO e GpioInt. O recurso GpioInt deve seguir imediatamente o recurso GpioIO e deve se referir ao mesmo número de pino. 
* Os recursos GPIO devem ser ordenados, aumentando o número de pino. 
* Cada recurso GpioIO e GpioInt deve conter exatamente um número de pino na lista de pinos. 
* O campo ShareType de ambos os descritores deve ser Shared 
* O campo EdgeLevel do descritor de GpioInt deve ser Edge 
* O campo ActiveLevel do descritor de GpioInt deve ser ActiveBoth 
* O campo PinConfig 
  * Deve ser o mesmo nos descritores de GpioIO e GpioInt 
  * Deve ser PullUp, PullDown ou PullNone. Não pode ser PullDefault.
  * A configuração de recepção deve coincidir com o estado de ativação do pino. Colocar o pino no modo de recepção especificado do estado de ativação não deve alterar o estado do pino. Por exemplo, se a folha de dados especifica que o pino sai com um puxão, especifique PinConfig como PullUp.  

O firmware, o UEFI e o código de inicialização de driver não mudarão o estado de ativação de um pino durante a inicialização. Somente o usuário sabe o que está anexado a um pino e, portanto, quais transições de estado são seguras. O estado de ativação de cada pino deve ser documentado para que os usuários possam projetar corretamente o hardware que faz interface com um pino. Um pino não deve mudar de estado inesperadamente durante a inicialização.  

#### <a name="supported-drive-modes"></a>Modos de unidade com suporte 

Se o controlador GPIO oferecer suporte integrado a resistências de conexão e de suspensão além da entrada de alta impedância e saída CMOS, você deverá especificar isso com a propriedade SupportedDriveModes opcional. 

```cpp
Package (2) { “GPIO-SupportedDriveModes”, 0xf }, 
```

A propriedade SupportedDriveModes indica quais modos de unidade são compatíveis com o controlador GPIO. No exemplo acima, todos os modos de unidade a seguir são compatíveis. A propriedade é uma máscara de bits dos valores a seguir: 

| Valor de sinalizador | Modo de unidade | Descrição |
|------------|------------|-------------|
| 0x1        | InputHighImpedance | O pino aceita entrada de impedância alta, o que corresponde ao valor "PullNone" na ACPI. |
| 0x2        | InputPullUp | O pino aceita uma resistência de conexão interna, que corresponde ao valor "PullUp" na ACPI. |
| 0x4        | InputPullDown | O pino aceita uma resistência de suspensão interna, que corresponde ao valor "PullDown" na ACPI. |
| 0x8        | OutputCmos | O pino aceita a geração de altas e baixas extremas (em vez de descarga aberta). |

InputHighImpedance e OutputCmos são compatíveis com quase todos os controladores GPIO. Se a propriedade SupportedDriveModes não for especificada, este será o padrão. 

Se o sinal de um GPIO passar por um comutador de nível antes de atingir um cabeçalho exposto, declare os modos de unidade compatíveis com o SOC, mesmo que o modo de unidade não seja observável no cabeçalho externo. Por exemplo, se um pino passa por um comutador de nível bidirecional que faz com que um pino pareça ter descarga aberta com conexão de resistência, você nunca observará um estado de alta impedância no cabeçalho exposto mesmo se o pino for configurado como uma entrada de alta impedância. Você ainda deve declarar que o pino aceita entrada de alta impedância. 

#### <a name="pin-numbering"></a>Numeração de pino 

O Windows oferece suporte a dois esquemas de numeração de pino: 

* Numeração de pino sequencial: os usuários veem números como 0, 1, 2... até o número de pinos expostos. 0 é o primeiro recurso GpioIo declarado no ASL, 1 é o segundo recurso GpioIo declarado no ASL e assim por diante. 
* Numeração de pino nativa: os usuários veem os números de pino especificados nos descritores de GpioIo, por exemplo, 4, 5, 12, 13, ... .  

```cpp
Package (2) { “GPIO-UseDescriptorPinNumbers”, 1 }, 
```

A propriedade **UseDescriptorPinNumbers** informa ao Windows para usar a numeração de pino nativa em vez da numeração de pino sequencial. Se a propriedade UseDescriptorPinNumbers não for especificada ou se o seu valor for zero, o Windows usará como padrão a numeração de pino sequencial. 

Se a numeração de pino nativa for usada, você também deverá especificar a propriedade **PinCount**. 

```cpp
Package (2) { “GPIO-PinCount”, 54 }, 
```

A propriedade **PinCount** deve corresponder ao valor retornado pela propriedade **TotalPins** no retorno de chamada [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx) do driver `GpioClx`. 

Escolha o esquema de numeração mais compatível com a documentação existente publicada de sua placa. Por exemplo, Raspberry Pi usa numeração de pino nativa porque muitos diagramas de pinos existentes usam a numeração de pino BCM2835. MinnowBoardMax usa a numeração de pino sequencial porque existem poucos diagramas de pinos existentes, e a numeração de pinos sequencial simplifica a experiência do desenvolvedor porque apenas 10 pinos são expostos dentre mais de 200 pinos. A decisão de usar a numeração de pino sequencial ou nativa deve ter como finalidade mostrar clareza para desenvolvedor. 

#### <a name="gpio-driver-requirements"></a>Requisitos de driver GPIO 

* Deve usar `GpioClx`
* Deve ser mapeado na memória SOC 
* Deve usar tratamento de interrupção de ActiveBoth emulado 

### <a name="uart"></a>UART 

Se o driver UART usa `SerCx` ou `SerCx2`, você pode usar o rhproxy para expor o driver ao modo do usuário. Os drivers UART que criam uma interface de dispositivo do tipo `GUID_DEVINTERFACE_COMPORT` não precisam usar o rhproxy. O driver `Serial.sys` da caixa de entrada é um desses casos.

Para expor um UART de estilo `SerCx` ao modo do usuário, declare um recurso `UARTSerialBus` como a seguir.

```cpp
// Index 2 
UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2 
    115200,                // InitialBaudRate: in bits ber second 
    ,                      // BitsPerByte: default to 8 bits 
    ,                      // StopBits: Defaults to one bit 
    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled 
    ,                      // IsBigEndian: default to LittleEndian 
    ,                      // Parity: Defaults to no parity 
    ,                      // FlowControl: Defaults to no flow control 
    32,                    // ReceiveBufferSize 
    32,                    // TransmitBufferSize 
    "\\_SB.URT2",          // ResourceSource: UART bus controller name 
    , 
    , 
    , 
    )
```

Somente o campo ResourceSource é fixo enquanto todos os outros campos são espaços reservados para os valores especificados no tempo de execução pelo usuário. 

A declaração de nome amigável correspondente é: 

```cpp
Package(2) { "bus-UART-UART2", Package() { 2 }}, 
```

Isso atribui o nome amigável "UART2" para o controlador, que é o identificador que os usuários usarão para acessar o barramento de modo do usuário.  

## <a name="runtime-pin-muxing"></a>Multiplexação de pino no tempo de execução 

Multiplexação de pino é a capacidade de usar o mesmo pino físico para funções diferentes. Vários periféricos no chip diferentes, como um controlador I2C, controlador SPI e controlador GPIO, podem ser encaminhados para o mesmo pino físico em um SOC. O bloco do multiplexador controla qual função está ativa no pino a qualquer momento. Tradicionalmente, o firmware é responsável por estabelecer atribuições de função na inicialização, e essa atribuição permanece estática durante a sessão de inicialização. A multiplexação de pino no tempo de execução adiciona a capacidade de reconfigurar atribuições de função de pino no tempo de execução. Permitir que os usuários escolham a função do pino no tempo de execução agiliza o desenvolvimento, pois permite que os usuários reconfigurem rapidamente os pinos de uma placa, e possibilita ao hardware dar suporte a uma gama maior de aplicativos do que uma configuração estática daria. 

Os usuários consomem o suporte à multiplexação para GPIO, I2C, SPI e UART sem ter que escrever código adicional. Quando um usuário abre um GPIO ou barramento usando [OpenPin()](https://msdn.microsoft.com/library/dn960157.aspx) ou [FromIdAsync()](https://msdn.microsoft.com/windows.devices.i2c.i2cdevice.fromidasync), os pinos físicos subjacentes são multiplexados automaticamente para a função solicitada. Se os pinos já estiverem sendo usados por uma função diferente, a chamada a OpenPin() ou FromIdAsync() falhará. Quando o usuário fecha o dispositivo descartando o objeto [GpioPin](https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.gpiopin.aspx), [I2cDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2cdevice.aspx), [SpiDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spidevice.aspx) ou [SerialDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.serialdevice.aspx), os pinos são liberados, o que permite que eles sejam abertos mais tarde para uma função diferente. 

O Windows contém suporte integrado à multiplexação de pino nas estruturas [GpioClx](https://msdn.microsoft.com/library/windows/hardware/hh439515.aspx), [SpbCx](https://msdn.microsoft.com/library/windows/hardware/hh406203.aspx) e [SerCx](https://msdn.microsoft.com/library/windows/hardware/dn265349.aspx). Essas estruturas funcionam juntas para alternar automaticamente um pino para a função correta quando um pino ou barramento GPIO é acessado. O acesso aos pinos é arbitrado para evitar conflitos entre vários clientes. Além desse suporte integrado, as interfaces e os protocolos de multiplexação de pino são de finalidade geral e podem ser estendidos para dar suporte a cenários e dispositivos adicionais. 

Este documento descreve primeiro as interfaces e os protocolos subjacentes envolvidos no muxing de pino e, em seguida, descreve como adicionar suporte a muxing de pino aos drivers de controlador GpioClx, SpbCx e SerCx. 

### <a name="pin-muxing-architecture"></a>Arquitetura de multiplexação de pino 

Esta seção descreve as interfaces e os protocolos subjacentes envolvidos na multiplexação de pino. Não é preciso necessariamente conhecer os protocolos subjacentes para dar suporte à multiplexação de pino com os drivers SpbCx/GpioClx/SerCx. Para obter detalhes sobre como dar suporte à multiplexação de pino com os drivers SpbCx/GpioCls/SerCx, consulte [Implementando o suporte à multiplexação de pino em drivers de cliente GpioClx](#supporting-muxing-support-in-gpioclx-client-drivers) e [Consumindo o suporte à multiplexação em drivers de controlador SpbCx e SerCx](#supporting-muxing-in-spbcx-and-sercx-controller-drivers). 

A multiplexação de pino é feito com a cooperação de vários componentes. 

* Servidores de multiplexação de pino são os drivers que controlam o bloco de controle de multiplexação de pino. Os servidores de multiplexação de pino recebem as solicitações de multiplexação de pino de clientes via solicitações para reservar recursos de multiplexação (via solicitações *IRP_MJ_CREATE*), e solicitações para alternar a função de um pino (via solicitações *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*). O servidor de multiplexação de pino geralmente é o driver GPIO, já que o bloco de multiplexação às vezes faz parte do bloco GPIO. Mesmo que o bloco de multiplexação seja um periférico separado, o driver GPIO é um lugar lógico para colocar a funcionalidade de multiplexação. 
* Clientes de multiplexação de pino são drivers que consomem multiplexação de pino. Os clientes de multiplexação de pino recebem recursos de multiplexação de pino do firmware da ACPI. Os recursos de multiplexação de pino são um tipo de recurso de conexão e são gerenciados pelo hub de recursos. Os clientes de multiplexação de pino reservam os recursos de multiplexação abrindo um identificador para o recurso. Para efeito de uma alteração de hardware, os clientes devem confirmar a configuração enviando uma solicitação *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*. Os clientes liberam os recursos de multiplexação de pino fechando o identificador, momento em que a configuração de multiplexação é revertida para seu estado padrão. 
* O firmware da ACPI especifica a configuração de multiplexação com recursos `MsftFunctionConfig()`. Os recursos MsftFunctionConfig expressam quais pinos, em qual configuração de multiplexação, são exigidos por um cliente. Os recursos MsftFunctionConfig contêm o número de função, a configuração de recepção e a lista de números de pino. Os recursos MsftFunctionConfig são fornecidos para clientes de multiplexação de pino como recursos de hardware, que são recebidos por drivers no retorno de chamada PrepareHardware semelhantemente aos recursos de conexão GPIO e SPB. Os clientes recebem uma ID de hub de recurso que pode ser usada para abrir um identificador para o recurso. 

> Você deve passar a opção de linha de comando `/MsftInternal` para `asl.exe` para compilar arquivos ASL que contêm descritores `MsftFunctionConfig()`, pois esses descritores estão atualmente sob a análise de trabalho da ACPI. Por exemplo: `asl.exe /MsftInternal dsdt.asl`

A sequência de operações envolvidas na multiplexação de pino é mostrada abaixo. 

![Interação entre servidor e cliente de multiplexação de pino](images/usermode-access-diagram-1.png)

1.  O cliente recebe recursos MsftFunctionConfig do firmware da ACPI no retorno de chamada [EvtDevicePrepareHardware()](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx).
2.  O cliente usa a função auxiliar do hub de recursos `RESOURCE_HUB_CREATE_PATH_FROM_ID()` para criar um caminho a partir da ID do recurso, em seguida, abre um identificador para o caminho (usando [ZwCreateFile()](https://msdn.microsoft.com/library/windows/hardware/ff566424.aspx), [IoGetDeviceObjectPointer()](https://msdn.microsoft.com/library/windows/hardware/ff549198.aspx) ou [WdfIoTargetOpen()](https://msdn.microsoft.com/library/windows/hardware/ff548634.aspx)).
3.  O servidor extrai a ID do hub de recursos do caminho do arquivo usando as funções auxiliares do hub de recursos `RESOURCE_HUB_ID_FROM_FILE_NAME()`, em seguida, consulta o hub de recursos para obter o descritor do recurso.
4.  O servidor executa a arbitragem de compartilhamento em cada ponto no descritor e conclui a solicitação IRP_MJ_CREATE.
5.  O cliente emite uma solicitação *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* no identificador recebido.
6.  Em resposta a *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*, o servidor executa a operação de multiplexação de hardware, tornando a função especificada ativa em cada pino.
7.  O cliente prossegue com as operações que dependem da configuração de multiplexação de pino solicitada.
8.  Quando não precisa mais que os pinos sejam multiplexados, o cliente fecha o identificador.
9.  Em resposta ao fechamento do identificador, o servidor reverte os pinos de volta para seu estado inicial.

### <a name="protocol-description-for-pin-muxing-clients"></a>Descrição do protocolo para clientes de multiplexação de pino

Esta seção descreve como um cliente consome a funcionalidade de multiplexação de pino. Isso não se aplica aos drivers de controlador `SerCx` e `SpbCx`, já que as estruturas implementam esse protocolo em nome dos drivers de controlador.

####    <a name="parsing-resources"></a>Recursos de análise

Um driver WDF recebe recursos `MsftFunctionConfig()` em sua rotina [EvtDevicePrepareHardware()](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx). Os recursos MsftFunctionConfig podem ser identificados pelos campos a seguir:

```cpp
CM_PARTIAL_RESOURCE_DESCRIPTOR::Type = CmResourceTypeConnection
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Class = CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Type = CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG
```

Uma rotina `EvtDevicePrepareHardware()` pode extrair recursos MsftFunctionConfig da seguinte maneira:

```cpp
EVT_WDF_DEVICE_PREPARE_HARDWARE evtDevicePrepareHardware;

_Use_decl_annotations_
NTSTATUS
evtDevicePrepareHardware (
    WDFDEVICE WdfDevice,
    WDFCMRESLIST ResourcesTranslated
    )
{
    PAGED_CODE();

    LARGE_INTEGER connectionId;
    ULONG functionConfigCount = 0;

    const ULONG resourceCount = WdfCmResourceListGetCount(ResourcesTranslated);
    for (ULONG index = 0; index < resourceCount; ++index) {
        const CM_PARTIAL_RESOURCE_DESCRIPTOR* resDescPtr =
            WdfCmResourceListGetDescriptor(ResourcesTranslated, index);

        switch (resDescPtr->Type) {
        case CmResourceTypeConnection:
            switch (resDescPtr->u.Connection.Class) {
            case CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG:
                switch (resDescPtr->u.Connection.Type) {
                case CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG:                    
                    switch (functionConfigCount) {
                    case 0:
                        // save the connection ID
                        connectionId.LowPart = resDescPtr->u.Connection.IdLowPart;
                        connectionId.HighPart = resDescPtr->u.Connection.IdHighPart;
                        break;
                    } // switch (functionConfigCount)
                    ++functionConfigCount;
                    break; // CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG

                } // switch (resDescPtr->u.Connection.Type)
                break; // CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
            } // switch (resDescPtr->u.Connection.Class)
            break;
        } // switch
    } // for (resource list)

    if (functionConfigCount < 1) {
        return STATUS_INVALID_DEVICE_CONFIGURATION;
    }
    // TODO: save connectionId in the device context for later use

    return STATUS_SUCCESS;
}
```

####    <a name="reserving-and-committing-resources"></a>Reservando e confirmando recursos

Quando um cliente quer realizar a multiplexação de pinos, ele reserva e confirma o recurso MsftFunctionConfig. O exemplo a seguir mostra como um cliente pode reservar e confirmar recursos MsftFunctionConfig.

```cpp
_IRQL_requires_max_(PASSIVE_LEVEL)
NTSTATUS AcquireFunctionConfigResource (
    WDFDEVICE WdfDevice,
    LARGE_INTEGER ConnectionId,
    _Out_ WDFIOTARGET* ResourceHandlePtr
    )
{
    PAGED_CODE();

    //
    // Form the resource path from the connection ID
    //
    DECLARE_UNICODE_STRING_SIZE(resourcePath, RESOURCE_HUB_PATH_CHARS);
    NTSTATUS status = RESOURCE_HUB_CREATE_PATH_FROM_ID(
            &resourcePath,
            ConnectionId.LowPart,
            ConnectionId.HighPart);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    
    //
    // Create a WDFIOTARGET
    //
    WDFIOTARGET resourceHandle;
    status = WdfIoTargetCreate(WdfDevice, WDF_NO_ATTRIBUTES, &resourceHandle);
    if (!NT_SUCCESS(status)) {
        return status;
    }

    //
    // Reserve the resource by opening a WDFIOTARGET to the resource
    //
    WDF_IO_TARGET_OPEN_PARAMS openParams;
    WDF_IO_TARGET_OPEN_PARAMS_INIT_OPEN_BY_NAME(
        &openParams,
        &resourcePath,
        FILE_GENERIC_READ | FILE_GENERIC_WRITE);

    status = WdfIoTargetOpen(resourceHandle, &openParams);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    //
    // Commit the resource
    //
    status = WdfIoTargetSendIoctlSynchronously(
            resourceHandle,
            WDF_NO_HANDLE,      // WdfRequest
            IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS,
            nullptr,            // InputBuffer
            nullptr,            // OutputBuffer
            nullptr,            // RequestOptions
            nullptr);           // BytesReturned
    
    if (!NT_SUCCESS(status)) {
        WdfIoTargetClose(resourceHandle);
        return status;
    }

    //
    // Pins were successfully muxed, return the handle to the caller
    //
    *ResourceHandlePtr = resourceHandle;
    return STATUS_SUCCESS;
}
```

O driver deve armazenar o WDFIOTARGET em uma das suas áreas de contexto para que ele possa ser fechado mais tarde. Quando o driver está pronto para liberar a configuração de multiplexação, ele deve fechar o identificador de recurso, chamando [WdfObjectDelete()](https://msdn.microsoft.com/library/windows/hardware/ff548734.aspx) ou [WdfIoTargetClose()](https://msdn.microsoft.com/library/windows/hardware/ff548586.aspx) se você pretende reutilizar o WDFIOTARGET.

```cpp
    WdfObjectDelete(resourceHandle);
```

Quando o cliente fecha o identificador de recurso, os pinos voltam para seu estado inicial e podem ser adquiridos por um cliente diferente.

### <a name="protocol-description-for-pin-muxing-servers"></a>Descrição de protocolo para servidores de multiplexação de pino

Esta seção descreve como um servidor de multiplexação de pino expõe sua funcionalidade aos clientes. Isso não se aplica a drivers de miniporta `GpioClx`, já que a estrutura implementa esse protocolo em nome dos drivers de cliente. Para obter detalhes sobre como dar suporte à multiplexação de pino em drivers de cliente `GpioClx`, consulte [Implementando o suporte à multiplexação em drivers de cliente GpioClx](#supporting-muxing-support-in-gpioclx-client-drivers).

####    <a name="handling-irpmjcreate-requests"></a>Manipulando solicitações IRP_MJ_CREATE

Os clientes abrem um identificador para um recurso quando eles querem reservar um recurso de multiplexação de pino. Um servidor de multiplexação de pino recebe solicitações *IRP_MJ_CREATE* por meio de uma operação de nova análise do hub de recursos. O componente de caminho à direita da solicitação *IRP_MJ_CREATE* contém a ID do hub de recursos, que é um inteiro de 64 bits em formato hexadecimal. O servidor deve extrair a ID do hub de recursos do nome do arquivo usando `RESOURCE_HUB_ID_FROM_FILE_NAME()` de reshub.h, e enviar *IOCTL_RH_QUERY_CONNECTION_PROPERTIES* para o hub de recursos para obter o descritor `MsftFunctionConfig()`.

O servidor deve validar o descritor e extrair o modo de compartilhamento e a lista de pinos do descritor. Ele deve realizar a arbitragem de compartilhamento dos pinos e se, for bem-sucedido, marcar os pinos como reservados antes de concluir a solicitação.

A arbitragem de compartilhamento terá êxito em geral se for bem-sucedida para cada pino da lista de pinos. Cada pino deve ser arbitrado da seguinte maneira:

*   Se o pino já não estiver reservado, a arbitragem de compartilhamento terá êxito.
*   Se o pino já estiver reservado como exclusivo, a arbitragem de compartilhamento falhará.
*   Se o pino já estiver reservado como compartilhado,
  * e a solicitação de entrada for compartilhada, a arbitragem de compartilhamento terá exito.
  * e a solicitação de entrada for exclusiva, a arbitragem de compartilhamento falhará.

Se a arbitragem de compartilhamento falhar, a solicitação deverá ser concluída com *STATUS_GPIO_INCOMPATIBLE_CONNECT_MODE*. Se a arbitragem de compartilhamento tiver êxito, a solicitação deverá concluída com *STATUS_SUCCESS*.

Observe que o modo de compartilhamento da solicitação de entrada deve ser extraído do descritor MsftFunctionConfig, não de [IrpSp -> Parameters.Create.ShareAccess](https://msdn.microsoft.com/library/windows/hardware/ff548630.aspx).

####    <a name="handling-ioctlgpiocommitfunctionconfigpins-requests"></a>Manipulando solicitações IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS

Depois que o cliente tiver reservado um recurso MsftFunctionConfig com êxito abrindo um identificador, ele poderá enviar *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* para solicitar que o servidor realize a operação de multiplexação de hardware em si. Quando o servidor recebe *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*, para cada pino da lista de pino, ele deve: 

*   Definir o modo de recepção especificado no membro PinConfiguration da estrutura PNP_FUNCTION_CONFIG_DESCRIPTOR no hardware.
*   Multiplexar o pino para a função especificada pelo membro FunctionNumber da estrutura PNP_FUNCTION_CONFIG_DESCRIPTOR.

Em seguida, o servidor deve concluir a solicitação com *STATUS_SUCCESS*.

O significado de FunctionNumber é definido pelo servidor, e é entendido que o descritor MsftFunctionConfig foi criado com o conhecimento de como o servidor interpreta esse campo.

Lembre-se de que, quando o identificador for fechado, o servidor terá que reverter os pinos para a configuração em que estavam quando IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS foi recebida, portanto, talvez o servidor precise salvar o estado dos pinos antes de modificá-los.

####    <a name="handling-irpmjclose-requests"></a>Manipulando solicitações IRP_MJ_CLOSE

Quando um cliente não requer mais um recurso de multiplexação, ela fecha seu identificador. Quando um servidor recebe uma solicitação *IRP_MJ_CLOSE*, ele deve reverter os pinos para o estado em que estavam quando *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* foi recebida. Se o cliente nunca enviou uma solicitação *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*, nenhuma ação será necessária. O servidor deve marcar os pinos como disponíveis em relação à arbitragem de compartilhamento e concluir a solicitação com *STATUS_SUCCESS*. Certifique-se sincronizar corretamente a manipulação de *IRP_MJ_CLOSE* com *IRP_MJ_CREATE*.

### <a name="authoring-guidelines-for-acpi-tables"></a>Criando diretrizes para tabelas ACPI

Esta seção descreve como fornecer recursos de multiplexação para drivers de cliente. Observe que você precisará do compilador ASL da Microsoft compilação 14327 ou posterior para compilar tabelas contendo recursos `MsftFunctionConfig()`. `MsftFunctionConfig()` os recursos são fornecidos para os clientes de multiplexação de pino como recursos de hardware. `MsftFunctionConfig()` os recursos devem ser fornecidos para os drivers que requerem alterações de multiplexação de pino, que são geralmente drivers do controlador SPB e seriais, mas não devem ser fornecidos para os drivers periféricos seriais e SPB, já que o driver de controlador manipula a configuração de multiplexação.
A macro da ACPI `MsftFunctionConfig()` é definida da seguinte maneira:

```cpp
  MsftFunctionConfig(Shared/Exclusive
                PinPullConfig,
                FunctionNumber,
                ResourceSource,
                ResourceSourceIndex,
                ResourceConsumer/ResourceProducer,
                VendorData) { Pin List }

```

* Compartilhado/exclusivo – Se exclusivo, o pino pode ser adquirido por um único cliente de cada vez. Se compartilhado, vários clientes compartilhados podem adquirir o recurso. Defina sempre como exclusivo, já que permitir que vários clientes não coordenados acessem um recurso mutável pode causar corridas aos dados e, portanto, resultados imprevisíveis. 
* PinPullConfig – um de 
  * PullDefault – usar a configuração de recepção de ativação padrão definida pelo SOC 
  * PullUp – habilitar a resistência de conexão 
  * PullDown – habilitar a resistência de suspensão 
  * PullNone – desabilitar todas as resistências de recepção 
* FunctionNumber – o número da função a ser programada no multiplexador. 
* ResourceSource – o caminho do namespace da ACPI do servidor de multiplexação de pino 
* ResourceSourceIndex – defina como 0 
* ResourceConsumer/ResourceProducer – defina como ResourceConsumer 
* VendorData – dados binários opcionais cujo significado é definido pelo servidor de multiplexação de pino. Isso geralmente deve ser deixado em branco
* Lista de pinos – uma lista separada por vírgulas de números de pinos aos quais a configuração se aplica. Quando o servidor de multiplexação de pino é um driver GpioClx, esses são os números de pino GPIO e têm o mesmo significado que os números de pino em um descritor GpioIo. 

O exemplo a seguir mostra como devemos fornecer um recurso MsftFunctionConfig() para um driver do controlador I2C. 

```cpp
Device(I2C1) 
{ 
    Name(_HID, "BCM2841") 
    Name(_CID, "BCMI2C") 
    Name(_UID, 0x1) 
    Method(_STA) 
    { 
        Return(0xf) 
    } 
    Method(_CRS, 0x0, NotSerialized) 
    { 
        Name(RBUF, ResourceTemplate() 
        { 
            Memory32Fixed(ReadWrite, 0x3F804000, 0x20) 
            Interrupt(ResourceConsumer, Level, ActiveHigh, Shared) { 0x55 } 
            MsftFunctionConfig(Exclusive, PullUp, 4, "\\_SB.GPI0", 0, ResourceConsumer, ) { 2, 3 } 
        }) 
        Return(RBUF) 
    } 
} 
```

Além dos recursos de memória e de interrupção geralmente exigidos por um driver de controlador, um recurso `MsftFunctionConfig()` também é especificado. Esse recurso permite que o driver do controlador I2C coloque os pinos 2 e 3 - gerenciados pelo nó do dispositivo em \\_SB.GPIO0 – na função 4 com resistência de conexão habilitada. 

## <a name="supporting-muxing-support-in-gpioclx-client-drivers"></a>Suporte à multiplexação em drivers de cliente GpioClx 

`GpioClx` tem suporte integrado à multiplexação de pino. Drivers de miniporta GpioClx (também chamados de "drivers de cliente GpioClx"), hardware do controlador GPIO da unidade. A partir do Windows 10 compilação 14327, os drivers de miniporta GpioClx podem adicionar suporte à multiplexação de pino implementando duas novas DDIs: 

* CLIENT_ConnectFunctionConfigPins – chamada pelo `GpioClx` para forçar o driver de miniporta a aplicar a configuração de multiplexação especificada. 
* CLIENT_DisconnectFunctionConfigPins – chamada pelo `GpioClx` para forçar o driver de miniporta a reverter a configuração de multiplexação especificada. 

Consulte [Funções de retorno de chamada de evento GpioClx](https://msdn.microsoft.com/library/windows/hardware/hh439464.aspx) para obter uma descrição dessas rotinas.

Além dessas duas novas DDIs, as DDIs existentes devem ser auditadas em relação à compatibilidade de multiplexação de pino: 

* CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt – CLIENT_ConnectIoPins é chamada pelo GpioClx para forçar o driver de miniporta a configurar um conjunto de pinos para entrada ou saída do GPIO. O GPIO e o MsftFunctionConfig são mutuamente exclusivos, ou seja, um pino nunca será ser conectado para GPIO e MsftFunctionConfig ao mesmo tempo. Como a função padrão de um pino não precisa ser GPIO, um pino não necessariamente precisa ser multiplexado para o GPIO quando ConnectIoPins é chamado. ConnectIoPins é obrigatório para a execução de todas as operações necessárias para tornar o pino pronto para E/S de GPIO, incluindo operações de multiplexação. *CLIENT_ConnectInterrupt* deve se comportar de forma semelhante, já que as interrupções podem ser consideradas como um caso especial de entrada de GPIO. 
* CLIENT_DisconnectIoPins/CLIENT_DisconnectInterrupt – esta rotina deve retornar os pinos para o estado em que estavam quando CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt foi chamada, a menos que o sinalizador PreserveConfiguration seja especificado. Além de reverter a direção dos pinos para seu estado padrão, a miniporta também deve reverter o estado de multiplexação de cada pino para o estado em que estava quando a rotina _Connect foi chamada. 

Por exemplo, presuma que a configuração de multiplexação padrão de um pino seja UART e o pino também pode ser usado como GPIO. Quando CLIENT_ConnectIoPins é chamada para conectar o pin para GPIO, ela deve multiplexar o pino para GPIO e, em CLIENT_DisconnectIoPins, ela deve multiplexar o pino de volta para UART. Em geral, as rotinas _Disconnect devem desfazer operações feitas pelas rotinas _Connect. 

## <a name="supporting-muxing-in-spbcx-and-sercx-controller-drivers"></a>Suporte à multiplexação em drivers de controlador SpbCx e SerCx 

A partir do Windows 10 compilação 14327, as estruturas `SpbCx` e `SerCx` contêm suporte integrado à multiplexação de pino, o que permite que os drivers de controlador `SpbCx` e `SerCx` sejam clientes de multiplexação de pino sem nenhuma alteração de código nos drivers de controlador em si. Por extensão, qualquer driver periférico SpbCx/SerCx que se conecta a um driver de controlador SpbCx/SerCx habilitado para multiplexação acionará a atividade de multiplexação de pino. 

O diagrama a seguir mostra as dependências entre cada um desses componentes. Como você pode ver, a multiplexação de pino apresenta uma dependência de drivers de controlador SerCx e SpbCx com o driver GPIO, que normalmente é responsável pela multiplexação. 

![Dependência de multiplexação de pino](images/usermode-access-diagram-2.png)

Durante o tempo de inicialização do dispositivo, as estruturas `SpbCx` e `SerCx` analisam todos os recursos `MsftFunctionConfig()` fornecidos como recursos de hardware para o dispositivo. SpbCx/SerCx, em seguida, adquire e libera os recursos de multiplexação de pino sob demanda.

`SpbCx` aplica a configuração de multiplexação de pino em seu manipulador *IRP_MJ_CREATE*, antes de chamar o retorno de chamada [EvtSpbTargetConnect()](https://msdn.microsoft.com/library/windows/hardware/hh450818.aspx) do driver do cliente. Se não tiver sido possível aplicar a configuração de multiplexação, o retorno de chamada `EvtSpbTargetConnect()` do driver do controlador não será chamado. Portanto, um driver de controlador SPB pode pressupor que os pinos são multiplexados para a função SPB no momento em que `EvtSpbTargetConnect()` é chamado.

`SpbCx` reverte a configuração de multiplexação de pino em seu manipulador *IRP_MJ_CLOSE*, após invocar o retorno de chamada [EvtSpbTargetDisconnect()](https://msdn.microsoft.com/library/windows/hardware/hh450820.aspx) do driver do controlador. O resultado é que os pinos são multiplexados para a função SPB sempre que um driver periférico abre um identificador para o driver do controlador SPB, e são multiplexados de volta quando o driver periférico fecha seu identificador.

`SerCx` se comporta de forma semelhante. `SerCx` obtém todos os recursos `MsftFunctionConfig()` em seu manipulador *IRP_MJ_CREATE* antes de invocar o retorno de chamada [EvtSerCx2FileOpen()](https://msdn.microsoft.com/library/windows/hardware/dn265209.aspx) do driver do controlador, e libera todos os recursos em seu manipulador IRP_MJ_CLOSE, antes de invocar o retorno de chamada [EvtSerCx2FileClose](https://msdn.microsoft.com/library/windows/hardware/dn265208.aspx) do driver do controlador.

A implicação da multiplexação de pino dinâmica para drivers de controlador `SerCx` e `SpbCx` é que eles devem ser capazes de tolerar que os pinos sejam multiplexados de volta da função SPB/UART em determinados momentos. Os drivers de controlador presumem que os pinos não serão multiplexados até que `EvtSpbTargetConnect()` ou `EvtSerCx2FileOpen()` seja chamado. Os pinos não precisam ser multiplexados para a função SPB/UART durante os retornos de chamada a seguir. A lista a seguir não está completa, mas representa as rotinas PNP mais comuns implementadas por drivers de controlador.

* DriverEntry 
* EvtDriverDeviceAdd 
* EvtDevicePrepareHardware/EvtDeviceReleaseHardware 
* EvtDeviceD0Entry/EvtDeviceD0Exit 

## <a name="verification"></a>Verificação 

Quando você estiver pronto para testar o rhproxy, será útil usar o seguinte procedimento passo a passo.

1. Verifique se os drivers de controlador `SpbCx`, `GpioClx` e `SerCx` estão carregando e funcionando corretamente.
1. Verifique se o `rhproxy` estiver presente no sistema. Não é encontrado em algumas edições e compilações do Windows.
1. Compile e carregue o nó do rhproxy usando `ACPITABL.dat`
1. Verifique se o nó do dispositivo `rhproxy` existe.
1. Verifique se o `rhproxy` está sendo carregado e iniciado
1. Verifique se os dispositivos esperados são expostos ao modo do usuário
1. Verifique se você consegue interagir com cada dispositivo na linha de comando
1. Verifique se você consegue interagir com cada dispositivo em um aplicativo UWP
1. Executar testes HLK

### <a name="verify-controller-drivers"></a>Verifique os drivers de controlador

Como o rhproxy expõe outros dispositivos no sistema ao modo do usuário, ele só funcionará se esses dispositivos já estiverem funcionando. A primeira etapa é verificar se os dispositivos (os controladores I2C, SPI, GPIO que deseja expor) já estão funcionando.

No prompt de comando, execute
```
devcon status *
```

Verifique a saída e se todos os dispositivos de interesse foram iniciados. Se um dispositivo tiver um código de problema, você precisará solucionar o problema quando o dispositivo não estiver carregando. Todos os dispositivos devem ter sido habilitados durante a exposição inicial da plataforma. A solução de problemas dos drivers de controlador `SpbCx`, `GpioClx` ou `SerCx` está além do escopo desse documento.

### <a name="verify-that-rhproxy-is-present-on-the-system"></a>Verifique se o rhproxy está presente no sistema.

Verifique se o serviço `rhproxy` está presente no sistema.

```
reg query HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\rhproxy
```

Se a chave do registro não estiver presente, o rhproxy não existe no sistema. O rhproxy está presente em todas as compilações do IoT Core e Windows Enterprise - compilação 15063 e mais recentes.

### <a name="compile-and-load-asl-with-acpitabldat"></a>Compilar e carregar ASL com ACPITABL.dat 

Agora que você já criou um nó de ASL de rhproxy, é hora de compilá-lo e carregá-lo. Você pode compilar o nó do rhproxy em um arquivo AML autônomo que pode ser acrescentado às tabelas ACPI do sistema. Como alternativa, se tiver acesso às fontes da ACPI do sistema, você pode inserir o nó do rhproxy diretamente nas tabelas ACPI da plataforma. No entanto, durante a exposição inicial, pode ser mais fácil usar `ACPITABL.dat`.

1. Crie um arquivo chamado suaplaca.asl e coloque o nó do dispositivo RHPX dentro de um DefinitionBlock: 
```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
        ...
        }
    }
}
```
2.  Baixe o [WDK](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk) e encontre o `asl.exe` em `C:\Program Files (x86)\Windows Kits\10\Tools\x64\ACPIVerify`
3.  Execute o comando a seguir para gerar o ACPITABL.dat:
```
asl.exe yourboard.asl
```
4.  Copie o arquivo ACPITABL.dat resultante para c:\windows\system32 no seu sistema em teste.
5.  Ative testsigning; no seu sistema em teste:
```
bcdedit /set testsigning on
```
6.  Reinicialize o sistema em teste. O sistema acrescentará as tabelas ACPI definidas no ACPITABL.dat às tabelas de firmware do sistema.

### <a name="verify-that-the-rhproxy-device-node-exists"></a>Verifique se o nó do dispositivo rhproxy existe.

Execute o comando a seguir para enumerar o nó do dispositivo rhproxy.
```
devcon status *msft8000
```
A saída do devcon deve indicar que o dispositivo está presente. Se o nó do dispositivo não estiver presente, as tabelas ACPI não foram adicionadas com êxito ao sistema.

### <a name="verify-that-rhproxy-is-loading-and-starting"></a>Verifique se o rhproxy está sendo carregado e iniciado

Verificar o status do rhproxy:
```
devcon status *msft8000
```
Se a saída indicar que o rhproxy foi iniciado, o rhproxy foi carregado e iniciado com êxito. Caso apareça um código de problema, você precisará investigá-lo. Alguns códigos de problema comuns são:
* Problema 51 - `CM_PROB_WAITING_ON_DEPENDENCY` - O sistema não está iniciando o rhproxy porque uma de suas dependências não foi carregada. Isso significa que os recursos passados ao rhproxy apontam para os nós de ACPI inválidos ou que os dispositivos de destino não estão sendo iniciados. Primeiro, verifique se todos os dispositivos estão sendo executados com sucesso (consulte "Verifique os drivers de controlador" acima). Em seguida, verifique o seu ASL e certifique-se de que todos os seus caminhos de recurso (por exemplo, `\_SB.I2C1`) estão corretos e apontam para nós válidos no seu DSDT.
* Problema 10 - `CM_PROB_FAILED_START` - O rhproxy não foi iniciado, muito provavelmente por causa de um problema de análise de recurso. Além de examinar seu ASL e conferir os índices de recurso no DSD, verifique se os recursos GPIO são especificados no aumento da ordem do número de pino.

### <a name="verify-that-the-expected-devices-are-exposed-to-usermode"></a>Verifique se os dispositivos esperados são expostos ao modo do usuário

Agora que o rhproxy está sendo executado, ele deve ter criado interfaces de dispositivos que podem ser acessadas pelo modo do usuário. Usaremos várias ferramentas de linha de comando para enumerar os dispositivos e verificar se estão presentes.

Clone o [https://github.com/ms-iot/samples](https://github.com/ms-iot/samples) repositório e compile o `GpioTestTool`, `I2cTestTool`, `SpiTestTool`, e `Mincomm` exemplos. Copie as ferramentas no dispositivo em teste e use os comandos a seguir para enumerar os dispositivos.
```
I2cTestTool.exe -list
SpiTestTool.exe -list
GpioTestTool.exe -list
MinComm.exe -list
```
Você deve verificar seus dispositivos e nomes amigáveis listados. Se não verificar os dispositivos e nomes amigáveis certos, confira seu ASL.

### <a name="verify-each-device-on-the-command-line"></a>Verifique cada dispositivo na linha de comando

A próxima etapa é usar as ferramentas de linha de comando para abrir e interagir com os dispositivos.

Exemplo de I2CTestTool:
```
I2cTestTool.exe 0x55 I2C1
> write {1 2 3}
> read 3
> writeread {1 2 3} 3
```

Exemplo de SpiTestTool:
```
SpiTestTool.exe -n SPI1
> write {1 2 3}
> read 3
```

Exemplo de GpioTestTool:
```
GpioTestTool.exe 12
> setdrivemode output
> write 0
> write 1
> setdrivemode input
> read
> interrupt on
> interrupt off
```

Exemplo de MinComm (Serial). Conecte Rx a Tx antes da execução:
```
MinComm "\\?\ACPI#FSCL0007#3#{86e0d1e0-8089-11d0-9ce4-08003e301f73}\0000000000000006"
(type characters and see them echoed back)
```

### <a name="verify-each-device-from-a-uwp-app"></a>Verifique cada dispositivo de um aplicativo UWP

Use as amostras a seguir para permitir que dispositivos funcionem na UWP.

| Exemplo | Link |
|------|------|
| IoT-GPIO | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-GPIO |
| IoT-I2C | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-I2C | 
| IoT-SPI | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-SPI |
| CustomSerialDeviceAccess | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomSerialDeviceAccess |

### <a name="run-the-hlk-tests"></a>Executar os testes HLK

Baixe o [Kit de Laboratório de Hardware (HLK)](https://docs.microsoft.com/windows-hardware/test/hlk/windows-hardware-lab-kit). Os seguintes testes estão disponíveis:
 * [Testes funcionais e de resistência GPIO WinRT](https://docs.microsoft.com/windows-hardware/test/hlk/testref/f1fc0922-1186-48bd-bfcd-c7385a2f6f96)
 * [Teste de gravação I2C WinRT (EEPROM necessário)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/2ab0df1b-3369-4aaf-a4d5-d157cb7bf578)
 * [Teste de leitura I2C WinRT (EEPROM necessário)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/ca91c2d2-4615-4a1b-928e-587ab2b69b04)
 * [Testes de endereço escravo não existentes I2C WinRT](https://docs.microsoft.com/windows-hardware/test/hlk/testref/2746ad72-fe5c-4412-8231-f7ed53d95e71)
 * [Testes funcionais avançados I2C WinRT (mbed LPC1768 necessário)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/a60f5a94-12b2-4905-8416-e9774f539f1d)
 * [Testes de verificação de frequência do relógio SPI WinRT (mbed LPC1768 necessário)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/50cf9ccc-bbd3-4514-979f-b0499cb18ed8)
 * [Testes de transferência de E/S SPI WinRT (mbed LPC1768 necessário)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/00c892e8-c226-4c71-9c2a-68349fed7113)
 * [Testes de verificação de distância SPI WinRT](https://docs.microsoft.com/windows-hardware/test/hlk/testref/20c6b079-62f7-4067-953f-e252bd271938)
 * [Testes de detecção de lacuna de transferência SPI WinRT (mbed LPC1768 necessário)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/6da79d04-940b-4c49-8f00-333bf0cfbb19)

Quando você selecionar o nó do dispositivo rhproxy no gerenciador de HLK, os testes aplicáveis serão selecionados automaticamente.

No gerenciador de HLK, selecione "Resource Hub Proxy device" (Dispositivo Proxy do Hub de Recursos):

![Captura de tela do gerenciador de HLK](images/usermode-hlk-1.png)

Em seguida, clique na guia de testes e selecione os testes I2C WinRT, Gpio WinRT e Spi WinRT.

![Captura de tela do gerenciador de HLK](images/usermode-hlk-2.png)

Clique em Run Selected (Executar Selecionado). Você encontrará mais documentação sobre cada teste clicando com o botão direito do mouse no teste e em "Test Description" (Descrição do Teste).

## <a name="resources"></a>Recursos

| Destination | Link |
|-------------|------|
| Especificação ACPI 5.0 | http://acpi.info/spec.htm |
| Asl.exe (compilação ASL da Microsoft) | https://msdn.microsoft.com/library/windows/hardware/dn551195.aspx |
| Windows.Devices.Gpio  | https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.aspx | 
| Windows.Devices.I2c | https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.aspx |
| Windows.Devices.Spi | https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.aspx |
| Windows.Devices.SerialCommunication | https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.aspx |
| Estrutura de execução e criação de testes (TAEF) | https://msdn.microsoft.com/library/windows/hardware/hh439725.aspx |
| SpbCx | https://msdn.microsoft.com/library/windows/hardware/hh450906.aspx |
| GpioClx   | https://msdn.microsoft.com/library/windows/hardware/hh439508.aspx |
| SerCx | https://msdn.microsoft.com/library/windows/hardware/ff546939.aspx |
| Testes MITT I2C | https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx |
| GpioTestTool | https://developer.microsoft.com/windows/iot/samples/GPIOTestTool |
| I2cTestTool   | https://developer.microsoft.com/windows/iot/samples/I2cTestTool | 
| SpiTestTool | https://developer.microsoft.com/windows/iot/samples/spitesttool |
| MinComm (Serial) |    https://github.com/ms-iot/samples/tree/develop/MinComm |
| Kit de Laboratório de Hardware (HLK) | https://msdn.microsoft.com/library/windows/hardware/dn930814.aspx |

## <a name="apendix"></a>Apêndice

### <a name="appendix-a---raspberry-pi-asl-listing"></a>Apêndice A - Listagem de ASL de Raspberry Pi

Pinos de cabeçalho:https://developer.microsoft.com/windows/iot/samples/PinMappingsRPi2

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{

    Scope (\_SB)
    {
        //
        // RHProxy Device Node to enable WinRT API
        //
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate()
            {
                // Index 0
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE0  - GPIO 8  - Pin 24
                    0,                     // Device selection (CE0)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 1
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE1  - GPIO 7  - Pin 26
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 2
                SPISerialBus(              // SCKL - GPIO 21 - Pin 40
                                           // MOSI - GPIO 20 - Pin 38
                                           // MISO - GPIO 19 - Pin 35
                                           // CE1  - GPIO 17 - Pin 11
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data
                // Index 3
                I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1)
                    0xFFFF,                // SlaveAddress: placeholder
                    ,                      // SlaveMode: default to ControllerInitiated
                    0,                     // ConnectionSpeed: placeholder
                    ,                      // Addressing Mode: placeholder
                    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name
                    ,
                    ,
                    )                      // VendorData

                // Index 4 - GPIO 4 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 4 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 4 }
                // Index 6 - GPIO 5 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 5 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 5 }
                // Index 8 - GPIO 6 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 6 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 6 }
                // Index 10 - GPIO 12 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 12 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 12 }
                // Index 12 - GPIO 13 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 13 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 13 }
                // Index 14 - GPIO 16 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 16 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 16 }
                // Index 16 - GPIO 18 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 18 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 18 }
                // Index 18 - GPIO 22 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 22 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 22 }
                // Index 20 - GPIO 23 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 23 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 23 }
                // Index 22 - GPIO 24 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 24 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 24 }
                // Index 24 - GPIO 25 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 25 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 25 }
                // Index 26 - GPIO 26 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 26 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 26 }
                // Index 28 - GPIO 27 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 27 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 27 }
                // Index 30 - GPIO 35 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 35 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 35 }
                // Index 32 - GPIO 47 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 47 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 47 }
            })

            Name(_DSD, Package()
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package()
                {
                    // Reference http://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md
                    // SPI 0
                    Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }},                       // Index 0 & 1
                    Package(2) { "SPI0-MinClockInHz", 7629 },                               // 7629 Hz
                    Package(2) { "SPI0-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // SPI 1
                    Package(2) { "bus-SPI-SPI1", Package() { 2 }},                          // Index 2
                    Package(2) { "SPI1-MinClockInHz", 30518 },                              // 30518 Hz
                    Package(2) { "SPI1-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI1-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // I2C1
                    Package(2) { "bus-I2C-I2C1", Package() { 3 }},
                    // GPIO Pin Count and supported drive modes
                    Package (2) { "GPIO-PinCount", 54 },
                    Package (2) { "GPIO-UseDescriptorPinNumbers", 1 },
                    Package (2) { "GPIO-SupportedDriveModes", 0xf },                        // InputHighImpedance, InputPullUp, InputPullDown, OutputCmos
                }
            })
        }
    }
}

```

### <a name="appendix-b---minnowboardmax-asl-listing"></a>Apêndice B - Listagem de ASL de MinnowBoardMax

Pinos de cabeçalho:https://developer.microsoft.com/windows/iot/samples/PinMappingsMBM

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate() 
            {  
                // Index 0 
                SPISerialBus(            // Pin 5, 7, 9 , 11 of JP1 for SIO_SPI
                    1,                     // Device selection
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    8,                     // databit len
                    ControllerInitiated,   // slave mode
                    8000000,               // Connection speed
                    ClockPolarityLow,      // Clock polarity
                    ClockPhaseSecond,      // clock phase
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                    ResourceConsumer,      // Resource usage
                    JSPI,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      // Vendor Data  
    
                // Index 1     
                I2CSerialBus(            // Pin 13, 15 of JP1, for SIO_I2C5 (signal)
                    0xFF,                  // SlaveAddress: bus address
                    ,                      // SlaveMode: default to ControllerInitiated
                    400000,                // ConnectionSpeed: in Hz
                    ,                      // Addressing Mode: default to 7 bit
                    "\\_SB.I2C6",          // ResourceSource: I2C bus controller name (For MinnowBoard Max, hardware I2C5(0-based) is reported as ACPI I2C6(1-based))
                    ,
                    ,
                    JI2C,                  // Descriptor Name: creates name for offset of resource descriptor
                    )                      // VendorData
    
                // Index 2
                UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    ,                      // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT2",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR2,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      
    
                // Index 3
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {0}  // Pin 21 of JP1 (GPIO_S5[00])
                // Index 4
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {0} 
    
                // Index 5
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {1}  // Pin 23 of JP1 (GPIO_S5[01])
                // Index 6
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {1}
    
                // Index 7
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {2}  // Pin 25 of JP1 (GPIO_S5[02])
                // Index 8
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {2} 
    
                // Index 9
                UARTSerialBus(           // Pin 6, 8, 10, 12 of JP1, for SIO_UART1
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    FlowControlHardware,   // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT1",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR1,              // DescriptorName: creates name for offset of resource descriptor
                    )  
    
                // Index 10
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {62}  // Pin 14 of JP1 (GPIO_SC[62])
                // Index 11
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {62} 

                // Index 12
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {63}  // Pin 16 of JP1 (GPIO_SC[63])
                // Index 13
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {63} 
    
                // Index 14
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {65}  // Pin 18 of JP1 (GPIO_SC[65])
                // Index 15
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {65} 
    
                // Index 16
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {64}  // Pin 20 of JP1 (GPIO_SC[64])
                // Index 17
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {64} 
    
                // Index 18
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {94}  // Pin 22 of JP1 (GPIO_SC[94])
                // Index 19
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {94} 
    
                // Index 20
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {95}  // Pin 24 of JP1 (GPIO_SC[95])
                // Index 21
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {95} 
    
                // Index 22
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {54}  // Pin 26 of JP1 (GPIO_SC[54])
                // Index 23
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {54}
            })
    
            Name(_DSD, Package() 
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package() 
                {
                    // SPI Mapping
                    Package(2) { "bus-SPI-SPI0", Package() { 0 }},

                    Package(2) { "SPI0-MinClockInHz", 100000 },
                    Package(2) { "SPI0-MaxClockInHz", 15000000 },
                    // SupportedDataBitLengths takes a list of support data bit length
                    // Example : Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8, 7, 16 }},
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32 }},
                     // I2C Mapping
                    Package(2) { "bus-I2C-I2C5", Package() { 1 }},
                    // UART Mapping
                    Package(2) { "bus-UART-UART2", Package() { 2 }},
                    Package(2) { "bus-UART-UART1", Package() { 9 }},
                }
            })
        }
    }
}
```

### <a name="appendix-c---sample-powershell-script-to-generate-gpio-resources"></a>Apêndice C - Script do Powershell de exemplo para gerar recursos GPIO

O script a seguir pode ser usado para gerar as declarações de recursos GPIO para Raspberry Pi:

```
$pins = @(
    @{PinNumber=4;PullConfig='PullUp'},
    @{PinNumber=5;PullConfig='PullUp'},
    @{PinNumber=6;PullConfig='PullUp'},
    @{PinNumber=12;PullConfig='PullDown'},
    @{PinNumber=13;PullConfig='PullDown'},
    @{PinNumber=16;PullConfig='PullDown'},
    @{PinNumber=18;PullConfig='PullDown'},
    @{PinNumber=22;PullConfig='PullDown'},
    @{PinNumber=23;PullConfig='PullDown'},
    @{PinNumber=24;PullConfig='PullDown'},
    @{PinNumber=25;PullConfig='PullDown'},
    @{PinNumber=26;PullConfig='PullDown'},
    @{PinNumber=27;PullConfig='PullDown'},
    @{PinNumber=35;PullConfig='PullUp'},
    @{PinNumber=47;PullConfig='PullUp'})
    
# generate the resources
$FIRST_RESOURCE_INDEX = 4
$resourceIndex = $FIRST_RESOURCE_INDEX
$pins | % {
    $a = @"
// Index $resourceIndex - GPIO $($_.PinNumber) - $($_.Name)
GpioIO(Shared, $($_.PullConfig), , , , "\\_SB.GPI0", , , , ) { $($_.PinNumber) }
GpioInt(Edge, ActiveBoth, Shared, $($_.PullConfig), 0, "\\_SB.GPI0",) { $($_.PinNumber) }
"@    
    Write-Host $a
    $resourceIndex += 2;
}
```
