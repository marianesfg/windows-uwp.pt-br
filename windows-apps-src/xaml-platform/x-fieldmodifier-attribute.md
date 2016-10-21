---
author: jwmsft
description: "Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso público em vez do comportamento padrão privado."
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
translationtype: Human Translation
ms.sourcegitcommit: 3144758352b99f8c145a3c7be8a6c43d6a002104
ms.openlocfilehash: f812c9498d5519aac8ab750f0c55423966a63464

---

# Atributo x:FieldModifier

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso **público** em vez do comportamento padrão **privado**.

## Uso do atributo XAML

``` syntax
<object x:FieldModifier="public".../>
```

## Dependências

[O atributo x:Name](x-name-attribute.md) também deve ser incluído no mesmo elemento.

## Comentários

O valor do atributo **x:FieldModifier** varia de acordo com a linguagem de programação. Os valores válidos são **private**, **public**, **protected**, **internal** ou **friend**. Para extensões de componentes C#, Microsoft Visual Basic ou Visual C++ (C++/CX), você pode inserir o valor de cadeia de caracteres "public" ou "Public"; o analisador não impõe maiúsculas e minúsculas nesse valor de atributo.

O acesso **Private** é o padrão.

**x:FieldModifier** só é relevante para elementos com um [atributo x:Name](x-name-attribute.md), pois esse nome é usado para fazer referência ao campo já que ele é público.

**Observação** O XAML do Windows Runtime não aceita **x:ClassModifier** ou **x:Subclass**.




<!--HONumber=Aug16_HO3-->


