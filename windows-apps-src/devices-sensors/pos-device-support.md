---
author: mukin
title: Suporte a dispositivos POS
description: "Este artigo contém informações sobre o suporte a dispositivo para cada família de dispositivos POS"
ms.author: mukin
ms.date: 05/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 58018385082f7f7e49edba0351d919bc840ade05
ms.sourcegitcommit: 53930c9871461f6106f785ae4fabb2296eb359f1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2017
---
# <a name="pos-device-support"></a>Suporte a dispositivos POS

## <a name="barcode-scanner"></a>Scanner de código de barras
| Conectividade | Suporte |
| -------------|-------------|
| USB          | <p>O Windows contém um driver de classe nativo para scanners de código de barras conectados por USB, que se baseia na especificação da Tabela de Uso de Scanner de PDV HID HID (8c) definida pela [USB.org](http://www.usb.org/developers/hidpage/). Veja a tabela abaixo para obter uma lista de dispositivos compatíveis conhecidos.  Consulte o manual do seu scanner de código de barras ou entre em contato com o fabricante para determinar se ele pode ser configurado no modo Scanner USB.HID.POS. </p><p>O Windows também é compatível com a implementação de drivers específicos do fornecedor para dar suporte a scanners de código de barras adicionais que não dão suporte ao padrão de Scanner USB.HID.POS. Entre em contato com o fabricante do scanner de código de barras para ver a disponibilidade de drivers específicos do fornecedor.</p>|
| Bluetooth    | <p>O Windows é compatível ao SPP SSI com base em scanners de código de barras de Bluetooth. Veja a tabela abaixo para obter uma lista de dispositivos compatíveis conhecidos.</p> |

### <a name="compatible-hardware"></a>Hardware compatível
| Categoria | Conectividade | Fabricante/modelo |
|--------------|-----------|-----------|
| **Scanners de mão 1D** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg (atualizável)|
| **Scanners de mão 1D** | **Bluetooth** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800 (antes conhecido como CHS 8Ci) <br/>|
|**Scanners de mão 2D** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg (atualizável)<br/>Honeywell Voyager 1602g<br/>Intermec SG20|
|**Scanners de mão 2D** | **Bluetooth** |Socket Mobile SocketScan S850 (antes conhecido como CHS 8Qi)|
| **Scanners de apresentação** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **Scanners de balcão** | **USB** |Honeywell Stratos 2700|
| **Mecanismos de varredura** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Dispositivos Windows Mobile**| **Internos** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Dispositivos Windows Mobile**| **Personalizado** | HP Elite X3 com suporte de scanner de código de barras |

## <a name="cash-drawer"></a>Caixa registradora
| Conectividade | Suporte |
| -------------|-------------|
| Rede/Bluetooth | <p> A conexão diretamente para a registradora pode ser feita pela rede ou por meio de Bluetooth, dependendo dos recursos da unidade registradora. </p>|
| Porta DK | <p> As caixas registradoras que não têm recursos de rede ou Bluetooth podem ser conectadas por meio da porta DK a uma impressora POS compatível ou do acessório Star Micronics DK-AirCash. </p>
| OPOS    | <p> Dá suporte a qualquer dispositivo de Caixa Registradora compatível com drivers OPOS e/ou fornece objetos de serviço OPOS. Instale os drivers OPOS de acordo com as instruções de instalação dos fabricantes do dispositivo em particular. Isso habilita USB e outra conectividade com base nas especificações do fabricante. </p> |

**Observação:**  Entre em contato com a Star Micronics para obter mais informações sobre o DK-AirCash.

### <a name="networkbluetooth"></a>Rede/Bluetooth
| Fabricante |    Model(os) |
|--------------|-----------|
| Registradora APG |    NetPRO, BluePRO |

## <a name="line-display"></a>Display de balcão
Dá suporte a qualquer dispositivo de display de balcão compatível com drivers OPOS e/ou fornece objetos de serviço OPOS. Instale os drivers OPOS de acordo com as instruções de instalação dos fabricantes do dispositivo em particular.

## <a name="magnetic-stripe-reader"></a>Leitor de tarjas magnéticas

O Windows contém um driver de classe nativo para leitores de tarjas magnéticas conectados por USB, que se baseia na especificação da Tabela de Uso de Scanner POS HID HID (8c) definida pela [USB.org](http://www.usb.org/developers/hidpage/).

### <a name="vendor-specific-support"></a>Suporte específico do fornecedor
O Windows oferece suporte para os seguintes leitores de tarja magnética da Magtek e da IDTech com base em suas ID do Fornecedor e ID do Produto (VID/PID).

| Fabricante |     Model(os) |    Número de peça |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>Específico do fornecedor personalizado
O Windows oferece suporte à implementação de drivers específicos do fornecedor adicionais para dar suporte a leitores de tarja magnética adicionais. Verifique a disponibilidade do leitor de tarja magnética junto ao fabricante.

## <a name="pos-printer"></a>Impressora POS
O Windows dá suporte à capacidade de as impressoras de recibo imprimirem quando conectadas à rede ou Bluetooth usando a linguagem de controle de impressora Epson ESC/POS. Para saber mais sobre ESC/POS, consulte [Epson ESC/POS com formatação](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting).

Enquanto as classes, as enumerações e as interfaces expostas na API dão suporte a impressoras de recibos, a impressoras de cheques e a impressoras gerenciais, a interface do driver dá suporte somente às impressoras de recibos. A tentativa de usar a impressora direto ou impressora de diário neste momento retornará um status de não implementada.

| Conectividade | Suporte |
| -------------|-------------|
| Rede/Bluetooth | <p> A conexão direta com a impressora POS pode ser feita pela rede ou por meio de Bluetooth, dependendo dos recursos da unidade de impressora POS. </p>|
| OPOS    | <p> Dá suporte a qualquer dispositivo de impressora POS compatível com drivers OPOS e/ou fornece objetos de serviço OPOS. Instale os drivers OPOS de acordo com as instruções de instalação dos fabricantes do dispositivo em particular. Isso habilita USB e outra conectividade com base nas especificações do fabricante. </p> |

### <a name="stationary-pos-printers-networkbluetooth"></a>Impressoras POS imóveis (rede/Bluetooth)
| Fabricante |    Model(os) |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>Impressoras POS móveis (Bluetooth)
| Fabricante |    Model(os) |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |