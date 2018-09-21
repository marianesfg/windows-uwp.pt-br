---
author: jwmsft
description: Explica como implementar uma propriedade anexada XAML como uma propriedade de dependência e como definir a convenção do acessador necessária para que a propriedade anexada possa ser usada no XAML.
title: Propriedades anexadas personalizadas
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.author: jimwalk
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: ce26242f1f5093afcbfb652a7d1736897975cb3a
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4090674"
---
# <a name="custom-attached-properties"></a>Propriedades anexadas personalizadas

Uma *propriedade anexada* é um conceito de XAML. Propriedades anexadas são geralmente definidas como uma forma especializada de propriedade de dependência. Este tópico explica como implementar uma propriedade anexada como uma propriedade de dependência e como definir a convenção do acessador necessário para que a propriedade anexada possa ser usada no XAML.

## <a name="prerequisites"></a>Pré-requisitos

Pressupomos que você compreende as propriedades de dependência sob a perspectiva de um consumidor de propriedades de dependência existentes e que você leu [Visão geral de propriedades de dependência](dependency-properties-overview.md). Você também deverá ter lido [Visão geral de propriedades anexadas](attached-properties-overview.md). Para seguir os exemplos deste tópico, você também tem que conhecer XAML e saber como gravar um aplicativo básico do Tempo de Execução do Windows em C++, C# ou Visual Basic.

## <a name="scenarios-for-attached-properties"></a>Cenários para propriedades anexadas

Você pode criar uma propriedade anexada quando, por algum motivo, precisa disponibilizar um mecanismo de definição de propriedade para outras classes que não a classe de definição. A maioria dos cenários comuns são layout e suporte de serviços. Exemplos de propriedades de layout existente são [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) e [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772). Em um cenário de layout, os elementos que existem como elementos filho para elementos de controle de layout podem expressar requisitos de layout para cada elemento pai. Cada um desses elementos configura um valor de propriedade que o pai define como propriedade anexada. Um exemplo do cenário de suporte de serviços na API do Tempo de Execução do Windows é o conjunto de propriedades anexadas de [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527), como [**ScrollViewer.IsZoomChainingEnabled**](https://msdn.microsoft.com/library/windows/apps/br209561).

> [!WARNING]
> Uma limitação existente da implementação XAML do Windows Runtime é que você não pode animar sua propriedade anexada personalizada.

## <a name="registering-a-custom-attached-property"></a>Registrando uma propriedade anexada personalizada

Se estiver definindo a propriedade anexada estritamente para uso em outros tipos, a classe em que a propriedade foi registrada não precisará derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Mas você realmente vai precisar ter o parâmetro de destino para que os acessadores usem **DependencyObject**, se seguir o modelo típico que determina que a propriedade anexada também será uma propriedade de dependência, de modo que você possa usar o repositório de propriedades existentes.

Defina sua propriedade anexada como uma propriedade de dependência declarando uma propriedade **public** **static** **readonly** do tipo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362). Essa propriedade é definida com o uso do valor de retorno do método [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833). O nome da propriedade deve coincidir com o nome da propriedade anexada especificada como o parâmetro **RegisterAttached** *name*, com a cadeia de caracteres "Property" acrescentada ao final. Esta é a convenção estabelecida para nomeação das entidades de propriedades de dependência em relação às propriedades que representam.

A diferença da área principal para definição de uma propriedade anexada personalizada em relação à propriedade de dependência personalizada está na forma como você define acessadores ou wrappers. Em vez de usar a técnica de wrapper descrita em [Propriedades de dependência personalizadas](custom-dependency-properties.md), você também deve fornecer estático **obter * * * PropertyName* e **Definir * * * PropertyName* métodos acessadores para a propriedade anexada. Os acessadores são usados principalmente pelo analisador XAML, embora qualquer outro chamador também possa usá-los para definir cenários não XAML.

> [!IMPORTANT]
> Se você não definir os acessadores corretamente, o processador XAML não pode acessar a propriedade anexada e qualquer pessoa que tenta usá-lo, provavelmente receberá um erro do analisador XAML. Além disso, as ferramentas de design e codificação normalmente dependem das convenções "\*Property" para nomeação de identificadores, quando elas encontram um propriedade de dependência personalizada em um assembly referenciado.

## <a name="accessors"></a>Acessadores

A assinatura do acessador **Get**_PropertyName_ deve ser esta.

`public static` _valueType_ **Get**_PropertyName_ `(DependencyObject target)`

