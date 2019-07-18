---
description: Este tópico mostra como criar APIs de C++/WinRT usando a estrutura de base **winrt::implements**, direta ou indiretamente.
title: Criar APIs com C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, implementação, implementar, classe de tempo de execução, ativação
ms.localizationpriority: medium
ms.openlocfilehash: 74d15b517c5ec6547115bc8ffdb44a2b742c68d6
ms.sourcegitcommit: a7a1e27b04f0ac51c4622318170af870571069f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717665"
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

Se o tipo estiver empacotado em um componente do Windows Runtime para consumo a partir de um aplicativo, então, ele precisará ser uma classe de tempo de execução.

> [!TIP]
> É recomendável que você declare cada classe de tempo de execução em seu próprio arquivo de linguagem IDL (.idl), para otimizar o desempenho do build ao editar um arquivo IDL e para a correspondência lógica de um arquivo IDL com seus arquivos de código-fonte gerados. O Visual Studio mescla todos os arquivos `.winmd` resultantes em um único arquivo com o mesmo nome do namespace raiz. O arquivo `.winmd` final será aquele que os consumidores de seu componente consultarão.

Aqui está um exemplo.

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
- **winrt::MyProject::implementation**. Este namespace contém tipos de implementação. Um objeto de um tipo de implementação não é um ponteiro; ele é um valor&mdash;um objeto de pilha C++ completo. Não construa um tipo de implementação diretamente; em vez disso, chame [**winrt::make**](/uwp/cpp-ref-for-winrt/make) passando o tipo de implementação como o parâmetro de modelo. Mostramos exemplos de **winrt::make** em ação anteriormente neste tópico, e há outro exemplo em [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).
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
> Esse tipo de bug com corrupção de memória é difícil de diagnosticar. Portanto, para compilações de depuração, uma asserção C++/WinRT ajuda a identificar esse erro usando um detector de pilha. No entanto, as corrotinas são alocadas no heap, de modo que você não obterá ajuda com esse erro se ele ocorrer dentro de uma corrotina.

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
