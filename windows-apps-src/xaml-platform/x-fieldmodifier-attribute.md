---
description: Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso público em vez do comportamento padrão privado.
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660911"
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

O valor do atributo **x:FieldModifier** varia de acordo com a linguagem de programação. Os valores válidos são **private**, **public**, **protected**, **internal** ou **friend**. Para C#, extensões de componentes do Microsoft Visual Basic ou Visual C++ (C + + c++ /CLI CX), você pode fornecer a cadeia de caracteres de valor "public" ou "Public"; o analisador não impõe o caso em que esse valor de atributo.

O acesso **Private** é o padrão.

**x:FieldModifier** só é relevante para elementos com um [atributo x:Name](x-name-attribute.md), pois esse nome é usado para fazer referência ao campo já que ele é público.

**Observação**  não dá suporte a XAML de tempo de execução do Windows **X:ClassModifier** ou **X:Subclass**.

