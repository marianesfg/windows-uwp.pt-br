---
author: mukin
title: "Scanner de código de barras"
description: "Este artigo contém informações sobre o ponto de família de serviços dos dispositivos do scanner de código de barras"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8d9ef9bc08fa666c2af1348450f7a5fb0a0c7b65
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="barcode-scanner"></a>Scanner de código de barras
Permite que os desenvolvedores de aplicativos acessem [scanners de código de barras](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodescanner) para recuperar dados decodificados de uma variedade de simbologias de códigos de barras como UPC e códigos QR dependendo do suporte do hardware. Consulte a classe [BarcodeSymbologies](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodesymbologies) para obter uma lista completa de simbologias compatíveis.

## <a name="requirements"></a>Requisitos
Os aplicativos que exigem esse namespace exigem a adição de "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) ao manifesto do pacote do aplicativo.

## <a name="device-support"></a>Suporte a dispositivos

### <a name="usb"></a>USB

#### <a name="hidscanner"></a>HID.Scanner
O Windows contém um driver de classe de scanner de código de barras que se baseia na página de uso HID.Scanner (8C) definida pelo USB.org. Esse driver de classe dará suporte a qualquer scanner de código de barras que implementa esse padrão, como: Fabricante    Modelo(s) Honeywell    1900GSR-2, 1200g-2 Intermec    SG20

Consulte o manual do seu scanner de código de barras ou entre em contato com o fabricante para determinar se ele pode ser configurado como USB.HID.Scanner.

#### <a name="hidvendor-specific"></a>HID.Específico do fornecedor
O Windows oferece suporte a implementação de drivers específicos do fornecedor para dar suporte a scanners de código de barras adicionais. Entre em contato com o fabricante do scanner de código de barras para disponibilidade caso o dispositivo não seja compatível com a caixa de entrada USB.HID.Scanner.

### <a name="bluetooth"></a>Bluetooth
#### <a name="serial-port-protocol-spp--simple-serial-interface-ssi"></a>Protocolo de porta serial (SPP) – Interface Serial simples (SSI)
O Windows é compatível ao SPP SSI com base em scanners de código de barras de Bluetooth.

| Fabricante |    Model(os) |
|--------------|-----------|
| Soquete Mobile |    **7 Séries CHS:** <br/> 7 Scanner de código de barras Ci 1D Imager <br/> 7Di 1D Durável, Imager Scanner de código de barras <br/> Scanner de código de barras de Laser 7Mi 1D <br/> 7Pi 1D Durável, LaserBarcode Scanner <br/> **Série de 700 DuraScan:** <br/> Scanner de código de barras D700 1D Imager <br/> Scanner de código de barras D730 1D Laser <br/> **Série de SocketScan 800** <br/> Scanner de código de barras S800 1D Imager (anteriormente conhecido como CHS 8Ci) <br/> Scanner de código de barras S850 2D Imager (anteriormente conhecido como CHS 8Qi)

## <a name="examples"></a>Exemplos
Veja a amostra de scanner de código de barras relativa a um exemplo de implementação.
+    [Amostra de scanner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
