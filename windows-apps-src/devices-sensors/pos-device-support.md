---
author: TerryWarwick
title: Suporte a dispositivos POS (Ponto de Serviço)
description: Este artigo contém informações sobre o suporte a hardware para cada uma das classes de dispositivos POS (Ponto de Serviço)
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb2468497115c9595f6fd17ab61b30caed507ab
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832090"
---
# <a name="supported-point-of-service-peripherals"></a>Periféricos de ponto de serviço compatíveis

## <a name="barcode-scanner"></a>Scanner de código de barras
| Conectividade | Suporte |
| -------------|-------------|
| USB          | <p>O Windows contém um driver de classe nativo para scanners de código de barras conectados por USB, que se baseia na especificação da Tabela de Uso de Scanner HID POS (8c) definida pela [USB.org](http://www.usb.org/developers/hidpage/). Consulte a tabela abaixo para obter uma lista dos dispositivos compatíveis conhecidos.  Consulte o manual do scanner de código de barras ou entre em contato com o fabricante para determinar como configurar o scanner no modo **Scanner USB.HID.POS** </p><p>O Windows também é compatível com a implementação de drivers específicos do fornecedor para dar suporte a scanners de código de barras adicionais que não dão suporte ao padrão de Scanner USB.HID.POS. Entre em contato com o fabricante do scanner de código de barras para ver a disponibilidade de drivers específicos do fornecedor.</p><p>Fabricantes de scanner de código de barras, consultem o [Guia de design de driver de scanner de código de barras](https://aka.ms/pointofservice-drv) para obter informações sobre como criar um driver de scanner de código de barras personalizado</p> |
| Bluetooth    | <p>O Windows oferece suporte ao protocolo SPP SSI com base em scanners de código de barras Bluetooth. Veja a tabela abaixo para obter uma lista de dispositivos compatíveis conhecidos. Consulte o manual do scanner de código de barras ou entre em contato com o fabricante para determinar como configurar o scanner no modo **SPP-SSI**</p> |
| Webcam       | <p>A partir do Windows 10, versão 1803, você pode ler códigos de barras por meio de uma lente de câmera padrão de um aplicativo universal do Windows. Recomenda-se o uso de uma câmera compatível com o foco automático e uma resolução mínima de 1920 x 1440.  Algumas câmeras de resolução inferior podem ler códigos de barras padrão se o código de barras for impresso de forma grande o suficiente.  Os códigos de barras com elementos mais finos podem exigir câmeras com maior resolução.</p>| 
|


### <a name="compatible-barcode-scanners"></a>Scanners de código de barras compatíveis
| Categoria | Conectividade | Fabricante/modelo |
|--------------|-----------|-----------|
| **Scanners de mão 1D** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg (atualizável)|
| **Scanners de mão 1D** | **Bluetooth** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800 (antes conhecido como CHS 8Ci) <br/>|
|**Scanners de mão 2D** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg (atualizável)<br/>Honeywell Voyager 1602g<br/>Intermec SG20<br/>Zebra DS2278<br/>Zebra DS8108 ¹<hr><small>¹ Firmware mínimo necessário 016 (2018.01.18). Atualizável usando [123Scan](http://www.zebra.com/123Scan)</small>|
|**Scanners de mão 2D** | **Bluetooth** |Socket Mobile SocketScan S850 (antes conhecido como CHS 8Qi)|
| **Scanners de apresentação** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **Scanners de balcão** | **USB** |Honeywell Stratos 2700|
| **Mecanismos de varredura** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Dispositivos Windows Mobile**| **Internos** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Dispositivos Windows Mobile**| **Personalizado** | HP Elite X3 com suporte de scanner de código de barras |


## <a name="cash-drawer"></a>Caixa registradora
| Conectividade | Suporte |
| -------------|-------------|
| Rede/Bluetooth | <p> A conexão diretamente para a registradora pode ser feita pela rede ou por meio de Bluetooth, dependendo dos recursos da unidade registradora. </p><p>Registradora APG: NetPRO, BluePRO</p> |
| Porta DK | <p> As caixas registradoras que não têm recursos de rede ou Bluetooth podem ser conectadas por meio da porta DK a uma impressora de recibo compatível ou do acessório Star Micronics DK-AirCash. </p>
| OPOS    | <p> Oferece suporte a qualquer caixa registradora compatível com OPOS via objetos de serviço OPOS fornecidos pelo fabricante. Instale os drivers OPOS de acordo com as instruções de instalação dos fabricantes de dispositivo. </p> |


## <a name="customer-display-linedisplay"></a>Exibição de cliente (LineDisplay)
Oferece suporte a qualquer exibição de linha compatível com OPOS via objetos de serviço OPOS fornecidos pelo fabricante. Instale os drivers OPOS de acordo com as instruções de instalação dos fabricantes de dispositivo.

## <a name="magnetic-stripe-reader"></a>Leitor de tarja magnética
O Windows oferece suporte aos seguintes leitores de tarja magnética da Magtek e da IDTech com base em suas ID de fornecedor e ID do produto (product ID) (VID/PID).

| Fabricante |    Model(os) |  Número de peça |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| | MiniMag (VID:0ACD PID:0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |  210730xx |
| | Dynamag (VID:0801 PID:0002) |   210401xx |

 O Windows oferece suporte à implementação de drivers adicionais específicos do fornecedor para dar suporte a leitores de tarja magnética adicionais. Verifique a disponibilidade do leitor de tarja magnética junto ao fabricante. Fabricantes de leitor de tarja magnética, consulte o [Guia de design do driver leitor de tarja magnética](https://aka.ms/pointofservice-drv) para obter informações sobre como criar um driver de leitor de tarja magnética personalizado.

## <a name="receipt-printer-posprinter"></a>Impressora de recibo (POSPrinter)
| Conectividade | Suporte |
| -------------|-------------|
| Rede e Bluetooth | <p>O Windows oferece suporte às impressoras de recibo conectadas em rede ou via Bluetooth usando a linguagem de controle de impressora Epson ESC/POS.  As impressoras listadas abaixo são descobertas automaticamente por meio de APIs POSPrinter. Impressoras de recibo adicionais que fornecem uma emulação de ESC/POS também podem funcionar, mas precisam ser associados usando um processo de [emparelhamento fora da faixa](https://aka.ms/pointofservice-oobpairing).</p><p>Observação: as estação de lista de separação e as estações de diário não são compatíveis com este método.</p> |
| OPOS    | <p> Oferece suporte a qualquer impressoras de recibo compatível com OPOS via objetos de serviço OPOS. Instale os drivers OPOS de acordo com as instruções de instalação dos fabricantes de dispositivo. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Impressoras de recibo estacionárias (rede/Bluetooth)
| Fabricante |    Model(os) |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Impressoras de recibo móveis (Bluetooth)
| Fabricante |    Model(os) |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |
