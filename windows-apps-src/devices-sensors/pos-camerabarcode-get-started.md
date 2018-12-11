---
title: Introdução ao scanner de código de barras da câmera
description: Aprendendo a usar o scanner de código de barras da câmera
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: b49ba463e39d09b915ce3925f94ae7d9f11a9a47
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8829063"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Introdução ao scanner de código de barras da câmera
## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Etapa 1: adicionar declarações de funcionalidades ao manifesto do seu aplicativo
1. No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2. Selecione a guia **Recursos**
3. Marque as caixas de seleção de **Webcam** e **PointOfService** 

>[!NOTE] 
> A funcionalidade **Webcam** é necessária para que decodificador do software receba quadros da câmera para decodificar e fornecer uma prévia do seu aplicativo.

## <a name="step-2-add-using-directives"></a>Etapa 2: adicionar usando diretivas

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```
## <a name="step-3-define-your-device-selector"></a>Etapa 3: definir o seletor do dispositivo

### **<a name="option-a-find-all-barcode-scanners"></a>Opção A: encontrar todos os scanners de código de barras**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();       
```

### **<a name="option-b-scoping-device-selector-to-connection-type"></a>Opção B: definir o escopo do seletor do dispositivo para o tipo de conexão**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-barcode-scanners"></a>Step 4: enumerar scanners de código de barras
Se não espera que a lista de dispositivos mude ao longo do tempo de vida do aplicativo, você pode enumerar um instantâneo só uma vez com *FindAllAsync*, mas, se acredita que a lista de scanners de código de barras pode mudar ao longo do tempo de vida do aplicativo, você deve usar um *DeviceWatcher* em vez disso.  

> [!Important] 
> Usar GetDefaultAsync para enumerar dispositivos PointOfService pode resultar em comportamento inconsistente, já que isso retorna o primeiro dispositivo encontrado na classe e pode variar de sessão para sessão.

### **<a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>Opção A: enumerar um instantâneo dos scanners de código de barras**
```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Consulte [*Enumerar um instantâneo de dispositivos*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) para obter mais informações sobre como usar *FindAllAsync*.

### **<a name="option-b-enumerate-and-watch-for-changes-in-available-barcode-scanners"></a>Opção B: enumerar e inspecionar alterações nos scanners de código de barras disponíveis**
```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);

// TODO: Add Event Listeners and Handlers
```
> [!TIP]
> Consulte [*Enumerar e inspecionar alterações de dispositivo*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) e [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) para obter mais informações.

## <a name="step-5-identify-camera-barcode-scanners"></a>Etapa 5: identificar scanners de código de barras da câmera
Um scanner de código de barras da câmera é criado dinamicamente, já que o Windows emparelha a(s) câmera(s) conectada(s) ao computador com um decodificador de software.  Cada câmera - o emparelhamento do decodificador é um scanner de código de barras totalmente funcional.

Para cada scanner de código de barras na coleção de dispositivos resultante, você pode determinar quais serão os scanners de código de barras da câmera e os scanners de código de barras físicos verificando a propriedade [*BarcodeScanner.VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId).  Uma non-NULL VideoDeviceID indicará que o objeto de scanner de código de barras da sua coleção de dispositivos é um scanner de código de barras da câmera.  Se você tiver mais de um scanner de código de barras de câmera, você pode querer criar uma coleção separada que exclui scanners de código de barras físicos. 

Os scanners de código de barras da câmera usando o decodificador fornecido com o Windows aparecerá como: 

> Microsoft BarcodeScanner (*nome da sua câmera aqui*)

Se a câmera está integrada ao chassi do computador, o nome pode diferenciar entre *frontal* e *traseira* se houver mais de uma câmera.  No futuro, os decodificadores de software adicionais podem estar disponíveis e realizar um esquema de nomenclatura diferente.

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Etapa 6: reivindicar o scanner de código de barras de câmera 
Use [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) para ter o uso exclusivo do scanner de código de barras da câmera.

## <a name="step-7-system-provided-preview"></a>Etapa 7: visualização fornecida do sistema
A visualização da câmera é necessária para que o usuário mire a câmera em códigos de barras com sucesso.  O Windows fornece uma visualização de câmera simples que iniciará um diálogo que habilita o controle básico do scanner de código de barras da câmera.  Basta chamar [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) para abrir o diálogo e [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) para abri-lo quando finalizado.

> [!TIP]
> Consulte [Visualização da hospedagem](pos-camerabarcode-hosting-preview.md) para hospedar a visualização do scanner de código de barras da câmera no aplicativo.

## <a name="step-8-initiate-scan"></a>Etapa 8: Verificação de iniciar 
Você pode iniciar o processo de verificação chamando [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).  
Dependendo do valor de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) o scanner pode verificar apenas um código de barras e depois parar ou verificar continuamente até que você chame [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Defina o valor desejado de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar o comportamento do scanner quando um código de barras é decodificado.

| Value | Description |
| ----- | ----------- |
| True   | Verificar apenas um código de barras e depois parar |
| False  | Verificar continuamente o códigos de barras sem parar |