---
title: Obter e entender os dados de código de barras
description: Saiba como obter e interpretar os dados de código de barras que você examinar.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605961"
---
# <a name="obtain-and-understand-barcode-data"></a>Obter e entender os dados de código de barras

Depois que você configurou o scanner de código de barras, certamente precisa de uma maneira de compreender os dados que você examinar. Quando você digitaliza um código de barras, o [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) é gerado. O [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) deve assinar esse evento. O **DataReceived** evento passa um [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) objeto, que você pode usar para acessar os dados de código de barras.

## <a name="subscribe-to-the-datareceived-event"></a>Assinar o evento DataReceived

Uma vez que um **ClaimedBarcodeScanner**, fazer com que ele assine os **DataReceived** eventos:

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

O manipulador de eventos será passado a **ClaimedBarcodeScanner** e uma **BarcodeScannerDataReceivedEventArgs** objeto. Você pode acessar os dados de código de barras por meio desse objeto [relatório](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) propriedade, que é do tipo [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obter os dados

Uma vez que o **BarcodeScannerReport**, você pode acessar e analisar os dados de código de barras. **BarcodeScannerReport** tem três propriedades:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): Os dados brutos, completa do código de barras.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): O rótulo do código de barras decodificada, que não inclui o cabeçalho, a soma de verificação e outras informações diversas.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): O tipo de rótulo de código de barras decodificada. Os valores possíveis são definidos na [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) classe.

Se você quiser acessar qualquer um **ScanDataLabel** ou **ScanDataType**, você deve primeiro definir [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) para **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obter o tipo de dados de verificação

Obter o tipo de rótulo de código de barras decodificada é bastante trivial&mdash;simplesmente chamamos [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) na **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obter o rótulo de dados de verificação

Para obter o rótulo de código de barras decodificada, há algumas coisas que você precisa estar atento. Somente determinados tipos de dados contêm texto codificado, portanto, você deve primeiro verificar se o Simbologia pode ser convertido em uma cadeia de caracteres e, em seguida, converter o buffer que recebemos de **ScanDataLabel** para uma cadeia de caracteres codificado em UTF-8.

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

Para obter os dados brutos, completos do código de barras, nós simplesmente convertemos o buffer que recebemos de **ScanData** em uma cadeia de caracteres.

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

Esses dados são, em geral, no formato conforme fornecido usando o scanner. Informações de cabeçalho e rodapé de mensagem são removidos, no entanto, uma vez que eles não contêm informações úteis para um aplicativo e provavelmente serão específicos do scanner.

Informações de cabeçalho comum são um caractere de prefixo (por exemplo, um caractere STX). Informações comuns de marcador são um caractere terminador (como um caractere de ETX CR) e um caractere de seleção de bloco se gerado pelo scanner.

Essa propriedade deve incluir um caractere de Simbologia se um é retornado pelo scanner (por exemplo, um **um** para UPC-A). Ele também deve incluir os dígitos de verificação se elas estiverem presentes no rótulo e retornado pelo scanner. (Observe que caracteres Simbologia e dígitos de verificação podem, ou podem não estar presentes, dependendo da configuração do scanner. O scanner será retorná-los se presente, mas não gerar ou calculá-los se eles estão ausentes.)

Alguns mercadorias talvez esteja marcada com um código de barras complementar. Esse código de barras normalmente é posicionado à direita do código de barras principal e consiste em um caracteres de dois ou cinco adicionais de informações. Se o scanner lê mercadorias que contém códigos de barras principais e complementares, os caracteres complementares são acrescentados aos caracteres principais, e o resultado é entregue para o aplicativo como um rótulo. (Observe que um scanner pode dar suporte a uma configuração que habilita ou desabilita a leitura dos códigos complementares).

Alguns mercadorias talvez esteja marcada com vários rótulos, às vezes chamados de *rótulos multisymbol* ou *hierárquico rótulos*. Esses códigos de barras normalmente são organizados verticalmente e podem ser a Simbologia iguais ou diferente. Se o scanner lê mercadorias que contém vários rótulos, cada código de barras é entregue para o aplicativo como um rótulo separado. Isso é necessário devido à falta de padronização desses tipos de código de barras atual. Não é possível determinar todas as variações com base nos dados de código de barras individuais. Portanto, o aplicativo será necessário determinar quando um código de barras vários rótulo foi lido com base nos dados retornados. (Observe que um scanner pode, ou pode não oferecer suporte a leitura dos vários rótulos).

Esse valor é definido antes de uma **DataReceived** evento sendo gerado para o aplicativo.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Consulte também
* [Scanner de código de barras](pos-barcodescanner.md)
* [Classe ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Classe BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Classe BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Classe BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
