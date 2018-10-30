---
author: TerryWarwick
title: Enumerar dispositivos de PointOfService
description: Saiba como enumerar dispositivos de PointOfService
ms.author: jken
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 10804b006cb7ab542c74e363af5134634b7651e3
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5759810"
---
# <a name="enumerating-point-of-service-devices"></a>Enumerar dispositivos de Ponto de Serviço
Nesta seção, você aprenderá como [definir um seletor de dispositivo](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) usado para consultar os dispositivos disponíveis para o sistema e usar esse seletor para enumerar dispositivos de Ponto de Serviço usando um dos seguintes métodos:

**Método 1:** [Use um seletor de dispositivo](#method-1:-use-a-device-picker)
<br/>
Exibir um seletor de dispositivo da interface do usuário e fazer com que o usuário escolha um dispositivo conectado. Esse método manipula a atualização da lista quando dispositivos são conectados e removidos e é mais simples e mais segura do que outros métodos.

**Método 2:** [Obter o primeiro dispositivo disponível](#Method-1:-get-first-available-device)<br />Use [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) para acessar o primeiro dispositivo disponível em uma classe de dispositivo de ponto de serviço específica.

**Método 3:** [Instantâneo de dispositivos](#Method-2:-Snapshot-of-devices)<br />Enumere um instantâneo dos dispositivos de ponto de serviço que estão presentes no sistema em um determinado momento. Isso é útil que você precisa criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) manterá resultados até que toda a enumeração seja concluída.

**Método 4:** [Enumerar e inspecionar](#Method-3:-Enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) é um modelo de enumeração mais avançado e flexível que permite enumerar dispositivos que estão presentes no momento e também receber notificações quando dispositivos são adicionados ou removidos do sistema.  Isso é útil quando você quiser manter uma lista atual de dispositivos em segundo plano para exibir na interface do usuário em vez de aguardar um instantâneo.

## <a name="define-a-device-selector"></a>Definir um seletor de dispositivo
Um seletor de dispositivo permitirá que você limite os dispositivos que está pesquisando ao enumerar dispositivos.  Isso permitirá que você apenas obter resultados relevantes e reduza o tempo que leva para enumerar os dispositivos desejados.

Você pode usar o método **GetDeviceSelector** para o tipo de dispositivo que você estiver procurando para obter o seletor de dispositivo desse tipo. Por exemplo, usar [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) fornecerá você com um seletor para enumerar todas as [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) conectados ao sistema, incluindo USB, rede e impressoras POS Bluetooth.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Os métodos de **GetDeviceSelector** para os tipos de dispositivo diferentes são:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

Usando um método **GetDeviceSelector** que leva um valor de [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) como um parâmetro, você pode restringir o seletor para enumerar local, rede ou dispositivos de POS Bluetooth conectadas, reduzindo o tempo necessário para a conclusão da consulta.  O exemplo a seguir mostra um uso desse método para definir um seletor que oferece suporte a apenas localmente anexados impressoras POS.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Consulte [Criar um seletor de dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) para criar cadeias de caracteres de seletor mais avançadas.

## <a name="method-1-use-a-device-picker"></a>Método 1: Usar um seletor de dispositivo

A classe [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) permite que você exiba um submenu do seletor que contém uma lista de dispositivos para o usuário serem escolhidas. Você pode usar a propriedade de [filtro](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) para escolher quais tipos de dispositivos para mostrar o seletor. Essa propriedade é do tipo [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Você pode adicionar os tipos de dispositivo para o filtro usando a propriedade [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) ou [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) .

Quando você estiver pronto para mostrar o seletor de dispositivo, você pode chamar o método [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) , que mostrará o seletor de interface do usuário e retornar o dispositivo selecionado. Você precisará especificar um [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) que determinará onde o submenu aparece. Esse método retornará um objeto [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) , portanto, para usá-lo com o ponto de serviço APIs, você precisará usar o método **FromIdAsync** para a classe de dispositivo específico que você deseja. Passe a propriedade [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) como o parâmetro do método *deviceId* e obter uma instância da classe de dispositivo, como o valor de retorno.

O trecho de código a seguir cria um **DevicePicker**, adiciona um filtro de scanner de código de barras, tem o usuário escolha um dispositivo e, em seguida, cria um objeto **BarcodeScanner** com base na ID de dispositivo:

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>Método 2: Obter o primeiro dispositivo disponível

A maneira mais simples de obter um dispositivo de ponto de serviço é usar **GetDefaultAsync** para obter o primeiro dispositivo disponível dentro de uma classe de dispositivo de ponto de serviço. 

O exemplo a seguir ilustra o uso de [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) para [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). O padrão de codificação é semelhante para todas as classes de dispositivo de ponto de serviço.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** deve ser usado com cuidado, pois pode retornar um dispositivo diferente de uma sessão para o próximo. Muitos eventos podem influenciar essa enumeração resultando em um outro dispositivo disponível primeiro, incluindo: 
> - Mudança de câmeras conectadas ao computador 
> - Alterar o ponto de entrada serviço dispositivos conectados ao computador
> - Mudança de conectado à rede dispositivos de ponto de serviço disponíveis na sua rede
> - Mudança nos dispositivos de ponto de serviço do Bluetooth dentro do alcance do seu computador 
> - Alterações na configuração de ponto de serviço 
> - Instalação de drivers ou objetivos de serviço OPOS
> - Instalação de extensões de ponto de serviço
> - Atualizar para o sistema operacional Windows

## <a name="method-3-snapshot-of-devices"></a>Método 3: Instantâneo de dispositivos

Em alguns cenários, talvez seja preciso criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário.  Nessas situações, você pode enumerar um instantâneo de dispositivos que estão conectados ou emparelhados com o sistema usando [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Essa método manterá qualquer resultado até que toda a enumeração seja concluída.

> [!TIP]
> É recomendável usar o método **GetDeviceSelector** com o parâmetro **PosConnectionTypes** ao usar **FindAllAsync** para limitar sua consulta ao tipo de conexão desejado.  Conexões de rede e Bluetooth podem atrasar os resultados, pois as enumerações devem ser concluída antes de resultados de **FindAllAsync** são retornados.

> [!CAUTION] 
> **FindAllAsync** retorna uma matriz de dispositivos.  A ordem dessa matriz pode alterar a cada sessão, portanto, não é recomendado depender em uma ordem específica usando um índice codificada na matriz.  Use as propriedades de [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para filtrar os resultados ou fornecer uma interface do usuário para o usuário serem escolhidas.

Este exemplo usa o seletor definido acima para tirar um instantâneo de dispositivos usando **FindAllAsync** e enumera por meio de cada um dos itens retornados pela coleção e grava o nome do dispositivo e a identificação na saída de depuração. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Ao trabalhar com APIs [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), frequentemente será necessário usar objetos [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obter informações sobre um dispositivo específico. Por exemplo, a propriedade [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) pode ser usada para recuperar e reutilizar o mesmo dispositivo se ele estiver disponível em uma sessão futura e a propriedade [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) pode ser usada para fins de exibição em seu aplicativo.  Consulte a página de referência [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para saber mais sobre as propriedades adicionais disponíveis.

## <a name="method-4-enumerate-and-watch"></a>Método 4: Enumerar e inspecionar

Um método mais avançado e flexível de enumeração de dispositivos é criar um [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Um inspetor de dispositivos enumera dispositivos dinamicamente, portanto, o aplicativo recebe notificações se dispositivos são adicionados, removidos ou modificados após a conclusão da enumeração inicial.  Um **DeviceWatcher** permitirá que você detecte quando um dispositivo conectado à rede fica online, um dispositivo Bluetooth estiver no intervalo, bem como se um dispositivo conectado localmente é desconectado para você poder tomar a ação apropriada no seu aplicativo.

Este exemplo usa o seletor definido acima para criar um **DeviceWatcher** , bem como define manipuladores de eventos para as notificações [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [removidos](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)e [atualizado](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) . É necessário preencher os detalhes das ações que você deseja implementar a cada notificação.

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
> Consulte [enumerar e inspecionar dispositivos]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) para obter mais detalhes sobre o uso de um **DeviceWatcher**.

## <a name="see-also"></a>Consulte também
* [Introdução ao Ponto de Serviço](pos-basics.md)
* [Classe DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Classe PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [Enumeração PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Classe BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Classe DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]