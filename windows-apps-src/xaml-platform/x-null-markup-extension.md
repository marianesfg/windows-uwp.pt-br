---
description: Na marcação XAML, especifica um valor nulo para uma propriedade.
title: Extensão de marcação xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
---

# Extensão de marcação {x:Null}

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, veja o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Na marcação XAML, especifica um valor **null** para uma propriedade.

## Uso do atributo XAML

``` syntax
<object property="{x:Null}" .../>
```

## Comentários

**null** é uma palavra-chave com referência nula para C# e C++. A palavra-chave do Microsoft Visual Basic para uma referência nula é **Nothing**.

O valor inicial padrão pode variar entre as propriedades de dependência, e não é necessariamente **null**. Além disso, muitas propriedades de dependência não aceitarão **null** como um valor (seja por marcação ou código) em função da implementação interna. Nesses casos, a definição de um valor de atributo XAML com **{x:Null}** pode resultar em uma exceção do analisador.

Alguns tipos do Windows Runtime são anuláveis. Quando um tipo que permite valor nulo ainda não tem **null** como o padrão, você pode usar **{x:Null}** no XAML para definir como o valor **null**. Ao usar extensões de componentes Visual C++ (C++/CX), os tipos que permitem valor nulo são representados como [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Ao usar linguagens Microsoft .NET, os tipos que permitem valor nulo são representados como [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## Tópicos relacionados

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 



<!--HONumber=Mar16_HO1-->


