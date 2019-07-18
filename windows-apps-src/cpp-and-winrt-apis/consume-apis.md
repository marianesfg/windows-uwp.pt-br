---
description: Este tópico mostra como consumir as APIs do C++/WinRT, sejam elas implementadas pelo Windows, um fornecedor de componentes de terceiros ou por você mesmo.
title: Utilizar APIs com C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, implementação, classe de tempo de execução, ativação
ms.localizationpriority: medium
ms.openlocfilehash: 88a4c65b20c2fb805baecb8a90498e8e4ec9b229
ms.sourcegitcommit: a7a1e27b04f0ac51c4622318170af870571069f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717628"
---
# <a name="consume-apis-with-cwinrt"></a>Utilizar APIs com C++/WinRT

Este tópico mostra como utilizar as APIs do [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), sejam elas parte do Windows, implementadas por um fornecedor de componentes de terceiros, ou implementada por você mesmo.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Se a API estiver em um namespace do Windows
Esse é o caso mais comum de utilização de uma API do Windows Runtime. Para cada tipo em um namespace do Windows definido nos metadados, C ++/WinRT define um equivalente C++ amigável (chamado de *tipo projetado*). Um tipo projetado tem o mesmo nome totalmente qualificado do tipo do Windows, mas é colocado no namespace C++ **winrt** usando a sintaxe do C++. Por exemplo, [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) é projetado no C++/WinRT como **winrt::Windows::Foundation::Uri**.

A seguir apresentamos um exemplo de código simples. Se quiser copiar e colar os exemplos de código a seguir diretamente no arquivo de código-fonte principal de um projeto de **Aplicativo de Console do Windows (C++/WinRT)** , primeiro defina **Não usar Cabeçalhos Pré-compilados** nas propriedades do projeto.

