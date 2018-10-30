---
title: Implantar perfis de scanner de código de barras com MDM
author: PatrickFarley
description: Perfis de scanner de código de barras podem ser implantados com um servidor MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.author: pafarley
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cfd9692620273952483ec7da65a69b643cb5bf4f
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762371"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implantar perfis de scanner de código de barras com MDM

**Observação**esse recurso requer o Windows 10 Mobile ou posterior.

Perfis de scanner de código de barras podem ser implantados com um servidor MDM. Para implantar os perfis, use *OemProfile* no [CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025) para colocá-los na pasta \\Data\\SharedData\\OEM\\Public\\Profile. Esses perfis de scanner, em seguida, podem ser usados pelos fabricantes do driver para definir as configurações que não são expostas por meio de superfície de API.

A Microsoft não define os detalhes específicos de um perfil de scanner ou como implementá-los.

## <a name="related-topics"></a>Tópicos relacionados
- [CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [Suporte de dispositivo de scanner de código de barras](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)