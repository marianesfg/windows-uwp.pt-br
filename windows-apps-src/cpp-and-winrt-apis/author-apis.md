---
author: stevewhims
description: Este tópico mostra como criar APIs de C++/WinRT, usando a estrutura de base **winrt::implements**, direta ou indiretamente.
title: Criar APIs com C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, implementação, implementar, classe de tempo de execução, ativação
ms.localizationpriority: medium
ms.openlocfilehash: 21670e0908a212341d401b4cbca314a9242b26a2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5754546"
---
# <a name="author-apis-with-cwinrt"></a>Criar APIs com C++/WinRT

Este tópico mostra como criar [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) APIs usando o [**WinRT:: Implements**](/uwp/cpp-ref-for-winrt/implements) base struct, direta ou indiretamente. Sinônimos parar *criar* neste contexto são *produzir* ou *implementar*. Este tópico aborda os seguintes cenários para a implementação de APIs em um tipo de C++/WinRT, nesta ordem.

- Você *não* está criando uma classe do Windows Runtime (classe do tempo de execução); você quer apenas implementar uma ou mais interfaces do Windows Runtime para consumo local dentro de seu aplicativo. Você pode derivar diretamente de **winrt::implements** neste caso e implementar funções.
- Você *está* criando uma classe de tempo de execução. Você pode criar um componente a ser consumido a partir de um aplicativo. Ou você pode estar criando um tipo a ser consumido da interface do usuário de XAML, e nesse caso você está implementando e consumindo uma classe de tempo de execução dentro da mesma unidade de compilação. Nesses casos, você permite que as ferramentas gerem classes que derivam de **winrt::implements**.

Em ambos os casos, o tipo que implementa suas APIs do C++/WinRT é chamado de *tipo de implementação*.

> [!IMPORTANT]
> É importante distinguir o conceito de um tipo de implementação daquele de um tipo projetado. O tipo projetado é descrito em [Consumir APIs com C++/WinRT](consume-apis.md).

## <a name="if-youre-not-authoring-a-runtime-class"></a>Se você *não* estiver criando uma classe de tempo de execução
O cenário mais simples é onde você está implementando uma interface do Windows Runtime para consumo local. Você não precisa de uma classe de tempo de execução; apenas uma classe C++ comum. Por exemplo, você pode estar escrevendo um aplicativo com base em [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication).

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

No Visual Studio, o **Visual C++** > **Universal do Windows** > **aplicativo principal (C++ c++ WinRT)** modelo de projeto ilustra o padrão **CoreApplication** . O padrão começa com a passagem de uma implementação de [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) para [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** usa a interface para criar a primeira exibição do aplicativo. Conceitualmente, **IFrameworkViewSource** tem esta aparência.

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

De novo, conceitualmente, a implementação de **CoreApplication::Run** faz isso.

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

Portanto, como desenvolvedor, implemente a interface **IFrameworkViewSource**. C++/WinRT têm o modelo de base estrutural [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) para tornar mais fácil implementar uma interface (ou várias) sem precisar recorrer a programação de estilo COM. Você só precisa derivar o tipo de **implementos**e depois implementar as funções da interface. Veja como fazer isso.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

Isso é feito pelo **IFrameworkViewSource**. A próxima etapa é retornar um objeto que implementa a interface **IFrameworkView**. Você pode optar por implementar essa interface no **Aplicativo**, também. Este próximo exemplo de código representa um aplicativo básico que receberá, pelo menos, uma janela em funcionamento na área de trabalho.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

Como seu tipo de **Aplicativo** *é um* **IFrameworkViewSource**, você pode passar apenas um para **Executar**.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App{});
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Se você estiver criando uma classe de tempo de execução em um componente do Tempo de Execução do Windows
Se o tipo é empacotado em um componente do Tempo de Execução do Windows para consumo de um aplicativo, então ele precisa ser uma classe de tempo de execução.

