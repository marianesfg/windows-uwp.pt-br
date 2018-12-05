---
description: Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso público em vez do comportamento padrão privado.
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8758756"
---
# <a name="xfieldmodifier-attribute"></a>Atributo x:FieldModifier


Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso **público** em vez do comportamento padrão **privado**.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Dependências

[O atributo x:Name](x-name-attribute.md) também deve ser incluído no mesmo elemento.

## <a name="remarks"></a>Comentários

O valor do atributo **x:FieldModifier** varia de acordo com a linguagem de programação. Os valores válidos são **private**, **public**, **protected**, **internal** ou **friend**. Para c#, Microsoft Visual Basic ou VisualC + + extensões de componente (C++ c++ /CX), você pode dar a cadeia de caracteres valor "public" ou "Public"; o analisador não impõe maiusculas e minúsculas nesse valor de atributo.

O acesso **Private** é o padrão.

**x:FieldModifier** só é relevante para elementos com um [atributo x:Name](x-name-attribute.md), pois esse nome é usado para fazer referência ao campo já que ele é público.

**Observação**XAML do Windows Runtime não dá suporte a **X:Subclass** ou **X:Subclass**.

