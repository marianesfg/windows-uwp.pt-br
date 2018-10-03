---
author: jwmsft
description: Explica como definir e implementar propriedades de dependência personalizadas para um aplicativo do Windows Runtime em C++, C# ou Visual Basic.
title: Propriedades de dependência personalizadas
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.author: jimwalk
ms.date: 07/12/2018
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
ms.openlocfilehash: ddeccfe4c5e198afd77eaa4a81fc017543291ba1
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4266031"
---
# <a name="custom-dependency-properties"></a>Propriedades de dependência personalizadas

Aqui explicamos como definir e implementar suas próprias propriedades de dependência para um aplicativo do Windows Runtime em C++, C# ou Visual Basic. Listamos os motivos pelos quais os desenvolvedores de aplicativos e os autores de componentes podem querer criar propriedades de dependência personalizadas. Descrevemos as etapas de implementação de uma propriedade de dependência personalizada e também algumas práticas recomendadas que podem melhorar o desempenho, a usabilidade ou a versatilidade da propriedade de dependência.

## <a name="prerequisites"></a>Pré-requisitos

Supomos que você leu a [Visão geral das propriedades de dependência](dependency-properties-overview.md) e que entende as propriedades de dependência sob a perspectiva de um consumidor de propriedades de dependência existentes. Para seguir os exemplos deste tópico, você também precisa conhecer XAML e saber como gravar um aplicativo básico do Windows Runtime em C++, C# ou Visual Basic.

## <a name="what-is-a-dependency-property"></a>O que é uma propriedade de dependência?

Para dar suporte a aplicação de estilo, vinculação de dados, animações e valores padrão para uma propriedade, é necessário implementá-la como uma propriedade de dependência. Os valores da propriedade de dependência não são armazenados como campos na classe, eles são armazenados pela estrutura xaml e são referenciados com uma chave, que é recuperada quando a propriedade é registrada com o sistema de propriedades do Windows Runtime chamando o método [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/hh701829).   As propriedades de dependência podem ser usadas apenas pelos tipos derivados de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Mas **DependencyObject** está muito alto na hierarquia, então, a maioria das classes pretendidas para interface do usuário e suporte à apresentação pode dar suporte a propriedades de dependência. Para saber mais sobre propriedades de dependência e um pouco da terminologia e das convenções usadas para descrevê-las nesta documentação, consulte [Visão geral das propriedades de dependência](dependency-properties-overview.md).

Exemplos de propriedades de dependência no Windows Runtime são: [**Control.Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**FrameworkElement.Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) e [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/br209702), entre outras.

A convenção é que cada propriedade de dependência exposta por uma classe tem uma propriedade **public** [static** **readonly](https://msdn.microsoft.com/library/windows/apps/br242362) correspondente do tipo DependencyProperty, que é exposta na mesma classe que fornece o identificador da propriedade de dependência. O nome do identificador segue esta convenção: o nome da propriedade de dependência com a cadeia de caracteres "Property" acrescentada ao final do nome. Por exemplo, o identificador **DependencyProperty** correspondente para a propriedade **Control.Background** é [**Control.BackgroundProperty**](https://msdn.microsoft.com/library/windows/apps/br209396). O identificador armazena as informações sobre a propriedade de dependência tal como foram registradas, e o identificador pode ser usado posteriormente para outras operações que envolvam a propriedade de dependência; por exemplo, chamar [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361).

## <a name="property-wrappers"></a>Wrappers de propriedade

As propriedades de dependência geralmente têm uma implementação de wrapper. Sem o wrapper, a única maneira de obter ou definir as propriedades é usando os métodos utilitários de propriedade de dependência [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) e [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) e passando o identificador para eles como um parâmetro. Este é um uso nada natural para algo que ostensivamente é uma propriedade. No entanto, com o wrapper, seu código e qualquer outro código que faça referência à propriedade de dependência podem usar uma sintaxe direta objeto-propriedade, que é natural da linguagem em uso.

Se implementar você mesmo uma propriedade de dependência personalizada e quiser torná-la pública e fácil de chamar, defina também os wrappers de propriedade. Os wrappers de propriedade também são úteis para informações básicas de relatório sobre a propriedade de dependência, para reflexão ou processos de análise estática. Especificamente, o wrapper é o local onde você coloca atributos como [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011).

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>Quando implementar uma propriedade de dependência

Sempre que você implementa uma propriedade pública de leitura/gravação em uma classe, assim que a classe é derivada do [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), você pode fazer com que a propriedade funcione como uma propriedade de dependência. Às vezes, a técnica clássica de apoiar sua propriedade com um campo particular é adequada. A definição da propriedade personalizada como uma propriedade de dependência nem sempre é necessária ou adequada. A escolha vai depender dos cenários aos quais sua propriedade dará suporte.

Considere implementar sua propriedade como uma propriedade de dependência para que ela permita um ou mais desses recursos do Tempo de Execução do Windows ou dos aplicativos do mesmo:

- Definir a propriedade por meio de um [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)
- Agir como uma propriedade de destino válida para vinculação de dados com [**{Binding}**](binding-markup-extension.md)
- Dar suporte a valores animados por meio de um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)
- Relatar quando o valor da propriedade for alterado por:
  - Ações executadas pelo próprio sistema de propriedades
  - Ambiente
  - Ações do usuário
  - Estilo de leitura e gravação

