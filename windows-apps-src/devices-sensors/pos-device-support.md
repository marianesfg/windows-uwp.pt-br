---
author: TerryWarwick
title: Suporte a dispositivos POS (Ponto de Serviço)
description: Este artigo contém informações sobre o suporte a hardware para cada uma das classes de dispositivos POS (Ponto de Serviço)
ms.author: jken
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df6e2c15260759f164a37b68365e0268633b22d5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5981027"
---
# <a name="supported-point-of-service-peripherals"></a>Periféricos de ponto de serviço compatíveis

## <a name="barcode-scanner"></a>Scanner de código de barras
| Conectividade | Suporte |
| -------------|-------------|
| USB          | <p>O Windows contém um driver de classe nativo para scanners de código de barras conectados por USB, que se baseia na especificação da Tabela de Uso de Scanner HID POS (8c) definida pela [USB.org](http://www.usb.org/developers/hidpage/). Consulte a tabela abaixo para obter uma lista dos dispositivos compatíveis conhecidos.  Consulte o manual do scanner de código de barras ou entre em contato com o fabricante para determinar como configurar o scanner no modo **Scanner USB.HID.POS** </p><p>O Windows também é compatível com a implementação de drivers específicos do fornecedor para dar suporte a scanners de código de barras adicionais que não dão suporte ao padrão de Scanner USB.HID.POS. Entre em contato com o fabricante do scanner de código de barras para ver a disponibilidade de drivers específicos do fornecedor.</p><p>Fabricantes de scanner de código de barras, consultem o [Guia de design de driver de scanner de código de barras](https://aka.ms/pointofservice-drv) para obter informações sobre como criar um driver de scanner de código de barras personalizado</p> |
| Bluetooth    | <p>O Windows oferece suporte ao protocolo SPP SSI com base em scanners de código de barras Bluetooth. Veja a tabela abaixo para obter uma lista de dispositivos compatíveis conhecidos. Consulte o manual do scanner de código de barras ou entre em contato com o fabricante para determinar como configurar o scanner no modo **SPP-SSI**</p> |
| Webcam       | <p>A partir do Windows 10, versão 1803, você pode ler códigos de barras por meio de uma lente de câmera padrão de um aplicativo universal do Windows. Recomenda-se o uso de uma câmera compatível com o foco automático e uma resolução mínima de 1920 x 1440.  Algumas câmeras de resolução inferior podem ler códigos de barras padrão se o código de barras for impresso de forma grande o suficiente.  Os códigos de barras com elementos mais finos podem exigir câmeras com maior resolução.</p>| 
|


| Fabricante  | Modelo                          | Funcionalidade | Conexão    | Tipo         | Modo                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| Código          | Reader™ 950                    | 2D         | USB          | Mão     | HID POS Scanner           |
| Código          | Reader™ 1021                   | 2D         | USB          | Mão     | HID POS Scanner           |
| Código          | Reader™ 1421                   | 2D         | USB          | Mão     | HID POS Scanner           |
| Código          | Reader™ 5000                   | 2D         | USB          | Apresentação | HID POS Scanner           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | Apresentação | HID POS Scanner           |
| Honeywell     | Granit 198Xi                   | 2D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | N5680                          | 2D         | Interna     | Componente    | HID POS Scanner           |
| Honeywell     | N3680                          | 2D         | Interna     | Componente    | HID POS Scanner           |
| Honeywell     | Órbita 7190g                    | 2D         | USB          | Apresentação | HID POS Scanner           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | Contador   | HID POS Scanner           |
| Honeywell     | Voyager 1200g                  | 1D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Voyager 1202g                  | 1D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Voyager 1202-bf                | 1D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Voyager 145Xg                  | 1D / 2D ¹   | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Xenon 1900g                    | 2D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Xenon 1902g                    | 2D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Xenon 1902g-bf                 | 2D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Xenon 1900h                    | 2D         | USB          | Mão     | HID POS Scanner           |
| Honeywell     | Xenon 1902h                    | 2D         | USB          | Mão     | HID POS Scanner           |
| HP            | Scanner de código de barras de valor (HR2150) | 2D         | USB          | Mão     | HID POS Scanner           |
| Intermec      | SG20                           | 2D         | USB          | Mão     | HID POS Scanner           |
| Soquete Mobile | CHS 7Ci                        | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | CHS 7Di                        | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | CHS 7mi                        | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | CHS 7Pi                        | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | Como CHS 8Ci                        | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | DuraScan D700                  | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | DuraScan D730                  | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | DuraScan D740                  | 2D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | SocketScan S700                | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | SocketScan S730                | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | SocketScan S740                | 2D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | SocketScan S800                | 1D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Soquete Mobile | SocketScan S850                | 2D         | Bluetooth    | Mão     | Perfil de porta serial (SPP) |
| Zebra         | DS2278                         | 2D         | USB          | Mão     | HID POS Scanner           |
| Zebra         | DS8108²                        | 2D         | USB          | Mão     | HID POS Scanner           |
|


¹ Upgradable para dar suporte a códigos de barras 2D por meio de Honeywell <br/>
Firmware de ² mínimo necessário 016 (2018.01.18) necessários. Atualizável usando Zebra [123Scan](http://www.zebra.com/123Scan). 


<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>Dispositivos do Windows com o scanner de código de barras interno
| Fabricante   | Modelo | Sistema operacional |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>Dispositivos Windows Mobile com o scanner de código de barras interno
| Fabricante   | Modelo | Sistema operacional |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ-E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| Zebra          | TC700j | Windows Mobile   |
| HP             | Elite X3 revestimento | Windows Mobile   |




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
