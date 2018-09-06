---
author: eliotcowley
title: Obter e compreender os dados do código de barras
description: Saiba como obter e interpretar os dados de código de barras que você digitalizar.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3374719"
---
# <a name="obtain-and-understand-barcode-data"></a>Obter e compreender os dados do código de barras

Depois de configurar o scanner de código de barras, claro precisa de um meio de compreender os dados que você digitalizar. Quando você digitaliza um código de barras, é gerado o evento [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) . O [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) deve se inscrever para este evento. O evento **DataReceived** passa um objeto [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) , que você pode usar para acessar os dados de código de barras.

## <a name="subscribe-to-the-datareceived-event"></a>Assinar o evento DataReceived

Depois que você tiver um **ClaimedBarcodeScanner**, que assina o evento **DataReceived** :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

O manipulador de eventos será passado a **ClaimedBarcodeScanner** e um objeto **BarcodeScannerDataReceivedEventArgs** . Você pode acessar os dados de código de barras por meio da propriedade [relatório](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) do objeto, que é do tipo [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obter os dados

Depois que você tiver o **BarcodeScannerReport**, você pode acessar e analisar os dados de código de barras. **BarcodeScannerReport** possui três propriedades:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): os dados de código de barras completo, raw.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): O rótulo decodificada do código de barras, que não inclui o cabeçalho, soma de verificação e outras informações variadas.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): O tipo de etiqueta de código de barras decodificada. Valores possíveis são definidos na classe [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) .

Se você desejar acessar **ScanDataLabel** ou **ScanDataType**, você precisa primeiro definir [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) como **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obter o tipo de dados de verificação

Obter o tipo de etiqueta de código de barras decodificada é bastante trivial&mdash;simplesmente chamamos [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) em **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obter o rótulo de dados da verificação

Para obter a etiqueta do código de barras decodificada, existem algumas coisas que você precisa estar ciente. Apenas certos tipos de dados contêm texto codificado, portanto, você deve primeiro verificar se a Simbologia pode ser convertido em uma sequência de caracteres e, em seguida, converter o buffer que obtemos do **ScanDataLabel** para uma sequência de caracteres codificada UTF-8.

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

### <a name="get-the-raw-scan-data"></a>Obter os dados de verificação bruta

Para obter o total, dados brutos do código de barras, nós simplesmente convertemos o buffer que obtemos do **ScanData** em uma cadeia de caracteres.

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

Esses dados são, em geral, no formato conforme fornecido com o scanner. Informações de cabeçalho e rodapé da mensagem são removidas, no entanto, uma vez que não contêm informações úteis para um aplicativo e devem ser específicos do scanner.

Informações de cabeçalho comuns são um caractere de prefixo (por exemplo, um caractere STX). Informações comuns de marcador são um caractere terminador (como um caractere ETX ou CR) e um caractere de seleção bloco se um for gerado pelo scanner.

Esta propriedade deve incluir um caractere de Simbologia se um for retornado pelo mecanismo de varredura ( **por exemplo, de um para** um UPC). Ele também deve incluir os dígitos de verificação se estiverem presentes no rótulo e retornado pelo mecanismo de varredura. (Observe que caracteres de Simbologia e dígitos de verificação talvez ou podem não estar presentes, dependendo da configuração do scanner. O mecanismo de varredura retornará-los a se apresentar, mas não gerar ou calculá-los se eles estiverem ausentes.)

Alguns produtos podem ser marcados com um código de barras complementar. Esse código de barras é normalmente colocado à direita do código de barras principal e consiste em um adicional dois ou cinco caracteres de informações. Se o scanner lê mercadorias que contém códigos de barras principais e complementares, os caracteres suplementares são acrescentados aos caracteres principais, e o resultado será enviado para o aplicativo como um rótulo. (Observe que um scanner pode suportar uma configuração que habilita ou desabilita a leitura de códigos suplementares).

Alguns produtos podem ser marcados com vários rótulos, às vezes chamados *multisymbol etiquetas* ou *rótulos hierárquicos*. Esses códigos de barras geralmente são organizados verticalmente e podem ser de Simbologia iguais ou diferente. Se o scanner lê mercadorias que contém vários rótulos, cada código de barras será enviado para o aplicativo como um rótulo separado. Isso é necessário devido à falta de padronização desses tipos de código de barras atual. Um não é capaz de determinar todas as variações com base nos dados do código de barras individuais. Portanto, o aplicativo será necessário determinar quando um código de barras do rótulo vários foi lida com base nos dados retornados. (Observe que um scanner talvez ou pode não suportar a leitura de várias etiquetas).

Esse valor é definido antes de um evento **DataReceived** sendo gerado para o aplicativo.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Veja também
* [Scanner de código de barras](pos-barcodescanner.md)
* [Classe de ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Classe de BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Classe de BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Classe de BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)