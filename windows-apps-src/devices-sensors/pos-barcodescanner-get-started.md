---
author: TerryWarwick
title: Introdução ao scanner de código de barras
description: Saiba como interagir com um scanner de código de barras de um aplicativo universal do Windows
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: ed0fa79f5bbdfdaf8ca1f3273fa8d741f17efe1d
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833042"
---
# <a name="getting-started-with-barcode-scanners"></a>Introdução ao(s) scanner(s) de código de barras

Saiba como interagir com um scanner de código de barras de um aplicativo universal do Windows.  Este tópico fornece informações sobre a funcionalidade específica do scanner de código de barras.

## <a name="configuring-your-barcode-scanner"></a>Configurando o scanner de código de barras
O scanner de código de barras pode ser configurado em vários modos diferentes.  É importante que o scanner de código de barras seja configurado corretamente para o aplicativo desejado.  Muitos scanners de código de barras podem ser configurados no modo **keyboard wedge** que faz com que o scanner de código de barras apareça como um teclado no Windows.  Isso permite que você verifique os códigos de barras nos aplicativos que não reconhecem scanners de código de barras, como o Bloco de notas do Windows.  Quando você verifica um código de barras nesse modo, os dados decodificados do scanner de código de barras são inseridos no ponto de inserção como se você tivesse digitado os dados usando o teclado.  Se você deseja ter mais controle sobre o scanner de código de barras do aplicativo UWP, você precisará configurá-lo em um modo que não seja o keyboard wedge.

