---
description: Identifica de forma exclusiva os elementos que são criados e usados como referência de recursos e que existam em um ResourceDictionary.
title: Atributo xKey
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cb42fcb17cfcad76989732b1a1482d9fbc85be5e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372320"
---
# <a name="xkey-attribute"></a>Atributo x:Key


Identifica de forma exclusiva os elementos que são criados e referenciados como origens e que existem em um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary).

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>Uso do atributo XAML (**ResourceDictionary** implícito)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| objeto | Qualquer objeto compartilhável. Veja [Referências de ResourceDictionary e recursos XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| stringKeyValue | Uma cadeia de caracteres verdadeira usada como chave, que deve estar de acordo com a gramática _XamlName_>. Veja a seção "Gramática XamlName" mais adiante. | 

##  <a name="xamlname-grammar"></a>Gramática XamlName

Veja a seguir a gramática normativa de uma cadeia de caracteres usada como chave na implementação XAML da Plataforma Universal do Windows (UWP):

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Os caracteres são restritos para o intervalo de ASCII menor e mais especificamente para o alfabeto romano em maiusculas e minúsculas, dígitos e o sublinhado (\_) caracteres.
-   Não há suporte à faixa de caracteres Unicode.
-   Um nome não pode começar com um dígito.

## <a name="remarks"></a>Comentários

Geralmente, os elementos filhos de um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) incluem um atributo **x:Key** que especifica um valor de chave exclusivo nesse dicionário. A exclusividade da chave é fiscalizada em tempo de carregamento pelo processador XAML. Os valores **x:Key** que não são exclusivos geram exceções de análise de XAML. Se for solicitado por [Extensão de marcação {StaticResource}](staticresource-markup-extension.md), uma chave não resolvida também vai gerar exceções de análise de XAML.

**x:Key** e [x:Name](x-name-attribute.md) não são conceitos idênticos. **x:Key** é usado exclusivamente em dicionários de recursos. x:Name é usado em todas as áreas de XAML. Uma chamada [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) que usa um valor de chave não recuperará um recurso inserido. Objetos definidos em um dicionário de recursos podem ter um **x:Key** um **x:Name** ou ambos. A chave e o nome não precisam corresponder.

Na sintaxe mostrada, observe que o objeto [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) está implícito no modo como o processador XAML produz um novo objeto para popular uma coleção de [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources).

O código equivalente à especificação de **x:Key** é qualquer operação que use uma chave com o [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) subjacente. Por exemplo, um **x:Key** aplicado na marcação de um recurso equivale ao valor do parâmetro *key* de **Insert** quando você adiciona o recurso a um **ResourceDictionary**.

Um item no dicionário de recursos pode omitir um valor para **x:Key** quando for um [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) ou [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) de destino; em cada um desses casos, a chave implícita do item de recurso é o valor **TargetType** interpretado como uma cadeia de caracteres. Para saber mais, veja [Guia de início rápido: estilo de controles](https://docs.microsoft.com/previous-versions/windows/apps/hh465498(v=win.10)) e [Referências de ResourceDictionary e recursos XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