Para Microsoft Visual Basic, é esta.

`Public Shared Function Get`_PropertyName_`(ByVal target As DependencyObject) As `_valueType_`)`

O objeto *target* pode ser de um tipo mais específico na sua implementação, mas deve derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). O valor de retorno de *valueType* também pode ser de um tipo mais específico na sua implementação. O tipo **Object** básico é aceitável, mas geralmente o que você quer é que a propriedade anexada imponha a segurança de tipo. O uso de digitação nas assinaturas getter e setter é uma técnica recomendada de segurança de tipo.

A assinatura para o **Definir * * * PropertyName* acessador deve ser esta.

`public static void Set`_PropertyName_` (DependencyObject target , `_valueType_` value)`

Para Visual Basic, é esta.

`Public Shared Sub Set`_PropertyName_` (ByVal target As DependencyObject, ByVal value As `_valueType_`)`

O objeto *target* pode ser de um tipo mais específico na sua implementação, mas deve derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). O objeto *value* e seu *valueType* também podem ser de um tipo mais específico na sua implementação. Lembre-se de que o valor desse método é a entrada que vem do processador XAML quando ele encontra a sua propriedade anexada na marcação. É preciso haver suporte para conversão de tipo ou extensão de marcação existente para o tipo usado, para que o tipo adequado possa ser criado com base num valor de atributo (que, em última análise, é apenas uma cadeia de caracteres). O tipo **Object** básico é aceitável, mas geralmente o que você quer é mais segurança de tipo. Para fazer isso, coloque a aplicação de tipo nos acessadores.

> [!NOTE]
> Também é possível definir uma propriedade anexada onde o uso pretendido é por meio da sintaxe de elemento de propriedade. Nesse caso, você não precisa de conversão de tipos para os valores, mas precisa garantir que os valores que você pretende possam ser criados em XAML. [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) é um exemplo de uma propriedade anexada existente que dá suporte apenas ao uso de elemento de propriedade.

## <a name="code-example"></a>Exemplo de código

Este exemplo de código mostra o registro de propriedade de dependência (usando o método [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833)) e também os acessadores **Get** e **Set** para uma propriedade anexada personalizada. No exemplo, o nome da propriedade anexada é `IsMovable`. Portanto, os nomes dos acessadores devem ser `GetIsMovable` e `SetIsMovable`. O proprietário da propriedade anexada é uma classe de serviço denominada `GameService` que não tem uma interface do usuário própria; sua finalidade é apenas fornecer os serviços de propriedade anexada quando for usada a propriedade anexada **GameService.IsMovable**.

