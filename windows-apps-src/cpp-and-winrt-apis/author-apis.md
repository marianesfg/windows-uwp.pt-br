---
description: Este tópico mostra como criar APIs de C++/WinRT usando a estrutura de base **winrt::implements**, direta ou indiretamente.
title: Criar APIs com C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, implementação, implementar, classe de tempo de execução, ativação
ms.localizationpriority: medium
ms.openlocfilehash: 18dc65198d476204cfd54bd241fbd3c9ac401155
ms.sourcegitcommit: 7ece8a9a9fa75e2e92aac4ac31602237e8b7fde5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485172"
---
# <a name="author-apis-with-cwinrt"></a>Criar APIs com C++/WinRT

Este tópico mostra como criar APIs [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) usando a struct base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), direta ou indiretamente. Sinônimos parar *criar*, neste contexto, são *produzir* ou *implementar*. Este tópico aborda os seguintes cenários para a implementação de APIs em um tipo C++/WinRT, nesta ordem.

> [!NOTE]
> Este tópico aborda os componentes do Windows Runtime, mas apenas no contexto do C++/WinRT. Se você estiver buscando conteúdo sobre componentes do Windows Runtime que abrange todas as linguagens do Windows Runtime, consulte [Componentes do Windows Runtime](/windows/uwp/winrt-components/).

- Você *não* está criando uma classe do Windows Runtime (classe de tempo de execução); você apenas implementará uma ou mais interfaces do Windows Runtime para consumo local dentro de seu aplicativo. Você irá derivar diretamente de **winrt::implements** neste caso, e implementará funções.
- Você *está* criando uma classe de tempo de execução. Você poderá estar criando um componente a ser consumido a partir de um aplicativo. Ou você poderá estar criando um tipo a ser consumido a partir da interface do usuário XAML e, nesse caso, você estará implementando e consumindo uma classe de tempo de execução dentro da mesma unidade de compilação. Nesses casos, você permitirá que as ferramentas gerem classes que derivam de **winrt::implements**.

Em ambos os casos, o tipo que implementa as APIs C++/WinRT é denominado *tipo de implementação*.

> [!IMPORTANT]
> É importante distinguir o conceito de um tipo de implementação daquele de um tipo projetado. O tipo projetado está descrito em [Consumir APIs com C++/WinRT](consume-apis.md).

## <a name="if-youre-not-authoring-a-runtime-class"></a>Se você *não* estiver criando uma classe de tempo de execução

O cenário mais simples é aquele em que você implementará uma interface do Windows Runtime para consumo local. Você não precisa de uma classe de tempo de execução; apenas uma classe C++ comum. Por exemplo, você pode estar escrevendo um aplicativo com base em [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication).

