---
author: jwmsft
description: Na marcação XAML, especifica um valor null para uma propriedade.
title: Extensão de marcação xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6033a01ee811977b3a37f820217005fdbd80616
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6442933"
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

Alguns tipos do Windows Runtime são anuláveis. Quando um tipo que permite valor nulo ainda não tem **null** como o padrão, você pode usar **{x:Null}** no XAML para definir como o valor **null**. Se usar VisualC + + extensões de componente (C++ c++ /CX), os tipos são representados como [**Platform:: ibox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Ao usar linguagens Microsoft .NET, os tipos que permitem valor nulo são representados como [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## <a name="related-topics"></a>Tópicos relacionados

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 