Definindo a propriedade anexada em C++ c++ /CX é um pouco mais complexo. Você precisa decidir como fatorar entre o cabeçalho e o arquivo de código. Além disso, você deve expor o identificador como uma propriedade com apenas um acessador **get**, pelos motivos abordados em [Propriedades de dependência personalizadas](custom-dependency-properties.md). No C++ c++ /CX, você deve definir essa relação propriedade-campo explicitamente em vez de confiar na palavra de **somente leitura** do .NET e implícitas fazendo de propriedades simples. Você também precisa executar o registro da propriedade anexada em uma função auxiliar que é executada apenas uma vez, quando o aplicativo é iniciado pela primeira vez, mas antes de qualquer página XAML que precisa da propriedade anexada seja carregada. O local típico para chamar as funções do auxiliar de registro da propriedade para qualquer e toda dependência ou propriedades anexadas está dentro do construtor **App** / [**Application**](https://msdn.microsoft.com/library/windows/apps/br242325) no código de seu arquivo app.xaml.

```csharp
public class GameService : DependencyObject
{
    public static readonly DependencyProperty IsMovableProperty = 
    DependencyProperty.RegisterAttached(
      "IsMovable",
      typeof(Boolean),
      typeof(GameService),
      new PropertyMetadata(false)
    );
    public static void SetIsMovable(UIElement element, Boolean value)
    {
        element.SetValue(IsMovableProperty, value);
    }
    public static Boolean GetIsMovable(UIElement element)
    {
        return (Boolean)element.GetValue(IsMovableProperty);
    }
}
```

```vb
Public Class GameService
    Inherits DependencyObject

    Public Shared ReadOnly IsMovableProperty As DependencyProperty = 
        DependencyProperty.RegisterAttached("IsMovable",  
        GetType(Boolean), 
        GetType(GameService), 
        New PropertyMetadata(False))

    Public Shared Sub SetIsMovable(ByRef element As UIElement, value As Boolean)
        element.SetValue(IsMovableProperty, value)
    End Sub

    Public Shared Function GetIsMovable(ByRef element As UIElement) As Boolean
        GetIsMovable = CBool(element.GetValue(IsMovableProperty))
    End Function
End Class
```

```cppwinrt
// GameService.idl
namespace UserAndCustomControls
{
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        Boolean IsMovable;
    }
}

// GameService.h
...
    bool IsMovable(){ return winrt::unbox_value<bool>(GetValue(m_IsMovableProperty)); }
    void IsMovable(bool value){ SetValue(m_IsMovableProperty, winrt::box_value(value)); }
    Windows::UI::Xaml::DependencyProperty IsMovableProperty(){ return m_IsMovableProperty; }

private:
    static Windows::UI::Xaml::DependencyProperty m_IsMovableProperty;
...

// GameService.cpp
...
Windows::UI::Xaml::DependencyProperty GameService::m_IsMovableProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsMovable",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<UserAndCustomControls::GameService>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false) }
);
...
```

```cpp
// GameService.h
#pragma once

#include "pch.h"
//namespace WUX = Windows::UI::Xaml;

namespace UserAndCustomControls {
    public ref class GameService sealed : public WUX::DependencyObject {
    private:
        static WUX::DependencyProperty^ _IsMovableProperty;
    public:
        GameService::GameService();
        void GameService::RegisterDependencyProperties();
        static property WUX::DependencyProperty^ IsMovableProperty
        {
            WUX::DependencyProperty^ get() {
                return _IsMovableProperty;
            }
        };
        static bool GameService::GetIsMovable(WUX::UIElement^ element) {
            return (bool)element->GetValue(_IsMovableProperty);
        };
        static void GameService::SetIsMovable(WUX::UIElement^ element, bool value) {
            element->SetValue(_IsMovableProperty,value);
        }
    };
}

// GameService.cpp
#include "pch.h"
#include "GameService.h"

using namespace UserAndCustomControls;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Documents;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Interop;
using namespace Windows::UI::Xaml::Media;

GameService::GameService() {};

GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## <a name="using-your-custom-attached-property-in-xaml"></a>Usando sua propriedade anexada personalizada no XAML

Depois de definir a propriedade anexada e incluir seus membros de suporte como parte de um tipo personalizado, disponibilize as definições para uso do XAML. Para isso, mapeie um namespace XAML que faça referência ao namespace do código contendo a classe relevante. Nos casos em que você tiver definido a propriedade anexada como parte de uma biblioteca, inclua essa biblioteca no pacote do aplicativo.

Um mapeamento de namespace XML para XAML é geralmente colocado no elemento raiz de uma página XAML. Por exemplo, para uma classe com o nome `GameService` no namespace `UserAndCustomControls` que contém as definições de propriedade anexada mostradas nos trechos anteriores, o mapeamento poderá ter esta aparência.

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

Usando o mapeamento, você pode definir sua propriedade anexada `GameService.IsMovable` em qualquer elemento que corresponda à definição de destino, incluindo um tipo existente definido pelo Tempo de Execução do Windows.

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

Se estiver definindo a propriedade em um elemento que também está no mesmo namespace XML mapeado, ainda assim inclua o prefixo no nome da propriedade anexada. Isso porque o prefixo qualifica o tipo de proprietário. O atributo da propriedade anexada não pode ser considerado como estando no mesmo namespace XML do elemento em que o atributo está incluído, mesmo que, pelas regras XML normais, os atributos possam herdar o namespace dos elementos. Por exemplo, se estiver definindo `GameService.IsMovable` em um tipo personalizado de `ImageWithLabelControl` (definição não exibida), e mesmo que ambos tenham sido definidos no mesmo namespace de código mapeado para o mesmo prefixo, o XAML ainda será este.

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> Se você estiver escrevendo uma UI XAML com C++, você deve incluir o cabeçalho para o tipo personalizado que define a propriedade anexada, sempre que uma página XAML usa esse tipo. Cada página XAML tem um cabeçalho code-behind .xaml.h associado. É aí que você deve incluir (usando **\#include**) o cabeçalho da definição do tipo de proprietário da propriedade anexada.

## <a name="value-type-of-a-custom-attached-property"></a>Tipo de valor de uma propriedade anexada personalizada

O tipo usado como o tipo de valor de uma propriedade anexada personalizada afeta o uso, a definição ou ambas. O tipo de valor da propriedade anexada é declarado em vários lugares: nas assinaturas dos métodos de acessador **Get** e **Set** e também como o parâmetro *propertyType* da chamada [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833).

O tipo de valor mais comum para propriedades anexadas (personalizadas ou não) é uma cadeia de caracteres simples. Isso porque as propriedades anexadas geralmente são criadas para uso de atributo XAML, e o uso de uma cadeia de caracteres como o tipo de valor deixa as propriedades mais leves. Outros primitivos que têm conversão nativa para os métodos de cadeia de caracteres – como inteiro, duplo ou um valor de enumeração – também são tipos de valor comuns para propriedades anexadas. Você pode usar outros tipos de valor (aqueles que não dão suporte para conversão nativa de cadeia de caracteres) como o valor de propriedade anexada. Entretanto, isso envolve inevitavelmente fazer uma escolha de uso ou de implementação:

- Você pode deixar a propriedade anexada como está, mas ela poderá dar suporte ao uso apenas quando for um elemento de propriedade e se o valor for declarado como um elemento de objeto. Nesse caso, o tipo de propriedade não permite o uso de XAML como um elemento de objeto. Para classes de referência existentes do Tempo de Execução do Windows, verifique a sintaxe XAML para garantir que o tipo dá suporte ao uso de elemento de objeto XAML.
- Você pode deixar a propriedade anexada como está, mas use-a somente em um uso de atributo via técnica de referência XAML, como **Binding** ou **StaticResource**, que possa ser expresso como uma cadeia de caracteres.

## <a name="more-about-the-canvasleft-example"></a>Mais sobre o exemplo **Canvas.Left**

Em exemplos anteriores dos usos de propriedades anexadas, mostramos as diferentes maneiras de definir a propriedade anexada [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771). Mas o que será que muda em relação a como o [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) interage com seu objeto, e quando isso acontece? Vamos examinar mais a fundo este exemplo específico, porque se você implementar uma propriedade anexada, será interessante ver o que mais uma típica classe de proprietário de propriedade anexada pretende fazer com os valores de propriedade anexada se encontrá-los em outros objetos.

A função principal de um [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) é ser um contêiner de layout de posição absoluta na interface do usuário. Os filhos de um **Canvas** são armazenados em uma propriedade definida pela classe base [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514). De todos os painéis, **Canvas** é o único que usa posicionamento absoluto. Ele teria inchado o modelo de objeto do tipo [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) comum para adicionar propriedades que só poderiam ser motivo de preocupação para **Canvas** e para aqueles casos de **UIElement** em que eles são elementos filho de um **UIElement**. A definição das propriedades de controle de layout de um **Canvas** para que sejam propriedades anexadas que qualquer **UIElement** pode usar mantém o modelo de objeto mais limpo.

Para ser um painel prático, [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) tem um comportamento que substitui os métodos [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) e [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) de nível de estrutura. É aí que **Canvas** realmente verifica se há valores de propriedades anexadas em seus filhos. Parte dos padrões **Measure** e **Arrange** é um loop que itera sobre qualquer conteúdo, e um painel tem a propriedade [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) que torna explícito o que é deve ser considerado o filho de um painel. Assim, o **Canvas** o comportamento do layout itera através desses filhos e faz chamadas de [**Canvas.GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) e [**Canvas.GetTop**](https://msdn.microsoft.com/library/windows/apps/br209270) estáticas em cada filho para ver se essas propriedades anexadas contêm um valor que não é padrão (o padrão é 0). Esses valores são usados para posicionar absolutamente cada filho no espaço de layout disponível de **Canvas** de acordo com os valores específicos fornecidos por cada filho e confirmados usando **Arrange**.

O código será semelhante este pseudocódigo.

```syntax
protected override Size ArrangeOverride(Size finalSize)
{
    foreach (UIElement child in Children)
    {
        double x = (double) Canvas.GetLeft(child);
        double y = (double) Canvas.GetTop(child);
        child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
    }
    return base.ArrangeOverride(finalSize); 
    // real Canvas has more sophisticated sizing
}
```

> [!NOTE]
> Para obter mais informações sobre como os painéis funcionam, consulte [Visão geral de painéis personalizados XAML](https://msdn.microsoft.com/library/windows/apps/mt228351).

## <a name="related-topics"></a>Tópicos relacionados

* [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833)
* [Visão geral das propriedades anexadas](attached-properties-overview.md)
* [Propriedades de dependência personalizadas](custom-dependency-properties.md)
* [Visão geral do XAML](xaml-overview.md)
