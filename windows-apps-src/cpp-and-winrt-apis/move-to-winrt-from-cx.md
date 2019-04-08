---
description: Este tópico mostra como fazer a portabilidade do código C++/CX para seu equivalente no C++/WinRT.
title: Mudar de C++/CX para C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: fe988bffbf024308fb5d43da7ed538e5330b58de
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635071"
---
# <a name="move-to-cwinrt-from-ccx"></a>Mudar de C++/CX para C++/WinRT

Este tópico mostra como portar o código em um [C + + c++ /CLI CX](/cpp/cppcx/visual-c-language-reference-c-cx) projeto para seu equivalente em [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Estratégias de portabilidade

Se você deseja portar gradualmente C + c++ /CLI código c++ /CX para C + + c++ /CLI WinRT, você pode. C + + c++ /CLI CX e C + + c++ /CLI código WinRT pode coexistir no mesmo projeto, com as exceções de suporte do compilador XAML e componentes de tempo de execução do Windows. Para essas duas exceções, será necessário direcionar o C + + para c++ /CLI CX ou C + + c++ /CLI WinRT dentro do mesmo projeto.

> [!IMPORTANT]
> Se seu projeto compila um aplicativo XAML, em seguida, um fluxo de trabalho, recomendamos que é primeiro criar um novo projeto no Visual Studio usando um do C + c++ /CLI WinRT modelos de projeto (consulte [suporte do Visual Studio para C + + c++ /CLI WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Em seguida, começar a copiar código-fonte e a marcação ao longo do C + + / projeto do CX. Você pode adicionar novas páginas XAML com **Project** \> **Adicionar Novo Item...** \> **Visual C++** > **página em branco (C + + / WinRT)**.
>
> Como alternativa, você pode usar um componente de tempo de execução do Windows para código fator fora do XAML C + + / CX projeto conforme você portá-lo. Mova tanta C + + c++ /CLI CX como você pode em um componente e, em seguida, altere o projeto XAML para C + + de código c++ /CLI WinRT. Ou, caso contrário, deixe o projeto XAML como C + + c++ /CLI CX, criar um novo c++ /CLI componente do WinRT e começar a portabilidade de C + + c++ /CLI para fora do projeto XAML e no componente de código do CX. Você também pode ter um C + + c++ /CLI projeto de componente do CX junto com um C + + c++ /CLI projeto de componente do WinRT dentro da mesma solução, os dois de referência do seu projeto de aplicativo e a porta gradualmente uns aos outros. Ver [interoperabilidade entre o C + + c++ /CLI WinRT e C + + c++ /CLI CX](interop-winrt-cx.md) para obter mais detalhes sobre como usar as projeções de dois linguagem no mesmo projeto.

> [!NOTE]
> O [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e o SDK do Windows declaram tipos no namespace raiz **Windows**. Um tipo do Windows projetado no C++/WinRT tem o mesmo nome totalmente qualificado do tipo do Windows, mas ele é colocado no namespace C++ **winrt**. Esses namespaces distintos permitem que você faça a transferência do C++/CX para o C++/WinRT em seu próprio ritmo.

Tendo em mente as exceções mencionadas acima, a primeira etapa na portabilidade C + c++ /CLI projeto CX C + c++ /CLI WinRT é adicionar manualmente C + + c++ /CLI WinRT suporte a ele (consulte [suporte do Visual Studio para C + + c++ /CLI WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para fazer isso, instale o [pacote do Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) em seu projeto. Abra o projeto no Visual Studio, clique em **Project** \> **gerenciar pacotes NuGet...** \> **Navegue**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e, em seguida, clique em **instalar** para instalar o pacote para o projeto. Um efeito dessa alteração é que o suporte para C++/CX é desativado no projeto. É uma boa ideia deixar suporte desativado para que as mensagens de build ajudarão-lo a localizar (e porta) todas as suas dependências no C + + c++ /CLI CX, ou você pode ativar o suporte novamente (nas propriedades do projeto **C/C++** \> **geral** \> **Consomem o Windows Runtime Extension** \> **Sim (/ZW)**) e a porta gradualmente.

Verifique se essa propriedade de projeto **gerais** \> **versão da plataforma de destino** é definido como 10.0.17134.0 (Windows 10, versão 1803) ou maior.

No arquivo de cabeçalho pré-compilado (em geral, `pch.h`), inclua `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Se você incluir qualquer cabeçalho de API C++/WinRT projetado do Windows (por exemplo, `winrt/Windows.Foundation.h`), não é necessário incluir explicitamente `winrt/base.h` dessa forma, pois será incluído automaticamente para você.

Caso o projeto também use os tipos da [Biblioteca de Modelos C++ do Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl), consulte [Mover para C++/WinRT a partir da WRL](move-to-winrt-from-wrl.md).

## <a name="parameter-passing"></a>Passagem de parâmetro
Quando estiver escrevendo C + + c++ /CLI, código-fonte CX passar C + + c++ /CLI tipos CX como parâmetros de função como chapéu (\^) referências.

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

No C++/WinRT, para funções síncronas, você deve usar parâmetros `const&` por padrão. Isso evita cópia e sobrecarga encaixada. Entretanto, as corrotinas deve usar passar por valor para garantir a captura pelo valor e evitar problemas de tempo de vida (para ver mais detalhes, consulte [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Um objeto do C++/WinRT é fundamentalmente um valor que mantém um ponteiro da interface para o objeto do Windows Runtime subjacente. Quando você copia um objeto do C++/WinRT, o compilador copia o ponteiro da interface encapsulado, aumentando sua contagem de referência. A destruição eventual da cópia envolve reduzir a contagem de referência. Portanto, apenas incorre na sobrecarga de uma cópia quando necessário.

## <a name="variable-and-field-references"></a>Referências de variáveis e campo
Quando estiver escrevendo C + + c++ /CLI código-fonte CX, usar o hat (\^) variáveis para fazer referência a objetos de tempo de execução do Windows e a seta (-&gt;) operador a referência a uma variável de hat.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Ao portar para C + equivalente c++ /CLI código do WinRT, você pode obter um longo caminho removendo os chapéus e alterar o operador de seta (-&gt;) para o operador ponto (.). C + + c++ /CLI WinRT projetado tipos são valores e não ponteiros.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

O construtor padrão para C + c++ /CLI ponteiro do CX hat inicializa-o como nulo. Aqui está um C + + c++ /CLI que criamos um variável/campo do tipo correto, mas que não tem de inicializada de exemplo de código CX. Em outras palavras, ele não inicialmente se referir a um **TextBlock**; temos a intenção de atribuir uma referência mais tarde.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Para o equivalente no C + + c++ /CLI WinRT, consulte [Delayed inicialização](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Propriedades
A extensões de linguagem C++/CX incluem o conceito de propriedades. Ao criar o código-fonte do C++/CX, você pode acessar uma propriedade como se fosse um campo. O C++ padrão não tem o conceito de propriedade, portanto, no C++/WinRT, você pode obter e definir funções.

Nos exemplos a seguir, **XboxUserId**, **UserState**, **PresenceDeviceRecords** e **Size** são propriedades.

### <a name="retrieving-a-value-from-a-property"></a>Recuperar um valor a partir de uma propriedade
Veja como obter um valor de propriedade no C++/CX.

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

O código-fone equivalente do C++/WinRT chama uma função com o mesmo nome da propriedade, mas sem os parâmetros.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Observe que a função **PresenceDeviceRecords** retorna um objeto do Windows Runtime com uma função **Size**. Como o objeto retornado também é um tipo projetado do C++/WinRT, desreferenciamos usando o operador ponto para chamar **Size**.

### <a name="setting-a-property-to-a-new-value"></a>Definir uma propriedade como um novo valor
Uma propriedade é definida com um novo valor de forma semelhante. Primeiro, no C++/CX.

```cppcx
record->UserState = newValue;
```

Para fazer o equivalente no C++/WinRT, você chama uma função com o mesmo nome da propriedade e passa um argumento.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Criar uma instância de uma classe
Você trabalha com C + c++ /CLI objeto CX por meio de um indicador para ele, normalmente conhecido como um chapéu (\^) referência. Você pode criar um novo objeto por meio da palavra-chave `ref new`, que chama [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) para ativar uma nova instância da classe do tempo de execução.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Um objeto do C++/WinRT é um valor; portanto, você pode alocá-lo na pilha ou como um campo de um objeto. Você *nunca* deve usar `ref new` (nem `new`) para alocar um objeto do C++/WinRT. Nos bastidores, **RoActivateInstance** ainda é chamado.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Se um recurso foi dispendioso para inicializar, é comum atrasar a inicialização até ser necessária.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

O mesmo código é transferido para o C++/WinRT. Observe o uso do construtor `nullptr`. Para saber mais sobre esse construtor, veja [Consumir APIs com o C++/WinRT](consume-apis.md).

```cppwinrt
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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Convertendo de uma classe base de tempo de execução para uma derivada
É comum ter uma referência para a base que você sabe que se refere a um objeto de um tipo derivado. No C + + c++ /CLI CX, use `dynamic_cast` à *cast* a referência para a base para uma referência para derivado. O `dynamic_cast` é realmente apenas uma chamada oculta para [ **QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Aqui está um exemplo típico&mdash;está manipulando um evento de alteração de propriedade de dependência, e você deseja converter de **DependencyObject** volta para o tipo real que possui a propriedade de dependência.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

O equivalente C + c++ /CLI WinRT código substitui o `dynamic_cast` com uma chamada para o [ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) função, que encapsula **QueryInterface**. Você também tem a opção de chamar [ **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), em vez disso, que gera uma exceção se a consulta para a interface necessária (a interface padrão do tipo que está solicitando) não é retornado. Aqui está um C + + c++ /CLI exemplo de código do WinRT.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>Processamento de eventos com um delegado
Veja um exemplo típico de processamento de eventos no C++/CX usando uma função lambda como um delegado nesse caso.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Isso é o equivalente no C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Em vez de uma função lambda, você pode optar por implementar seu delegado como uma função livre como uma função de ponteiro para membro. Para obter mais informações, consulte [Processar eventos usando delegados no C++/WinRT](handle-events.md).

Se estiver transferindo de uma base de código C++/CX em que os eventos e delegados são usados internamente (e não em binários), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) ajudará você a replicar esse padrão no C++/WinRT. Consulte também [parametrizadas delegados, sinais simples e retornos de chamada dentro de um projeto](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Revogar um delegado
No C++/CX, você usa o operador `-=` para revogar um registro de evento anterior.

```cppcx
myButton->Click -= token;
```

Isso é o equivalente no C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Para obter mais informações e opções, consulte [Revogar um delegado registrado](handle-events.md#revoke-a-registered-delegate).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Mapear os tipos **Platform** do C++/CX para tipos do C++/WinRT
O C++/CX fornece vários tipos de dados no namespace **Platform**. Esses tipos não são do C++ padrão, portanto, podem ser usados somente ao habilitar as extensões de linguagem do Windows Runtime (propriedade do projeto do Visual Studio **C/C++** > **Geral** > **Consumir extensão do Windows Runtime** > **Sim (/ZW)**). A tabela abaixo ajuda você a fazer a compatibilização dos tipos **Platform** para seus equivalente no C++/WinRT. Depois de fazer isso, como o C++/WinRT é o C++ padrão, você pode desativar a opção `/ZW`.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform:: Agile\^** | [**WinRT::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform:: array\^** | Ver [porta **Platform:: array\^**](#port-platformarray) |
| **Platform:: Exception\^** | [**WinRT::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform:: invalidargumentexception\^** | [**WinRT::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform:: Object\^** | **WinRT::Windows::Foundation::IInspectable** |
| **Platform:: String\^** | [**WinRT::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Porta **Platform:: Agile\^**  para **winrt::agile_ref**
O **Platform:: Agile\^**  tipo no C + + c++ /CX representa uma classe de tempo de execução do Windows que pode ser acessada de qualquer thread. O C + + c++ /CLI é equivalente do WinRT [ **winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

No C++/CX.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

No C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Porta **Platform:: array\^**
As opções incluem o uso de uma lista de inicializadores, uma **std:: array**, ou uma **std:: Vector**. Para obter mais informações e exemplos de código, consulte [listas de inicializadores padrão](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) e [Standard matrizes e vetores de](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresulterror"></a>Porta **Platform:: Exception\^**  para **winrt::hresult_error**
O **Platform:: Exception\^**  tipo é gerado no C + + c++ /CLI CX quando uma API de tempo de execução do Windows retorna um S não\_Okey HRESULT. O equivalente para C++/WinRT é [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

A porta para C + + c++ /CLI WinRT, altere qualquer código que usa **Platform:: Exception\^**  usar **winrt::hresult_error**.

No C++/CX.

```cppcx
catch (Platform::Exception^ ex)
```

No C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

O C++/WinRT fornece essas classes de exceção.

| Tipo de exceção | Classe base | HRESULT |
| ---- | ---- | ---- |
| [**WinRT::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | chamar [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
| [**WinRT::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **WinRT::hresult_error** | E_ACCESSDENIED |
| [**WinRT::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **WinRT::hresult_error** | ERROR_CANCELLED |
| [**WinRT::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **WinRT::hresult_error** | E_CHANGED_STATE |
| [**WinRT::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **WinRT::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**WinRT::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **WinRT::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**WinRT::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **WinRT::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**WinRT::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **WinRT::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**WinRT::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **WinRT::hresult_error** | E_INVALIDARG |
| [**WinRT::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **WinRT::hresult_error** | E_NOINTERFACE |
| [**WinRT::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **WinRT::hresult_error** | E_NOTIMPL |
| [**WinRT::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **WinRT::hresult_error** | E_BOUNDS |
| [**WinRT::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **WinRT::hresult_error** | RPC_E_WRONG_THREAD |

Observe que cada classe (pela classe base **hresult_error**) fornece uma função [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function), que retorna o HRESULT do erro e uma função [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function), que retorna a representação da cadeia de caracteres desse HRESULT.

Veja um exemplo de geração de uma exceção no C++/CX.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

E o equivalente em C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Porta **Platform:: Object\^**  para **winrt::Windows::Foundation::IInspectable**
Como todos os tipos do C++/WinRT, **winrt::Windows::Foundation::IInspectable** é um tipo de valor. Veja como inicializar uma variável desse tipo como null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Porta **Platform:: string\^**  para **winrt::hstring**
**Platform:: string\^**  é equivalente ao tipo de ABI de HSTRING de tempo de execução do Windows. Para o C++/WinRT, o equivalente é [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Mas com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de cadeia de caracteres longas da Biblioteca Padrão do C++, como **std::wstring** e/ou literais de cadeias de caracteres longas. Para obter mais detalhes e exemplos de código, consulte [Processamento de cadeia de caracteres no C++/WinRT](strings.md).

Com C + + c++ /CLI CX, você pode acessar o [ **Platform::String::Data** ](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) propriedade para recuperar a cadeia de caracteres como um estilo C **constante_t\***  matriz (por exemplo, para passar ele **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Para fazer o mesmo com C++/WinRT, você pode usar a função [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) para obter uma versão de cadeia de caracteres C-style terminada em null, assim como de **std::wstring**.

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

Quando se trata de implementação de APIs que aceitam ou retornam cadeias de caracteres, normalmente, você altera qualquer C + + c++ /CLI código c++ /CX que usa **Platform:: string\^**  usar **winrt::hstring** em vez disso.

Veja um exemplo de uma API do C++/CX API que recebe a cadeia de caracteres.

```cppcx
void LogWrapLine(Platform::String^ str);
```

Para o C++/WinRT, você pode declarar a API no [MIDL 3.0](/uwp/midl-3) dessa forma.

```idl
// LogType.idl
void LogWrapLine(String str);
```

A cadeia de ferramentas do C++/WinRT gera o código-fonte semelhante a isso.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString)

C + + c++ /CLI CX fornece o [Object:: ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) método.

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C + + c++ /CLI WinRT diretamente não oferece esse recurso, mas você pode desativar a alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>APIs Importantes
* [WinRT::delegate struct modelo](/uwp/cpp-ref-for-winrt/delegate)
* [struct WinRT::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [struct WinRT::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Criar eventos em C + + c++ /CLI WinRT](author-events.md)
* [Simultaneidade e operações assíncronas com C + + c++ /CLI WinRT](concurrency.md)
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Manipular eventos usando delegados em C + + c++ /CLI WinRT](handle-events.md)
* [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md)
* [Referência de Microsoft Interface Definition Language 3.0](/uwp/midl-3)
* [Mudar do WRL para o C++/WinRT](move-to-winrt-from-wrl.md)
* [Cadeia de caracteres de tratamento em C + + c++ /CLI WinRT](strings.md)
