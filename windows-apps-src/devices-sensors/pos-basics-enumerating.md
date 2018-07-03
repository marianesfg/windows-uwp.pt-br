---
author: TerryWarwick
title: Enumerar dispositivos de PointOfService
description: Saiba como enumerar dispositivos de PointOfService
ms.author: jken
ms.date: 06/8/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: be1fdc42295fc03ff86a69e287a4089abe547689
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018434"
---
# <a name="enumerating-point-of-service-devices"></a>Enumerar dispositivos de Ponto de Serviço
Nesta seção, você aprenderá como [**definir um seletor de dispositivo**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) usado para consultar os dispositivos disponíveis para o sistema e usar esse seletor para enumerar dispositivos de Ponto de Serviço usando um dos seguintes métodos:

**Método 1:** [**Obter o primeiro dispositivo disponível**](#Method-1:-get-first-available-device)<br />Nesta seção, você aprenderá como usar **GetDefaultAsync** para acessar o primeiro dispositivo disponível em uma classe de dispositivo PointOfService específica.

**Método 2:** [**Instantâneo de dispositivos**](#Method-2:-Snapshot-of-devices)<br />Nesta seção, você aprenderá como enumerar um instantâneo dos dispositivos de PointOfService que estão presentes no sistema em um determinado momento. Isso é útil que você precisa criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário. FindAllAsync manterá os resultados até que toda a enumeração seja concluída.

**Método 3:** [**Enumerar e inspecionar**](#Method-3:-Enumerate-and-watch)<br />Nesta seção você aprenderá sobre um modelo de enumeração mais avançado e flexível que permite enumerar dispositivos presentes no momento, assim como receber notificações quando dispositivos são adicionados ou removidos do sistema.  Isso é útil quando você quiser manter uma lista atual de dispositivos em segundo plano para exibir na interface do usuário em vez de aguardar um instantâneo.
 

---
## <a name="define-a-device-selector"></a>Definir um seletor de dispositivo
Um seletor de dispositivo permitirá que você limite os dispositivos que está pesquisando ao enumerar dispositivos.  Isso permitirá que você obtenha apenas os resultados relevantes e reduza o tempo necessário para enumerar os dispositivos desejados.  

O [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) fornecerá um seletor para enumerar todas as POSPrinters vinculadas ao sistema, incluindo POSPrinters USB, de rede e Bluetooth.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();   

```

Com o método [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector_Windows_Devices_PointOfService_PosConnectionTypes_), que considera [**PosConnectionTypes**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) como um parâmetro, você pode restringir o seletor para enumerar POSPrinters vinculadas locais, de rede ou Bluetooth, reduzindo o tempo necessário para a conclusão da consulta.  O exemplo a seguir mostra o uso desse método para definir um seletor que oferece suporte somente para POSPrinters vinculadas localmente.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);   

```
> [!TIP]
> Consulte [**Criar um seletor de dispositivos**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) para criar cadeias de caracteres de seletor mais avançadas.

---

## <a name="method-1-get-first-available-device"></a>Método 1: obter o primeiro dispositivo disponível

A maneira mais simples de obter um dispositivo PointOfService é usar **GetDefaultAsync** para obter o primeiro dispositivo disponível em uma classe de dispositivo PointOfService. 

O exemplo a seguir ilustra o uso de [**GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) para BarcodeScanner. O padrão de codificação é semelhante para todas as classes de dispositivo PointOfService.

```Csharp

using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();

```

> [!CAUTION]
> GetDefaultAsync deve ser usado com cuidado, pois pode retornar um dispositivo diferente de uma sessão para a seguinte. Muitos eventos podem influenciar essa enumeração resultando em um outro dispositivo disponível primeiro, incluindo: 
> - Mudança de câmeras conectadas ao computador 
> - Mudança de dispositivos PointOfService conectados ao computador
> - Mudança nos dispositivos PointOfService conectados e disponíveis na sua rede
> - Mudança nos dispositivos PointOfService Bluetooth dentro do alcance do computador 
> - Mudanças na configuração de PointOfService 
> - Instalação de drivers ou objetivos de serviço OPOS
> - Instalação de extensões de PointOfService
> - Atualizar para o sistema operacional Windows

---

## <a name="method-2-snapshot-of-devices"></a>Método 2: instantâneo de dispositivos

Em alguns cenários, talvez seja preciso criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário.  Nessas situações, você pode enumerar um instantâneo de dispositivos que estão conectados ou emparelhados com o sistema usando [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Essa método manterá qualquer resultado até que toda a enumeração seja concluída.

> [!TIP]
> É recomendável usar o método GetDeviceSelector com o parâmetro PosConnectionTypes ao usar FindAllAsync para limitar sua consulta ao tipo de conexão desejado.  As conexões de rede e Bluetooth podem atrasar os resultados, pois as enumerações devem ser concluída antes de retornar os resultados de FindAllAsync.

>[!CAUTION] 
>FindAllAsync retorna uma matriz de dispositivos.  A ordem dessa matriz pode alterar a cada sessão, portanto, não é recomendado depender em uma ordem específica usando um índice codificada na matriz.  Use a propriedade DeviceInformation para filtrar os resultados ou fornecer uma interface do usuário para o usuário escolher.

Este exemplo usa o seletor definido acima para tirar um instantâneo de dispositivos usando FindAllAsync e enumera por meio de cada um dos itens retornados pela coleção e grava o nome do dispositivo e a identificação na saída de depuração. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Ao trabalhar com APIs [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), frequentemente será necessário usar objetos [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obter informações sobre um dispositivo específico. Por exemplo, a propriedade [**DeviceInformation.ID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) pode ser usada para recuperar e reutilizar o mesmo dispositivo se estiver disponível em uma sessão futura e a propriedade [**DeviceInformation.Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) pode ser usada para exibir as finalidade no aplicativo.  Consulte a página de referência [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para saber mais sobre as propriedades adicionais disponíveis.

---

## <a name="method-3-enumerate-and-watch"></a>Método 3: enumerar e inspecionar

Um método mais avançado e flexível de enumeração de dispositivos é criar um [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Um inspetor de dispositivos enumera dispositivos dinamicamente, portanto, o aplicativo recebe notificações se dispositivos são adicionados, removidos ou modificados após a conclusão da enumeração inicial.  Um DeviceWatcher permitirá que você detecte quando um dispositivo conectado em rede fica online, um dispositivo Bluetooth estiver ao alcance e se um dispositivo conectado localmente é desconectado para você poder tomar a ação apropriada no aplicativo.

Esse exemplo usa o seletor definido acima para criar um DeviceWatcher, bem como define manipuladores de eventos para as notificações Adicionado, Removido e Atualizado. É necessário preencher os detalhes das ações que você deseja implementar a cada notificação.

```Csharp

using Windows.Devices.Enumeration;

DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

> [!TIP]
> Consulte [**Enumerar e inspecionar dispositivos**]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) para obter mais informações sobre o uso de um DeviceWatcher
