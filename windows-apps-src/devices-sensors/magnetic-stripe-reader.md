---
author: mukin
title: "Leitor de tarja magnética"
description: "Este artigo contém informações sobre o ponto de leitor de tarja magnética da família de serviço de dispositivos"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a11fe7a63c0444ac986e7bfe0d50472249e5196e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="magnetic-stripe-reader"></a>Leitor de tarja magnética

Permite que os desenvolvedores de aplicativos acessem [leitores de tarja magnética](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.magneticstripereader) para recuperar dados de cartões de tarja magnética, como cartões de crédito/débito, cartões de fidelidade, cartões de acesso etc.

## <a name="requirements"></a>Requisitos
Os aplicativos que exigem esse namespace exigem a adição de "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) ao manifesto do pacote do aplicativo.

## <a name="device-support"></a>Suporte a dispositivos
### <a name="usb"></a>USB
### <a name="supported-vendor-specific"></a>Específico do fornecedor com suporte
O Windows oferece suporte para os seguintes leitores de tarja magnética da Magtek e da IDTech com base em suas ID do Fornecedor e ID do Produto (VID/PID).

| Fabricante |     Model(os) |    Número de peça |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>Específico do fornecedor personalizado
O Windows oferece suporte à implementação de drivers específicos do fornecedor adicionais para dar suporte a leitores de tarja magnética adicionais. Verifique a disponibilidade do leitor de tarja magnética junto ao fabricante.

## <a name="examples"></a>Exemplos
Veja a amostra de leitor de tarja magnética para obter uma implementação de exemplo.
+    [Amostra de leitor de tarja magnética](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
