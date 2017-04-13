---
author: DBirtolo
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: Dispositivos, sensores e consumo de energia
description: "Para proporcionar uma experiência de qualidade para seus usuários, talvez seja necessário integrar dispositivos externos ou sensores ao seu aplicativo."
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7915f626d5e86aed0c47f4d2f1efc61348385ba8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="devices-sensors-and-power"></a>Dispositivos, sensores e consumo de energia

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Para proporcionar uma experiência de qualidade para seus usuários, talvez seja necessário integrar dispositivos externos ou sensores ao seu aplicativo. Aqui estão alguns exemplos de recursos que você pode adicionar ao seu aplicativo usando a tecnologia descrita nesta seção.

-   Proporcionando experiência de impressão avançada
-   Integrando sensores de movimento e orientação ao seu jogo
-   Conectando-se a um dispositivo diretamente ou por meio de um protocolo de rede

| Tópico | Descrição |
|-------|-------------|
| [Habilitar os recursos do dispositivo](enable-device-capabilities.md) | Este tutorial descreve como declarar recursos do dispositivo no Microsoft Visual Studio. Isso permite que o aplicativo use câmeras, microfones, sensores de localização e outros dispositivos. | 
| [Habilitar o acesso de modo do usuário para Windows IoT](enable-usermode-access.md) | Este tutorial descreve como habilitar o acesso de modo do usuário para GPIO, I2C, SPI e UART no Windows 10 IoT Core. |
| [Enumerar dispositivos](enumerate-devices.md) | O namespace de enumeração permite localizar dispositivos que estejam conectados internamente ao sistema, externamente ou que possam ser detectados por protocolos de rede ou sem fio. |
| [Emparelhar dispositivos](pair-devices.md) | Alguns dispositivos precisam ser emparelhados antes de serem usados. O namespace [<strong>Windows.Devices.Enumeration</strong>](https://msdn.microsoft.com/library/windows/apps/BR225459) dá suporte a três maneiras diferentes de emparelhar dispositivos. |
| [Emparelhamento fora de banda](out-of-band-pairing.md) | Esta seção descreve como o emparelhamento fora de banda permite que os aplicativos se conectem a certos dispositivos sem necessidade de descoberta. | 
| [Sensores](sensors.md) | Os sensores permitem ao aplicativo identificar a relação entre um dispositivo e o mundo físico ao redor dele. Os sensores podem informar ao aplicativo a direção, a orientação e o movimento do dispositivo. |
| [Bluetooth](bluetooth.md) | Esta seção contém artigos sobre como integrar Bluetooth a aplicativos UWP (Plataforma Universal do Windows), incluindo como usar RFCOMM, GATT e anúncios LE (baixa energia). | 
| [Impressão e digitalização](printing-and-scanning.md) | Esta seção descreve como imprimir e digitalizar em seu aplicativo Universal do Windows. | 
| [Impressão 3D](3d-printing.md) | Esta seção descreve como utilizar a funcionalidade de impressão 3D em seu aplicativo Universal do Windows. |
| [Criar um aplicativo de cartão inteligente NFC](host-card-emulation.md) | O Windows Phone 8.1 dava suporte a aplicativos de emulação de cartão NFC usando um elemento seguro baseado em SIM, mas esse modelo exigia que aplicativos de pagamento seguro estivessem rigidamente acoplados a MNO (operadoras de rede móvel). Isso limitava a variedade de soluções de pagamento possíveis por outros comerciantes ou desenvolvedores que não estão associados a MNOs. No Windows 10 Mobile, apresentamos uma nova tecnologia de emulação de cartão, chamada Emulação de Cartão Host (HCE). A tecnologia HCE permite que seu aplicativo se comunique diretamente com um leitor de cartão NFC. Este tópico ilustra como a Emulação de Cartão Host (HCE) funciona em dispositivos Windows 10 Mobile, e como você pode desenvolver um aplicativo HCE para que os clientes possam acessar seus serviços usando um telefone, em vez de um cartão físico, sem colaborar com uma MNO. |
| [Obter informações sobre a bateria](get-battery-info.md) | Saiba como obter informações detalhadas sobre a bateria usando APIs no namespace [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/Dn895017). |