## <a name="checklist-for-defining-a-dependency-property"></a>Lista de verificação para definição de uma propriedade de dependência

A definição de uma propriedade de dependência pode ser considerada como um conjunto de conceitos. Esses conceitos não são necessariamente etapas de procedimento, pois vários conceitos podem ser abrangidos em uma única linha de código, na implementação. Esta lista fornece apenas uma rápida visão geral. Explicaremos cada conceito em mais detalhes, posteriormente neste tópico, e mostraremos um exemplo de código em várias linguagens.

- Registre o nome da propriedade no sistema de propriedades (chame [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)) especificando um tipo de proprietário e o tipo do valor de propriedade.
  - Há um parâmetro necessário ao [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) que espera metadados de propriedade. Especifique **null** para isso ou se desejar um comportamento alterado da propriedade ou um valor padrão baseado em metadados que possa ser restaurado chamando [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), especifique uma instância de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.propertymetadata).
- Defina um identificador [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) como um membro de propriedade **public static readonly** no tipo de proprietário.
- Defina uma propriedade de wrapper seguindo o modelo de acessador de propriedade usado na linguagem que está sendo implementada. O nome da propriedade de wrapper deve corresponder à cadeia de caracteres *name* usada em [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829). Implemente **get** e os acessadores **set** para conectar o wrapper à respectiva propriedade de dependência, chamando [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) and [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) e passando seu próprio identificador de propriedade como um parâmetro.
- (Opcional) Coloque atributos como [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) no wrapper.

> [!NOTE]
> Você está definindo uma propriedade anexada personalizada, você geralmente omite o wrapper. Em vez disso, escreva um estilo diferente de acessador que possa ser usado pelo processador XAML. Veja [Propriedades anexadas personalizadas](custom-attached-properties.md). 

## <a name="registering-the-property"></a>Registrando a propriedade