```cppwinrt
// main.cpp
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

O cabeçalho `winrt/Windows.Foundation.h` incluído faz parte do SDK, e pode ser encontrado dentro da pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Os cabeçalhos nessa pasta contêm tipos de namespace do Windows projetados em C++/WinRT. Neste exemplo, `winrt/Windows.Foundation.h` contém **winrt::Windows::Foundation::Uri**, que é o tipo projetado para a classe de tempo de execução [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Sempre que desejar usar um tipo de namespace do Windows, inclua o cabeçalho do C++/WinRT correspondente a esse namespace. As diretivas `using namespace` são opcionais, mas convenientes.

No exemplo de código acima, após iniciar o C++/WinRT, alocamos em pilhas um valor do tipo projetado **winrt::Windows::Foundation::Uri** por meio de um de seus construtores documentados publicamente ([**Uri(String)** ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_), neste exemplo). Este é o caso de uso mais comum, e normalmente é tudo o que você precisa fazer. Quando tiver um valor de tipo projetado do C++/WinRT, é possível tratá-lo como se fosse uma instância do tipo real do Windows Runtime, pois já tem todos os mesmos membros.

Na verdade, esse valor projetado é um proxy; essencialmente, é apenas um ponteiro inteligente de um objeto existente. A chamada de construtores do valor projetado [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) para criar uma instância da classe existente do Windows Runtime (**Windows.Foundation.Uri**, neste caso) e armazenar a interface padrão do objeto no novo valor projetado. Como mostrado abaixo, suas chamadas para os membros do valor projetado na verdade delegam, por meio do ponteiro inteligente, para o objeto existente, que é onde ocorrem alterações de estado.

![O tipo projetado Windows::Foundation::Uri](images/uri.png)

Quando o valor `contosoUri` não está mais no escopo, ele destrói e libera a referência à interface padrão. Se essa é a última referência ao objeto existente **Windows.Foundation.Uri** do Windows Runtime, o objeto existente também é destruído.

> [!TIP]
> Um *tipo projetado* é um wrapper ao longo de uma classe de tempo de execução para utilizar suas APIs. Uma *interface projetada* é um wrapper sobre uma interface do Windows Runtime.

## <a name="cwinrt-projection-headers"></a>Cabeçalhos de projeção do C++/WinRT
Para utilizar APIs de namespace do Windows a partir do C++/WinRT, é preciso incluir os cabeçalhos da pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. É comum que um tipo em um namespace subordinado faça referência aos tipos no namespace pai imediato. Consequentemente, cada cabeçalho de projeção do C++/WinRT inclui automaticamente o arquivo de cabeçalho do namespace pai para que você não *precise* incluí-lo explicitamente. No entanto, se você fizer isso, não haverá nenhum erro.

Por exemplo, para o namespace [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates), as definições de tipo do C++/WinRT equivalentes estão em `winrt/Windows.Security.Cryptography.Certificates.h`. Os tipos em **Windows::Security::Cryptography::Certificates** exigem tipos no namespace pai **Windows::Security::Cryptography** e os tipos nesse namespace podem exigir tipos no próprio pai, **Windows::Security**.

Então, ao incluir `winrt/Windows.Security.Cryptography.Certificates.h`, esse arquivo inclui `winrt/Windows.Security.Cryptography.h`; e `winrt/Windows.Security.Cryptography.h` inclui `winrt/Windows.Security.h`. E é aqui que a trilha acaba, pois não há um `winrt/Windows.h`. Esse processo de inclusão transitiva para no namespace de segundo nível.

Esse processo inclui transitivamente os arquivos de cabeçalho que fornecem as *declarações* e *implementações* necessárias para as classes definidas nos namespaces pai.

Um membro de um tipo em um namespace pode fazer referência a um ou mais tipos de outros namespaces não relacionados. Para que o compilador faça a compilação dessa definições de membros, deve ver as declarações de tipo para o fechamento de todos esses tipos. Consequentemente, cada cabeçalho de projeção do C++/WinRT inclui os cabeçalhos de namespace necessários para *declarar* quaisquer tipos dependentes. Ao contrário do namespaces pai, esse processo *não* usa as *implementações* para tipos mencionados.

> [!IMPORTANT]
> Quando você realmente deseja *usar* um tipo (instanciar, chamar métodos, etc.) declarado em um namespace não relacionado, deve incluir o arquivo de cabeçalho de namespace apropriado para o tipo. Somente *declarações*, e não *implementações*, são incluídas automaticamente.

Por exemplo, se você incluir somente `winrt/Windows.Security.Cryptography.Certificates.h`, isso fará com que as declarações sejam obtidas desses namespaces (e assim por diante, transitivamente).

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

Ou seja, algumas APIs têm encaminhamento declarado em um cabeçalho que você incluiu. Entretanto, suas definições estão em um cabeçalho ainda não incluído. Portanto, ao chamar [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri), você receberá um erro de vinculador indicando que o membro não está definido. A solução é usar `#include <winrt/Windows.Foundation.h>` explicitamente. Em geral, ao ver um erro de vinculador assim, inclua o cabeçalho nomeado para o namespace da API e recompile.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>Acessar membros pelo objeto, por uma interface ou pela ABI
Com a projeção do C++/WinRT, a representação do tempo de execução de uma classe do Windows Runtime é somente uma interface ABI subjacente. No entanto, para sua conveniência, é possível codificar em relação às classes da maneira que o autor pretendia. Por exemplo, chame o método **ToString** de um [**Uri**](/uwp/api/windows.foundation.uri) como se fosse um método da classe (na verdade, por baixo dos panos, é um método na interface **IStringable** separada).

