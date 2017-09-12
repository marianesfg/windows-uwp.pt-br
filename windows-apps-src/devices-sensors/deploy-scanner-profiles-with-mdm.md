---
title: "Implantar perfis de scanner de código de barras com MDM"
author: PatrickFarley
description: "Perfis de scanner de código de barras podem ser implantados com um servidor MDM."
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.openlocfilehash: a63a09e64b6e2b935963a3f49ed7cbc6b82bdcef
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2017
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implantar perfis de scanner de código de barras com MDM

**Observação**  Esse recurso requer o Windows 10 Mobile ou posterior.

Perfis de scanner de código de barras podem ser implantados com um servidor MDM. Para implantar os perfis, use *OemProfile* no [CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025) para colocá-los na pasta \\Data\\SharedData\\OEM\\Public\\Profile. Esses perfis de scanner, em seguida, podem ser usados pelos fabricantes do driver para definir as configurações que não são expostas por meio de superfície de API.

A Microsoft não define os detalhes específicos de um perfil de scanner ou como implementá-los.

## <a name="related-topics"></a>Tópicos relacionados
[Scanner de código de barras](barcode-scanner.md)

[CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025)