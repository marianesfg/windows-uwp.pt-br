---
title: Implantar perfis de scanner de código de barras com MDM
description: Perfis de scanner de código de barras podem ser implantados com um servidor MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f537833385582678b215804cac9a16002618c7e4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684820"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implantar perfis de scanner de código de barras com MDM

**Observação**  este recurso requer o Windows 10 Mobile ou posterior.

Perfis de scanner de código de barras podem ser implantados com um servidor MDM. Para implantar os perfis, use *OemProfile* no [EnterpriseExtFileSystem CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp) para colocá-los no \\data\\SharedData\\OEM\\pasta de perfil\\pública. Esses perfis de scanner, em seguida, podem ser usados pelos fabricantes do driver para definir as configurações que não são expostas por meio de superfície de API.

A Microsoft não define os detalhes específicos de um perfil de scanner ou como implementá-los.

## <a name="related-topics"></a>Tópicos relacionados
- [CSP EnterpriseExtFileSystem](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Suporte ao dispositivo de scanner de código de barras](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)