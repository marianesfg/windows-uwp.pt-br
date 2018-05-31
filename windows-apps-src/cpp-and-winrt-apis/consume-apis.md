---
author: stevewhims
description: Este tópico mostra como consumir as APIs do C++/WinRT, sejam elas implementadas pelo Windows, um fornecedor de componentes de terceiros, ou por você mesmo.
title: Consumir APIs com C++/WinRT
ms.author: stwhi
ms.date: 04/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, implementação, classe de tempo de execução, ativação
ms.localizationpriority: medium
ms.openlocfilehash: e777aca8f1d9f3892f67b10c785c056b14c8070c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832240"
---
# <a name="consume-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Consumir APIs com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Este tópico mostra como consumir as APIs do C++/WinRT, sejam elas parte do Windows implementadas por um fornecedor de componentes de terceiros, ou implementada por você mesmo.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Se a API estiver em um namespace do Windows
Esse é o caso mais comum em que você consumirá uma API do Windows Runtime. A seguir está um exemplo simples de código.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
}
```

O cabeçalho `winrt/Windows.Foundation.h` incluído faz parte do SDK, encontrado dentro da pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Os cabeçalhos nessa pasta contêm APIs do Windows projetadas em C++/WinRT. Sempre que desejar usar um tipo de namespace do Windows, inclua o cabeçalho de projeção do C++/WinRT correspondente a esse namespace. As diretivas de `using namespace` são opcionais, mas convenientes.

Neste exemplo, `winrt/Windows.Foundation.h` contém o tipo projetado para a classe de tempo de execução [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Um *tipo projetado* é um wrapper ao longo de uma classe de tempo de execução para fins de consumo de suas APIs. Uma *interface projetada* é um wrapper sobre uma interface do Windows Runtime.

No exemplo de código acima, após inicializar C++/WinRT, construímos o tipo projetado **Uri** através de um de seus construtores documentados publicamente ([**Uri(String)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_), neste exemplo). Para isso, o caso de uso mais comum, que é tudo o que você precisa fazer.

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Se a API é implementada em um componente do Tempo de Execução do Windows
Esta seção se aplica se você criou o componente por conta própria ou se ele veio por meio de um fornecedor.

> [!NOTE]
> Para obter informações sobre a disponibilidade atual do Visual Studio Extension (VSIX) de C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild de C++/WinRT), veja [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

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

## <a name="important-apis"></a>APIs Importantes
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar eventos com C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
