---
description: Explica como implementar uma propriedade anexada XAML como uma propriedade de dependência e como definir a convenção do acessador necessária para que a propriedade anexada possa ser usada no XAML.
title: Propriedades anexadas personalizadas
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.date: 07/18/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f23d66acc9371fd7b23b6770a0c7be6d16f86be4
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340617"
---
# <a name="custom-attached-properties"></a>Propriedades anexadas personalizadas

Uma *propriedade anexada* é um conceito de XAML. Propriedades anexadas são geralmente definidas como uma forma especializada de propriedade de dependência. Este tópico explica como implementar uma propriedade anexada como uma propriedade de dependência e como definir a convenção do acessador necessário para que a propriedade anexada possa ser usada no XAML.

## <a name="prerequisites"></a>Pré-requisitos

Pressupomos que você compreende as propriedades de dependência sob a perspectiva de um consumidor de propriedades de dependência existentes e que você leu [Visão geral de propriedades de dependência](dependency-properties-overview.md). Você também deverá ter lido [Visão geral de propriedades anexadas](attached-properties-overview.md). Para seguir os exemplos deste tópico, você também precisa conhecer XAML e saber como gravar um aplicativo básico do Windows Runtime em C++, C# ou Visual Basic.

## <a name="scenarios-for-attached-properties"></a>Cenários para propriedades anexadas

Você pode criar uma propriedade anexada quando, por algum motivo, precisa disponibilizar um mecanismo de definição de propriedade para outras classes que não a classe de definição. A maioria dos cenários comuns são layout e suporte de serviços. Exemplos de propriedades de layout existente são [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) e [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top). Em um cenário de layout, os elementos que existem como elementos filho para elementos de controle de layout podem expressar requisitos de layout para cada elemento pai. Cada um desses elementos configura um valor de propriedade que o pai define como propriedade anexada. Um exemplo do cenário de suporte de serviços na API do Tempo de Execução do Windows é o conjunto de propriedades anexadas de [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), como [**ScrollViewer.IsZoomChainingEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoomchainingenabled).

> [!WARNING]
> Uma limitação existente da implementação Windows Runtime XAML é que você não pode animar sua propriedade anexa personalizada.

## <a name="registering-a-custom-attached-property"></a>Registrando uma propriedade anexada personalizada

Se estiver definindo a propriedade anexada estritamente para uso em outros tipos, a classe em que a propriedade foi registrada não precisará derivar de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). Mas você realmente vai precisar ter o parâmetro de destino para que os acessadores usem **DependencyObject**, se seguir o modelo típico que determina que a propriedade anexada também será uma propriedade de dependência, de modo que você possa usar o repositório de propriedades existentes.

Defina a propriedade anexada como uma propriedade de dependência, declarando uma propriedade **somente leitura** **estática** e **pública** do tipo [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty). Essa propriedade é definida com o uso do valor de retorno do método [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached). O nome da propriedade deve corresponder ao nome da propriedade anexada que você especificar como o parâmetro de *nome* **RegisterAttached** , com a cadeia de caracteres "Property" adicionada ao final. Esta é a convenção estabelecida para nomeação das entidades de propriedades de dependência em relação às propriedades que representam.

A diferença da área principal para definição de uma propriedade anexada personalizada em relação à propriedade de dependência personalizada está na forma como você define acessadores ou wrappers. Em vez de usar a técnica de wrapper descrita em [Propriedades de dependência personalizadas](custom-dependency-properties.md), também é preciso fornecer os métodos estáticos **Get**_PropertyName_ e **Set**_PropertyName_ como acessadores da propriedade anexada. Os acessadores são usados principalmente pelo analisador XAML, embora qualquer outro chamador também possa usá-los para definir cenários não XAML.

> [!IMPORTANT]
> Se você não definir os acessadores corretamente, o processador XAML não poderá acessar sua propriedade anexada e qualquer pessoa que tentar usá-la provavelmente obterá um erro de analisador XAML. Além disso, as ferramentas de design e codificação geralmente dependem das convenções "\*Property" para identificadores de nomenclatura quando eles encontram uma propriedade de dependência personalizada em um assembly referenciado.

## <a name="accessors"></a>Acessadores

A assinatura do acessador **Get**_PropertyName_ deve ser esta.

`public static` _ValueType_ **Get**_PropertyName_ `(DependencyObject target)`

Para Microsoft Visual Basic, é esta.

`Public Shared Function Get`_PropertyName_`(ByVal target As DependencyObject) As `_ValueType_`)`

O objeto *target* pode ser de um tipo mais específico na sua implementação, mas deve derivar de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). O valor de retorno de *valueType* também pode ser de um tipo mais específico na sua implementação. O tipo **Object** básico é aceitável, mas geralmente o que você quer é que a propriedade anexada imponha a segurança de tipo. O uso de digitação nas assinaturas getter e setter é uma técnica recomendada de segurança de tipo.

