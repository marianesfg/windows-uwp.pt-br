---
author: TerryWarwick
title: Trabalhar com simbologias do scanner de código de barras
description: Este artigo contém informações sobre simbologias do scanner de código de barras.
ms.author: jken
ms.date: 05/3/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: f6e03d62316a1b842330f39ac958e4471a895815
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874517"
---
# <a name="working-with-symbologies"></a>Trabalhando com simbologias
Uma [simbologia de código de barras](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) é o mapeamento de dados para um formato específico de código de barras. Algumas simbologias comuns incluem UPC, código 128, código QR etc. As APIs de scanner de código de barras da UWP permitem que um aplicativo controle como o scanner processará essas simbologias sem a necessidade de configurar manualmente o scanner. 

## <a name="determine-which-symbologies-are-supported"></a>Determinar quais simbologias são compatíveis 
Desde que seu aplicativo possa ser usado com modelos diferentes de scanner de código de barras de vários fabricantes, é possível consultar o scanner para determinar a lista de simbologias com as quais ele é compatível.  Isso pode ser útil se o aplicativo precisar de uma simbologia específica que talvez não seja compatível com todos os scanners ou se você precisar habilitar simbologias que foram desabilitadas no scanner manual ou programaticamente.
Quando você tiver um objeto [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) usando [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), chame [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obter uma lista de simbologias compatíveis com o dispositivo.

## <a name="determine-if-a-specific-symbology-is-supported"></a>Determinar se uma simbologia específica é compatível
Para determinar se o scanner é compatível com uma simbologia específica, você pode chamar [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

## <a name="changing-which-symbologies-are-recognized"></a>Mudando quais simbologias serão reconhecidas
Em alguns casos, você pode usar um subconjunto de simbologias compatíveis com o scanner de código de barras.  Isso é especialmente útil para bloquear simbologias que você não pretende usar no aplicativo. Por exemplo, para garantir que um usuário verifique o código de barras direito, você pode restringir a verificação a UPC ou EAN ao adquirir SKUs de item e restringir a verificação ao código 128 ao adquirir números de série.
Quando já souber quais simbologias são compatíveis com o scanner, você pode definir as simbologias que você quer que ele reconheça.  Isso pode ser feito após o estabelecimento de um objeto [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) usando [ClaimScannerAsyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Você pode chamar [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para habilitar um conjunto específico de simbologias enquanto aquelas omitidas na lista são desabilitadas.

## <a name="restricting-scan-data-by-data-length"></a>Restringindo dados de verificação por tamanho de dados
Algumas simbologias possuem tamanho variável, como código 39 ou código 128.  Os códigos de barras dessa simbologia podem estar localizados perto de cada uma contendo dados diferentes, geralmente com tamanho específico. Definir o tamanho específico dos dados de que você precisa pode evitar verificações inválidas.

| Method    | Description |
| :-------- | :---------- |
| [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | Permite que você configure uma variedade de tamanhos desejados dos dados decodificados e como o scanner manipula o dígito de verificação. |
| [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Permite que você recupere as configurações atuais de tamanho e dígito de verificação. |

> [!Important] 
> Certifique-se de que o scanner de código de barras permite o uso de atributos de simbologia verificando primeiro as seguintes propriedades. 
> - [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [ICheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)