`WINRT_ASSERT` é uma definição de macro e se expande para [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

Essa conveniência é obtida por meio de uma consulta na interface apropriada. Entretanto, você está sempre no controle. É possível optar por trocar um pouco dessa conveniência por desempenho ao recuperar a interface IStringable por conta própria e usando-a diretamente. No exemplo de código abaixo, obtém-se um ponteiro de interface IStringable real no tempo de execução (por meio de uma consulta única). Depois disso, a chamada para **ToString** é direta e evita qualquer chamada para **QueryInterface**.

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

Escolha essa técnica se souber que chamará vários métodos na mesma interface.

A propósito, é possível acessar membros no nível da ABI, se quiser. O exemplo de código abaixo mostra como fazer isso, e é possível saber mais e obter exemplos de código em [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md).

```cppwinrt
#include <Windows.Foundation.h>
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
using namespace winrt::Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };

    int port{ contosoUri.Port() }; // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri{
        contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>() };
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>Inicialização atrasada

Até mesmo o construtor padrão de um tipo projetado faz com que um objeto existente do Windows Runtime seja criado. É possível construir uma variável de um tipo projetado sem construir um objeto do Windows Runtime, para que seja possível atrasar esse trabalho, se desejar. Declare a variável ou o campo usando o construtor especial do C++/WinRT **std::nullptr_t** para o tipo projetado. a projeção de C++/WinRT injeta este construtor em cada classe de tempo de execução.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

#define MAX_IMAGE_SIZE 1024

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

int main()
{
    winrt::init_apartment();
    Sample s;
    // ...
    s.DelayedInit();
}
```

Todos os construtores no tipo projetado, *exceto* pelo construtor **std::nullptr_t**, resultam na criação do objeto existente do Windows Runtime. O construtor **std::nullptr_t** é, essencialmente, inoperante. Ele espera que o objeto projetado seja iniciado posteriormente. Portanto, se uma classe de tempo de execução tem ou não um construtor padrão, use essa técnica para uma inicialização atrasada eficiente.

Essa consideração afeta outros lugares em que se chama o construtor padrão, como em vetores e mapas. Considere este exemplo de código, para o qual será necessário um projeto **Aplicativo em Branco (C++/WinRT)** .

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

A atribuição cria um novo **TextBlock** e o substitui imediatamente por `value`. A solução é a seguinte.

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

### <a name="dont-delay-initialize-by-mistake"></a>Não inicializar com atraso por engano

Tenha cuidado para não invocar o construtor **std::nullptr_t** por engano. A resolução de conflitos do compilador o favorece em detrimento dos construtores de fábrica. Por exemplo, considere estas duas definições de classe de tempo de execução.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox();
}

// Gift.idl
runtimeclass Gift
{
    Gift(GiftBox giftBox); // You can create a gift inside a box.
}
```

Digamos que queremos construir um **Gift** que não está dentro de uma caixa (um **Gift** construído com um **GiftBox** não inicializado). Primeiro, vamos ver a maneira *errada* de fazer isso. Sabemos que há um construtor **Gift** que usa um **GiftBox**. Mas, se estivermos tentados a passar um **GiftBox** nulo (invocando o construtor **Gift** por meio da inicialização uniforme, como fazemos abaixo), *não* teremos o resultado desejado.

```cppwinrt
// These are *not* what you intended. Doing it in one of these two ways
// actually *doesn't* create the intended backing Windows Runtime Gift object;
// only an empty smart pointer.

Gift gift{ nullptr };
auto gift{ Gift(nullptr) };
```

O que você terá aqui é um **Gift** não inicializado. Você não recebe um **Gift** com um **GiftBox** não inicializado. Esta é a forma *correta* de fazer isso.

```cppwinrt
// Doing it in one of these two ways creates an initialized
// Gift with an uninitialized GiftBox.

Gift gift{ GiftBox{ nullptr } };
auto gift{ Gift(GiftBox{ nullptr }) };
```

No exemplo incorreto, passar um literal `nullptr` resolve em favor do construtor de inicialização com atraso. Para resolver em favor do construtor de fábrica, o tipo do parâmetro deve ser **GiftBox**. Você ainda terá a opção de passar explicitamente um **GiftBox** com inicialização com atraso, conforme mostrado no exemplo correto.

O exemplo a seguir *também* está correto, pois o parâmetro tem o tipo GiftBox e não **std::nullptr_t**.

```cppwinrt
GiftBox giftBox{ nullptr };
Gift gift{ giftBox }; // Calls factory constructor.
```

A ambiguidade surge apenas quando você passa um literal `nullptr`.

## <a name="dont-copy-construct-by-mistake"></a>Não construir por cópia por engano.

Este aviso é semelhante ao descrito na seção [Não inicializar com atraso por engano](#dont-delay-initialize-by-mistake) acima.

Além do construtor de inicialização com atraso, a projeção de C++/WinRT também injeta um construtor de cópia em cada classe de tempo de execução. Trata-se de um construtor de parâmetro único que aceita o mesmo tipo que o objeto que está sendo construído. Os ponteiro inteligente resultante aponta para o mesmo objeto do Windows Runtime que é apontado pelo seu parâmetro de construtor. O resultado são dois objetos de ponteiro inteligente apontando para o mesmo objeto.

Esta é uma definição de classe de tempo de execução que usaremos nos exemplos de código.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox(GiftBox biggerBox); // You can place a box inside a bigger box.
}
```

Vamos supor que queremos construir um **GiftBox** dentro de um **GiftBox** maior.

