---
author: stevewhims
description: Este tópico mostra como consumir as APIs do C++/WinRT, sejam elas implementadas pelo Windows, um fornecedor de componentes de terceiros, ou por você mesmo.
title: Consumir APIs com C++/WinRT
ms.author: stwhi
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, implementação, classe de tempo de execução, ativação
ms.localizationpriority: medium
ms.openlocfilehash: 136abd5e3312b7a387ccc3b7c993d4e70d8ef0d4
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4148341"
---
# <a name="consume-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Consumir APIs com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Este tópico mostra como consumir as APIs do C++/WinRT, sejam elas parte do Windows implementadas por um fornecedor de componentes de terceiros, ou implementada por você mesmo.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Se a API estiver em um namespace do Windows
Esse é o caso mais comum em que você consumirá uma API do Windows Runtime. Para cada tipo em um namespace do Windows definido nos metadados, C ++/WinRT define um equivalente C++ amigável (chamado de *tipo projetado*). Um tipo projetado tem o mesmo nome totalmente qualificado do tipo do Windows, mas ele é colocado no namespace C++ **winrt** usando a sintaxe do C++. Por exemplo, [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) é projetado no C++/WinRT como **winrt::Windows::Foundation::Uri**.

A seguir está um exemplo simples de código.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
    Uri combinedUri = contosoUri.CombineUri(L"products");
}
```

O cabeçalho `winrt/Windows.Foundation.h` incluído faz parte do SDK, encontrado dentro da pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Os cabeçalhos nessa pasta contêm tipos de namespace do Windows projetados em C++/WinRT. Neste exemplo, `winrt/Windows.Foundation.h` contém **winrt::Windows::Foundation::Uri**, que é o tipo projetado para a classe de tempo de execução [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Sempre que desejar usar um tipo de namespace do Windows, inclua o cabeçalho do C++/WinRT correspondente a esse namespace. As diretivas de `using namespace` são opcionais, mas convenientes.

No exemplo de código acima, após inicializar C++/WinRT, alocamos em pilhas um valor do tipo projetado **winrt::Windows::Foundation::Uri** por meio de um de seus construtores documentados publicamente ([**Uri(String)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_), neste exemplo). Para isso, o caso de uso mais comum, que normalmente é tudo o que você precisa fazer. Quando você tiver um valor de tipo projetado do C++/WinRT, é possível tratá-lo como se fosse uma instância do tipo real do Windows Runtime, pois já tem todos os mesmos membros.

Na verdade, esse valor projetada é um proxy; essencialmente, é apenas um ponteiro inteligente de um objeto subjacente. A chamada de construtores do valor projetado [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) para criar uma instância da classe de suporte do Windows Runtime (**Windows.Foundation.Uri**, neste caso) e armazenar a interface padrão do objeto no novo valor projetado. Como mostrado abaixo, suas chamadas para os do valor projetada delegam, por meio do ponteiro inteligente, para o objeto de suporte, que é onde ocorrem alterações de estado.

![O tipo Windows::Foundation::Uri projetado](images/uri.png)

Quando o valor `contosoUri` não estiver mais no escopo, e destrói e libera a referência à interface padrão. Se essa referência é a última referência ao objeto de suporte **Windows.Foundation.Uri** do Windows Runtime, o objeto de suporte também é destruído.

> [!TIP]
> Um *tipo projetado* é um wrapper ao longo de uma classe de tempo de execução para fins de consumo de suas APIs. Uma *interface projetada* é um wrapper sobre uma interface do Windows Runtime.

## <a name="cwinrt-projection-headers"></a>Cabeçalhos de projeção do C++/WinRT
Para consumir APIs de namespace do Windows do C++/WinRT, você deve incluir os cabeçalhos da pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. É comum para um tipo em um namespace subordinado referenciar tipos no namespace pai imediato. Consequentemente, cada cabeçalho de projeção do C++/WinRT inclui automaticamente o arquivo de cabeçalho do namespace pai para que você não *precise* inclui-lo explicitamente. Embora, nesse caso, não exista nenhum erro.

Por exemplo, para o namespace [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates), as definições de tipo do C++/WinRT equivalentes residem em `winrt/Windows.Security.Cryptography.Certificates.h`. Os tipos em **Windows::Security::Cryptography::Certificates** exigem tipos no namespace pai **Windows::Security::Cryptography** e os tipos nesse namespace podem exigir tipos no próprio pai, **Windows::Security**.

Então, ao incluir `winrt/Windows.Security.Cryptography.Certificates.h`, esse arquivo inclui `winrt/Windows.Security.Cryptography.h`; e `winrt/Windows.Security.Cryptography.h`inclui `winrt/Windows.Security.h`. É onde a trilha para, pois não há nenhum `winrt/Windows.h`. Esse processo de inclusão transitiva para no namespace de segundo nível.

Esse processo inclui temporariamente os arquivos de cabeçalho que fornecem as *declarações* e *implementações* necessárias para as classes definidas nos namespaces pai.

Um membro de um tipo em um namespace pode fazer referência a um ou mais tipos de outros namespaces não relacionados. Para que o compilador faça a compilação dessa definições de membros, ele deve ver as declarações de tipo para o fechamento de todos esses tipos. Consequentemente, cada cabeçalho de projeção do C++/WinRT inclui os cabeçalhos de namespace necessários para *declarar* quaisquer tipos dependentes. Ao contrário do namespaces pai, esse processo *não* usa as *implementações* para tipos mencionados.

> [!IMPORTANT]
> Quando você realmente deseja *usar* um tipo (instanciar, chamar métodos etc.) declarado em um namespace não relacionado, você deve incluir o arquivo de cabeçalho de namespace apropriado para o tipo. Somente *declarações* e não *implementações* são incluídas automaticamente.

Por exemplo, se você incluir somente `winrt/Windows.Security.Cryptography.Certificates.h`, isso faz com que declarações sejam obtidos dos namespaces (e assim por diante, temporariamente).

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

Ou seja, algumas APIs são de encaminhamento declarado em um cabeçalho que você incluiu. Mas suas definições estão em um cabeçalho ainda não incluído. Portanto, ao chamar [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri), você receberá um erro de vinculador indicando que o membro não é definido. A solução é `#include <winrt/Windows.Foundation.h>` explicitamente. Em geral, ao ver um erro de vinculador assim, inclua o cabeçalho nomeado para o namespace da API e recompile.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>Acessar membros pelo objeto, por meio de uma interface ou ABI
Com a projeção do C++/WinRT, a representação do tempo de execução de uma classe do Windows Runtime é a interface de ABI subjacente. No entanto, para sua conveniência, é possível codificar em relação às classes da maneira que o autor pretendia. Por exemplo, você pode chamar o método **ToString** de um [**Uri**](/uwp/api/windows.foundation.uri) como se fosse um método da classe (na verdade, por trás é um método na interface separada de **IStringable**).

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

