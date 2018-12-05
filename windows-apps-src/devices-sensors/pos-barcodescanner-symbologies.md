---
title: Trabalhar com simbologias do scanner de código de barras
description: Este artigo contém informações sobre simbologias do scanner de código de barras.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 690b6b8ee688f62dcae375ed48e07797c921bf43
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8690570"
---
# <a name="working-with-symbologies"></a>Trabalhando com simbologias
Uma [simbologia de código de barras](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) é o mapeamento de dados para um formato específico de código de barras. Algumas simbologias comuns incluem UPC, código 128, código QR e assim por diante.  O scanner de código de barras da plataforma Universal do Windows APIs permitem que um aplicativo controle como o scanner processará essas simbologias sem configurar manualmente o scanner. 

## <a name="determine-which-symbologies-are-supported"></a>Determinar quais simbologias são compatíveis 
Desde que seu aplicativo possa ser usado com modelos diferentes de scanner de código de barras de vários fabricantes, é possível consultar o scanner para determinar a lista de simbologias com as quais ele é compatível.  Isso pode ser útil se o aplicativo precisar de uma simbologia específica que talvez não seja compatível com todos os scanners ou se você precisar habilitar simbologias que foram desabilitadas no scanner manual ou programaticamente.

Quando você tiver um objeto [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) usando [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), chame [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obter uma lista de simbologias compatíveis com o dispositivo.

O exemplo a seguir obtém uma lista das simbologias do scanner de código de barras com suporte e exibe-los em um bloco de texto:

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>Determinar se uma simbologia específica é compatível
Para determinar se o scanner é compatível com uma Simbologia específica, você pode chamar [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

O exemplo a seguir verifica se o scanner de código de barras é compatível com a Simbologia de **Code32** :

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Alterar quais simbologias serão reconhecidas
Em alguns casos, você pode usar um subconjunto de simbologias compatíveis com o scanner de código de barras.  Isso é especialmente útil para bloquear simbologias que você não pretende usar no aplicativo. Por exemplo, para garantir que um usuário verifique o código de barras direito, você pode restringir a verificação a UPC ou EAN ao adquirir SKUs de item e restringir a verificação ao código 128 ao adquirir números de série.

Quando já souber quais simbologias são compatíveis com o scanner, você pode definir as simbologias que você quer que ele reconheça.  Isso pode ser feito após o estabelecimento de um objeto de [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) usando [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Você pode chamar [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para habilitar um conjunto específico de simbologias enquanto aquelas omitidas na lista são desabilitadas.

O exemplo a seguir define as simbologias ativas de um scanner de código de barras declarado como [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) e [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Atributos de Simbologia de código de barras
Simbologias do código de barras diferentes podem ter atributos diferentes, como vários apoio decodificar comprimentos, transmitindo o dígito de verificação para o host como parte dos dados brutos e verifique a validação de dígito. Com a classe [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) , você pode obter e definir esses atributos para um determinado Simbologia [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) e código de barras.

Você pode obter os atributos de um determinado Simbologia com [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_). O trecho de código a seguir obtém os atributos de Simbologia o Upca para um **ClaimedBarcodeScanner**.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Quando você terminar de modificar os atributos e está prontos para defini-los, você pode chamar [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync). Esse método retorna um **bool**, que é **verdadeiro** se os atributos foram definidos com êxito.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Restringir a verificação de dados por tamanho de dados
Algumas simbologias possuem tamanho variável, como código 39 ou código 128.  Códigos de barras dessas simbologias podem estar localizados próximos uns dos outros contendo dados diferentes, geralmente com tamanho específico. Definir o tamanho específico dos dados de que você precisa pode evitar verificações inválidas.

Antes de definir o tamanho de decodificação, verifique se o Simbologia de código de barras dá suporte a vários tamanhos com [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported). Depois que você sabe que ele é compatível, você pode definir o [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), que é do tipo [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). Essa propriedade pode ser qualquer um dos seguintes valores:

* **AnyLength**: tamanhos de qualquer número de decodificação.
* **Discreto**: decodificar tamanhos de caracteres de byte único [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) ou [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) .
* **Intervalo**: decodificar tamanhos entre **DecodeLength1** e **DecodeLength2** caracteres de byte único. A ordem de **DecodeLength1** e o **DecodeLength2** não questão (qualquer um pode ser maior ou menor do que o outro).

Por fim, você pode definir os valores de **DecodeLength1** e **DecodeLength2** para controlar o tamanho dos dados de que você precisa.

O trecho de código a seguir demonstra como definir o tamanho de decodificação:

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>Transmissão de dígito de verificação

Você pode definir em uma Simbologia de outro atributo é se o dígito de verificação será transmitido para o host como parte dos dados brutos. Antes de configurar isso, certifique-se de que o Simbologia dá suporte à seleção transmissão dígito com [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). Em seguida, defina se a transmissão de dígito de verificação está habilitada com [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled).

O trecho de código a seguir demonstra a transmissão de dígito de verificação de configuração:

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>Validação de dígito de verificação

Você também pode definir se o dígito de verificação de código de barras será validado. Antes de configurar isso, certifique-se de que o Simbologia dá suporte à seleção validação dígito com [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). Em seguida, defina se a validação de dígito de verificação está habilitada com [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled).

O trecho de código a seguir demonstra a validação de dígito de verificação de configuração:

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Consulte também

* [Scanner de código de barras](pos-barcodescanner.md)
* [Classe BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [Classe BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Classe ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [Classe BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [Enumeração BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)