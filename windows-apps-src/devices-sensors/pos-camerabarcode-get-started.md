---
title: Introdução ao scanner de código de barras da câmera
description: Aprendendo a usar o scanner de código de barras da câmera
ms.date: 09/02/2019
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: b35ff6b183a6344fbc8da6b44a6cb81ea695a1c9
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243289"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Introdução ao scanner de código de barras da câmera

Os trechos de código usados aqui são apenas para fins de demonstração. Para obter um exemplo funcional, consulte o [exemplo de scanner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner).

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Etapa 1: Adicionar declarações de funcionalidade ao manifesto do aplicativo

1. No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2. Selecione a guia **Recursos**
3. Marque as caixas de seleção de **Webcam** e **PointOfService**

>[!NOTE]
> A funcionalidade **Webcam** é necessária para que decodificador do software receba quadros da câmera para decodificar e fornecer uma prévia do seu aplicativo.

## <a name="step-2-add-using-directives"></a>Etapa 2: Adicionar diretivas using

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>Etapa 3: Definir o seletor de dispositivos

### <a name="option-a-find-all-barcode-scanners"></a>**Opção A: Localizar todos os scanners de código de barras**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**Opção B: Seletor do dispositivo de escopo para o tipo de conexão**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>Etapa 4: Enumerar todos os scanners de código de barras

Se você não espera que a lista de dispositivos seja alterada durante o tempo de vida do seu aplicativo, você pode enumerar um instantâneo apenas uma vez com [DeviceInformation. FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync), mas se acreditar que a lista de scanners de código de barras poderá mudar durante o tempo de vida do seu no lugar do aplicativo, você deve usar um [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) .  

> [!Important]
> Usar GetDefaultAsync para enumerar dispositivos PointOfService pode resultar em comportamento inconsistente, já que isso retorna o primeiro dispositivo encontrado na classe e pode variar de sessão para sessão.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**Opção A: Enumerar um instantâneo de scanners de código de barras**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Consulte [*Enumerar um instantâneo de dispositivos*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) para obter mais informações sobre como usar *FindAllAsync*.

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**Opção B: Enumerar scanners de código de barras disponíveis e observar alterações nos scanners disponíveis**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> Consulte [*Enumerar e inspecionar alterações de dispositivo*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) e [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) para obter mais informações.

## <a name="step-5-identify-camera-barcode-scanners"></a>Etapa 5: Identificar scanners de código de barras da câmera

Um scanner de código de barras da câmera é criado dinamicamente, já que o Windows emparelha a(s) câmera(s) conectada(s) ao computador com um decodificador de software.  Cada câmera - o emparelhamento do decodificador é um scanner de código de barras totalmente funcional.

Para cada scanner de código de barras na coleção de dispositivos resultante, você pode diferenciar os scanners de código de barras da câmera e os scanners de código de barras físicos, verificando a propriedade [*BarcodeScanner. VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) .  Um VideoDeviceID não nulo indica que o objeto de scanner de código de barras da sua coleção de dispositivos é um scanner de código de barras da câmera.  Se você tiver mais de um scanner de código de barras de câmera, talvez queira criar uma coleção separada que exclua scanners de código de barras físicos.

Scanners de código de barras da câmera usando o decodificador fornecido com o Windows são identificados como:

> Microsoft BarcodeScanner (*nome da sua câmera aqui*)

Se você tiver mais de uma câmera e elas estiverem incorporadas ao chassi do seu computador, o nome poderá ser diferenciado entre as câmeras dianteiras e *traseiras* .

> [!NOTE]
> No futuro, os decodificadores de software adicionais com esquemas de nomenclatura diferentes podem ser liberados.

Quando o DeviceWatcher é iniciado (etapa 4), ele é enumerado por meio de cada dispositivo conectado. Aqui, adicionamos os scanners disponíveis a uma coleção de scanner de código de barras e associamos a coleção a uma ListBox.

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

Quando o SelectedIndex da caixa de listagem é alterado (o primeiro item é selecionado por padrão no trecho anterior), consultamos as informações do dispositivo.

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Etapa 6: Reivindicar o scanner de código de barras da câmera

Use [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) para ter o uso exclusivo do scanner de código de barras da câmera.

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>Etapa 7: Versão prévia fornecida pelo sistema

A visualização da câmera é necessária para que o usuário mire a câmera em códigos de barras com sucesso.  O Windows fornece uma visualização de câmera simples que inicia uma caixa de diálogo para o controle básico do scanner de código de barras da câmera.  Basta chamar [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) para abrir o diálogo e [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) para abri-lo quando finalizado.

> [!TIP]
> Consulte [Visualização da hospedagem](pos-camerabarcode-hosting-preview.md) para hospedar a visualização do scanner de código de barras da câmera no aplicativo.

## <a name="step-8-initiate-scan"></a>Etapa 8: Iniciar verificação

Você pode iniciar o processo de verificação chamando [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Dependendo do valor de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) , o verificador poderá examinar apenas um código de barras e, em seguida, parar ou verificar continuamente até que você chame [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Defina o valor desejado de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar o comportamento do scanner quando um código de barras é decodificado.

| Valor | Descrição |
| ----- | ----------- |
| True   | Verificar apenas um código de barras e depois parar |
| False  | Verificar continuamente o códigos de barras sem parar |

## <a name="see-also"></a>Consulte também

### <a name="samples"></a>Exemplos

- [Exemplo de scanner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
