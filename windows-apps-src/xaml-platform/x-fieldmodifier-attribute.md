---
author: jwmsft
description: Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso público em vez do comportamento padrão privado.
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: cad84be24836bc6a33a4ab08f1ca4fa2d9e97512
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.locfileid: "200676"
---
# <a name="xfieldmodifier-attribute"></a>Atributo x:FieldModifier

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso **público** em vez do comportamento padrão **privado**.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Dependências

[O atributo x:Name](x-name-attribute.md) também deve ser incluído no mesmo elemento.

## <a name="remarks"></a>Comentários

O valor do atributo **x:FieldModifier** varia de acordo com a linguagem de programação. Os valores válidos são **private**, **public**, **protected**, **internal** ou **friend**. Para extensões de componentes C#, Microsoft Visual Basic ou Visual C++ (C++/CX), você pode inserir o valor de cadeia de caracteres "public" ou "Public"; o analisador não impõe maiúsculas e minúsculas nesse valor de atributo.

O acesso **Private** é o padrão.

**x:FieldModifier** só é relevante para elementos com um [atributo x:Name](x-name-attribute.md), pois esse nome é usado para fazer referência ao campo já que ele é público.

**Observação** O XAML do Windows Runtime não aceita **x:ClassModifier** ou **x:Subclass**.

