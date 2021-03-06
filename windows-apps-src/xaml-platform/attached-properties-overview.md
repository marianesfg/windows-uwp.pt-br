---
description: Explica o conceito de uma propriedade anexada no XAML e fornece alguns exemplos.
title: Visão geral das propriedades anexadas
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: d3892857baa29e2275845cb077e5ad9ea3166ada
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340606"
---
# <a name="attached-properties-overview"></a>Visão geral das propriedades anexadas

Uma *propriedade anexada* é um conceito de XAML. As propriedades anexadas habilitam pares de propriedade/valor adicionais a serem definidas em um objeto, mas as propriedades não fazem parte da definição do objeto original. Tipicamente, as propriedades anexadas são definidas como uma forma especializada de propriedade de dependência que não tem um wrapper de propriedade convencional no modelo de objeto do tipo de proprietário.

## <a name="prerequisites"></a>Pré-requisitos

Consideramos que você entende o conceito básico das propriedades de dependência e já leu a [Visão geral das propriedades de dependência](dependency-properties-overview.md).

## <a name="attached-properties-in-xaml"></a>Propriedades anexadas em XAML

Em XAML, você define propriedades anexadas usando a sintaxe _AttachedPropertyProvider.PropertyName_. Veja um exemplo de como é possível definir [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) em XAML.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> Estamos apenas usando [**Canvas. Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) como uma propriedade anexada de exemplo sem explicar totalmente por que você a usaria. Se quiser saber mais sobre a finalidade de **Canvas.Left** e como [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) manipula seus filhos de layout, consulte o tópico de referência [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) ou [Definir layouts com XAML](https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml).

## <a name="why-use-attached-properties"></a>Por que usar propriedades anexadas?

As propriedades anexadas são uma maneira de evitar as convenções de codificação que podem impedir que objetos diferentes em um relacionamento transmitam informações entre si em tempo de execução. É certamente possível colocar as propriedades em uma classe base comum para que cada objeto possa simplesmente obter e definir essa propriedade. Mas por fim o grande número de cenários em que você pode querer fazer isso inchará suas classes base com propriedades compartilháveis. Isso pode inclusive introduzir casos em que talvez haja duas centenas de descendentes tentando usar uma propriedade. Isso não é um ótimo design de classe. Para resolver isso, o conceito de propriedade anexada habilita um objeto para atribuir um valor a uma propriedade cuja estrutura de classe não define. A classe de definição pode ler o valor de objetos filho em tempo de execução depois que vários objetos são criados em uma árvore de objetos.

Por exemplo, elementos filho podem usar propriedades anexadas para informar o elemento pai sobre o modo como são apresentados na interface do usuário. Esse é o caso com a propriedade anexada [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left). **Canvas.Left** é criada como uma propriedade anexada porque ela define elementos contidos dentro de um elemento [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), em vez de dentro da **Canvas** propriamente dita. Qualquer elemento filho possível então usa **Canvas.Left** e [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top) para especificar seu deslocamento de layout dentro do pai do contêiner de layout **Canvas**. As propriedades anexadas possibilitam que isso funcione sem desorganizar o modelo de objeto do elemento base com muitas propriedades que se aplicam somente a um dos muitos contêineres de layout possíveis. Em vez disso, muitos contêineres de layout implementam seu próprio conjunto de propriedades anexadas.

Para implementar a propriedade anexada, a classe [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) define um campo estático [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) chamado [**Canvas.LeftProperty**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.leftproperty). Em seguida, **Canvas** fornece os métodos [**SetLeft**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.setleft) e [**GetLeft**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.getleft) como acessadores públicos para a propriedade anexada, para habilitar a configuração de XAML e o acesso do valor de tempo de execução. Para XAML e para o sistema de propriedades de dependência, esse conjunto de APIs satisfaz um padrão que habilita uma sintaxe XAML específica para propriedades anexadas e armazena o valor no repositório de propriedades de dependência.

## <a name="how-the-owning-type-uses-attached-properties"></a>Como o tipo proprietário usa as propriedades anexadas

Embora as propriedades anexadas possam ser definidas em qualquer elemento XAML (ou em qualquer [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) subjacente), isso não significa automaticamente que a configuração da propriedade produz um resultado tangível ou que o valor seja acessado. O tipo que define a propriedade anexada normalmente é semelhante a um destes cenários:

- O tipo que define a propriedade anexada é o pai em um relacionamento de outros objetos. Os objetos filho definirão valores para a propriedade anexada. O tipo proprietário da propriedade anexada tem algum comportamento inato que itera por meio de seus elementos filho, obtém os valores e atua sobre esses valores em algum momento na vida útil do objeto (uma ação de layout, [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) etc.)
- O tipo que define a propriedade anexada é usado como elemento filho de uma variedade de possíveis elementos pai e modelos de conteúdo, mas as informações não são necessariamente informações de layout.
- A propriedade anexada relata informações para um serviço, e não para outro elemento de interface do usuário.

Para saber mais sobre esses cenários e tipos de propriedade, consulte a seção "Mais sobre Canvas.Left" de [Propriedades anexadas personalizadas](custom-attached-properties.md).

