---
title: Converter entre cadeias de caracteres e dados binários
description: Este exemplo de código mostra como converter entre cadeias de caracteres e dados binários em um aplicativo da Plataforma Universal do Windows (UWP).
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: b3c3a3f6f831186302fc32b1f510919da40c57cc
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862029"
---
# <a name="convert-between-strings-and-binary-data"></a>Converter entre cadeias de caracteres e dados binários



Este exemplo de código mostra como converter entre cadeias de caracteres e dados binários em um aplicativo da Plataforma Universal do Windows (UWP).

```cs
public void ConvertData()
{
    // Create a string to convert.
    String strIn = "Input String";

    // Convert the string to UTF16BE binary data.
    IBuffer buffUTF16BE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16BE);

    // Convert the string to UTF16LE binary data.
    IBuffer buffUTF16LE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16LE);

    // Convert the string to UTF8 binary data.
    IBuffer buffUTF8 = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf8);
}
```