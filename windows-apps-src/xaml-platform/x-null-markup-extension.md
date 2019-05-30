---
description: Na marcação XAML, especifica um valor null para uma propriedade.
title: Extensão de marcação xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0589249c65d301e3e74b305b92842fe4d3f61929
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372284"
---
# <a name="xnull-markup-extension"></a>Extensão de marcação {x:Null}


Na marcação XAML, especifica um valor **null** para uma propriedade.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Comentários

**null** é uma palavra-chave com referência nula para C# e C++. A palavra-chave do Microsoft Visual Basic para uma referência nula é **Nothing**.

O valor inicial padrão pode variar entre as propriedades de dependência, e não é necessariamente **null**. Além disso, muitas propriedades de dependência não aceitarão **null** como um valor (seja por marcação ou código) em função da implementação interna. Nesses casos, a definição de um valor de atributo XAML com **{x:Null}** pode resultar em uma exceção do analisador.

Alguns tipos do Windows Runtime são anuláveis. Quando um tipo que permite valor nulo ainda não tem **null** como o padrão, você pode usar **{x:Null}** no XAML para definir como o valor **null**. Se usando o Visual C++ extensões de componentes (C++/CX), tipos anuláveis são representados como [ **Platform:: ibox<T>** ](https://docs.microsoft.com/cpp/cppcx/platform-ibox-interface). Ao usar linguagens Microsoft .NET, os tipos que permitem valor nulo são representados como [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN).

## <a name="related-topics"></a>Tópicos relacionados

* [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN)
* [**IReference<T>** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_)
 