## <a name="attached-properties-in-code"></a>Propriedades anexadas no código

Propriedades anexadas não têm os wrappers de propriedade típicos para fácil acesso de obtenção e definição, se comparadas às propriedades de dependência. Isso acontece porque a propriedade anexada não necessariamente faz parte do modelo de objeto focado no código para instâncias onde a propriedade foi definida. (É possível, mas incomum, definir uma propriedade que seja uma propriedade anexada que outros tipos possam definir por si só e que, ao mesmo tempo, tenha um uso de propriedade convencional no tipo proprietário.)

Há duas formas de definir uma propriedade anexada em código: usar as APIs do sistema de propriedades ou usar os acessadores de padrão XAML. As duas técnicas são bastante equivalentes em termos de resultado final. Assim, a escolha é basicamente uma questão de estilo de codificação.

### <a name="using-the-property-system"></a>Usando o sistema de propriedades

As propriedades anexadas do Windows Runtime são implementadas como propriedades de dependência, portanto, o sistema de propriedades pode armazenar os valores no repositório de propriedades de dependência. Sendo assim, as propriedades anexadas expõem um identificador de propriedade de dependência na classe proprietária.

Para definir uma propriedade anexada no código, chame o método [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) e passe o campo [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) que serve como identificador dessa propriedade anexada. (Você também passa o valor a ser definido.)

Para obter o valor de uma propriedade anexada no código, chame o método [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) e passe novamente o campo [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) que serve como identificador dessa propriedade anexada.

### <a name="using-the-xaml-accessor-pattern"></a>Usando o padrão de acessador XAML

Um processador XAML deve ser capaz de definir esses valores de propriedade anexada quando o XAML é analisado em uma árvore de objetos. O tipo proprietário da propriedade anexada deve implementar métodos acessadores dedicados denominados segundo a forma **Get**_NomeDaPropriedade_ e **Set**_NomeDaPropriedade_. Esses métodos acessadores dedicados também são uma boa maneira de obter ou definir a propriedade anexada em código. Sob a perspectiva de código, uma propriedade anexada é similar a um campo existente que contém acessadores de método em vez de acessadores de propriedade. Esse campo existente pode existir em qualquer objeto, não sendo necessário defini-lo especificamente.

O próximo exemplo mostra como você pode definir uma propriedade anexada no código usando a API do acessador XAML. Neste exemplo, `myCheckBox` é uma instância da classe [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox). A última linha é o código que efetivamente define o valor; as linhas anteriores apenas estabelecem as instâncias e o relacionamento pai-filho. A última linha não comentada será a sintaxe se você usar o sistema de propriedades. A última linha comentada será a sintaxe se você usar o padrão do acessador XAML.

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>Propriedades anexadas personalizadas

Para obter exemplos de código sobre como definir propriedades anexadas personalizadas e saber mais sobre os cenários que as utilizam, consulte [Propriedades anexadas personalizadas](custom-attached-properties.md).

## <a name="special-syntax-for-attached-property-references"></a>Sintaxe especial para referências de propriedades anexadas

O ponto no nome de uma propriedade anexada é uma parte essencial do padrão de identificação. Ocorrem ambiguidades quando uma sintaxe ou situação considera que o ponto tem outro significado. Por exemplo, o ponto é considerado como a passagem de um modelo-objeto para um caminho de associação. Na maioria dos casos que envolvem essa ambiguidade, há uma sintaxe especial para uma propriedade anexada que habilita o ponto interno a ser analisado como o separador _proprietário_ **.** _propriedade_ de uma propriedade anexada.

- Para especificar uma propriedade anexada como parte de um caminho de destino para uma animação, coloque o nome da propriedade anexada entre parênteses ("()") — por exemplo, "(Canvas.Left)". Para obter mais informações, consulte [Sintaxe do Property-path](property-path-syntax.md).

> [!WARNING]
> Uma limitação existente da implementação Windows Runtime XAML é que você não pode animar uma propriedade anexa personalizada.

- Para especificar uma propriedade anexada como a propriedade de destino para uma referência de recurso de um arquivo de recurso para **x:Uid**, use uma sintaxe especial que injeta um estilo de código, totalmente qualificado **usando:** declaração dentro de colchetes ("\[\]"), para criar uma quebra de escopo deliberada. Por exemplo, supondo que exista um elemento `<TextBlock x:Uid="Title" />`, a chave de recurso no arquivo de recurso que tem como alvo o valor **Canvas. Top** nessa instância é "title.\[usando: Windows. UI. XAML. Controls\]Canvas. Top ". Para obter mais informações sobre arquivos de recurso e XAML, consulte [Início rápido: Traduzindo recursos da interface do usuário](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10)).

## <a name="related-topics"></a>Tópicos relacionados

- [Propriedades anexadas personalizadas](custom-attached-properties.md)
- [Visão geral das propriedades de dependência](dependency-properties-overview.md)
- [Definir layouts com XAML](https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml)
- [Início rápido: convertendo recursos de interface do usuário](https://docs.microsoft.com/previous-versions/windows/apps/hh943060(v=win.10))
- [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue)
- [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue)
