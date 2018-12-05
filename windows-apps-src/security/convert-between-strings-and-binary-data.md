---
title: Converter entre cadeias de caracteres e dados binários
description: Este exemplo de código mostra como converter entre cadeias de caracteres e dados binários em um aplicativo da Plataforma Universal do Windows (UWP).
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: ae2de8f009da1873c9aebf4f60ef315b36c7d744
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8758446"
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