> [!TIP]
> Recomendamos que você declare cada classe de tempo de execução em seu próprio arquivo de Linguagem de definição de interface (.idl) para otimizar o desempenho da compilação ao editar um arquivo IDL e para correspondência lógica de um arquivo IDL com os arquivos de código-fonte gerado. O Visual Studio mescla todos os arquivos `.winmd` resultantes em um único arquivo com o mesmo nome do namespace raiz. O arquivo `.winmd` final será aquele que os consumidores do seu componente consultam.

Veja um exemplo.

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

Este IDL declara uma classe do Windows Runtime (tempo de execução). Uma classe de tempo de execução é um tipo que pode ser ativado e consumido por meio de interfaces COM modernas, normalmente entre limites executáveis. Quando você adiciona um arquivo IDL ao seu projeto e compilação, a cadeia de ferramentas de C++/WinRT (`midl.exe` e `cppwinrt.exe`) gera um tipo de implementação para você. Usando o exemplo de IDL acima, o tipo de implementação é um stub de estrutura C++ chamado **winrt::MyProject::implementation::MyRuntimeClass** nos arquivos de código fonte denominados `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` e `MyRuntimeClass.cpp`.

O tipo de implementação tem esta aparência.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        hstring Name();
        void Name(hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Observe o padrão de polimorfismo associado a F sendo usado (**MyRuntimeClass** usa a si mesmo como um argumento de modelo para sua base, **MyRuntimeClassT**). Isso também é chamado de padrão de modelo curiosamente recorrente (CRTP). Se você seguir a cadeia de herança para cima, você se deparará com **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

Então, nesse cenário, na raiz da herança hierarquia está o modelo de estrutura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) mais uma vez.

