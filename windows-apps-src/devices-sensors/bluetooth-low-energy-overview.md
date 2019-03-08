---
title: Bluetooth Low Energy
description: Este tópico fornece uma visão geral do Bluetooth LE em aplicativos UWP.
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, bluetooth, bluetooth LE, low energy, gatt, gap, central, periférico, cliente, servidor, inspetor, editor
ms.localizationpriority: medium
ms.openlocfilehash: 1714a4c21852a7582325fc26f7e6a1f0f969126e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629031"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth Low Energy (LE) é uma especificação que define protocolos para descoberta e comunicação entre os dispositivos de baixo consumo de energia. A descoberta de dispositivos é feita por meio do protocolo GAP (Perfil de Acesso Genérico). Após a descoberta, a comunicação entre dispositivos é concluída por meio do protocolo GATT (Atributo Genérico). Este tópico fornece uma visão geral do Bluetooth LE em aplicativos UWP. Para ver mais detalhes sobre o Bluetooth LE, veja a [Especificação básica do Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) versão 4.0, em que o Bluetooth LE foi introduzido. 

![Funções do Bluetooth LE](images/gatt-roles.png)

*Funções de GATT e GAP foram introduzidas no Windows 10 versão 1703*

Os protocolos GATT e GAP podem ser implementados em seu aplicativo UWP usando os namespaces a seguir.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central e Periférico
As duas funções principais de descoberta são chamadas Central e Periférico. Em geral, o Windows opera no modo Central e se conecta a diversos dispositivos Periféricos. 

## <a name="attributes"></a>Atributos
Um acrônimo comum que você verá nas APIs Bluetooth do Windows é o GATT (Atributo Genérico). O Perfil GATT define a estrutura de dados e os modos de operação pelos quais dois dispositivos Bluetooth LE se comunicam. O atributo é o bloco de construção principal de GATT. Os principais tipos de atributos são serviços, características e descritores. Esses atributos têm desempenhos diferentes entre clientes e servidores e, portanto, é mais útil discutir a interação entre eles nas seções relevantes. 

![Hierarquia de atributo típica em um perfil comum](images/gatt-service.png)

*O serviço de frequência cardíaca é expressa na forma de API de servidor GATT*

## <a name="client-and-server"></a>Cliente e Servidor
Depois que uma conexão tiver sido estabelecida, o dispositivo que contém os dados (geralmente um sensor IoT pequeno ou um acessório) é conhecido como o Servidor. O dispositivo que usa esses dados para executar uma função é conhecido como o Cliente. Por exemplo, um computador Windows (Cliente) lê dados de um monitor de frequência cardíaca (Servidor) para controlar se um usuário está treinando da forma ideal. Para saber mais, veja os tópicos [Cliente GATT](gatt-client.md) e [Servidor GATT](gatt-server.md).

## <a name="watchers-and-publishers-beacons"></a>Inspetores e Editores (Sinalizadores)
Além das funções Central e Periférico, existem as funções Observador e Emissora. As Emissoras normalmente são conhecidas como Sinalizadores, elas não se comunicam pela GATT pois usam ao espaço limitado fornecido no pacote de Anúncio para comunicação. Da mesma forma, um Observador não precisa estabelecer uma conexão para receber dados, ele procura anúncios nas proximidades. Para configurar o Windows para observar os anúncios próximos, use a classe [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher). Para transmitir as cargas de sinalizador de difusão, use a classe [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher). Para saber mais, veja o tópico [Advertisement](ble-beacon.md).

## <a name="see-also"></a>Consulte também
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificação principal de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)