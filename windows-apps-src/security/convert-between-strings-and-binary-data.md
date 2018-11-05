---
title: Converter entre cadeias de caracteres e dados binários
description: Este exemplo de código mostra como converter entre cadeias de caracteres e dados binários em um aplicativo da Plataforma Universal do Windows (UWP).
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 030fe81685fbc6caea0b9847b366384476f6298f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6034242"
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