> [!NOTE]
> Para obter informações sobre como instalar e usar o C++/WinRT Visual Studio Extension (VSIX) e o pacote NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira as informações de [suporte do Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

No Visual Studio, o modelo de projeto do Aplicativo Core Windows Universal** >  **do **Visual Studio C++**  > ** (C++/WinRT)** ilustra o padrão **CoreApplication**. O padrão começa com a passagem de uma implementação de [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) para [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

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

Conceitualmente, mais uma vez, a implementação de **CoreApplication::Run** faz isto.

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

Portanto, como desenvolvedor, implemente a interface **IFrameworkViewSource**. C++/WinRT têm o modelo de struct base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) para tornar mais fácil a implementação de uma interface (ou várias) sem precisar recorrer à programação de estilo COM. Você só precisa derivar o tipo de **implementos**e, depois, implementar as funções da interface. Veja aqui como fazer isso.

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

Isso é feito pelo **IFrameworkViewSource**. A próxima etapa será retornar um objeto que implementa a interface **IFrameworkView**. Você poderá optar por implementar essa interface no **Aplicativo** também. Este próximo exemplo de código representa um aplicativo básico que receberá, pelo menos, uma janela em funcionamento na área de trabalho.

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

Como seu tipo de **Aplicativo** *é um* **IFrameworkViewSource**, você poderá passar apenas um para **Executar**.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Se você estiver criando uma classe de tempo de execução em um componente do Windows Runtime

Se o tipo estiver empacotado em um componente do Windows Runtime para consumo a partir de um aplicativo, então, ele precisará ser uma classe de tempo de execução. Declare uma classe de tempo de execução em um arquivo de IDL (linguagem IDL da Microsoft) (.idl) (confira [Como fatorar classes de tempo de execução em arquivos MIDL (.idl)](#factoring-runtime-classes-into-midl-files-idl)).

Cada arquivo IDL resulta em um arquivo `.winmd`, e o Visual Studio mescla todos eles em um único arquivo com o mesmo nome do namespace raiz. O arquivo `.winmd` final será aquele que os consumidores de seu componente consultarão.

Veja abaixo um exemplo de declaração de uma classe de tempo de execução em um arquivo IDL.

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

Esse IDL declara uma classe do Windows Runtime (tempo de execução). Uma classe de tempo de execução é um tipo que pode ser ativado e consumido por meio de interfaces COM modernas, normalmente entre limites executáveis. Quando você adiciona um arquivo IDL ao seu projeto e ao build, a cadeia de ferramentas C++/WinRT (`midl.exe` e `cppwinrt.exe`) gera um tipo de implementação. Para ver um exemplo do fluxo de trabalho do arquivo IDL em ação, consulte [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md).

Usando o exemplo de IDL acima, o tipo de implementação é um struct stub do C++ chamado **winrt::MyProject::implementation::MyRuntimeClass** nos arquivos de código fonte denominados `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` e `MyRuntimeClass.cpp`.

O tipo de implementação tem esta aparência.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Observe o padrão de polimorfismo associado a F sendo usado (**MyRuntimeClass** usa a si mesmo como um argumento de modelo para sua base, **MyRuntimeClassT**). Isso também é chamado de padrão de modelo curiosamente recorrente (CRTP). Se você seguir a cadeia de herança para cima, você se deparará com **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

Então, nesse cenário, na raiz da hierarquia de herança está o modelo de struct base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) mais uma vez.

Para obter mais detalhes, o código e um passo a passo das APIs de criação em um componente do Windows Runtime, veja como [criar eventos em C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Se você estiver criando uma classe de tempo de execução para ser referenciada em sua interface de usuário XAML

Se o tipo é referenciado por sua interface de usuário XAML, ele precisará ser uma classe de tempo de execução, mesmo que esteja no mesmo projeto que o XAML. Embora eles geralmente sejam ativados entre limites executáveis, uma classe de tempo de execução pode ser usada dentro da unidade de compilação que a implementa.

Nesse cenário, você estará criando *e* consumindo as APIs. O procedimento para a implementação da classe de tempo de execução é basicamente o mesmo para um componente do Windows Runtime. Confira a seção anterior&mdash;[se você estiver criando uma classe de tempo de execução em um componente do Windows Runtime](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component). O único detalhe que difere é que, no IDL, a cadeia de ferramentas C++/WinRT gera não apenas um tipo de implementação, mas também um tipo projetado. É importante entender que dizer apenas "**MyRuntimeClass**", neste cenário, pode ser ambíguo. Há várias entidades com esse nome e de tipos diferentes.

- **MyRuntimeClass** é o nome de uma classe de tempo de execução. Mas isso é uma abstração: que quer dizer declarado no IDL e implementado em alguma linguagem de programação.
- **MyRuntimeClass** é o nome do struct do C++ **winrt::MyProject::implementation::MyRuntimeClass**, que é a implementação C++/WinRT da classe de tempo de execução. Como vimos, se houver projetos de implementação e consumo separados, então, esse struct existirá somente no projeto de implementação. Esse é *o tipo de implementação* ou *a implementação*. Esse tipo é gerado (pela ferramenta `cppwinrt.exe`) nos arquivos `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` e `MyRuntimeClass.cpp`.
- **MyRuntimeClass** é o nome do tipo projetado na forma do struct do C++ **winrt::MyProject::MyRuntimeClass**. Se houver projetos de implementação e consumo separados, então, esse struct existirá somente no projeto de consumo. Esse é *o tipo projetado* ou *a projeção*. Esse tipo é gerado (por `cppwinrt.exe`) no arquivo `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h`.

![Tipo projetado e tipo de implementação](images/myruntimeclass.png)

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

Para obter um exemplo de um passo a passo para implementar a interface **INotifyPropertyChanged** em uma classe de tempo de execução, confira os [controles XAML; associar a uma propriedade C++/WinRT](binding-property.md).

O procedimento para o consumo da classe de tempo de execução neste cenário está descrito em [Consumir APIs com C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).

## <a name="factoring-runtime-classes-into-midl-files-idl"></a>Como fatorar classes de tempo de execução em arquivos MIDL (.idl)

Os modelos de projeto e item do Visual Studio produzem um arquivo IDL separado para cada classe de tempo de execução. Isso fornece uma correspondência lógica entre um arquivo IDL e seus arquivos de código-fonte gerados.

No entanto, se você consolidar todas as classes de tempo de execução do projeto em um único arquivo IDL, isso poderá melhorar significativamente o tempo de build. Se, de outra forma, você tiver dependências `import` complexas (ou circulares) entre eles, a consolidação poderá ser realmente necessária. Talvez você ache mais fácil criar e examinar as classes de tempo de execução se elas estiverem juntas.

## <a name="runtime-class-constructors"></a>Construtores de classe de tempo de execução

Veja a seguir alguns pontos que devem ser observados nas listagens que vimos acima.

- Cada construtor que você declarar em seu IDL faz com que um construtor seja gerado tanto no tipo de implementação quanto no tipo projetado. Os construtores declarados de IDL são usados para consumir a classe de tempo de execução de uma unidade de compilação *diferente*.
- Quer você tenha ou não construtores declarados em IDL, uma sobrecarga de construtor que usa **std::nullptr_t** será gerada em no tipo projetado. Chamar o construtor **std::nullptr_t** é a *primeira de duas etapas* para consumir a classe de tempo de execução *na mesma* unidade de compilação. Para obter mais detalhes e um exemplo de código, confira as informações sobre [consumir APIs com C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Se você estiver consumindo a classe de tempo de execução na *mesma* unidade de compilação, você também poderá implementar construtores não padrão diretamente no tipo de implementação (que está em `MyRuntimeClass.h`).

> [!NOTE]
> Se você espera que sua classe de tempo de execução seja consumida por uma unidade de compilação diferente (o que é comum), inclua construtores em seu IDL (pelo menos um construtor padrão). Ao fazer isso, você também terá uma implementação do alocador junto com seu tipo de implementação.
> 
> Se você quiser criar e consumir a classe de tempo de execução somente dentro da mesma unidade de compilação, não declare nenhum construtor no IDL. Você não precisará de uma implementação de alocador, e ela não será gerada. O construtor padrão do tipo de implementação será excluído, mas é possível editá-lo facilmente e usá-lo como padrão.
> 
> Se quiser criar e consumir a classe de tempo de execução somente dentro da mesma unidade de compilação, e precisar de parâmetros do construtor, crie s construtores necessários diretamente no tipo de implementação.

## <a name="runtime-class-methods-properties-and-events"></a>Métodos, propriedades e eventos de classe de tempo de execução

Já vimos que o fluxo de trabalho é usar o IDL para declarar sua classe de tempo de execução e seus membros e, em seguida, a ferramenta gera protótipos e implementações de stub para você. Quanto a esses protótipos gerados automaticamente para os membros da classe de tempo de execução, você *pode* editá-los para que eles passem tipos diferentes dos tipos que você declarar em seu IDL. No entanto, você poderá fazer isso somente enquanto o tipo que declarar no IDL possa ser encaminhado para o tipo que declarar na versão implementada.

Aqui estão alguns exemplos.

- Você pode relaxar os tipos de parâmetro. Por exemplo, se no IDL seu método usar uma **SomeClass**, você poderá optar por alterá-la para **IInspectable** em sua implementação. Isso funciona porque qualquer **SomeClass** pode ser encaminhada para **IInspectable** (o inverso, obviamente, não funcionaria).
- Você pode aceitar um parâmetro copiável por valor, em vez de por referência. Por exemplo, altere `SomeClass const&` para `SomeClass const&`. Isso é necessário quando você precisa evitar capturar uma referência em uma corrotina (consulte [Passagem de parâmetros](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)).
- Você pode relaxar o valor retornado. Por exemplo, você pode alterar **void** para [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget).

Os dois últimos são muito úteis quando você está escrevendo um manipulador de eventos assíncronos.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>Criação de instâncias, retorno de tipos de implementação e interfaces

Nesta seção, teremos como exemplo um tipo de implementação denominado **MyType**, que implementa as interfaces [**IStringable**](/uwp/api/windows.foundation.istringable) e [**IClosable**](/uwp/api/windows.foundation.iclosable).

Você pode derivar a **MyType** diretamente de [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) (não é uma classe de tempo de execução).

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

Ou você poderá gerá-la em um IDL (esta é uma classe de tempo de execução).

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

Para ir de **MyType** a um objeto **IStringable** ou **IClosable** que você poderá usar ou retornar como parte da projeção, chame o modelo de função [**winrt::make**](/uwp/cpp-ref-for-winrt/make). **make** retorna a interface padrão do tipo de implementação.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> No entanto, se você estiver fazendo referência a seu tipo na interface de usuário XAML, haverá um tipo de implementação e um tipo projetado no mesmo projeto. Nesse caso, **make** retorna uma instância do tipo projetado. Para obter um exemplo de código desse cenário, confira os [controles XAML; associar a uma propriedade C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

É possível apenas usar `istringable` (no exemplo de código acima) para chamar os membros da interface **IStringable**. Mas a interface C++/WinRT (que é uma interface projetada) deriva de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Portanto, você poderá chamar [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) nela para consultar outros tipos ou interfaces projetadas, que você também poderá usar ou retornar.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Se você precisar acessar todos os membros da implementação, e depois retornar uma interface para um chamador, use o modelo de função [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** retorna um [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) embalando o tipo de implementação. É possível acessar os membros de todas as suas interfaces (usando o operador de seta), retorná-los para o chamador em sua forma atual, ou poderá chamá-los **como** estão nelas e retornar o objeto de interface resultante para um chamador.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

A classe **MyType** não é parte da projeção; ela é a implementação. Mas, dessa forma você poderá chamar seus métodos de implementação diretamente, sem a sobrecarga de uma chamada de função virtual. No exemplo acima, mesmo que **MyType::ToString** use a mesma assinatura do método projetado em **IStringable**, você chamará o método não virtual diretamente, sem cruzar a interface binária do aplicativo (ABI). O **com_ptr** simplesmente contém um ponteiro para o struct **MyType**, portanto você também poderá acessar qualquer outro detalhe interno de **MyType** por meio da variável `myimpl` e do operador de seta.

Nos casos em que você tem um objeto de interface e sabe que é uma interface na implementação, será possível voltar para a implementação usando o modelo de função [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self). Novamente, esta é uma técnica que evita chamadas de função virtuais e permite obter diretamente na implementação.

> [!NOTE]
> Se você ainda não instalou o SDK do Windows, versão 10.0.17763.0 (Windows 10, versão 1809) ou posterior, você precisará chamar [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) em vez de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

Aqui está um exemplo. Há outro exemplo no artigo [Implementar a classe personalizada **BgLabelControl**](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class).

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Mas somente o objeto original da interface mantém uma referência. Se *você* quiser mantê-lo nele, poderá chamar [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function).

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

O tipo de implementação em si não deriva de **winrt::Windows::Foundation::IUnknown**, portanto, ele não tem a função **as**. Mesmo assim, você pode instanciar um deles e acessar os membros de todas as suas interfaces. Mas se você fizer isso, não retorne a instância do tipo de implementação bruta ao chamador. Em vez disso, use uma das técnicas exibidas acima e retorne uma interface projetada, ou um **com_ptr**.

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

Se tiver uma instância do tipo de implementação e precisar passá-la para uma função que espera o tipo projetado correspondente, você poderá fazer isso. Há um operador de conversão no tipo de implementação (desde que o tipo de implementação tenha sido gerado pela ferramenta `cppwinrt.exe`), que torna isso possível. Você poderá passar um valor do tipo de implementação diretamente para um método que espera um valor do tipo projetado correspondente. A partir de uma função de membro do tipo de implementação, você poderá passar `*this` para um método que espera um valor do tipo projetado correspondente.

```cppwinrt
// MyProject::MyType is the projected type; the implementation type would be MyProject::implementation::MyType.

void MyOtherType::DoWork(MyProject::MyType const&){ ... }

...

void FreeFunction(MyProject::MyOtherType const& ot)
{
    MyType myimpl;
    ot.DoWork(myimpl);
}

...

void MyType::MemberFunction(MyProject::MyOtherType const& ot)
{
    ot.DoWork(*this);
}
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Derivando de um tipo que tem um construtor não padrão

[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) é um exemplo de um construtor não padrão. Não há nenhum construtor padrão, portanto, para construir um **ToggleButtonAutomationPeer**, você precisará passar um *owner*. Consequentemente, se derivar de **ToggleButtonAutomationPeer**, será necessário fornecer um construtor que leve um *owner* e passe-o para a base. Vamos ver um exemplo disso na prática.

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

O construtor gerado para o tipo de implementação tem esta aparência.

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

A única informação ausente é que você precisa passar esse parâmetro do construtor para a classe base. Lembra-se do padrão de polimorfismo F associado, mencionado acima? Depois de se familiarizar com os detalhes desse padrão, conforme usado pelo C++/WinRT, você poderá descobrir como sua classe base é chamada (ou você poderá examinar apenas o arquivo de cabeçalho da classe de implementação). É como chamar o construtor da classe base nesse caso.

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

Até você fazer a edição descrita acima (para passar esse parâmetro do construtor para a classe base), o compilador sinalizará o construtor e mostrará que não há um construtor padrão apropriado disponível em um tipo chamado (neste caso) **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** . Essa é, na verdade, a classe base da classe base do seu tipo de implementação.

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>Namespaces: tipos projetados, tipos de implementação e fábricas

Como você viu anteriormente neste tópico, uma classe de tempo de execução C++/WinRT existe na forma de mais de uma classe C++ em mais de um namespace. Sendo assim, o nome **MyRuntimeClass** tem um significado no namespace **winrt::MyProject** e um significado diferente no namespace **winrt::MyProject::implementation**. Observe qual namespace que você tem no contexto atualmente e, em seguida, use os prefixos de namespace se precisar de um nome de um namespace diferente. Vamos examinar com mais detalhes os namespaces em questão.

- **winrt::MyProject**. Este namespace contém tipos projetados. Um objeto de tipo projetado é um proxy; ele é essencialmente um ponteiro inteligente para um objeto de backup, podendo esse objeto de backup ser implementado aqui em seu projeto ou em outra unidade de compilação.
- **winrt::MyProject::implementation**. Este namespace contém tipos de implementação. Um objeto de um tipo de implementação não é um ponteiro; ele é um valor&mdash;um objeto de pilha C++ completo. Não construa um tipo de implementação diretamente; em vez disso, chame [**winrt::make**](/uwp/cpp-ref-for-winrt/make) passando o tipo de implementação como o parâmetro de modelo. Mostramos exemplos de **winrt::make** em ação anteriormente neste tópico, e há outro exemplo em [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage). Confira também [Diagnosticar alocações diretas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).
- **winrt::MyProject::factory_implementation**. Este namespace contém fábricas. Um objeto no namespace dá suporte a [**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory).

Esta tabela mostra a qualificação de namespace mínima que você precisa usar em diferentes contextos.

|O namespace que está no contexto|Para especificar o tipo projetado|Para especificar o tipo projetado|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> Quando quiser retornar um tipo projetado de sua implementação, tenha cuidado para não criar uma instância do tipo de implementação escrevendo `MyRuntimeClass myRuntimeClass;`. As técnicas e o código corretos para esse cenário são mostrados anteriormente neste tópico, na seção [Criando instâncias e retornando tipos de implementação e interfaces](#instantiating-and-returning-implementation-types-and-interfaces).
>
> O problema com `MyRuntimeClass myRuntimeClass;` nesse cenário é que ele cria um objeto **winrt::MyProject::implementation::MyRuntimeClass** na pilha. Esse objeto (do tipo de implementação) se comporta como o tipo projetado de algumas maneiras&mdash;é possível invocar métodos nele da mesma forma e ele até mesmo é convertido em um tipo de projetado. Mas o objeto é destruído, de acordo com as regras normais de C++, quando o escopo é encerrado. Portanto, se você tiver retornado um tipo projetado (um ponteiro inteligente) para o objeto, esse ponteiro estará pendente.
>
> Esse tipo de bug com corrupção de memória é difícil de diagnosticar. Portanto, para compilações de depuração, uma asserção C++/WinRT ajuda a identificar esse erro usando um detector de pilha. No entanto, as corrotinas são alocadas no heap, de modo que você não obterá ajuda com esse erro se ele ocorrer dentro de uma corrotina. Para saber mais, confira [Diagnosticar alocações diretas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>Usando tipos projetados e tipos de implementação com vários recursos de C++/WinRT

Veja vários lugares onde um recurso de C++/WinRT espera um tipo e qual tipo ele espera (tipo projetado, tipo de implementação ou ambos).

|Recurso|Aceita|Observações|
|-|-|-|
|`T` (representando um ponteiro inteligente)|Projetado|Consulte o aviso em [Namespaces: tipos projetados, tipos de implementação e fábricas](#namespaces-projected-types-implementation-types-and-factories) sobre o uso do tipo de implementação por engano.|
|`agile_ref<T>`|Ambos|Se você usar o tipo de implementação, o argumento do construtor deverá ser `com_ptr<T>`.|
|`com_ptr<T>`|Implementação|Usar o tipo projetado gera o erro: `'Release' is not a member of 'T'`.|
|`default_interface<T>`|Ambos|Se você usar o tipo de implementação, a primeira interface implementada será retornada.|
|`get_self<T>`|Implementação|Usar o tipo projetado gera o erro: `'_abi_TrustLevel': is not a member of 'T'`.|
|`guid_of<T>()`|Ambos|Retorna o GUID da interface padrão.|
|`IWinRTTemplateInterface<T>`<br>|Projetado|Usar o tipo de implementação compila, mas isso é um erro&mdash;consulte o aviso em [Namespaces: tipos projetados, tipos de implementação e fábricas](#namespaces-projected-types-implementation-types-and-factories).|
|`make<T>`|Implementação|Usar o tipo projetado gera o erro: `'implements_type': is not a member of any direct or indirect base class of 'T'`|
| `make_agile(T const&amp;)`|Ambos|Se você usar o tipo de implementação, o argumento deverá ser `com_ptr<T>`.|
| `make_self<T>`|Implementação|Usar o tipo projetado gera o erro: `'Release': is not a member of any direct or indirect base class of 'T'`|
| `name_of<T>`|Projetado|Se usar o tipo de implementação, você receberá o GUID em cadeias de caracteres da interface padrão.|
| `weak_ref<T>`|Ambos|Se você usar o tipo de implementação, o argumento do construtor deverá ser `com_ptr<T>`.|

## <a name="opt-in-to-uniform-construction-and-direct-implementation-access"></a>Aceitar a construção uniforme e o acesso direto de implementação

Esta seção descreve um recurso do C++/WinRT 2.0 que é aceito, embora esteja habilitado por padrão para novos projetos. Para um projeto existente, será necessário aceitar configurando a ferramenta `cppwinrt.exe`. No Visual Studio, defina a propriedade do projeto **Common Properties** > **C++/WinRT** > **Optimized** como *Yes*. Isso tem o efeito de adicionar `<CppWinRTOptimized>true</CppWinRTOptimized>` ao arquivo de projeto. E tem o mesmo efeito que adicionar a opção ao invocar `cppwinrt.exe` da linha de comando.

A opção `-opt[imize]` habilita o que geralmente é chamado de *construção uniforme*. Com a construção uniforme (ou *unificada*), você usa a própria projeção de linguagem do C++/WinRT para criar e usar seus tipos de implementação (tipos implementados por seu componente, para consumo por aplicativos) com eficiência e sem nenhuma dificuldade de carregador.

Antes de descrever o recurso, vamos mostrar primeiro a situação *sem* a construção uniforme. Para ilustrar, começaremos com o exemplo da classe do Windows Runtime.

```idl
// MyClass.idl
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void Method();
        static void StaticMethod();
    }
}
```

Conforme o desenvolvedor do C++ se familiariza com a biblioteca do C++/WinRT, convém usar a classe como esta.

```cppwinrt
using namespace winrt::MyProject;

MyClass c;
c.Method();
MyClass::StaticMethod();
```

E isso seria perfeitamente razoável, desde que o código de consumo mostrado não resida no mesmo componente que implementa essa classe. Como uma projeção de linguagem, o C++/WinRT protege você, como desenvolvedor, da ABI (a interface binária de aplicativo baseada em COM definida pelo Windows Runtime). O C++/WinRT não chama diretamente na implementação; ele viaja pela ABI.

Consequentemente, na linha de código em que você está construindo um objeto **MyClass** (`MyClass c;`), a projeção do C++/WinRT chama [**RoGetActivationFactory**](/windows/win32/api/roapi/nf-roapi-rogetactivationfactory) para recuperar a classe ou a fábrica de ativação e, em seguida, o usa essa fábrica para criar o objeto. A última linha, da mesma forma, usa a fábrica para fazer o que parece ser uma chamada de método estático. Tudo isso exige que as classes sejam registradas e que seu módulo implemente o ponto de entrada [**DllGetActivationFactory**](/previous-versions/br205771(v=vs.85)). O C++/WinRT tem um cache de fábrica muito rápido; portanto, nada disso causa um problema para um aplicativo que consome seu componente. O problema é que, em seu componente, você acabou de fazer algo um pouco problemático.

Em primeiro lugar, não importa a rapidez do cache de fábrica do C++/WinRT, chamar por meio de **RoGetActivationFactory** (ou até mesmo chamadas subsequentes por meio do cache de fábrica) sempre será mais lento do que chamar diretamente na implementação. Uma chamada a **RoGetActivationFactory** seguida por [**IActivationFactory::ActivateInstance**](/windows/win32/api/activation/nf-activation-iactivationfactory-activateinstance) seguido por [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)) não será obviamente tão eficiente quanto usar a expressão `new` em C++ para um tipo definido localmente. Como consequência, os desenvolvedores do C++/WinRT experientes estão acostumados a usar as funções auxiliares [**winrt::make**](/uwp/cpp-ref-for-winrt/make) ou [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) ao criar objetos dentro de um componente.

```cppwinrt
// MyClass c;
MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
```

Mas, como você pode ver, isso não é tão conveniente nem conciso. É necessário usar uma função auxiliar para criar o objeto, além de desfazer a ambiguidade entre o tipo de implementação e o tipo projetado.

Em segundo lugar, o uso da projeção para criar a classe significa que sua fábrica de ativação será armazenada em cache. Normalmente, é isso o que você deseja, mas se a fábrica residir no mesmo módulo (DLL) que está realizando a chamada, você terá efetivamente fixado a DLL e impedido que ela nunca fosse descarregada. Em muitos casos, isso não importa; no entanto, alguns componentes do sistema *devem* dar suporte ao descarregamento.

É aí que entra o termo *construção uniforme*. Independentemente de o código de criação residir em um projeto que simplesmente consuma a classe ou residir no projeto que realmente *implemente* a classe, você pode usar livremente a mesma sintaxe para criar o objeto.

```cppwinrt
// MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
MyClass c;
```

Quando você cria seu projeto de componente com a opção `-opt[imize]`, a chamada por meio da projeção de idioma é compilada para a mesma chamada eficiente à função **winrt::make** que cria diretamente o tipo de implementação. Isso torna sua sintaxe simples e previsível e evita qualquer impacto de chamar por meio da fábrica e a fixação do componente no processo. Além dos projetos de componente, isso também é útil para aplicativos XAML. Ignorar **RoGetActivationFactory** para classes implementadas no mesmo aplicativo permite que você as construa (sem precisar estar registrado) de todas as formas possíveis se elas estivessem fora do componente.

A construção uniforme aplica-se a *qualquer* chamada atendida pela fábrica nos bastidores. Praticamente, isso significa que a otimização funciona tanto para construtores quanto para membros estáticos. Veja o exemplo original novamente.

```cppwinrt
MyClass c;
c.Method();
MyClass::StaticMethod();
```

Sem `-opt[imize]`, a primeira e a última instruções exigem chamadas por meio do objeto de fábrica. *Com* `-opt[imize]`, nem uma delas exige. E essas chamadas são compiladas diretamente com relação à implementação e, inclusive, têm o potencial de serem embutidas. O que conversa com o outro termo frequentemente usado ao falar sobre `-opt[imize]`, ou seja, o acesso *direto de implementação*.

As projeções de linguagem são convenientes, mas, quando você pode acessar diretamente a implementação, você pode e deve aproveitar isso para produzir o código mais eficiente possível. O C++/WinRT pode fazer isso por você, sem forçá-lo a deixar a segurança e a produtividade da projeção.

Essa é uma alteração da falha, porque o componente deve cooperar para permitir que a projeção de linguagem alcance e acesse diretamente seus tipos de implementação. Como o C++/WinRT é uma biblioteca somente de cabeçalho, você pode olhar para dentro e ver o que está acontecendo. Sem `-opt[imize]`, o construtor **MyClass** e o membro **StaticMethod** são definidos pela projeção como esta.

```cppwinrt
namespace winrt::MyProject
{
    inline MyClass::MyClass() :
        MyClass(impl::call_factory<MyClass>([](auto&& f){
            return f.template ActivateInstance<MyClass>(); }))
    {
    }
    inline void MyClass::StaticMethod()
    {
        impl::call_factory<MyClass, MyProject::IClassStatics>([&](auto&& f) {
            return f.StaticMethod(); });
    }
}
```

Não é necessário seguir todas as instruções acima; a intenção é mostrar que as duas chamadas envolvem uma chamada para uma função chamada **call_factory**. Essa é a pista de que essas chamadas envolvem o cache de fábrica e não estão acessando diretamente a implementação. *Com* `-opt[imize]`, essas mesmas funções não são definidas. Em vez disso, elas são declaradas pela projeção e suas definições são deixadas até o componente.

O componente pode, então, fornecer definições que chamam diretamente para a implementação. Agora, chegamos à alteração da falha. Essas definições são geradas quando você usa `-component` e `-opt[imize]`, e são exibidas em um arquivo chamado `Type.g.cpp`, em que *Type* é o nome da classe de tempo de execução que está sendo implementada. É por isso que você pode atingir vários erros de vinculador quando habilita `-opt[imize]` pela primeira vez em um projeto existente. É necessário incluir esse arquivo gerado para sua implementação a fim de unir as coisas.

Em nosso exemplo, `MyClass.h` pode ter essa aparência (independentemente se `-opt[imize]` está sendo usado).

```cppwinrt
// MyClass.h
#pragma once
#include "MyClass.g.h"
 
namespace winrt::MyProject::implementation
{
    struct MyClass : ClassT<MyClass>
    {
        MyClass() = default;
 
        static void StaticMethod();
        void Method();
    };
}
namespace winrt::MyProject::factory_implementation
{
    struct MyClass : ClassT<MyClass, implementation::MyClass>
    {
    };
}
```

Seu `MyClass.cpp` é o local em que tudo isso se reúne.

```cppwinrt
#include "pch.h"
#include "MyClass.h"
#include "MyClass.g.cpp" // !!It's important that you add this line!!
 
namespace winrt::MyProject::implementation
{
    void MyClass::StaticMethod()
    {
    }
 
    void MyClass::Method()
    {
    }
}
```

Portanto, para usar a construção uniforme em um projeto existente, é necessário editar o arquivo `.cpp` de cada implementação para que você `#include <Sub/Namespace/Type.g.cpp>` depois da inclusão (e da definição) da classe de implementação. Esse arquivo fornece as definições dessas funções que a projeção deixou indefinida. Veja a aparência dessas definições dentro do arquivo `MyClass.g.cpp`.

```cppwinrt
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

E isso conclui perfeitamente a projeção com chamadas eficientes diretamente para a implementação, evitando chamadas ao cache de fábrica e atendendo ao vinculador.

A última coisa que o `-opt[imize]` faz por você é alterar a implementação do `module.g.cpp` do seu projeto (o arquivo que ajuda você a implementar as exportações **DllGetActivationFactory** e **DllCanUnloadNow** de sua DLL) de tal forma que os builds incrementais tendem a ser muito mais rápidos ao eliminar o acoplamento de tipo forte que era exigido pelo C++/WinRT 1.0. Isso é geralmente conhecido como *fábricas de tipo apagado*. Sem `-opt[imize]`, o arquivo `module.g.cpp` gerado para seu componente começa incluindo as definições de todas as suas classes de implementação&mdash;the `MyClass.h`, neste exemplo. Em seguida, ele cria diretamente a fábrica de implementação para cada classe como esta.

```cppwinrt
if (requal(name, L"MyProject.MyClass"))
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
```

Novamente, você não precisa seguir todos os detalhes. É útil ver que isso requer a definição completa para todas as classes implementadas por seu componente. Isso pode ter um efeito significativo sobre seu loop interno, uma vez que qualquer alteração em uma única implementação fará `module.g.cpp` ser recompilado. Com o `-opt[imize]`, isso não acontece mais. Em vez disso, duas coisas acontecem com o arquivo `module.g.cpp` gerado. A primeira é que ele não inclui mais nenhuma classe de implementação. Neste exemplo, ele não incluirá `MyClass.h`. Em vez disso, ele cria as fábricas de implementação sem conhecer sua implementação.

```cppwinrt
void* winrt_make_MyProject_MyClass();
 
if (requal(name, L"MyProject.MyClass"))
{
    return winrt_make_MyProject_MyClass();
}
```

Obviamente, não há necessidade de incluir suas definições, e cabe ao vinculador resolver a definição da função **winrt_make_Component_Class**. É claro que você não precisa pensar nisso, porque o arquivo `MyClass.g.cpp` gerado para você (e que você anteriormente incluiu para dar suporte à construção uniforme) também define essa função. Veja a totalidade do arquivo `MyClass.g.cpp` gerado para este exemplo.

```cppwinrt
void* winrt_make_MyProject_MyClass()
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

Como você pode ver, a função **winrt_make_MyProject_MyClass** cria diretamente a fábrica de sua implementação. Isso tudo significa que você pode alterar qualquer implementação determinada e que o `module.g.cpp` não precisa ser recompilado. Somente quando você adiciona ou remove classes do Windows Runtime que o `module.g.cpp` será atualizado e precisará ser recompilado.

## <a name="overriding-base-class-virtual-methods"></a>Como substituir métodos virtuais da classe base

A classe derivada poderá ter problemas com métodos virtuais se a classe base e a derivada forem classes definidas pelo aplicativo, mas o método virtual for definido em uma classe avô do Windows Runtime. Na prática, isso acontecerá se for feita a derivação de classes XAML. O restante desta seção continua após o exemplo dado em [Classes derivadas](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#derived-classes).

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

A hierarquia é [**Windows::UI::Xaml::Controls::Page**](/uwp/api/windows.ui.xaml.controls.page) \<- **BasePage** \<- **DerivedPage**. O método **BasePage::OnNavigatedFrom** substitui corretamente [**Page::OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom), mas **DerivedPage::OnNavigatedFrom** não substitui **BasePage::OnNavigatedFrom**.

Aqui, **DerivedPage** reutiliza a vtable **IPageOverrides** de **BasePage**, o que significa que ela não substitui o método **IPageOverrides::OnNavigatedFrom**. Uma possível solução exige que a própria **BasePage** seja uma classe de modelo e tenha sua implementação inteiramente em um arquivo de cabeçalho, mas isso torna as coisas complicadas e inaceitáveis.

Como alternativa, declare o método **OnNavigatedFrom** como explicitamente virtual na classe base. Dessa forma, quando a entrada de vtable para **DerivedPage::IPageOverrides::OnNavigatedFrom** chamar **BasePage::IPageOverrides::OnNavigatedFrom**, o produtor chamará **BasePage::OnNavigatedFrom**, que (devido à sua natureza virtual) acabará chamando **DerivedPage::OnNavigatedFrom**.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        // Note the `virtual` keyword here.
        virtual void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

Isso exige que todos os membros da hierarquia de classe concordem com o valor retornado e os tipos de parâmetro do método **OnNavigatedFrom**. Se eles discordarem, você deverá usar a versão acima como o método virtual e encapsular as alternativas.

## <a name="important-apis"></a>APIs Importantes
* [modelo de struct winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [função winrt::com_ptr::copy_from](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [modelo da função winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)
* [modelo da função winrt::get_self](/uwp/cpp-ref-for-winrt/get-self)
* [modelo de struct winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [modelo da função winrt::make](/uwp/cpp-ref-for-winrt/make)
* [modelo da função winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)
* [função winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Controles XAML; associar a uma propriedade C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
