---
title: Codificar e decodificar dados
description: Este exemplo de código mostra como codificar e decodificar dados hexadecimais e base64 em um aplicativo da Plataforma Universal do Windows (UWP).
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 3c4e694dca3c84c7e94e513d8bb10a3f405bbc86
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8938753"
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
