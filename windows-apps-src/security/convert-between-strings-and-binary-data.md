---
title: "Converter entre cadeias de caracteres e dados binários"
description: "Este exemplo de código mostra como converter entre cadeias de caracteres e dados binários em um app da Plataforma Universal do Windows (UWP)."
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 3317c1c05c8a9a51f0a4d3c363db9c92809c1919
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-between-strings-and-binary-data"></a>Converter entre cadeias de caracteres e dados binários


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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