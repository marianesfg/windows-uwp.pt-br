---
author: jwmsft
description: "Na marcação XAML, especifica um valor null para uma propriedade."
title: "Extensão de marcação xNull"
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7be1caeca3427f75263019dbdba92c8695b6dde3
ms.lasthandoff: 02/07/2017

---

# <a name="xnull-markup-extension"></a>Extensão de marcação {x:Null}

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Na marcação XAML, especifica um valor **null** para uma propriedade.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Comentários

**null** é uma palavra-chave com referência nula para C# e C++. A palavra-chave do Microsoft Visual Basic para uma referência nula é **Nothing**.

O valor inicial padrão pode variar entre as propriedades de dependência, e não é necessariamente **null**. Além disso, muitas propriedades de dependência não aceitarão **null** como um valor (seja por marcação ou código) em função da implementação interna. Nesses casos, a definição de um valor de atributo XAML com **{x:Null}** pode resultar em uma exceção do analisador.

Alguns tipos do Windows Runtime são anuláveis. Quando um tipo que permite valor nulo ainda não tem **null** como o padrão, você pode usar **{x:Null}** no XAML para definir como o valor **null**. Ao usar extensões de componentes Visual C++ (C++/CX), os tipos que permitem valor nulo são representados como [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Ao usar linguagens Microsoft .NET, os tipos que permitem valor nulo são representados como [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## <a name="related-topics"></a>Tópicos relacionados

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 


