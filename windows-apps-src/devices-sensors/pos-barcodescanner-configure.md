---
title: Configurar um scanner de código de barras
description: Saiba como configurar um scanner de código de barras para o aplicativo desejado.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8466c56ef73a1c38c67e28cf52de7f380e6c563a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321581"
---
# <a name="configure-a-barcode-scanner"></a>Configurar um scanner de código de barras

O scanner de código de barras pode ser configurado em vários modos diferentes.  É importante que o scanner de código de barras seja configurado corretamente para o aplicativo desejado.

Muitos scanners de código de barras podem ser configurados no modo **keyboard wedge** que faz com que o scanner de código de barras apareça como um teclado no Windows.  Isso permite que você verifique os códigos de barras nos aplicativos que não reconhecem scanners de código de barras, como o Bloco de notas do Windows.  Quando você verifica um código de barras nesse modo, os dados decodificados do scanner de código de barras são inseridos no ponto de inserção como se você tivesse digitado os dados usando o teclado.  Se você deseja ter mais controle sobre o scanner de código de barras do aplicativo UWP, você precisará configurá-lo em um modo que não seja o keyboard wedge.

## <a name="usb-barcode-scanner"></a>Scanner de código de barras USB
Um scanner de código de barras conectado por USB deve ser configurado no modo **HID POS Scanner** para funcionar com o driver do scanner de código de barras incluído no Windows. Esse driver é uma implementação do **HID ponto de venda uso de tabelas** Especificação publicada para [USB HID](https://www.usb.org/hid).  Consulte a documentação do scanner de código de barras ou contate o fabricante do scanner de código de barras para obter instruções para habilitar o modo **HID POS Scanner**.  Uma vez configurado como **HID POS Scanner**, seu scanner de código de barras aparecerá no Gerenciador de Dispositivos no nó **POS Barcode Scanner** como **POS HID Barcode scanner**.

O fabricante do scanner de código de barras também pode ter um driver específico do fornecedor que é compatível com as APIs do scanner de código de barras da UWP usando um modo diferente de **HID POS Scanner**.  Se você já tiver instalado um driver fornecido pelo fabricante compatível com as APIs de Scanner de código de barras de UWP, você poderá ver um dispositivo específico do fornecedor listado em **Scanner de código de barras de POS** no Gerenciador de dispositivos.

## <a name="bluetooth-barcode-scanner"></a>Scanner de código de barras Bluetooth
Um scanner conectado por Bluetooth deve ser configurado no modo **Serial Port Protocol - Simple Serial Interface (SPP-SSI)** para funcionar com as APIs de scanner de código de barras da UWP.  Consulte a documentação do scanner de código de barras ou contate o fabricante do scanner de código de barras para obter instruções para habilitar o **SPP-SSI mode**.

Antes de poder usar o scanner de código de barras Bluetooth deve emparelhá-lo usando **Configurações > dispositivos > Bluetooth & outros dispositivos > Adicionar Bluetooth ou outro dispositivo**.

Você pode iniciar e controlar o processo de emparelhamento usando o [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) namespace.  Ver [par dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices) para obter mais informações.

[!INCLUDE [feedback](./includes/pos-feedback.md)]