Para a que a propriedade seja uma propriedade de dependência, registre-a em um repositório de propriedades mantido pelo sistema de propriedades do Windows Runtime.  Para registrar a propriedade, chame o método [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

Para as linguagens Microsoft .NET (C# e Microsoft Visual Basic), chame [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) no corpo da classe (dentro da classe, mas fora de qualquer definição de membro). O identificador é fornecido pela chamada do método [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) como o valor de retorno. A chamada [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) geralmente é feita como um construtor estático ou como parte da inicialização de uma propriedade **public static readonly** do tipo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) como parte de sua classe. Essa propriedade expõe o identificador da propriedade de dependência. Veja a seguir exemplos da chamada de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

> [!NOTE]
> Registrando a propriedade de dependência como parte do identificador de definição de propriedade é a implementação típica, mas você também pode registrar uma propriedade de dependência no construtor estático da classe. Essa abordagem poderá fazer sentido se você precisar de mais de uma linha de código para inicializar a propriedade de dependência.

Para C++ c++ /CX, você tem opções sobre como dividir a implementação entre o cabeçalho e o arquivo de código. A divisão típica é declarar o próprio identificador como propriedade **public static** no cabeçalho, com uma implementação **get**, mas nenhum **set**. A implementação **get** se refere a um campo particular, que é uma instância [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) não inicializada. Também é possível declarar os wrappers e as implementações **get** e **set** do wrapper. Nesse caso, o cabeçalho inclui uma implementação mínima. Se os wrappers precisarem da atribuição Tempo de Execução do Windows, defina também o atributo no cabeçalho. Coloque a chamada [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) no arquivo de código dentro de uma função auxiliar que só é executada quando o aplicativo é inicializado pela primeira vez. Use o valor de retorno de **Register** para preencher os identificadores estáticos, mas não inicializados declarados no cabeçalho, que você inicialmente definiu como **nullptr** no escopo raiz do arquivo de implementação.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

```cppwinrt
// ImageWithLabelControl.idl
namespace ImageWithLabelControlApp
{
    runtimeclass ImageWithLabelControl : Windows.UI.Xaml.Controls.Control
    {
        ImageWithLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}

// ImageWithLabelControl.h
...
private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
...

// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr }
);
...
```

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties();
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};

//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties()
{ 
    if (_LabelProperty == nullptr)
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr);
    } 
}
```

> [!NOTE]
> Para C++ c++ /CX codificar, o motivo por que você tem um campo privado e uma propriedade pública de somente leitura que revela o [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) é para que outros chamadores que usam sua propriedade de dependência também podem usar o utilitário do sistema de propriedades APIs que exigem a identificador seja público. Se você mantiver o identificador como particular, as pessoas não poderão usar essas APIs de utilitários. Exemplos dessas APIs e cenários incluem [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) ou [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) por opção, [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/br242358),  [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) e [**Setter.Property**](https://msdn.microsoft.com/library/windows/apps/br208836). Você não pode usar um campo público para isso porque as regras de metadados do Windows Runtime não permitem campos públicos.

## <a name="dependency-property-name-conventions"></a>Convenções de nomes de propriedade de dependência

Há convenções de nomenclatura para propriedades de dependência; siga-as em todas as circunstâncias, a não ser que surjam situações excepcionais. A propriedade de dependência propriamente dita tem um nome básico ("Label" no exemplo anterior) que é fornecido como o primeiro parâmetro de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829). O nome deve ser exclusivo em cada tipo de registro e o requisito de exclusividade se aplica também a todos os membros herdados. As propriedades de dependência herdadas de tipos base já são consideradas como parte do tipo de registro; não é possível registrar novamente os nomes de propriedades herdadas.

> [!WARNING]
> Embora o nome fornecido que aqui pode ser qualquer identificador de cadeia de caracteres que seja válido na programação da linguagem escolhida, você geralmente deseja ser capaz de definir sua propriedade de dependência também em XAML. Para definir em XAML, o nome da propriedade deve ser um nome XAML válido. Para obter mais informações, consulte [Visão geral de XAML](xaml-overview.md).

Ao criar a propriedade de identificador, combine o nome de propriedade registrado com o sufixo "Property" ("LabelProperty", por exemplo). Essa propriedade será o seu identificador da propriedade de dependência e será usada como uma entrada para as chamadas [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) e [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) que você faz nos seus próprios wrappers de propriedade. Ela também é usada pelo sistema de propriedades e outros processadores XAML, como [**{x: Bind}**](x-bind-markup-extension.md)

## <a name="implementing-the-wrapper"></a>Implementando o wrapper

O wrapper de propriedade deve chamar [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) na implementação **get** e [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) na implementação **set**.

> [!WARNING]
> Em circunstâncias excepcionais, as implementações de wrapper devem executar apenas as operações [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) e [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) . Caso contrário, você obterá um comportamento diferente quando a propriedade for definida via XAML, em comparação à definição via código. Por uma questão de eficiência, o analisador XAML ignora os wrappers ao definir as propriedades de dependência e se comunica com o repositório de backup por meio do **SetValue**.

```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

