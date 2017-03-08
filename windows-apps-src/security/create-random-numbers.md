---
title: "Criar números aleatórios"
description: "Este exemplo de código mostra como criar um número aleatório ou buffer para uso em criptografia em um app UWP (Plataforma Universal do Windows)."
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 362bb264320fcf1256559543ce4607abd50260ed
ms.lasthandoff: 02/07/2017

---

# <a name="create-random-numbers"></a>Criar números aleatórios


\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este exemplo de código mostra como criar um número aleatório ou buffer para uso em criptografia em um app UWP (Plataforma Universal do Windows).

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