### <a name="usb-barcode-scanner"></a>Scanner de código de barras USB
Um scanner de código de barras conectado por USB deve ser configurado no modo **HID POS Scanner** para funcionar com o driver do scanner de código de barras incluído no Windows. Esse driver é uma implementação da especificação **HID Point of Sale Usage Tables** publicada em [**USB-HID**](http://www.usb.org/developers/hidpage/).  Consulte a documentação do scanner de código de barras ou contate o fabricante do scanner de código de barras para obter instruções para habilitar o modo **HID POS Scanner**.  Uma vez configurado como **HID POS Scanner**, seu scanner de código de barras aparecerá no Gerenciador de Dispositivos no nó **POS Barcode Scanner** como **POS HID Barcode scanner**.
O fabricante do scanner de código de barras também pode ter um driver específico do fornecedor que é compatível com as APIs do scanner de código de barras da UWP usando um modo diferente de **HID POS Scanner**.  Se já tiver instalado um driver fornecido pelo fabricante compatível com as APIs do scanner de código de barras da UWP, você poderá ver um dispositivo específico do fornecedor listado em **POS Barcode Scanner** no Gerenciador de Dispositivos.

### <a name="bluetooth-barcode-scanner"></a>Scanner de código de barras Bluetooth
Um scanner conectado por Bluetooth deve ser configurado no modo **Serial Port Protocol - Simple Serial Interface (SPP-SSI)** para funcionar com as APIs de scanner de código de barras da UWP.  Consulte a documentação do scanner de código de barras ou contate o fabricante do scanner de código de barras para obter instruções para habilitar o **SPP-SSI mode**.  
Antes de poder usar o scanner de código de barras Bluetooth, você deve emparelhá-lo usando Configurações - Dispositivos - Bluetooth e outros dispositivos - Adicionar Bluetooth ou outro dispositivo.  
Você pode iniciar e controlar o evento de emparelhamento usando o namespace **Windows.Devices.Enumeration**.  Consulte [**Emparelhar dispositivos**](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices) para obter mais informações.

## <a name="working-with-symbologies"></a>Trabalhando com simbologias
Uma [**simbologia de código de barras**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) é o mapeamento de dados para um formato específico de código de barras. Algumas simbologias comuns incluem UPC, código 128, código QR etc. As APIs de scanner de código de barras da UWP permitem que um aplicativo controle como o scanner processará essas simbologias sem a necessidade de configurar manualmente o scanner. 

### <a name="determine-which-symbologies-are-supported"></a>Determinar quais simbologias são compatíveis 
Desde que seu aplicativo possa ser usado com modelos diferentes de scanner de código de barras de vários fabricantes, é possível consultar o scanner para determinar a lista de simbologias com as quais ele é compatível.  Isso pode ser útil se o aplicativo precisar de uma simbologia específica que talvez não seja compatível com todos os scanners ou se você precisar habilitar simbologias que foram desabilitadas no scanner manual ou programaticamente.
Uma vez que você tenha um objeto [**BarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) usando [**BarcodeScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), chame [**GetSupportedSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obter uma lista de simbologias compatíveis com o dispositivo.

### <a name="determine-if-a-specific-symbology-is-supported"></a>Determinar se uma simbologia específica é compatível
Para determinar se o scanner é compatível com uma simbologia específica, você pode chamar [**IsSymbologySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

### <a name="changing-which-symbologies-are-recognized"></a>Mudando quais simbologias serão reconhecidas
Em alguns casos, você pode usar um subconjunto de simbologias compatíveis com o scanner de código de barras.  Isso é especialmente útil para bloquear simbologias que você não pretende usar no aplicativo. Por exemplo, para garantir que um usuário verifique o código de barras direito, você pode restringir a verificação a UPC ou EAN ao adquirir SKUs de item e restringir a verificação ao código 128 ao adquirir números de série.
Quando já souber quais simbologias são compatíveis com o scanner, você pode definir as simbologias que você quer que ele reconheça.  Isso pode ser feito após o estabelecimento de um objeto [**ClaimedBarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) usando [**ClaimScannerAsyc**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Você pode chamar [**SetActiveSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para habilitar um conjunto específico de simbologias enquanto aquelas omitidas na lista são desabilitadas.

### <a name="restricting-scan-data-by-data-length"></a>Restringindo dados de verificação por tamanho de dados
Algumas simbologias possuem tamanho variável, como código 39 ou código 128.  Os códigos de barras dessa simbologia podem estar localizados perto de cada uma contendo dados diferentes, geralmente com tamanho específico. Definir o tamanho específico dos dados de que você precisa pode evitar verificações inválidas.

| Method    | Description |
| :-------- | :---------- |
| [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | Permite que você configure uma variedade de tamanhos desejados dos dados decodificados e como o scanner manipula o dígito de verificação. |
| [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Permite que você recupere as configurações atuais de tamanho e dígito de verificação. |

> [!Important] 
> Certifique-se de que o scanner de código de barras permite o uso de atributos de simbologia verificando primeiro as seguintes propriedades: [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) ou [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Permite que você recupere as configurações atuais de tamanho e dígito de verificação. :
> - [**IsDecodeLengthSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [**ICheckDigitTransmissionSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [**IsCheckDigitValidationSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

## <a name="using-software-trigger-with-barcode-scanners"></a>Usando o gatilho de software com scanners de código de barras
### <a name="initiate-scan-from-software"></a>Iniciar verificação de software
Pode ser útil controlar o ato de verificação de software se você estiver usando um scanner de código de barras no modo de apresentação ou se o scanner não tiver um gatilho físico, como um scanner de código de barras baseado em câmera. Você pode iniciar o processo de verificação chamando [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).  
Dependendo do valor de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) o scanner pode verificar apenas um código de barras e depois parar ou verificar continuamente até que você chame [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Defina o valor desejado de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar o comportamento do scanner quando um código de barras é decodificado.

| Value | Description |
| ----- | ----------- |
| True   | Verificar apenas um código de barras e depois parar |
| False  | Verificar continuamente o códigos de barras sem parar |


> [!Important]
> Certifique-se de que o scanner de código de barras permite o uso de gatilho de software verificando primeiro a propriedade [**IsSoftwareTriggerSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).