```vb
Public Property Label() As String
    Get
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String)
        SetValue(LabelProperty, value)
    End Set
End Property
```

```cppwinrt
// ImageWithLabelControl.h
...
winrt::hstring Label()
{
    return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
}

void Label(winrt::hstring const& value)
{
    SetValue(m_labelProperty, winrt::box_value(value));
}
...
```

```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value);
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>Metadados para uma propriedade de dependência personalizada

Quando metadados de propriedade são atribuídos a uma propriedade de dependência, os mesmos metadados são aplicados a essa propriedade em todas as instâncias do tipo de propriedade-proprietário ou respectivas subclasses. Nos metadados de propriedade, você pode especificar dois comportamentos:

- Um valor padrão que o sistema de propriedades atribui a todos os casos da propriedade.
- Um método estático de retorno de chamada que é automaticamente invocado no sistema de propriedades sempre que uma alteração do valor de propriedade é detectada.

### <a name="calling-register-with-property-metadata"></a>Chamando o Registro com metadados de propriedade

Nos exemplos anteriores de chamada de [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829), passamos um valor nulo para o parâmetro *propertyMetadata*. Para permitir que uma propriedade de dependência forneça um valor padrão ou use um retorno de chamada de propriedade alterada, você deve definir uma instância de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) que forneça uma ou duas dessas funcionalidades.

Normalmente, você fornece um [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) como uma instância criada embutida, dentro dos parâmetros de [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

> [!NOTE]
> Se você estiver definindo uma implementação de [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) , você deve usar o método utilitário [**PropertyMetadata. Create**](https://msdn.microsoft.com/library/windows/apps/hh702099) em vez de chamar um construtor [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) para definir a instância de **PropertyMetadata** .

Este próximo exemplo modifica os exemplos de [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) mostrados antes fazendo referência a uma instância de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) com um valor de [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770). A implementação do retorno de chamada "OnLabelChanged" é mostrada posteriormente nesta seção.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty =
    DependencyProperty.Register("Label",
      GetType(String),
      GetType(ImageWithLabelControl),
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr, Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty =
    DependencyProperty::Register("Label",
    Platform::String::typeid,
    ImageWithLabelControl::typeid,
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>Valor padrão

Você pode especificar um valor padrão para uma propriedade de dependência de forma que a propriedade sempre retorna um valor padrão específico quando não está definido. Esse valor pode ser diferente do valor padrão inerente para o tipo dessa propriedade.

Se um valor padrão não for especificado, o valor padrão de uma propriedade de dependência será nulo para um tipo de referência, ou para o padrão do tipo de referência de um tipo de valor, ou para o primitivo de linguagem (por exemplo, 0 para número inteiro ou cadeia de caracteres vazia para uma cadeia de caracteres). O principal motivo para estabelecer um valor padrão é que esse valor será restaurado quando você chamar [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) na propriedade. O estabelecimento de um valor padrão em cada propriedade pode ser mais conveniente do que o estabelecimento de valores padrão em construtores, particularmente para tipos de valor. Entretanto, para tipos de referência, verifique se o estabelecimento de um valor padrão não cria um padrão singleton não intencional. Para saber mais, veja [Práticas recomendadas](#best-practices), posteriormente neste tópico.

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

> [!NOTE]
> Não se registre com um valor padrão de [**UnsetValue**](https://msdn.microsoft.com/library/windows/apps/br242371). Se fizer isso, os consumidores da propriedade poderão ficar confusos e haverá consequências indesejadas no sistema de propriedades.

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

Em alguns cenários, você está definindo propriedades de dependência para objetos que são usados em mais de um thread da interface do usuário. Isso pode ocorrer quando você define um objeto de dados que é usado por vários aplicativos ou um controle que é usado em mais de um aplicativo. Você pode ativar a troca do objeto entre diferentes threads da interface do usuário fornecendo uma implementação de [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) em vez de uma instância do valor padrão, que é vinculado ao thread que registrou a propriedade. Basicamente, um [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) define um alocador para valores padrão. O valor retornado por **CreateDefaultValueCallback** sempre é associado ao thread **CreateDefaultValueCallback** da interface do usuário atual que está usando o objeto.

Para definir metadados que especifiquem um [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812), você deve chamar [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702115) para retornar uma instância de metadados; os construtores de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) não têm uma assinatura que inclua um parâmetro **CreateDefaultValueCallback**.

O padrão de implementação típico para um [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) é criar uma nova classe [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), definir o valor da específico de cada propriedade do **DependencyObject** para o padrão desejado e depois retornar a nova classe como uma referência a **Object** por meio do valor de retorno do método **CreateDefaultValueCallback**.

### <a name="property-changed-callback-method"></a>Método de retorno de chamada de propriedade alterada

É possível definir um método de retorno de chamada de propriedade alterada para definir as interações da sua propriedade com outras propriedades de dependência ou para atualizar uma propriedade interna ou estado do objeto sempre que a propriedade mudar. Se o retorno de chamada for invocado, o sistema de propriedades determinará se houve alterações em um valor de propriedade efetivo. Como o método de retorno de chamada é estático, o parâmetro *d* do retorno de chamada é importante, pois ele informa qual instância da classe relatou uma alteração. Uma implementação típica usa a propriedade [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) dos dados de evento e processos que, de alguma maneira, são importantes, geralmente executando alguma outra alteração no objeto passado como *d*. Outras respostas a uma alteração de propriedade são a rejeição do valor relatado por **NewValue**, a restauração do [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) ou a definição de valor como uma restrição programática aplicada ao **NewValue**.

O próximo exemplo mostra uma implementação [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770). Ela implementa o método referenciado nos exemplos de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) anteriores como parte dos argumentos de construção dos [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771). O cenário abrangido por esse retorno de chamada é a classe que também contém uma propriedade calculada somente leitura, com o nome "HasLabelValue" (implementação não exibida). Sempre que a propriedade "Label" for reavaliada, esse método de retorno de chamada será invocado, e o retorno de chamada habilitará o valor calculado dependente a permanecer em sincronização com as alterações na propriedade de dependência.

```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```

```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```

```cppwinrt
void ImageWithLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto iwlc{ d.as<ImageWithLabelControlApp::ImageWithLabelControl>() };
    auto s{ winrt::unbox_value<winrt::hstring>(e.NewValue()) };
    iwlc.HasLabelValue(s.size() != 0);
}
```

```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>Comportamento alterado da propriedade para estruturas e enumerações