Para obter mais detalhes, código e um passo a passo das APIs de criação em um componente do Tempo de Execução do Windows, consulte [Criar eventos em C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Se você estiver criando uma classe de tempo de execução para ser referenciado em sua interface de usuário de XAML
Se o tipo é referenciado por sua interface de usuário de XAML, em seguida, ele deve ser uma classe de tempo de execução, mesmo que esteja no mesmo projeto como o XAML. Embora eles geralmente sejam ativados entre limites executáveis, uma classe de tempo de execução pode, em vez disso, ser usada dentro da unidade de compilação que a implementa.

Nesse cenário, você está criando *e* consumindo as APIs. O procedimento para a implementação de sua classe de tempo de execução é basicamente o mesmo para um componente do Tempo de Execução do Windows. Então, veja a seção anterior&mdash;[Se você estiver criando uma classe de tempo de execução em um componente do Tempo de Execução do Windows](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component). O único detalhe que difere é que, a partir do IDL, a cadeia de ferramentas do C++/WinRT gera não apenas um tipo de implementação, mas também um tipo projetado. É importante entender que dizer apenas "**MyRuntimeClass**" neste cenário pode ser ambíguo; Há várias entidades com esse nome, de tipos diferentes.

- **MyRuntimeClass** é o nome de uma classe de tempo de execução. Mas isso é uma abstração: declarado em IDL e implementado em alguma linguagem de programação.
- **MyRuntimeClass** é o nome da estrutura C++ **winrt::MyProject::implementation::MyRuntimeClass**, que é a implementação de C++/WinRT da classe de tempo de execução. Como vimos, se houver projetos de implementação e consumo separados, então essa estrutura existe somente no projeto de implementação. Isso é *o tipo de implementação* ou *a implementação*. Esse tipo é gerado (pela ferramenta `cppwinrt.exe`) nos arquivos `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` e `MyRuntimeClass.cpp`.
- **MyRuntimeClass** é o nome do tipo projetado em uma forma da estrutura C++ **winrt::MyProject::MyRuntimeClass**. Se houver projetos de implementação e consumo separados, então essa estrutura existe somente no projeto de consumo. Isso é *o tipo projetado* ou *a projeção*. Esse tipo é gerado (por `cppwinrt.exe`) no arquivo `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h`.

![Tipo projetado e o tipo de implementação](images/myruntimeclass.png)

Estas são as partes do tipo projetado relevantes para este tópico.

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

Para um exemplo de passo a passo para implementar a interface do **INotifyPropertyChanged** em uma classe de tempo de execução, consulte [Controles XAML; associar a uma propriedade C++/WinRT](binding-property.md).

O procedimento para o consumo de sua classe de tempo de execução nesse cenário é descrito em [Consumir APIs com C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).

## <a name="runtime-class-constructors"></a>Construtores de classe de tempo de execução
Aqui estão alguns pontos a se observar nas listagens que vimos acima.

- Cada construtor que você declarar em seu IDL faz com que um construtor seja gerado tanto em seu tipo de implementação quanto em seu tipo projetado. Construtores declarados de IDL são usados para consumir a classe de tempo de execução de uma unidade de compilação *diferente*.
- Se você tiver construtores de IDL declarados ou não, uma sobrecarga de construtor que leva `nullptr_t` é gerada em seu tipo projetado. Chamar o construtor `nullptr_t` é *a primeira das duas etapas* em consumir a classe de tempo de execução da *mesma* unidade de compilação. Para obter mais detalhes e um exemplo de código, consulte [Consumir APIs com C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Se você está consumindo a classe de tempo de execução da *mesma* unidade de compilação, em seguida, você também poderá implementar construtores não padrão diretamente no tipo de implementação (que está em `MyRuntimeClass.h`).

> [!NOTE]
> Se você espera que sua classe de tempo de execução seja consumida por uma unidade de compilação diferente (o que é comum), então inclua construtores em sua IDL (pelo menos um construtor padrão). Ao fazer isso, você também terá uma implementação de fábrica junto com seu tipo de implementação.
> 
> Se você quiser criar e consumir sua classe de tempo de execução somente dentro da mesma unidade de compilação, então não declare quaisquer construtores em sua IDL. Você não precisa de uma implementação de fábrica e uma não será gerada. Seu construtor padrão do tipo de implementação será excluído, mas você pode editá-lo facilmente e usar o padrão em vez disso.
> 
> Se quiser criar e consumir sua classe de tempo de execução somente dentro da mesma unidade de compilação, você precisa de parâmetros de construtor, depois criar os construtores que você precisa diretamente no seu tipo de implementação.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>Criação de instância e retorno de tipos de implementação e interfaces
Para esta seção, vamos levar como um exemplo um tipo de implementação denominado **MyType**, que implementa as interfaces [**IStringable**](/uwp/api/windows.foundation.istringable) e [**IClosable**](/uwp/api/windows.foundation.iclosable).

Você pode derivar **MyType** diretamente do [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) (não é uma classe de tempo de execução).

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

Ou você pode gerá-la de uma IDL (é uma classe de tempo de execução).

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

Para ir de **MyType** a um **IStringable** ou objeto **IClosable** que você pode usar ou retornar como parte de sua projeção, você pode chamar o modelo de função [**winrt::make**](/uwp/cpp-ref-for-winrt/make). **tornar** retorna a interface padrão do tipo de implementação.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> No entanto, se você estiver fazendo referência a seu tipo de sua interface de usuário do XAML, então haverá um tipo de implementação e um tipo projetado no mesmo projeto. Nesse caso, **faça** retorna uma instância do tipo projetado. Para obter um exemplo de código desse cenário, consulte [Controles XAML; associar a uma propriedade C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

Nós só podemos usar `istringable` (no exemplo de código acima) para chamar membros da interface **IStringable**. Mas uma interface C++/WinRT (que é uma interface projetada) deriva de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Portanto, você pode chamar [**IUnknown:: as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**IUnknown:: Try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) da consulta de outros tipos projetados ou interfaces, que você pode também usar ou retornar.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Se você precisar acessar todos os membros da implementação, e depois retornar uma interface para um chamador, então use o modelo de função [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** retorna um [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) embalando o tipo de implementação. Você pode acessar os membros de todas as suas interfaces (usando o operador de seta), você pode retorná-lo para o chamador em sua forma atual, ou você pode chamar **como** está nele e retornar o objeto de interface resultante para um chamador.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

A classe **MyType** não é parte da projeção; ela é a implementação. Mas, dessa forma você pode chamar seus métodos de implementação diretamente, sem a sobrecarga de uma chamada de função virtual. No exemplo acima, mesmo que **MyType::ToString** use a mesma assinatura do método projetada em **IStringable**, nós chamamos o método não virtual diretamente, sem cruzar a interface do binário do aplicativo (ABI). O **com_ptr** simplesmente contém um ponteiro para a estrutura **MyType**, então você também pode acessar outros detalhes internos de **MyType** por meio da variável `myimpl` e o operador de seta.

No caso em que você tiver um objeto de interface e souber que é uma interface em sua implementação, em seguida, você pode voltar a implementação usando o modelo de função [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) . Novamente, é uma técnica que evita chamadas de função virtuais e permite que você obtenha diretamente na implementação.

> [!NOTE]
> Se você ainda não tiver instalado o SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809) ou posterior, em seguida, você precisará chamar [**WinRT:: from_abi**](/uwp/cpp-ref-for-winrt/from-abi) em vez de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

Veja um exemplo. Há outro exemplo no [implemente a classe de controle personalizado **BgLabelControl** ](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class).

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Mas somente o objeto original da interface mantém uma referência. Se *você* quiser mantê-lo nele, então você pode chamar [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function).

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

O tipo de implementação em si não deriva de **winrt::Windows::Foundation::IUnknown**, portanto, ele não tem a função **como**. Mesmo assim, você pode instanciar um e acessar os membros de todas as suas interfaces. Mas se você fizer isso, não retorne a instância do tipo de implementação bruta ao chamador. Em vez disso, use uma das técnicas exibidas acima e retorne uma interface projetada, ou um **com_ptr**.

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

Se você tiver uma instância do seu tipo de implementação e precisar passá-la para uma função que espera o tipo projetado correspondente, então você pode fazer isso. Existe um operador de conversão em seu tipo de implementação (desde que o tipo de implementação foi gerado pelo `cppwinrt.exe` ferramenta) que torna isso possível.

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Derivando de um tipo que tem um construtor não padrão
[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)**](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) é um exemplo de um construtor não padrão. Não há nenhum padrão construtor; então, para construir um **ToggleButtonAutomationPeer**, você precisa passar um *proprietário*. Consequentemente, se você derivar de **ToggleButtonAutomationPeer**, então você precisa fornecer um construtor que leva um *proprietário* e passá-lo para a base. Vamos ver um exemplo disso na prática.

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

O construtor gerado para seu tipo de implementação tem esta aparência.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

A única informação ausente é que você precisa passar esse parâmetro do construtor para a classe base. Lembra do padrão de polimorfismo F associado que mencionamos acima? Depois que você estiver familiarizado com os detalhes desse padrão como usado pelo C++/WinRT, você pode descobrir do que sua classe base é chamada (ou você pode examinar apenas no arquivo de cabeçalho da sua classe de implementação). Isso é como chamar o construtor da classe base nesse caso.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

O construtor de classe base espera um **ToggleButton**. E **MySpecializedToggleButton** *é um* **ToggleButton**.

Até você fazer a edição descrita acima (para passar esse parâmetro do construtor para a classe base), o compilador sinalizará o construtor e mostrará que não há um construtor padrão apropriado disponível em um tipo chamado (neste caso) **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;**. Essa é, na verdade, a classe base da classe base de seu tipo de implementação.

## <a name="important-apis"></a>APIs Importantes
* [Modelo de estrutura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [função WinRT::com_ptr::copy_from](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)
* [Modelo de função winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)
* [modelo de função WinRT::get_self](/uwp/cpp-ref-for-winrt/get-self)
* [Modelo de estrutura winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Modelo de função winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)
* [Função winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