```cppwinrt
GiftBox bigBox{ ... };

// These are *not* what you intended. Doing it in one of these two ways
// copies bigBox's backing-object-pointer into smallBox.
// The result is that smallBox == bigBox.

GiftBox smallBox{ bigBox };
auto smallBox{ GiftBox(bigBox) };
```

A forma *correta* de fazer isso é chamar a fábrica de ativação explicitamente.

```cppwinrt
GiftBox bigBox{ ... };

// These two ways call the activation factory explicitly.

GiftBox smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
auto smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Se a API é implementada em um componente do Windows Runtime
Esta seção se aplica se você criou o componente por conta própria ou se ele veio por meio de um fornecedor.

> [!NOTE]
> Para saber mais sobre como instalar e usar a VSIX (Extensão do Visual Studio) para C++/WinRT e o pacote do NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira [Suporte ao Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

No seu projeto de aplicativo, faça referência ao arquivo de metadados do Windows Runtime do componente do Windows Runtime (`.winmd`) e compile. Durante a compilação, a ferramenta `cppwinrt.exe` gera uma biblioteca C++ padrão que descreve completamente, ou *projeta*, a superfície de API do componente. Em outras palavras, a biblioteca gerada contém os tipos projetados para o componente.

Em seguida, assim como acontece com um tipo de namespace do Windows, inclua um cabeçalho e construa o tipo projetado por meio de um de seus construtores. O código de inicialização do seu projeto de aplicativo registra a classe de tempo de execução, e o construtor do tipo projetado chama [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) para ativar a classe de tempo de execução do componente mencionado.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Para saber mais, obter códigos e ver um passo a passo do consumo de APIs implementadas em um componente do Windows Runtime, confira [Criar eventos em C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>Se a API é implementada no projeto utilizado
Um tipo utilizado a partir da interface de usuário do XAML deve ser uma classe de tempo de execução, mesmo que esteja no mesmo projeto do XAML.

Para esse cenário, gere um tipo projetado a partir de metadados do Windows Runtime da classe de tempo de execução (`.winmd`). Novamente, inclua um cabeçalho, mas desta vez construa o tipo projetado por meio de seu construtor **std::nullptr_t**. Esse construtor não realiza qualquer inicialização, portanto, em seguida é preciso atribuir um valor para a instância por meio da função auxiliar [**winrt::make**](/uwp/cpp-ref-for-winrt/make), passando quaisquer argumentos de construtor necessários. Uma classe de tempo de execução implementada no mesmo projeto que o código utilizado não precisa ser registrada, nem instanciada por meio de ativação do Windows Runtime/COM.

É preciso um projeto **Aplicativo em Branco (C++/WinRT))** para este exemplo de código.

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
    m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

Para saber mais, obter códigos e ver um passo a passo sobre o consumo de uma classe de tempo de execução implementada no projeto de consumo, confira [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Instanciar e retornar tipos e interfaces projetados
Veja um exemplo da aparência dos tipos projetados e interfaces ao utilizar seu projeto. Lembre-se de que um tipo projetado (como o deste exemplo), é gerado por ferramenta e não é algo que você pode criar por conta própria.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** é um tipo projetado; as interfaces projetadas incluem **IMyRuntimeClass**, **IStringable** e **IClosable**. Este tópico mostra as diferentes maneiras de como instanciar um tipo projetado. Este é um lembrete e um resumo, usando **MyRuntimeClass** como exemplo.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- É possível acessar os membros de todas as interfaces de um tipo projetado.
- E retornar um tipo projetado para um chamador.
- Tipos projetados e interfaces derivam de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Assim, chame [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) em um tipo projetado ou interface para consultar outras interfaces projetadas, que também podem ser usadas ou retornadas para um chamador. A função de membro **as** funciona como [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)).

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
A maneira conveniente e direta de criar um objeto de C++/WinRT é a seguinte.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

Pode haver momentos em que se deseja criar o alocador de ativação por conta própria e, em seguida, criar objetos a partir dele para sua conveniência. Veja alguns exemplos mostrando como fazer isso usando o modelo de função [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory).

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
* [Interface QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Função RoActivateInstance](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Classe Windows::Foundation::Uri](/uwp/api/windows.foundation.uri)
* [Modelo de função winrt::get_activation_factory](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar eventos em C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md)
* [Introdução ao C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Controles XAML; associar a uma propriedade C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