Se uma [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) é do tipo enumeração ou estrutura, o retorno de chamada pode ser invocado mesmo que os valores internos da estrutura ou o valor da enumeração não tenham mudado. Isso é diferente de um sistema primitivo, como uma cadeia de caracteres, onde ele é invocado somente se o valor mudou. Esse é um efeito colateral de operações de boxing e unboxing nesses valores realizadas internamente. Se você tem um método [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) para uma propriedade em que seu valor é uma enumeração ou estrutura, precisa comparar [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) e [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) convertendo você mesmo os valores e usando os operadores de comparação sobrecarregados disponíveis para os valores agora convertidos. Ou, se não há um desses operadores disponíveis (o que pode ocorrer para uma estrutura personalizada), talvez seja necessário comparar os valores individuais. Normalmente, você não faria nada se o os valores não tivessem mudado.

```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```

```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```

```cppwinrt
static void OnVisibilityValueChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto oldVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.OldValue()) };
    auto newVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.NewValue()) };

    if (newVisibility != oldVisibility)
    {
        // The value really changed; invoke your property-changed logic here.
    }
    // Otherwise, OnVisibilityValueChanged was invoked because of boxing; do nothing.
}
```

```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>Práticas recomendadas

Tenha em mente as seguintes considerações de práticas recomendadas ao definir sua propriedade de dependência personalizada.

### <a name="dependencyobject-and-threading"></a>DependencyObject e threading

Todas as instâncias de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) devem ser criadas no thread da interface do usuário que está associado à [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) mostrada por um aplicativo do Tempo de Execução do Windows. Embora seja necessário que cada **DependencyObject** seja criada no thread da interface do usuário principal, os objetos podem ser acessados usando uma referência de dispatcher de outros threads chamando [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616).

Os aspectos de threading de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) são relevantes porque em geral isso significa que somente o código que é executado no thread da interface do usuário pode mudar ou até mesmo ler o valor de uma propriedade de dependência. Em geral, é possível evitar os problemas com threading em códigos de interface do usuário típicos que usam corretamente padrões **async** e threads de trabalho em segundo plano. Normalmente, você tem problemas de threading relacionados a **DependencyObject** somente quando define seus próprios tipos de **DependencyObject** e tenta usá-los para fontes de dados ou outros cenários em que um **DependencyObject** não é necessariamente apropriado.

### <a name="avoiding-unintentional-singletons"></a>Evitando singletons não intencionais

Um singleton não intencional poderá acontecer se você estiver declarando uma propriedade de dependência que usa um tipo de referência e então chamar um construtor desse tipo de referência como parte do código que estabelece os [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771). O resultado é que todos os usos da propriedade de dependência compartilham os **PropertyMetadata** e, por isso, tentam compartilhar o único tipo de referência que você construiu. Todas as subpropriedades desse tipo de valor, definidas pela propriedade de dependência, são então propagadas para os demais objetos de maneiras provavelmente indesejadas.

Você pode usar construtores de classe para definir os valores iniciais de uma propriedade de dependência de tipo de referência caso queira um valor diferente de nulo, mas lembre-se de que isso pode ser considerado um valor local para a finalidade de [Visão geral de propriedades de dependência](dependency-properties-overview.md). Talvez seja mais adequado usar um modelo para esse objetivo, se a classe der suporte a modelos. Outra maneira de evitar um padrão singleton, mas ainda fornecer um padrão útil, é expor uma propriedade estática no tipo de referência que fornece um padrão adequado aos valores dessa classe.

### <a name="collection-type-dependency-properties"></a>Propriedades de dependência de tipo de coleção

As propriedades de dependência de tipo de coleção têm alguns aspectos adicionais a serem considerados.

Propriedades de dependência de tipo de coleção são relativamente raras na API do Tempo de Execução do Windows. Na maioria dos casos, é possível usar coleções em que os itens são uma subclasse [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), mas a propriedade de coleção propriamente dita é implementada como uma propriedade convencional de CLR ou C++. Isso porque as coleções não são necessariamente adequadas a alguns cenários típicos envolvendo propriedades de dependência. Por exemplo:

- Você normalmente não anima uma coleção.
- Você geralmente não popula antecipadamente os itens de uma coleção com estilos ou um modelo.
- Embora a associação a coleções seja um cenário importante, uma coleção não precisa ser uma propriedade de dependência para então ser uma fonte de associação. Para destinos de associação, é mais comum o uso de subclasses de [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803) ou de [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) para dar suporte aos itens de coleção ou o uso de padrões de modelo de exibição. Para saber mais sobre associação de/para coleções, consulte [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).
- As notificações de alterações de coleção são melhor tratadas por meio de interfaces, como **INotifyPropertyChanged** ou **INotifyCollectionChanged**, ou com a derivação do tipo de coleção de [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx).

Entretanto, cenários de propriedades de dependência de tipo de coleção realmente existem. As próximas três seções fornecerão algumas diretrizes sobre como implementar uma propriedade de dependência de tipo de coleção.

### <a name="initializing-the-collection"></a>Inicializando a coleção

Ao criar uma propriedade de dependência, você pode estabelecer um valor padrão por meio dos metadados da propriedade de dependência. Mas, cuidado. Não use uma coleção estática de singleton como valor padrão. Em vez disso, defina deliberadamente o valor de coleção como uma coleção (instância) exclusiva, como parte da lógica do construtor de classe, para a classe de proprietário da propriedade de coleção.

### <a name="change-notifications"></a>Notificações de alterações

A definição da coleção como uma propriedade de dependência não fornece automaticamente uma notificação de alteração dos itens da coleção devido ao sistema de propriedades que invoca o método de retorno de chamada "PropertyChanged". Se você quiser notificações para coleções ou itens de coleções (por exemplo, para um cenário de vinculação de dados), implemente a interface **INotifyPropertyChanged** ou **INotifyCollectionChanged**. Para obter mais informações, consulte [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

### <a name="dependency-property-security-considerations"></a>Considerações sobre segurança da propriedade de dependência

Declare as propriedades de dependência como propriedades públicas. Declare os identificadores de propriedade de dependência como membros **estáticos públicos somente leitura**. Mesmo que você tente declarar outros níveis de acesso permitidos por uma linguagem (por exemplo, **protected**), uma propriedade de dependência sempre pode ser acessada por meio do identificador em combinação com as APIs do sistema de propriedades. A declaração do identificador de propriedade de dependência como interno ou particular não funcionará porque o sistema de propriedades não poderá funcionar corretamente.

As propriedades de wrapper existem apenas por conveniência. Os mecanismos de segurança aplicados aos wrappers podem ser ignorados com a chamada de [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) ou de [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361). Assim, mantenha as propriedades de wrapper públicas, caso contrário, chamadores legítimos terão muita dificuldade para usá-las e você não fornecerá qualquer benefício real de segurança.

O Tempo de Execução do Windows não fornece um modo para registrar uma propriedade de dependência personalizada como somente leitura.

### <a name="dependency-properties-and-class-constructors"></a>Propriedades de dependência e construtores de classe

Há um princípio geral de que os construtores de classe não devem chamar métodos virtuais. Isso porque os construtores podem ser chamados para fazer a inicialização básica de um construtor de classe derivado. A entrada do método virtual por meio do construtor pode ocorrer quando a instância de objeto em construção ainda não foi totalmente inicializada. Ao derivar de qualquer classe já derivada do [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), lembre-se de que o próprio sistema de propriedades chama e expõe internamente os métodos virtuais como parte de seus serviços. Para evitar possíveis problemas na inicialização do tempo de execução, não defina valores de propriedades de dependência em construtores de classes.

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>Registrando as propriedades de dependência para aplicativos C++/CX

A implementação para registrar uma propriedade em C++/CX é mais complicada que em C#, devido à separação em cabeçalho e arquivo de implementação, e também porque a inicialização no escopo raiz do arquivo de implementação não é uma prática recomendada. (As extensões de componente do Visual C++ (C++/CX) colocam o código inicializador estático do âmbito raiz diretamente no **DllMain**, enquanto que os compiladores de C# atribuem os inicializadores estáticos a classes e, assim, evitam problemas de bloqueio de carga do **DllMain**.) A melhor prática aqui é declarar uma função auxiliar que faz todo o registro de propriedade de dependência para uma classe, uma função por classe. Então, para cada classe personalizada que seu aplicativo consumir, será necessário fazer referência à função de registro auxiliar que é exposta por cada classe personalizada que deseja usar. Chame cada função de registro auxiliar uma vez, como parte do [**Application constructor**](https://msdn.microsoft.com/library/windows/apps/br242325) (`App::App()`), antes de `InitializeComponent`. Esse construtor só é executado quando o aplicativo é realmente referenciado pela primeira vez. Ele não será executado novamente se um aplicativo suspenso for retomado, por exemplo. Além disso, como pode ser visto no exemplo de registro anterior de C++, a verificação do **nullptr** em torno de cada chamada de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) é importante: é a segurança de que nenhum chamador da função pode registrar a propriedade duas vezes. Uma segunda chamada de registro, provavelmente, travaria o aplicativo sem essa verificação, porque o nome da propriedade seria uma duplicação. Você pode ver esse padrão de implementação nos exemplos de controle personalizado e de usuário [XAML](http://go.microsoft.com/fwlink/p/?linkid=238581) se analisar o código para a versão C++/CX do exemplo.

## <a name="related-topics"></a>Tópicos relacionados

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)
- [Visão geral das propriedades de dependência](dependency-properties-overview.md)
- [Usuário XAML e exemplo de controles personalizados](http://go.microsoft.com/fwlink/p/?linkid=238581)
 