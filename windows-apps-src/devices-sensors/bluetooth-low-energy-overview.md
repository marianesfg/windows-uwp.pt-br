---
author: msatranjr
title: Bluetooth de baixa energia
description: Este tópico fornece uma visão geral de Bluetooth LE nos UWP aplicativos.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, bluetooth, bluetooth LE, baixa energia, gatt, lacuna, central, periféricos, cliente, server, Inspetor, publisher
ms.localizationpriority: medium
ms.openlocfilehash: 0d6cc1fb5a0b3cb96748b99c490b23a9e1df128f
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "304418"
---
# <a name="bluetooth-low-energy"></a>Bluetooth de baixa energia
Energia de baixa Bluetooth (LE) é uma especificação que define protocolos para descoberta e a comunicação entre dispositivos eficiente de energia. Descoberta de dispositivos é feita por meio do protocolo de perfil de acesso genérico (GAP). Depois de descoberta, comunicação de dispositivo para dispositivo é feita por meio do protocolo de atributo genérico (GATT). Este tópico fornece uma visão geral de Bluetooth LE nos UWP aplicativos. Para ver mais detalhes sobre Bluetooth LE, consulte a [Especificação de núcleo Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) versão 4.0, onde LE Bluetooth foi introduzido. 

![Funções de LE Bluetooth](images/gatt-roles.png)

*Funções GATT e lacuna introduzidas no Windows 10 versão 1703*

Protocolos GATT e lacuna podem ser implementados de seu aplicativo UWP usando os seguintes namespaces.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central e periféricos
As duas funções principais de descoberta são chamadas Central e periféricos. Em geral, o Windows opera em modo Central e se conecta a vários dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Um acrônimo comuns, que você verá em APIs de Bluetooth do Windows é um atributo genérico (GATT). O perfil GATT define a estrutura dos dados e modos de operação pelo qual os dois dispositivos Bluetooth LE se comunicar. O atributo é o principal bloco de construção de GATT. Os principais tipos de atributos são descritores, características e serviços. Esses atributos executam de forma diferente entre clientes e servidores, portanto, é mais útil discutir sua interação nas seções relevantes. 

![Hierarquia de atributo típica em um perfil comum](images/gatt-service.png)

*O serviço de taxa de coração é expresso em forma de API do servidor de GATT*

## <a name="client-and-server"></a>Cliente e servidor
Depois que uma conexão tiver sido estabelecida, o dispositivo que contém os dados (normalmente um sensor IoT pequeno ou vestir) é conhecido como o servidor. O dispositivo que usa esses dados para executar uma função é conhecido como o cliente. Por exemplo, um PC com Windows (cliente) lê dados de um monitor de taxa de coração (servidor) para controlar se um usuário está funcionando ideal check-out. Para obter mais informações, consulte os tópicos [GATT cliente](gatt-client.md) e [Servidor de GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Observadores e editores (avisos)
Além das funções Central e periféricos, há funções observador e emissora. Transmissores são comumente referidas como avisos, eles não se comunicar pela GATT porque eles usam o espaço limitado fornecido no pacote de anúncio para comunicação. Da mesma forma, não tem um observador estabelecer uma conexão para receber dados, ele buscará anúncios vizinhos. Para configurar o Windows para observar os próximos anúncios, use a classe de [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Para transmitir beacon cargas, use a classe de [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obter mais informações, consulte o tópico de [anúncio](ble-beacon.md) .

## <a name="see-also"></a>Veja também
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificação de núcleo Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)