Essa conveniência é obtida por meio de uma consulta para a interface apropriada. Mas você está sempre no controle. Você pode optar por trocar um pouco dessa conveniência por desempenho recuperando a interface IStringable por conta própria e usando-a diretamente. No exemplo de código abaixo, você pode obter um ponteiro de interface IStringable real no tempo de execução (por meio de uma única consulta). Depois disso, a chamada para **ToString** é direta e evita qualquer chamada para **QueryInterface**.

```cppwinrt
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

Você pode escolher essa técnica se souber que chamará vários métodos na mesma interface.

A propósito, é possível acessar membros no nível de ABI, se quiser. O exemplo de código abaixo mostra como fazer isso, e há mais detalhes e exemplos de código em [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md).

```cppwinrt
int port = contosoUri.Port(); // Access the Port "property" accessor via C++/WinRT.

winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri = contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>();
HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
```

## <a name="delayed-initialization"></a>Inicialização atrasada
Até mesmo o construtor padrão de um tipo projetado faz com que um objeto de suporte do Windows Runtime seja criado. É possível construir uma variável de um tipo projetado sem construir um objeto do Windows Runtime (para que você pode atrasar esse trabalho até mais tarde), se desejar. Declare a variável ou o campo usando o construtor especial do C++/WinRT `nullptr_t` para o tipo projetado.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

Todos os construtores no tipo projetado *exceto* o construtor `nullptr_t` resultam na criação do objeto de suporte do Windows Runtime. O construtor `nullptr_t` é essencialmente uma não operação. Ele espera que o objeto projetado seja inicializado em um momento subsequente. Portanto, se uma classe de tempo de execução tem um construtor padrão ou não, você pode usar essa técnica para a inicialização atrasada eficiente.

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Se a API é implementada em um componente do Tempo de Execução do Windows
Esta seção se aplica se você criou o componente por conta própria ou se ele veio por meio de um fornecedor.

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

No seu projeto de aplicativo, referencie o arquivo de metadados do Windows Runtime do componente do Tempo de Execução do Windows (`.winmd`) e compile. Durante a compilação, a ferramenta `cppwinrt.exe` gera uma biblioteca C++ padrão que descreve completamente&mdash;ou *projeta*&mdash;a superfície de API do componente. Em outras palavras, a biblioteca gerada contém os tipos projetados para o componente.

Em seguida, assim como acontece com um tipo de namespace do Windows, inclua um cabeçalho e construa o tipo projetado por meio de um de seus construtores. O código de inicialização do seu projeto de aplicativo registra a classe de tempo de execução, e o construtor do tipo projetado chama [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) para ativar a classe de tempo de execução do componente referenciado.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Para obter mais detalhes, código e um passo a passo do consumo de APIs implementadas em um componente do Tempo de Execução do Windows, consulte [Criar eventos em C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>Se a API é implementada no projeto de consumo
Um tipo consumido a partir da interface de usuário do XAML deve ser uma classe de tempo de execução, mesmo que ele esteja no mesmo projeto como o XAML.

Para esse cenário, você deve gerar um tipo projetado de metadados do Windows Runtime da classe de tempo de execução (`.winmd`). Novamente, inclua um cabeçalho, mas desta vez construa o tipo projetado por meio de seu construtor `nullptr`. Esse construtor não realiza qualquer inicialização, portanto, em seguida você deve atribuir um valor para a instância por meio da função auxiliar [**winrt::make**](/uwp/cpp-ref-for-winrt/make), passando quaisquer argumentos de construtor necessários. Uma classe de tempo de execução implementada no mesmo projeto que o código não precisa ser registrada, nem instanciada por meio de ativação do Windows Runtime/COM.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

Para obter mais detalhes, código e um passo a passo sobre consumo de uma classe de tempo de execução implementada no projeto do consumo, consulte [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Criação de instância e retorno de tipos projetados e interfaces
Veja um exemplo de como os tipos e interfaces projetados podem parecer em seu projeto de consumo.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** é um tipo projetado; interfaces projetadas incluem **IMyRuntimeClass**, **IStringable** e **IClosable**. Este tópico mostra as diferentes maneiras de como você pode instanciar um tipo projetado. Aqui está um lembrete e um resumo, usando **MyRuntimeClass** como exemplo.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- Você pode acessar os membros de todas as interfaces de um tipo projetado.
- Você pode retornar um tipo projetado para o chamador.
- Tipos projetados e interfaces derivam de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Assim, você pode chamar [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) em um tipo projetado ou interface para consultar outras interfaces projetadas, que você também pode usar ou retornar para o chamador. A função de membro **como** funciona como [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521).

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>Alocadores de ativação
A maneira conveniente e direta de criar um objeto C++/WinRT é a seguinte.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

Pode haver momentos em que você vai desejar criar o alocador de ativação por conta própria e, em seguida, criar objetos a partir dele para sua conveniência. Veja alguns exemplos mostrando como fazer isso usando o modelo de função [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory).

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
auto factory = winrt::get_activation_factory<CurrencyFormatter, ICurrencyFormatterFactory>();
CurrencyFormatter currency = factory.CreateCurrencyFormatterCode(L"USD");
```

```cppwinrt
using namespace winrt::Windows::Foundation;
...
auto factory = winrt::get_activation_factory<Uri, IUriRuntimeClassFactory>();
Uri account = factory.CreateUri(L"http://www.contoso.com");
```

As classes nos dois exemplos acima são tipos de um namespace do Windows. No exemplo a seguir, **BankAccountWRC::BankAccount** é um tipo personalizado implementado em um componente do Windows Runtime.

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="important-apis"></a>APIs Importantes
* [Interface QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Função RoActivateInstance](https://msdn.microsoft.com/library/br224646)
* [Classe Foundation](/uwp/api/windows.foundation.uri)
* [modelo de função winrt::get_activation_factory](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar eventos com C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md)
* [Introdução ao C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
