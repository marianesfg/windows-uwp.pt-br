---
author: stevewhims
description: Este tópico mostra como fazer a portabilidade do código C++/CX para seu equivalente no C++/WinRT.
title: Mudar do C++/CX para C++/WinRT
ms.author: stwhi
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 35fe84747624c9a855df5520322546b83772379b
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5943767"
---
# <a name="move-to-cwinrt-from-ccx"></a>Mudar de C++/CX para C++/WinRT

Este tópico mostra como a portabilidade do código em um [C++ c++ /CX](/cpp/cppcx/visual-c-language-reference-c-cx) projeto para seu equivalente no [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Estratégias de portabilidade

Se você quiser fazer a portabilidade gradualmente C++ c++ código CX para C++ c++ WinRT, é possível. C++ c++ /CX e C++ c++ WinRT código pode coexistir no mesmo projeto, com as exceções de suporte ao compilador XAML e componentes de tempo de execução do Windows. Para essas duas exceções, será necessário direcionar o C + c++ /CX ou C++ c++ WinRT dentro do mesmo projeto.

> [!IMPORTANT]
> Se seu projeto é compilado um aplicativo XAML, em seguida, um fluxo de trabalho que é recomendável é primeiro criar um novo projeto no Visual Studio usando um dos C++ c++ modelos de projeto do WinRT (consulte [suporte do Visual Studio para C++ c++ /WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). Em seguida, inicie a cópia de código-fonte e marcação ao longo do C++ c++ projeto CX. Você pode adicionar novas páginas XAML com **projeto** \> **Adicionar Novo Item...**  \>  **Visual C++** > **página em branco (C++ c++ WinRT)**.
>
> Como alternativa, você pode usar um componente do tempo de execução do Windows para código de fator fora do XAML C + c++ /CX projeto conforme você portá-lo. Mova máximo C + c++ CX como você pode em um componente e, em seguida, altere o projeto XAML para C++ de código c++ WinRT. Ou else deixar o projeto XAML como C++ c++ CX, crie um novo C + c++ componente WinRT e começar a compatibilizar C++ c++ código CX fora do projeto XAML e do componente. Você também poderia ter C++ c++ projeto de componente CX junto com C++ c++ projeto de componente WinRT dentro da mesma solução, faça referência ambos do seu projeto de aplicativo e gradualmente portabilidade de um para o outro. Consulte [interoperabilidade entre C++ c++ /WinRT e C++ c++ /CX](interop-winrt-cx.md) para obter mais detalhes sobre como usar as duas projeções de linguagem no mesmo projeto.

> [!NOTE]
> O [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) e o SDK do Windows declaram tipos no namespace raiz **Windows**. Um tipo do Windows projetado no C++/WinRT tem o mesmo nome totalmente qualificado do tipo do Windows, mas ele é colocado no namespace C++ **winrt**. Esses namespaces distintos permitem que você faça a transferência do C++/CX para o C++/WinRT em seu próprio ritmo.

Que ostentam em mente as exceções mencionadas acima, a primeira etapa da transferência C++ c++ projeto CX para C++ c++ WinRT é adicionar manualmente C++ c++ WinRT suporte a ele (consulte [suporte do Visual Studio para C++ c++ /WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). Para fazer isso, edite o arquivo `.vcxproj`, encontre `<PropertyGroup Label="Globals">` e, no grupo de propriedades, defina a propriedade `<CppWinRTEnabled>true</CppWinRTEnabled>`. Um efeito dessa alteração é que o suporte para C++/CX é desativado no projeto. É uma boa ideia para deixar o suporte desativado para que as mensagens de compilação ajudam você a localização (e porta) todas as dependência no C++ c++ /CX ou você pode ativar o suporte (nas propriedades do projeto, **C/C++** \> **Geral** \> **Consume Windows Runtime Extensão** \> **Sim (/ZW)**) e transferir gradualmente.

Verifique se essa propriedade de projeto **Geral** \> **Versão da plataforma de destino** é definido como 10.0.17134.0 (Windows 10, versão 1803) ou superior.

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

Ao fazer a portabilidade para o C++ equivalente c++ código WinRT, basicamente remover os circunflexos e altera o operador de seta (-&gt;) para o operador ponto (.), porque C++ c++ WinRT projetado tipos são valores e não ponteiros.

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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Conversão de uma classe de base de tempo de execução para um derivadas
É comum ter uma referência para a base que você sabe que se refere a um objeto de um tipo derivado. No C++ c++ /CX, use `dynamic_cast` à *conversão* a referência para a base para uma referência-para-derivada. O `dynamic_cast` é realmente apenas uma chamada oculta para [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Aqui está um exemplo típico&mdash;você está manipulando um evento de alteração de propriedade de dependência, e você deseja reconverter de **DependencyObject** o tipo real que possui a propriedade de dependência.

```cpp
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

O equivalente C + c++ WinRT código substitui o `dynamic_cast` com uma chamada para a função [**IUnknown:: Try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) , que encapsula **QueryInterface**. Você também tem a opção de chamar [**IUnknown:: as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), em vez disso, que gera uma exceção se estiver consultando a interface necessária (a interface padrão do tipo que você está solicitando) não é retornado. Aqui está um C + c++ exemplo de código do WinRT.

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

Se estiver transferindo de uma base de código C++/CX em que os eventos e delegados são usados internamente (e não em binários), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) ajudará você a replicar esse padrão no C++/WinRT. Consulte também [Parameterized delegados, sinais simples e retornos de chamada dentro de um projeto](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

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
| **Plataforma:: Agile\ ^** | [**WinRT:: agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Porta **Platform:: Agile\ ^** para **WinRT:: agile_ref**
O **Platform:: Agile\ ^** tipo em C++ c++ /CX representa uma classe de tempo de execução do Windows que pode ser acessada de qualquer thread. C++ c++ WinRT equivalente é [**WinRT:: agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

No C++/CX.

```cpp
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

No C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
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
* [Manejar eventos usando delegados em C++/WinRT](handle-events.md)
* [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md)
* [Referência de linguagem IDL da Microsoft 3.0](/uwp/midl-3)
* [Mudar do WRL para C++/WinRT](move-to-winrt-from-wrl.md)
* [Processamento da cadeia de caracteres em C++/WinRT](strings.md)
