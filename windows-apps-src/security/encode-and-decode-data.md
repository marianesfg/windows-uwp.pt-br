---
title: Codificar e decodificar dados
description: Este exemplo de código mostra como codificar e decodificar dados hexadecimais e base64 em um aplicativo da Plataforma Universal do Windows (UWP).
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5c826edcd8f7ce625f3cb42df69136f500e93f4e
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690352"
---
# <a name="encode-and-decode-data"></a>Codificar e decodificar dados



Este exemplo de código mostra como codificar e decodificar dados hexadecimais e base64 em um aplicativo da Plataforma Universal do Windows (UWP).

```cs
public void EncodeDecodeBase64()
{
    // Define a Base64 encoded string.
    String strBase64 = "uiwyeroiugfyqcajkds897945234==";

    // Decoded the string from Base64 to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromBase64String(strBase64);

    // Encode the buffer back into a Base64 string.
    String strBase64New = CryptographicBuffer.EncodeToBase64String(buffer);
}

public void EncodeDecodeHex()
{
    // Define a hexadecimal string.
    String strHex = "30310AFF";

    // Decode a hexadecimal string to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromHexString(strHex);

    // Encode the buffer back into a hexadecimal string.
    String strHexNew = CryptographicBuffer.EncodeToHexString(buffer);
}
```
