---
title: Implantar perfis de scanner de código de barras com MDM
description: Perfis de scanner de código de barras podem ser implantados com um servidor MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dbcaa683e2c7a2bb18d88fcba03e10fa951d4459
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626151"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implantar perfis de scanner de código de barras com MDM

**Observação**  este recurso requer o Windows 10 Mobile ou posterior.

Perfis de scanner de código de barras podem ser implantados com um servidor MDM. Para implantar os perfis, use *OemProfile* na [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) colocá-los para o \\dados\\SharedData\\OEM\\ Público\\pasta de perfil. Esses perfis de scanner, em seguida, podem ser usados pelos fabricantes do driver para definir as configurações que não são expostas por meio de superfície de API.

A Microsoft não define os detalhes específicos de um perfil de scanner ou como implementá-los.

## <a name="related-topics"></a>Tópicos relacionados
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [Suporte de dispositivo de scanner de código de barras](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)