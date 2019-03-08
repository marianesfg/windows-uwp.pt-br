---
title: Enumerar dispositivos de PointOfService
description: Saiba como enumerar dispositivos de PointOfService
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 27d25864941b9d73c9b12e6329eab79fac1b15bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620901"
---
# <a name="enumerating-point-of-service-devices"></a>Enumerar dispositivos de Ponto de Serviço
Nesta seção, você aprenderá como [definem um seletor de dispositivo](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) que é usado para consultar dispositivos disponíveis para o sistema e use esse seletor para enumerar os dispositivos de ponto de serviço usando um dos seguintes métodos:

**Método 1:** [Usar um seletor de dispositivo](#method-1-use-a-device-picker)
<br/>
Exibir um seletor de dispositivo da interface do usuário e peça que o usuário escolha um dispositivo conectado. Esse método manipula atualizando a lista quando os dispositivos são conectados e removidos e é mais simples e mais seguro do que outros métodos.

**Método 2:** [Obter o primeiro dispositivo disponível](#method-2-get-first-available-device)<br />Use [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) para acessar o primeiro dispositivo disponível em uma classe de dispositivo de ponto de serviço específica.

**Método 3:** [Instantâneo dos dispositivos](#method-3-snapshot-of-devices)<br />Enumere um instantâneo dos dispositivos de ponto de serviço que estão presentes no sistema em um determinado ponto no tempo. Isso é útil que você precisa criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) manterá os resultados até que toda a enumeração é concluída.

**Método 4:** [Enumerar e assistir](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) é um modelo de enumeração de mais eficiente e flexível que permite que você enumerar os dispositivos que estão atualmente presentes e também receberá notificações quando dispositivos são adicionados ou removidos do sistema.  Isso é útil quando você quiser manter uma lista atual de dispositivos em segundo plano para exibir na interface do usuário em vez de aguardar um instantâneo.

## <a name="define-a-device-selector"></a>Definir um seletor de dispositivo
Um seletor de dispositivo permitirá que você limite os dispositivos que está pesquisando ao enumerar dispositivos.  Isso permitirá que você apenas obter resultados relevantes e reduzir o tempo necessário para enumerar os dispositivos desejados.

Você pode usar o **GetDeviceSelector** método para o tipo de dispositivo que você está procurando para obter o seletor de dispositivo para esse tipo. Por exemplo, usando [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) fornecerá a você com um seletor para enumerar todas as [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) conectado ao sistema, incluindo as impressoras USB, Bluetooth POS e de rede.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

O **GetDeviceSelector** métodos para os tipos de dispositivo diferentes são:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

Usando um **GetDeviceSelector** método que usa um [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) valor como um parâmetro, você pode restringir seu seletor para enumerar local, rede ou dispositivos de POS anexado por Bluetooth, reduzindo o tempo necessário para conclusão da consulta.  O exemplo a seguir mostra um uso desse método para definir um seletor que dá suporte a apenas localmente anexado impressoras de POS.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Ver [criar um seletor de dispositivo](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) para a criação de mais avançado cadeias de caracteres do seletor.

## <a name="method-1-use-a-device-picker"></a>Método 1: Usar um seletor de dispositivo

O [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) classe permite que você exibir um submenu de seletor que contém uma lista de dispositivos para o usuário à sua escolha. Você pode usar o [filtro](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) propriedade para escolher quais tipos de dispositivos para mostrar no seletor de. Essa propriedade é do tipo [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Você pode adicionar tipos de dispositivo para filtrar usando o [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) ou [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) propriedade.

Quando você estiver pronto para mostrar o seletor de dispositivo, você pode chamar o [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) método, que mostrará o seletor de interface do usuário e retornar o dispositivo selecionado. Você precisará especificar uma [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) determinará onde o submenu é exibido. Esse método retornará um [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) do objeto, portanto, para usá-lo com o ponto de serviço de APIs, você precisará usar o **FromIdAsync** método para a classe de dispositivo específico que você deseja. Você passa o [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) propriedade como o método *deviceId* parâmetro e obtenha uma instância da classe de dispositivo como o valor retornado.

O trecho de código a seguir cria uma **DevicePicker**, adiciona um filtro de scanner de código de barras a ele, tem o usuário escolher um dispositivo e, em seguida, cria um **BarcodeScanner** objeto com base na ID do dispositivo:

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

A maneira mais simples para obter um dispositivo de ponto de serviço é usar **GetDefaultAsync** para obter o primeiro dispositivo disponível dentro de uma classe de dispositivo de ponto de serviço. 

O exemplo a seguir ilustra o uso de [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) para [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). O padrão de codificação é semelhante para todas as classes de dispositivo de ponto de serviço.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** deve ser usada com cuidado, pois ele pode retornar um dispositivo diferente de uma sessão para o próximo. Muitos eventos podem influenciar essa enumeração resultando em um outro dispositivo disponível primeiro, incluindo: 
> - Mudança de câmeras conectadas ao computador 
> - Alterar ponto de entrada serviço dispositivos conectados ao computador
> - Alterar em rede dispositivos conectados ao ponto de serviço disponíveis na sua rede
> - Alterar em dispositivos Bluetooth Point of Service dentro do alcance do seu computador 
> - Alterações na configuração de ponto de serviço 
> - Instalação de drivers ou objetivos de serviço OPOS
> - Instalação de extensões de ponto de serviço
> - Atualizar para o sistema operacional Windows

## <a name="method-3-snapshot-of-devices"></a>Método 3: Instantâneo dos dispositivos

Em alguns cenários, talvez seja preciso criar sua própria interface do usuário ou enumerar dispositivos sem exibir uma interface para o usuário.  Nessas situações, você pode enumerar um instantâneo dos dispositivos que estão atualmente conectada ou emparelhada com o sistema usando [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Essa método manterá qualquer resultado até que toda a enumeração seja concluída.

> [!TIP]
> É recomendável usar o **GetDeviceSelector** método com o **PosConnectionTypes** parâmetro ao usar **FindAllAsync** para limitar sua consulta para o tipo de conexão desejado.  Conexões de rede e Bluetooth podem atrasar os resultados como suas enumerações devem ser concluída antes **FindAllAsync** os resultados são retornados.

> [!CAUTION] 
> **FindAllAsync** retorna uma matriz de dispositivos.  A ordem dessa matriz pode alterar a cada sessão, portanto, não é recomendado depender em uma ordem específica usando um índice codificada na matriz.  Use [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) propriedades para filtrar os resultados ou fornecer uma interface do usuário para o usuário à sua escolha.

Este exemplo usa o seletor definido acima para criar um instantâneo de dispositivos que usam **FindAllAsync** , em seguida, enumera por meio de cada um dos itens retornados pela coleção e grava o nome do dispositivo e a ID para a saída de depuração. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Ao trabalhar com o [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) APIs, você frequentemente precisará usar [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) objetos para obter informações sobre um dispositivo específico. Por exemplo, o [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) propriedade pode ser usada para recuperar e reutilizar o mesmo dispositivo se ele estiver disponível em uma sessão futura e o [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) propriedade pode ser usada para exibição finalidades em seu aplicativo.  Consulte a [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) página de referência para obter informações sobre as propriedades adicionais disponíveis.

## <a name="method-4-enumerate-and-watch"></a>Método 4: Enumerar e assistir

É a criação de um método mais eficiente e flexível de enumeração de dispositivos de uma [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Um inspetor de dispositivos enumera dispositivos dinamicamente, portanto, o aplicativo recebe notificações se dispositivos são adicionados, removidos ou modificados após a conclusão da enumeração inicial.  Um **DeviceWatcher** permitirá que você detecte quando um conectado à rede dispositivo estiver online, um dispositivo Bluetooth está no intervalo, bem como se um dispositivo conectado localmente é desconectado, para que você pode tomar a ação apropriada no seu aplicativo.

Este exemplo usa o seletor definido acima para criar uma **DeviceWatcher** , bem como define manipuladores de eventos para o [adicionado](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [removido](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed), e [atualizado ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) notificações. É necessário preencher os detalhes das ações que você deseja implementar a cada notificação.

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
> Ver [dispositivos Enumerate e assista]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) para obter mais detalhes sobre o uso de uma **DeviceWatcher**.

## <a name="see-also"></a>Consulte também
* [Introdução ao ponto de serviço](pos-basics.md)
* [Classe DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Classe PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [Enumeração PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Classe BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Classe DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]