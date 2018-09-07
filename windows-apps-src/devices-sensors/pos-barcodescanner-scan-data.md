---
author: eliotcowley
title: Obter e compreender os dados de código de barras
description: Saiba como obter e interpretar os dados de código de barras que você digitaliza.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3665613"
---
# <a name="obtain-and-understand-barcode-data"></a>Obter e compreender os dados de código de barras

Depois que você configurou o scanner de código de barras, você precisa obviamente uma maneira de entender os dados que você digitaliza. Quando você verifica um código de barras, o evento [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) é acionado. O [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) deve assinar esse evento. O evento **DataReceived** passa um objeto [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) , que você pode usar para acessar os dados de código de barras.

## <a name="subscribe-to-the-datareceived-event"></a>Inscrever-se para o evento DataReceived

Quando você tiver um **ClaimedBarcodeScanner**, ele tem a assinar o evento **DataReceived** :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

O manipulador de eventos será passado a **ClaimedBarcodeScanner** e um objeto **BarcodeScannerDataReceivedEventArgs** . Você pode acessar os dados de código de barras por meio da propriedade de [relatório](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) do objeto, que é do tipo [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obter os dados

Quando você tiver o **BarcodeScannerReport**, você pode acessar e analisar os dados de código de barras. **BarcodeScannerReport** tem três propriedades:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): os dados de código de barras completo, bruto.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): O rótulo de código de barras decodificada, que não inclui o cabeçalho, soma de verificação e outras informações de diversas.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): O tipo de rótulo de código de barras decodificada. Os valores possíveis são definidos na classe [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) .

Se você quiser acesse **ScanDataLabel** ou **ScanDataType**, primeiro você deve definir [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) como **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obter o tipo de dados de verificação

Obter o tipo de rótulo de código de barras decodificada é bastante simples&mdash;simplesmente chamamos [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) em **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obter o rótulo de dados de verificação

Para obter o rótulo de código de barras decodificada, há algumas coisas que você precisa estar ciente. Somente determinados tipos de dados contêm texto codificado, portanto, você deve verificar primeiro se o Simbologia pode ser convertido em uma cadeia de caracteres e, em seguida, converter o buffer que obtemos do **ScanDataLabel** para uma cadeia de caracteres codificada UTF-8.

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>Obter os dados brutos de verificação

Para obter completo, dados brutos do código de barras, nós simplesmente convertemos o buffer que obtemos do **ScanData** em uma cadeia de caracteres.

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

Esses dados são, em geral, no formato conforme entregue do scanner. Informações de cabeçalho e trailer de mensagem é removido, no entanto, pois eles não contêm informações úteis para um aplicativo e devem ser específicas do scanner.

Informações de cabeçalho comum são um caractere de prefixo (por exemplo, um caractere STX). Informações comuns de trailer são um caractere terminador (por exemplo, um caractere ETX ou CR) e um caractere de seleção de bloco se um é gerado pelo scanner.

Essa propriedade deve incluir um caractere de Simbologia caso um seja retornado pelo scanner ( **por exemplo, de um para** um UPC). Ele também deve incluir dígitos de verificação se eles estiverem presentes no rótulo e retornados pelo scanner. (Observe que caracteres Simbologia e dígitos de verificação podem ou não ser presentes, dependendo da configuração de scanner. O scanner retornará-los a se apresentar, mas não gerar ou calcule se eles estiverem ausentes.)

Alguns mercadoria pode ser marcada com um código de barras complementar. Esse código de barras é geralmente colocado à direita do código de barras principal e consiste em um caracteres dois ou cinco adicionais de informações. Se o scanner lê mercadoria que contém códigos de barras principais e complementares, os caracteres complementares são acrescentados aos caracteres principais, e o resultado é entregue ao aplicativo como um rótulo. (Observe que um scanner pode oferecer suporte a uma configuração que habilita ou desabilita a leitura de códigos complementares).

Alguns mercadoria pode ser marcada com vários rótulos, às vezes chamados de *rótulos multisymbol* ou *rótulos hierárquicos*. Esses códigos de barras geralmente são organizados verticalmente e podem ser de Simbologia a mesmas ou diferente. Se o scanner lê mercadoria que contém vários rótulos, cada código de barras é entregue ao aplicativo como um rótulo separado. Isso é necessário devido à falta de padronização desses tipos de código de barras atual. Um não é capaz de determinar todas as variações com base nos dados do código de barras individuais. Portanto, o aplicativo será necessário determinar quando um código de barras de rótulo vários foi lido com base nos dados retornados. (Observe que um scanner pode ou pode não oferecer suporte a leitura dos rótulos vários).

Esse valor é definido antes de um evento **DataReceived** sendo gerado para o aplicativo.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Veja também
* [Scanner de código de barras](pos-barcodescanner.md)
* [Classe ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Classe BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Classe BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Classe BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)