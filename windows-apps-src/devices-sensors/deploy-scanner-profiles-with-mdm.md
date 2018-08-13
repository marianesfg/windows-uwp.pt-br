---
title: Implantar perfis de scanner de código de barras com MDM
author: PatrickFarley
description: Perfis de scanner de código de barras podem ser implantados com um servidor MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.author: pafarley
ms.date: 09/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef7f1029573d2ff98e744ceb44b108a67a7c0d0b
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018079"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implantar perfis de scanner de código de barras com MDM

**Observação**  Esse recurso requer o Windows 10 Mobile ou posterior.

Perfis de scanner de código de barras podem ser implantados com um servidor MDM. Para implantar os perfis, use *OemProfile* no [CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025) para colocá-los na pasta \\Data\\SharedData\\OEM\\Public\\Profile. Esses perfis de scanner, em seguida, podem ser usados pelos fabricantes do driver para definir as configurações que não são expostas por meio de superfície de API.

A Microsoft não define os detalhes específicos de um perfil de scanner ou como implementá-los.

## <a name="related-topics"></a>Tópicos relacionados
- [CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [Suporte de dispositivo de scanner de código de barras](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)