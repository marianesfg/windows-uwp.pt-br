---
title: Criar números aleatórios
description: Este exemplo de código mostra como criar um número aleatório ou buffer para uso em criptografia em um aplicativo UWP (Plataforma Universal do Windows).
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: a128535617c97e73b5a389db827fcf8c579b7f13
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5642580"
---
# <a name="create-random-numbers"></a>Criar números aleatórios



Este exemplo de código mostra como criar um número aleatório ou buffer para uso em criptografia em um aplicativo UWP (Plataforma Universal do Windows).

```cs
public string GenerateRandomData()
{
    // Define the length, in bytes, of the buffer.
    uint length = 32;

    // Generate random data and copy it to a buffer.
    IBuffer buffer = CryptographicBuffer.GenerateRandom(length);

    // Encode the buffer to a hexadecimal string (for display).
    string randomHex = CryptographicBuffer.EncodeToHexString(buffer);

    return randomHex;
}

public uint GenerateRandomNumber()
{
    // Generate a random number.
    uint random = CryptographicBuffer.GenerateRandomNumber();
    return random;
}
```