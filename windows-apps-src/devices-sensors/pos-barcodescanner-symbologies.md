---
title: Trabalhar com simbologias do scanner de código de barras
description: Este artigo contém informações sobre simbologias do scanner de código de barras.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: ee78ffbc49fdcb7f8e87844dea1e2ce29297e9f3
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63816691"
---
# <a name="working-with-symbologies"></a>Trabalhando com simbologias
Uma [simbologia de código de barras](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) é o mapeamento de dados para um formato específico de código de barras. Alguns simbologias comuns incluem UPC, 128 de código, código QR e assim por diante.  O scanner de código de barras da plataforma Universal do Windows APIs permitem que um aplicativo controlar como o scanner processa esses simbologias sem configurar manualmente o verificador. 

## <a name="determine-which-symbologies-are-supported"></a>Determinar quais simbologias são compatíveis 
Desde que seu aplicativo possa ser usado com modelos diferentes de scanner de código de barras de vários fabricantes, é possível consultar o scanner para determinar a lista de simbologias com as quais ele é compatível.  Isso pode ser útil se o aplicativo precisar de uma simbologia específica que talvez não seja compatível com todos os scanners ou se você precisar habilitar simbologias que foram desabilitadas no scanner manual ou programaticamente.

Quando você tiver um objeto [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) usando [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), chame [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obter uma lista de simbologias compatíveis com o dispositivo.

O exemplo a seguir obtém uma lista das simbologias com suporte do scanner de código de barras e os exibe em um bloco de texto:

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
Para determinar se o scanner suporta uma Simbologia específica, você pode chamar [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

O exemplo a seguir verifica se o scanner de código de barras dá suporte a **Code32** Simbologia:

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Alterar quais simbologias são reconhecidas
Em alguns casos, você pode usar um subconjunto de simbologias compatíveis com o scanner de código de barras.  Isso é especialmente útil para bloquear simbologias que você não pretende usar no aplicativo. Por exemplo, para garantir que um usuário verifique o código de barras direito, você pode restringir a verificação a UPC ou EAN ao adquirir SKUs de item e restringir a verificação ao código 128 ao adquirir números de série.

Quando já souber quais simbologias são compatíveis com o scanner, você pode definir as simbologias que você quer que ele reconheça.  Isso pode ser feito depois que você tiver estabelecido uma [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) do objeto usando [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Você pode chamar [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para habilitar um conjunto específico de simbologias enquanto aquelas omitidas na lista são desabilitadas.

O exemplo a seguir define as simbologias Active Directory de um scanner de código de barras solicitadas para [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) e [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Atributos de Simbologia de código de barras
Simbologias do código de barras diferente podem ter atributos diferentes, como suporte várias decodificar comprimentos, transmitindo o dígito de verificação para o host como parte dos dados brutos e verificar a validação de dígito. Com o [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) classe, você pode obter e definir esses atributos para um determinado [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) e Simbologia de código de barras.

Você pode obter os atributos de um determinado Simbologia com [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_). O trecho de código a seguir obtém os atributos do Simbologia Upca para um **ClaimedBarcodeScanner**.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Quando você terminar de modificar os atributos e estão prontos para defini-las, você pode chamar [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync). Esse método retorna um **bool**, que é **verdadeiro** se os atributos foram definidos com êxito.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Restringe o comprimento de dados por data de verificação
Algumas simbologias possuem tamanho variável, como código 39 ou código 128.  Códigos de barras desses simbologias podem estar localizados próximos uns dos outros que contém dados diferentes, quase sempre de tamanho específico. Definir o tamanho específico dos dados de que você precisa pode evitar verificações inválidas.

Antes de definir o comprimento de decodificação, verifique se o Simbologia de código de barras é compatível com vários comprimentos com [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported). Quando você souber que tem suporte, você pode definir as [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), que é do tipo [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). Essa propriedade pode ser qualquer um dos seguintes valores:

* **AnyLength**: Decodificar comprimentos de qualquer número.
* **Discreto**: Decodificar comprimentos deles [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) ou [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) caracteres de byte único.
* **Intervalo**: Decodificar comprimentos entre **DecodeLength1** e **DecodeLength2** caracteres de byte único. A ordem dos **DecodeLength1** e **DecodeLength2** importa (qualquer um pode ser maior ou menor que o outro).

Por fim, você pode definir os valores de **DecodeLength1** e **DecodeLength2** para controlar o comprimento dos dados que você precisa.

O trecho de código a seguir demonstra como definir o comprimento de decodificação:

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

### <a name="check-digit-transmission"></a>Transmissão do dígito de verificação

Você pode definir em uma Simbologia de outro atributo é se o dígito de verificação será transmitido para o host como parte dos dados brutos. Antes de essa configuração, verifique se o Simbologia dá suporte à verificação de transmissões de dígito [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). Em seguida, defina se a transmissão de dígito de verificação está habilitada com [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled).

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

Você também pode definir se o dígito de verificação de código de barras será validado. Antes de essa configuração, verifique se o Simbologia dá suporte à verificação de validação de dígitos com [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). Em seguida, defina se a validação do dígito de verificação está habilitada com [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled).

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