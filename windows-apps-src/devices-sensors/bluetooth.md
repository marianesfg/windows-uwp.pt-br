---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: Bluetooth
description: Esta seção contém artigos sobre como integrar Bluetooth a aplicativos UWP (Plataforma Universal do Windows), incluindo como usar RFCOMM, GATT e anúncios LE (baixa energia).
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469561"
---
# <a name="bluetooth"></a>Bluetooth
Esta seção contém artigos sobre como integrar o Bluetooth a aplicativos da Plataforma Universal do Windows (UWP). Há duas tecnologias bluetooth diferentes que você pode optar por implementar em seu app.

> [!Important]
> Você deve declarar o recurso "Bluetooth" em *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>Bluetooth Clássico (RFCOMM)
Antes do Bluetooth LE, os dispositivos normalmente usavam esse protocolo para se comunicar usando o Bluetooth. Esse protocolo é simples e útil para a comunicação entre dispositivos sem a necessidade de economia de energia. Para saber mais sobre esse protocolo, incluindo exemplos de código, veja o tópico [Bluetooth RFCOMM](send-or-receive-files-with-rfcomm.md).

## <a name="bluetooth-low-energy-le"></a>Bluetooth Low-Energy (LE)
O Bluetooth Low Energy (LE) é uma especificação que define protocolos para a descoberta e a comunicação entre dispositivos com um requisito de uso de energia eficiente. Para saber mais, incluindo exemplos de código, veja o tópico [Bluetooth Low Energy](bluetooth-low-energy-overview.md).

## <a name="see-also"></a>Consulte Também
- [Perguntas frequentes de desenvolvedores para Bluetooth](bluetooth-dev-faq.md)