---
author: IvorB
ms.assetid: E9ADC88F-BD4F-4721-8893-0E19EA94C8BA
title: Emparelhamento fora de banda
description: "O emparelhamento fora de banda permite que os aplicativos se conectem a um periférico Ponto de Serviço sem a necessidade de descoberta."
translationtype: Human Translation
ms.sourcegitcommit: 0bf96b70a915d659c754816f4c115f3b3f0a5660
ms.openlocfilehash: 283f0a0cfc7b3827e70ea79490818bc259d98ad1

---
# Emparelhamento fora de banda

O emparelhamento fora de banda permite que os aplicativos se conectem a um periférico Ponto de Serviço sem a necessidade de descoberta. Os aplicativos devem usar o namespace [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/windows.devices.pointofservice.aspx) passar uma cadeia de caracteres formatada especificamente (blob fora de banda) para o método **FromIdAsync** adequado para o periférico desejado. Quando **FromIdAsync** é executado, o dispositivo host se emparelha e se conecta ao periférico antes que a operação retorne ao chamador.

## Formato de blob fora de banda

```json
    "connectionKind":"Network",
    "physicalAddress":"AA:BB:CC:DD:EE:FF",
    "connectionString":"192.168.1.1:9001",
    "peripheralKinds":"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}",
    "providerId":"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}",
    "providerName":"PrinterProtocolProvider.dll"
```

**connectionKind** - O tipo de conexão. Valores válidos são "Rede" e "Bluetooth".

**physicalAddress** - O endereço MAC do periférico. Por exemplo, em caso de uma impressora de rede, isso seria o endereço MAC que é fornecido pela folha de teste da impressora no formato de AA:BB:CC:DD:EE:FF.

**connectionString** - A cadeia de caracteres de conexão do periférico. Por exemplo, no caso de uma impressora de rede, isso seria o endereço IP fornecido pela folha de teste da impressora no formato 192.168.1.1:9001. Esse campo é omitido para todos os periféricos de Bluetooth.

**peripheralKinds** - O GUID para o tipo de dispositivo. Valores válidos são:

| Tipo de dispositivo | GUID |
| ---- | ---- |
| *Impressora POS* | C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33 |
| *Scanner de código de barras* | C243FFBD-3AFC-45E9-B3D3-2BA18BC7EBC5 |
| *Caixa registradora* | 772E18F2-8925-4229-A5AC-6453CB482FDA |


**providerId** - O GUID para a classe de provedor de protocolo. Valores válidos são:

| Classe de provedor de protocolo | GUID |
| ---- | ---- |
| *Impressora de rede ESC/POS genérica* | 02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2 |
| *Impressora ESC/POS BT genérica* | CCD5B810-95B9-4320-BA7E-78C223CAF418 |
| *Impressora Epson BT* | 94917594-544F-4AF8-B53B-EC6D9F8A4464 |
| *Impressora de rede Epson* | 9F0F8BE3-4E59-4520-BFBA-AF77614A31CE |
| *Impressora de rede Star* | 1E3A32C2-F411-4B8C-AC91-CC2C5FD21996 |
| *Scanner Socket BT* | 6E7C8178-A006-405E-85C3-084244885AD2 |
| *Registradora de rede APG* | E619E2FE-9489-4C74-BF57-70AED670B9B0 |
| *Registradora APG BT* | 332E6550-2E01-42EB-9401-C6A112D80185 |


**providerName** - O nome da DLL do provedor. Os provedores padrão são:

| Provedor | Nome da DLL |
| ---- | ---- |
| Impressora | PrinterProtocolProvider.dll |
| Caixa registradora | CashDrawerProtocolProvider.dll |
| Scanner | BarcodeScannerProtocolProvider.dll |

## Exemplo de uso: impressora de rede

```csharp
String oobBlobNetworkPrinter =
  "{\"connectionKind\":\"Network\"," +
  "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
  "\"connectionString\":\"192.168.1.1:9001\"," +
  "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
  "\"providerId\":\"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}\"," +
  "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobNetworkPrinter);
```

## Exemplo de uso: impressora Bluetooth

```csharp
string oobBlobBTPrinter =
    "{\"connectionKind\":\"Bluetooth\"," +
    "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
    "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
    "\"providerId\":\"{CCD5B810-95B9-4320-BA7E-78C223CAF418}\"," +
    "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobBTPrinter);

```



<!--HONumber=Aug16_HO3-->


