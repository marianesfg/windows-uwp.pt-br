---
description: Identifica de forma exclusiva os elementos de objetos para obter acesso ao objeto instanciado de code-behind ou código geral.
title: Atributo xName
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 187a937c82c83f4653e58e7699a21051d03b09e7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372299"
---
# <a name="xname-attribute"></a>Atributo x:Name


Identifica de forma exclusiva os elementos de objetos para obter acesso ao objeto instanciado de code-behind ou código geral. Após aplicação em um modelo de programação de suporte, **x:Name** pode ser considerado equivalente à variável que mantém a referência do objeto, conforme retornada por um construtor.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| XAMLNameValue | Uma cadeia em conformidade com as restrições da gramática XamlName. |

##  <a name="xamlname-grammar"></a>Gramática XamlName

A seguir, você encontrará a gramática normativa para uma cadeia usada como chave nesta implementação XAML:

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
-   Um nome não pode começar com um dígito. Algumas implementações de ferramenta preceder um caractere de sublinhado (\_) para uma cadeia de caracteres se o usuário fornece um dígito como caractere inicial ou o ferramenta poupando **X:Name** valores com base em outros valores que contêm dígitos.

## <a name="remarks"></a>Comentários

O atributo **x:Name** torna-se o nome de um campo criado no código subjacente quando o código XAML é processado. Esse campo mantém uma referência ao objeto. O processo de criação desse campo é realizado pelas etapas de destino do MSBuild, que também são responsáveis por associar as classes parciais de um arquivo XAML e seu code-behind. Esse comportamento não é necessariamente especificado pela linguagem XAML. É a implementação particular que a programação da Plataforma Universal do Windows (UWP) para XAML aplica para usar **x:Name** em seus modelos de programação e aplicativo.

Cada atributo **x:Name** definido deve ser exclusivo em um namescope XAML. Geralmente, um namescope XAML é definido no nível do elemento raiz de uma página carregada e contém todos os elementos sob esse elemento em uma única página XAML. Namescopes XAML adicionais são definidos por qualquer modelo de controle ou de dados que seja definido nessa página. No tempo de execução, outro namescope XAML é criado para a raiz da árvore de objetos que é criada de um modelo de controle aplicado e também por árvores de objetos criadas através de uma chamada a [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load). Para saber mais, veja [Namescopes XAML](xaml-namescopes.md).

As ferramentas de design muitas vezes geram valores **x:Name** automaticamente para elementos quando são introduzidos na superfície de design. O esquema de geração automática varia dependendo do designer usado, mas um esquema típico é gerar uma cadeia que comece com o nome da classe que apoia o elemento, seguido por um inteiro adiantado. Por exemplo, se você inserir o primeiro elemento [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) no designer, poderá ver que, no XAML, esse elemento tem o valor de atributo **x:Name** de "Button1".

**x:Name** não pode ser definido na sintaxe do elemento da propriedade XAML ou em códigos que usam [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue). **x:Name** só pode ser definido usando sintaxe de atributo XAML em elementos.

**Observação**  especificamente para C++aplicativos /CX, um campo de suporte para uma **X:Name** referência não foi criada para o elemento raiz de um arquivo XAML ou página. Se você precisa mencionar o objeto raiz do code-behind C++, use outras APIs ou outro percurso de árvore. Por exemplo, você pode chamar [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) para um elemento filho nomeado conhecido e depois chamar [**Parent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.parent).

### <a name="xname-and-other-name-properties"></a>x:Name e outras propriedades de Name

Alguns tipos usados no XAML da UWP também têm uma propriedade chamada **Name**. Por exemplo, [**FrameworkElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.name) e [**TextElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement.name).

Se **Name** estiver disponível como uma propriedade definível em um elemento, **Name** e **x:Name** poderão ser usados de forma intercambiável em XAML, mas ocorrerá erro se os dois atributos forem especificados no mesmo elemento. Também há casos em que há uma propriedade **Name** mas que é somente leitura (como [**VisualState.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate.name)). Se essa for a situação, use sempre **x:Name** para nomear esse elemento no XAML, e o **Name** somente leitura existirá para algum cenário de código menos comum.

**Observação**  [**FrameworkElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.name) geralmente não deve ser usada como uma maneira de mudar valores originariamente definidos por **x:Name**, apesar de haver alguns cenários de exceção a essa regra geral. Em cenários típicos, a criação e definição de namescopes XAML é uma operação do processador XAML. A modificação de **FrameworkElement.Name** no tempo de execução pode resultar em um alinhamento inconsistente de nomenclatura de campo privado/namescope XAML, que será difícil de controlar no code-behind.

### <a name="xname-and-xkey"></a>x:Name e x:Key

**x:Name** pode ser aplicado como um atributo aos elementos em um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) para atuar como substituto do [atributo x:Key](x-key-attribute.md). (A regra é que todos os elementos em uma **ResourceDictionary** deve ter um atributo X:Key ou X:Name.) Isso é comum para [Storyboarded animações](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations). Para obter mais informações, consulte a seção de [Referências de ResourceDictionary e recursos XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

