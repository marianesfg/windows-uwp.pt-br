---
author: jwmsft
description: "Identifica de forma exclusiva os elementos que são criados e usados como referência de recursos e que existam em um ResourceDictionary."
title: Atributo xKey
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
translationtype: Human Translation
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: 00d801dc3ebb8894f8e21ba0c1b9f3aecc981f30

---

# Atributo x:Key

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Identifica de forma exclusiva os elementos que são criados e referenciados como origens e que existem em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).

## Uso do atributo XAML

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## Uso do atributo XAML (**ResourceDictionary** implícito)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## Valores XAML

| Termo | Descrição |
|------|-------------|
| objeto | Qualquer objeto compartilhável. Veja [Referências de ResourceDictionary e recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273). |
| stringKeyValue | Uma cadeia de caracteres verdadeira usada como chave, que deve estar de acordo com a gramática _XamlName_>. Veja a seção "Gramática XamlName" mais adiante. | 

##  Gramática XamlName

Veja a seguir a gramática normativa de uma cadeia de caracteres usada como chave na implementação XAML da Plataforma Universal do Windows (UWP):

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Os caracteres são restritos à faixa ASCII inferior e, mais especificamente, às letras maiúsculas e minúsculas, aos dígitos e ao caractere de sublinhado (\_) do alfabeto romano.
-   Não há suporte à faixa de caracteres Unicode.
-   Um nome não pode começar com um dígito.

## Comentários

Geralmente, os elementos filhos de um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) incluem um atributo **x:Key** que especifica um valor de chave exclusivo nesse dicionário. A exclusividade da chave é fiscalizada em tempo de carregamento pelo processador XAML. Os valores **x:Key** que não são exclusivos geram exceções de análise de XAML. Se for solicitado por [Extensão de marcação {StaticResource}](staticresource-markup-extension.md), uma chave não resolvida também vai gerar exceções de análise de XAML.

**x:Key** e [x:Name](x-name-attribute.md) não são conceitos idênticos. **x:Key** é usado exclusivamente em dicionários de recursos. x:Name é usado em todas as áreas de XAML. Uma chamada [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) que usa um valor de chave não recuperará um recurso inserido.

Na sintaxe mostrada, observe que o objeto [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) está implícito no modo como o processador XAML produz um novo objeto para popular uma coleção de [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740).

O código equivalente à especificação de **x:Key** é qualquer operação que use uma chave com o [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) subjacente. Por exemplo, um **x:Key** aplicado na marcação de um recurso equivale ao valor do parâmetro *key* de **Insert** quando você adiciona o recurso a um **ResourceDictionary**.

Um item no dicionário de recursos pode omitir um valor para **x:Key** quando for um [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) ou [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) de destino; em cada um desses casos, a chave implícita do item de recurso é o valor **TargetType** interpretado como uma cadeia de caracteres. Para saber mais, veja [Guia de início rápido: estilo de controles](https://msdn.microsoft.com/library/windows/apps/hh465498) e [Referências de ResourceDictionary e recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).




<!--HONumber=Jun16_HO4-->


