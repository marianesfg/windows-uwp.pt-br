---
description: Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso público em vez do comportamento padrão privado.
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
---

# Atributo x:FieldModifier

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, veja o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Modifica o comportamento de compilação XAML de forma que os campos de referência de objetos nomeados sejam definidos com o acesso **público** em vez do comportamento padrão **privado**.

## Uso do atributo XAML

``` syntax
<object x:FieldModifier="public".../>
```

## Dependências

[O atributo x:Name](x-name-attribute.md) também deve ser incluído no mesmo elemento.

## Comentários

O valor do atributo **x:FieldModifier** varia de acordo com a linguagem de programação. Qual cadeia de caracteres a ser usada dependerá de como cada linguagem implementa seu **CodeDomProvider** e dos conversores de tipo que ela retorna para definir o significado de **TypeAttributes.Public** e **TypeAttributes.NotPublic**. Para extensões de componentes C#, Microsoft Visual Basic ou Visual C++ (C++/CX), você pode inserir o valor de cadeia de caracteres "public" ou "Public"; o analisador não impõe maiúsculas e minúsculas nesse valor de atributo.

Você também pode especificar **NonPublic** (**internal** em C# ou C++/CX, **Friend** em Visual Basic) mas isso não é o mais comum. O acesso interno não tem nenhuma aplicação no modelo de geração de código XAML do Windows Runtime. O padrão é o acesso particular.

**x:FieldModifier** só é relevante para elementos com um [atributo x:Name](x-name-attribute.md), pois esse nome é usado para fazer referência ao campo assim porque ele é público.

**Observação** O XAML do Windows Runtime não aceita **x:ClassModifier** ou **x:Subclass**.



<!--HONumber=Mar16_HO1-->


