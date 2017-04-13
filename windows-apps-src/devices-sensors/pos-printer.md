---
author: mukin
title: Impressora POS
description: "Este artigo contém informações sobre o ponto de família da impressora de serviço dos dispositivos"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d8340af651157bb6fae82785812f259c16d0a6c0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pos-printer"></a>Impressora POS

Permite que os desenvolvedores de aplicativo imprimam quando conectados à rede ou Bluetooth [impressoras de recibo](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.posprinter) usando a linguagem de controle de impressora Epson ESC/POS.

## <a name="requirements"></a>Requisitos
Os aplicativos que exigem esse namespace exigem a adição de "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) ao manifesto do pacote do aplicativo.

## <a name="device-support"></a>Suporte a dispositivos
O Windows dá suporte à capacidade de as impressoras de recibo imprimirem quando conectadas à rede ou Bluetooth usando a linguagem de controle de impressora Epson ESC/POS. Para saber mais sobre ESC/POS, consulte [Epson ESC/POS com formatação](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting).

Enquanto as classes, enumerações e interfaces expostas na impressora de recibo com suporte API, passam direto pela impressora, como impressora de diário, a interface da unidade suporta apenas impressora de recibo. A tentativa de usar a impressora direto ou impressora de diário neste momento retornará um status de não implementada.

O suporte é atualmente limitado aos modelos de dispositivo de rede e Bluetooth listados nas tabelas abaixo. Impressoras conectadas por USB atualmente não têm suporte. Verifique novamente para obter suporte adicional a ser adicionado no futuro.

### <a name="stationary-pos-printers-network-bluetooth"></a>Impressoras POS imóveis (rede, Bluetooth)
| Fabricante |    Model(os) |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>Impressoras POS móveis (Bluetooth)
| Fabricante |    Model(os) |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |

## <a name="examples"></a>Exemplos
Veja o exemplo de impressora POS para um exemplo de implementação.
+ [Exemplo de impressora POS](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