A assinatura do acessador **Set**_PropertyName_ deve ser esta.

`public static void Set`_PropertyName_` (DependencyObject target , `_ValueType_` value)`

Para Visual Basic, é esta.

`Public Shared Sub Set`_PropertyName_` (ByVal target As DependencyObject, ByVal value As `_ValueType_`)`

O objeto *target* pode ser de um tipo mais específico na sua implementação, mas deve derivar de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). O objeto *value* e seu *valueType* também podem ser de um tipo mais específico na sua implementação. Lembre-se de que o valor desse método é a entrada que vem do processador XAML quando ele encontra a sua propriedade anexada na marcação. É preciso haver suporte para conversão de tipo ou extensão de marcação existente para o tipo usado, para que o tipo adequado possa ser criado com base num valor de atributo (que, em última análise, é apenas uma cadeia de caracteres). O tipo **Object** básico é aceitável, mas geralmente o que você quer é mais segurança de tipo. Para fazer isso, coloque a aplicação de tipo nos acessadores.

> [!NOTE]
> Também é possível definir uma propriedade anexada na qual o uso pretendido é por meio da sintaxe do elemento Property. Nesse caso, você não precisa de conversão de tipos para os valores, mas precisa garantir que os valores que você pretende possam ser criados em XAML. [**VisualStateManager. VisualStateGroups**](https://docs.microsoft.com/dotnet/api/system.windows.visualstatemanager) é um exemplo de uma propriedade anexa existente que dá suporte apenas ao uso do elemento de propriedade.

## <a name="code-example"></a>Exemplo de código

Este exemplo de código mostra o registro de propriedade de dependência (usando o método [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)) e também os acessadores **Get** e **Set** para uma propriedade anexada personalizada. No exemplo, o nome da propriedade anexada é `IsMovable`. Portanto, os nomes dos acessadores devem ser `GetIsMovable` e `SetIsMovable`. O proprietário da propriedade anexada é uma classe de serviço denominada `GameService` que não tem uma interface do usuário própria; sua finalidade é apenas fornecer os serviços de propriedade anexada quando for usada a propriedade anexada **GameService.IsMovable**.

Definir a propriedade anexada C++em/CX é um pouco mais complexo. Você precisa decidir como fatorar entre o cabeçalho e o arquivo de código. Além disso, você deve expor o identificador como uma propriedade com apenas um acessador **get**, pelos motivos abordados em [Propriedades de dependência personalizadas](custom-dependency-properties.md). Em C++/CX, você deve definir essa relação de campo de propriedade explicitamente, em vez de depender de palavras-chave do .NET **ReadOnly** e do apoio implícito de propriedades simples. Você também precisa executar o registro da propriedade anexada em uma função auxiliar que é executada apenas uma vez, quando o aplicativo é iniciado pela primeira vez, mas antes de qualquer página XAML que precisa da propriedade anexada seja carregada. O local típico para chamar as funções do auxiliar de registro da propriedade para qualquer e toda dependência ou propriedades anexadas está dentro do construtor **App** / [**Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.-ctor) no código de seu arquivo app.xaml.

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
    [default_interface]
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        static Boolean GetIsMovable(Windows.UI.Xaml.DependencyObject target);
        static void SetIsMovable(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// GameService.h
...
    static Windows::UI::Xaml::DependencyProperty IsMovableProperty() { return m_IsMovableProperty; }
    static bool GetIsMovable(Windows::UI::Xaml::DependencyObject const& target) { return winrt::unbox_value<bool>(target.GetValue(m_IsMovableProperty)); }
    static void SetIsMovable(Windows::UI::Xaml::DependencyObject const& target, bool value) { target.SetValue(m_IsMovableProperty, winrt::box_value(value)); }

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

## <a name="setting-your-custom-attached-property-from-xaml-markup"></a>Definindo a propriedade anexada personalizada da marcação XAML

> [!NOTE]
> Se você estiver usando C++/WinRT, pule para a seção a seguir ([definindo sua propriedade anexa personalizada imperativamente com C++/WinRT](#setting-your-custom-attached-property-imperatively-with-cwinrt)).

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
> Se você estiver escrevendo uma interface do usuário C++XAML com o/CX, deverá incluir o cabeçalho do tipo personalizado que define a propriedade anexada, sempre que uma página XAML usar esse tipo. Cada página XAML tem um cabeçalho code-behind associado (. XAML. h). É aqui que você deve incluir (usando **\#include**) o cabeçalho para a definição do tipo de proprietário da propriedade anexada.

## <a name="setting-your-custom-attached-property-imperatively-with-cwinrt"></a>Definindo sua propriedade personalizada anexada de imperativa C++com/WinRT

Se você estiver usando C++/WinRT, poderá acessar uma propriedade anexa personalizada do código imperativo, mas não da marcação XAML. O código a seguir mostra como.

```xaml
<Image x:Name="gameServiceImage"/>
```

```cppwinrt
// MainPage.h
...
#include "GameService.h"
...

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    GameService::SetIsMovable(gameServiceImage(), true);
}
...
```

## <a name="value-type-of-a-custom-attached-property"></a>Tipo de valor de uma propriedade anexada personalizada

O tipo usado como o tipo de valor de uma propriedade anexada personalizada afeta o uso, a definição ou ambas. O tipo de valor da propriedade anexada é declarado em vários lugares: nas assinaturas dos métodos de acessador **Get** e **Set** e também como o parâmetro *propertyType* da chamada [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached).

O tipo de valor mais comum para propriedades anexadas (personalizadas ou não) é uma cadeia de caracteres simples. Isso porque as propriedades anexadas geralmente são criadas para uso de atributo XAML, e o uso de uma cadeia de caracteres como o tipo de valor deixa as propriedades mais leves. Outros primitivos que têm conversão nativa para os métodos de cadeia de caracteres – como inteiro, duplo ou um valor de enumeração – também são tipos de valor comuns para propriedades anexadas. Você pode usar outros tipos de valor (aqueles que não dão suporte para conversão nativa de cadeia de caracteres) como o valor de propriedade anexada. Entretanto, isso envolve inevitavelmente fazer uma escolha de uso ou de implementação:

- Você pode deixar a propriedade anexada como está, mas ela poderá dar suporte ao uso apenas quando for um elemento de propriedade e se o valor for declarado como um elemento de objeto. Nesse caso, o tipo de propriedade não permite o uso de XAML como um elemento de objeto. Para classes de referência existentes do Tempo de Execução do Windows, verifique a sintaxe XAML para garantir que o tipo dá suporte ao uso de elemento de objeto XAML.
- Você pode deixar a propriedade anexada como está, mas use-a somente em um uso de atributo via técnica de referência XAML, como **Binding** ou **StaticResource**, que possa ser expresso como uma cadeia de caracteres.

## <a name="more-about-the-canvasleft-example"></a>Mais sobre o exemplo **Canvas.Left**

Em exemplos anteriores dos usos de propriedades anexadas, mostramos as diferentes maneiras de definir a propriedade anexada [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left). Mas o que será que muda em relação a como o [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) interage com seu objeto, e quando isso acontece? Vamos examinar mais a fundo este exemplo específico, porque se você implementar uma propriedade anexada, será interessante ver o que mais uma típica classe de proprietário de propriedade anexada pretende fazer com os valores de propriedade anexada se encontrá-los em outros objetos.

A função principal de um [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) é ser um contêiner de layout de posição absoluta na interface do usuário. Os filhos de um **Canvas** são armazenados em uma propriedade definida pela classe base [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.children). De todos os painéis, **Canvas** é o único que usa posicionamento absoluto. Ele teria inchado o modelo de objeto do tipo [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) comum para adicionar propriedades que só poderiam ser motivo de preocupação para **Canvas** e para aqueles casos de **UIElement** em que eles são elementos filho de um **UIElement**. A definição das propriedades de controle de layout de um **Canvas** para que sejam propriedades anexadas que qualquer **UIElement** pode usar mantém o modelo de objeto mais limpo.

Para ser um painel prático, [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) tem um comportamento que substitui os métodos [**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) e [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de nível de estrutura. É aí que **Canvas** realmente verifica se há valores de propriedades anexadas em seus filhos. Parte dos padrões **Measure** e **Arrange** é um loop que itera sobre qualquer conteúdo, e um painel tem a propriedade [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.children) que torna explícito o que é deve ser considerado o filho de um painel. Assim, o **Canvas** o comportamento do layout itera através desses filhos e faz chamadas de [**Canvas.GetLeft**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.getleft) e [**Canvas.GetTop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.gettop) estáticas em cada filho para ver se essas propriedades anexadas contêm um valor que não é padrão (o padrão é 0). Esses valores são usados para posicionar absolutamente cada filho no espaço de layout disponível de **Canvas** de acordo com os valores específicos fornecidos por cada filho e confirmados usando **Arrange**.

O código é semelhante a este pseudocódigo.

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
> Para obter mais informações sobre como os painéis funcionam, consulte [visão geral dos painéis personalizados XAML](https://docs.microsoft.com/windows/uwp/layout/custom-panels-overview).

## <a name="related-topics"></a>Tópicos relacionados

* [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)
* [Visão geral das propriedades anexadas](attached-properties-overview.md)
* [Propriedades de dependência personalizadas](custom-dependency-properties.md)
* [Visão geral do XAML](xaml-overview.md)
