---
author: stevewhims
description: Este tópico mostra como fazer a portabilidade do código C++/CX para seu equivalente no C++/WinRT.
title: Mudar do C++/CX para C++/WinRT
ms.author: stwhi
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: da6226158056cbbf0b51b46be0b17fe7e478dd01
ms.sourcegitcommit: 929fa4b3273862dcdc76b083bf6c3b2c872dd590
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1935744"
---
# <a name="move-to-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-from-ccx"></a>Mover do C++/CX para [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Este tópico mostra como transferir o código do [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) para seu equivalente no C++/WinRT.

> [!NOTE]
> O [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e o SDK do Windows declaram tipos no namespace raiz **Windows**. Um tipo do Windows projetado no C++/WinRT tem o mesmo nome totalmente qualificado do tipo do Windows, mas ele é colocado no namespace C++ **winrt**. Esses namespaces distintos permitem que você faça a transferência do C++/CX para o C++/WinRT em seu próprio ritmo.

A primeira etapa da transferência para o C++/WinRT é adicionar automaticamente o suporte para C++/WinRT ao seu projeto (consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). Para fazer isso, edite o arquivo `.vcxproj`, encontre `<PropertyGroup Label="Globals">` e, no grupo de propriedades, defina a propriedade `<CppWinRTEnabled>true</CppWinRTEnabled>`. Um efeito dessa alteração é que o suporte para C++/CX é desativado no projeto. É recomendado deixar o suporte desativado para que você possa encontrar e transferir todas as dependência no C++/CX ou você pode ativar o suporte (nas propriedades do projeto, **C/C++** \> **Geral** \> **Consumir a extensão do Windows Runtime** \> **Sim (/ZW)**), e transferir gradualmente.

Defina a propriedade do projeto **Geral** \> **Versão da plataforma de destino** como 10.0.17134.0 (Windows 10, versão 1803) ou posterior.

No arquivo de cabeçalho pré-compilado (em geral, `pch.h`), inclua `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Se você incluir qualquer cabeçalho de API C++/WinRT projetado do Windows (por exemplo, `winrt/Windows.Foundation.h`), não é necessário incluir explicitamente `winrt/base.h` dessa forma, pois será incluído automaticamente para você.

Caso o projeto também use os tipos da [Biblioteca de Modelos C++ do Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl), consulte [Mover para C++/WinRT a partir da WRL](move-to-winrt-from-wrl.md).

## <a name="parameter-passing"></a>Passagem de parâmetro
Ao criar o código-fonte do C++/CX, você passa os tipos C++/CX como parâmetros de função e referência de circunflexo (\^).

```cpp
void LogPresenceRecord(PresenceRecord^ record);
```

No C++/WinRT, para funções síncronas, você deve usar parâmetros `const&` por padrão. Isso evita cópia e sobrecarga encaixada. Entretanto, as corrotinas deve usar passar por valor para garantir a captura pelo valor e evitar problemas de tempo de vida (para ver mais detalhes, consulte [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Um objeto do C++/WinRT é fundamentalmente um valor que mantém um ponteiro da interface para o objeto do Windows Runtime subjacente. Quando você copia um objeto do C++/WinRT, o compilador copia o ponteiro da interface encapsulado, aumentando sua contagem de referência. A destruição eventual da cópia envolve reduzir a contagem de referência. Portanto, apenas incorre na sobrecarga de uma cópia quando necessário.

## <a name="variable-and-field-references"></a>Referências de variáveis e campo
Ao criar o código-fonte do C++/CX, você usa as variáveis de circunflexo (\^) para fazer referência a objetos do Windows Runtime e o operador de seta (-&gt;) para desreferenciar uma variável desse tipo.

```cpp
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Quando estiver convertendo para o código equivalente do C++/WinRT, você remove os circunflexos e altera a operador de seta (-&gt;) par ao operador de ponto (.), pois os tipos projetados do C++/WinRT são valores e não ponteiros.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

## <a name="properties"></a>Propriedades
A extensões de linguagem C++/CX incluem o conceito de propriedades. Ao criar o código-fonte do C++/CX, você pode acessar uma propriedade como se fosse um campo. O C++ padrão não tem o conceito de propriedade, portanto, no C++/WinRT, você pode obter e definir funções.

Nos exemplos a seguir, **XboxUserId**, **UserState**, **PresenceDeviceRecords** e **Size** são propriedades.

### <a name="retrieving-a-value-from-a-property"></a>Recuperar um valor a partir de uma propriedade
Veja como obter um valor de propriedade no C++/CX.

```cpp
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

```cpp
record->UserState = newValue;
```

Para fazer o equivalente no C++/WinRT, você chama uma função com o mesmo nome da propriedade e passa um argumento.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Criar uma instância de uma classe
Você trabalha com um objeto do C++/CX por meio de um identificador, conhecido como uma referência de circunflexo (\^). Você pode criar um novo objeto por meio da palavra-chave `ref new`, que chama [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) para ativar uma nova instância da classe do tempo de execução.

```cpp
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

```cpp
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

## <a name="event-handling-with-a-delegate"></a>Processamento de eventos com um delegado
Veja um exemplo típico de processamento de eventos no C++/CX usando uma função lambda como um delegado nesse caso.

```cpp
auto token = myButton->Click += ref new RoutedEventHandler([&](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
});
```

Isso é o equivalente no C++/WinRT.

```cppwinrt
auto token = myButton().Click([&](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
});
```

Em vez de uma função lambda, você pode optar por implementar seu delegado como uma função livre como uma função de ponteiro para membro. Para obter mais informações, consulte [Processar eventos usando delegados no C++/WinRT](handle-events.md).

Se estiver transferindo de uma base de código C++/CX em que os eventos e delegados são usados internamente (e não em binários), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) ajudará você a replicar esse padrão no C++/WinRT. Consulte também [winrt::delegate&lt;... T&gt;](author-events.md#winrtdelegate-t).

## <a name="revoking-a-delegate"></a>Revogar um delegado
No C++/CX, você usa o operador `-=` para revogar um registro de evento anterior.

```cpp
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
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Faça a compatibilização de **Platform::Object\^** para **winrt::Windows::Foundation::IInspectable**
Como todos os tipos do C++/WinRT, **winrt::Windows::Foundation::IInspectable** é um tipo de valor. Veja como inicializar uma variável desse tipo como null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Faça a compatibilização de **Platform::String\^** para **winrt::hstring**
**Platform::String\^** é equivalente ao tipo ABI de HSTRING do Windows Runtime. Para o C++/WinRT, o equivalente é [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Mas com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de cadeia de caracteres longas da Biblioteca Padrão do C++, como **std::wstring** e/ou literais de cadeias de caracteres longas. Para obter mais detalhes e exemplos de código, consulte [Processamento de cadeia de caracteres no C++/WinRT](strings.md).

Com o C++/CX, você pode acessar a propriedade [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) para recuperar a cadeia de caracteres como uma matriz **const wchar_t\*** C-style (por exemplo, passar para **std::wcout**).

```C++
auto var = titleRecord->TitleName->Data();
```

Para fazer o mesmo com C++/WinRT, você pode usar a função [**hstring::c_str**](/uwp/api/windows.foundation.uri#hstringcstr-function) para obter uma versão de cadeia de caracteres C-style terminada em null, assim como de **std::wstring**.

```C++
auto var = titleRecord.TitleName().c_str();
```

Quando se trata de implementar APIs que recebem ou retornam cadeias de caracteres, normalmente você altera qualquer código do C++/CX que usa **Platform::String\^** para em vez disso usar **winrt::hstring**.

Veja um exemplo de uma API do C++/CX API que recebe a cadeia de caracteres.

```cpp
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

### <a name="port-platformexception-to-winrthresulterror"></a>Faça a transferência de **Platform::Exception\^** para **winrt::hresult_error**
O tipo **Platform::Exception\^** é produzido no C++/CX quando uma API do Windows Runtime retorna um HRESULT diferente de S\_OK. O equivalente para C++/WinRT é [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Para fazer a portabilidade para o C++/WinRT, altere todos os códigos que usam **Platform::Exception\^** e use **winrt::hresult_error**.

No C++/CX.

```cpp
catch (Platform::Exception^ ex)
```

No C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

O C++/WinRT fornece essas classes de exceção.

| Tipo de exceção | Classe base | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | chamar [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

Observe que cada classe (pela classe base **hresult_error**) fornece uma função [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function), que retorna o HRESULT do erro e uma função [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function), que retorna a representação da cadeia de caracteres desse HRESULT.

Veja um exemplo de geração de uma exceção no C++/CX.

```cpp
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

E o equivalente em C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

## <a name="important-apis"></a>APIs Importantes
* [Modelo de struct winrt::delegate](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Criar eventos com C++/WinRT](author-events.md)
* [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md)
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Processar eventos usando delegados em C++/WinRT](handle-events.md)
* [Referência de linguagem IDL da Microsoft 3.0](/uwp/midl-3)
* [Mudar do WRL para C++/WinRT](move-to-winrt-from-wrl.md)
* [Processamento da cadeia de caracteres em C++/WinRT](strings.md)