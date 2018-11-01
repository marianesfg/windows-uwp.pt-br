---
author: msatranjr
title: Bluetooth Low Energy
description: Este tópico fornece uma visão geral de Bluetooth LE em aplicativos UWP.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: Windows 10, uwp, bluetooth, bluetooth LE, baixo consumo de energia, gatt, lacuna, central, periférico, cliente, servidor, Inspetor, fornecedor
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871588"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth baixa energia (LE) é uma especificação que define protocolos para descoberta e comunicação entre os dispositivos de consumo de energia. Descoberta de dispositivos é feita por meio do protocolo de perfil de acesso genérico (intervalo). Após a detecção, comunicação de dispositivo para dispositivo é feita por meio do protocolo de atributo genérico (GATT). Este tópico fornece uma visão geral de Bluetooth LE em aplicativos UWP. Para ver mais detalhes sobre Bluetooth LE, consulte a [Especificação de núcleo Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) versão 4.0, onde foi introduzido Bluetooth LE. 

![Funções do Bluetooth LE](images/gatt-roles.png)

*Funções de GATT e lacuna foram introduzidas no Windows 10 versão 1703*

Protocolos GATT e lacuna podem ser implementados em seu aplicativo UWP usando os seguintes namespaces.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Periféricos e Central
As duas funções principais de descoberta são chamadas Central e periférico. Em geral, o Windows funciona no modo Central e se conecta a vários dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Um acrônimo comuns que você verá em APIs Bluetooth do Windows é GATT (atributo genérico). O perfil de GATT define a estrutura dos dados e modos de operação pelo qual se comunicar dois dispositivos Bluetooth LE. O atributo é o bloco de construção principal de GATT. Os principais tipos de atributos são serviços, características e descritores. Esses atributos executam de maneira diferente entre clientes e servidores, portanto, é mais útil discutir sua interação nas seções relevantes. 

![Hierarquia de atributo típica em um perfil comum](images/gatt-service.png)

*O serviço cardíaca é expresso em forma de API do servidor GATT*

## <a name="client-and-server"></a>Cliente e servidor
Depois que uma conexão é estabelecida, o dispositivo que contém os dados (geralmente um sensor IoT pequeno ou dispositivo acessório) é conhecido como o servidor. O dispositivo que usa esses dados para executar uma função é conhecido como o cliente. Por exemplo, um computador com Windows (cliente) lê dados de um monitor de frequência cardíaca (servidor) para controlar se um usuário está funcionando-out forma ideal. Para obter mais informações, consulte os tópicos [GATT cliente](gatt-client.md) e [Servidor de GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Inspetores e editores (Beacons)
Além das funções Central e do periférico, há funções observador e emissora. As emissoras normalmente são chamadas de Beacons, eles não se comunicam pela GATT porque eles usam o espaço limitado fornecido no pacote de anúncio para comunicação. Da mesma forma, não tem um observador estabelecer uma conexão para receber dados, ele procura por anúncios nas proximidades. Para configurar o Windows para observar nas proximidades anúncios, use a classe [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Para cargas de beacon de difusão, use a classe [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obter mais informações, consulte o tópico de [anúncio](ble-beacon.md) .

## <a name="see-also"></a>Consulte também
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificação de núcleo de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)