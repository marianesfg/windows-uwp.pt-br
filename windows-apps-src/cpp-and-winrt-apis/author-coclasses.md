---
description: C++/WinRT pode ajudá-lo a criar componentes COM clássicos, exatamente como ajuda a criar classes no Windows Runtime.
title: Criar componentes COM com C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, criar, COM, componente
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5ff3677c3624974759d1f6ff21d6e53cf9d33144
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "71344513"
---
# <a name="author-com-components-with-cwinrt"></a>Criar componentes COM com C++/WinRT

O [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) pode ajudá-lo a criar componentes COM (Component Object Model) clássicos (ou coclasses), exatamente como ajuda a criar classes no Windows Runtime. Este tópico mostra como fazer isso.

## <a name="how-cwinrt-behaves-by-default-with-respect-to-com-interfaces"></a>Como C++/WinRT se comporta, por padrão, em relação a interfaces COM

O modelo [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) de C++/WinRT é a base da qual suas classes de runtime e as fábricas de ativação derivam direta ou indiretamente.

Por padrão, **winrt::implements** dá suporte apenas a interfaces baseadas em [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) e ignora silenciosamente interfaces COM clássicas. Qualquer chamada de **QueryInterface** (QI) para interfaces COM clássicas, consequentemente, falharão com **E_NOINTERFACE**.

Em alguns instantes, você verá como resolver essa situação, mas primeiro vamos ver um exemplo de código para ilustrar o que acontece por padrão.

```idl
// Sample.idl
runtimeclass Sample
{
    Sample();
    void DoWork();
}

// Sample.h
#include "pch.h"
#include <shobjidl.h> // Needed only for this file.

namespace winrt::MyProject
{
    struct Sample : implements<Sample, IInitializeWithWindow>
    {
        IFACEMETHOD(Initialize)(HWND hwnd);
        void DoWork();
    }
}
```

E este é o código do cliente para consumir a classe **Sample**.

```cppwinrt
// Client.cpp
Sample sample; // Construct a Sample object via its projection.

// This next line crashes, because the QI for IInitializeWithWindow fails.
sample.as<IInitializeWithWindow>()->Initialize(hwnd); 
```

A boa notícia é que, para fazer com que **winrt::implements** dê suporte a interfaces COM clássicas, basta apenas incluir `unknwn.h` antes de incluir qualquer cabeçalho de C++/WinRT.

Você pode fazer isso explicitamente ou indiretamente incluindo alguns outros arquivos de cabeçalho, como `ole2.h`. Um método recomendado é incluir o arquivo de cabeçalho `wil\cppwinrt.h`, que faz parte das [Bibliotecas de Implementação do Windows (WIL)](https://github.com/Microsoft/wil). O arquivo de cabeçalho `wil\cppwinrt.h` não apenas garante que `unknwn.h` seja incluído antes de `winrt/base.h`, ele também configura tudo de forma que C++/WinRT e WIL entendem as exceções e os códigos de erro um do outro.

## <a name="a-simple-example-of-a-com-component"></a>Um exemplo simples de um componente COM

Este é um exemplo simples de um componente COM escrito usando C++/WinRT. Esta é uma lista completa de um miniaplicativo, de modo que você pode experimentar o código colando-o em `pch.h` e em `main.cpp` de um novo projeto do **Aplicativo de Console do Windows (C++/WinRT)** .

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

Confira também [Consumir componentes COM com C++/WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Um exemplo mais realista e interessante

O restante deste tópico orienta sobre a criação de um projeto de aplicativo de console mínimo que usa C++/WinRT para implementar uma coclasse básica (componente COM ou classe COM) e a fábrica de classes. O aplicativo de exemplo mostra como fornecer uma notificação do sistema com um botão de retorno de chamada e a coclasse (que implementa a interface COM **INotificationActivationCallback**) permite ao aplicativo iniciar e retornar a chamada quando o usuário clica nesse botão na notificação do sistema.

Para saber mais sobre a área do recurso de notificação do sistema, acesse [Enviar uma notificação do sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Nenhum dos exemplos de código nesta seção da documentação usa o C++/WinRT, então recomendamos dar preferência ao código mostrado neste tópico.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Criar um projeto de Aplicativo de Console do Windows (ToastAndCallback)

Comece criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Aplicativo de Console do Windows (C++/WinRT)** chamado *ToastAndCallback*.

Abra `pch.h` e adicione `#include <unknwn.h>` antes de incluir qualquer cabeçalho C++/WinRT. Este é o resultado; é possível substituir o conteúdo do `pch.h` pela listagem.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Abra `main.cpp` e remova as diretivas de uso geradas pelo modelo do projeto. No lugar delas, insira o código a seguir (o que nos dá as bibliotecas, os cabeçalhos e os nomes de tipo que precisamos). Este é o resultado; é possível substituir o conteúdo de `main.cpp` por esta listagem (também removemos o código de `main` da listagem abaixo, porque substituiremos essa função posteriormente).

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;

int main() { }
```

O projeto ainda não será compilado; depois de terminar de adicionar o código, você será solicitado a compilar e executar.

## <a name="implement-the-coclass-and-class-factory"></a>Implementar a fábrica de classe e de coclasse

No C++/WinRT, implementa-se fábricas de classe e de coclasse, derivando do struct básico [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). Imediatamente após as três diretivas de uso mostradas acima (e antes de `main`), cole este código para implementar seu componente de ativador COM para a notificação do sistema.

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

A implementação da coclasse acima segue o mesmo padrão demonstrado em [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Portanto, você pode usar a mesma técnica para implementar interfaces COM e interfaces do Windows Runtime. Os componentes COM e as classes do Windows Runtime expõem seus recursos por meio de interfaces. Cada interface COM deriva da [**interface IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown). O Windows Runtime é baseado em COM, uma diferença é que as interfaces do Windows Runtime derivam da [**interface IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (e **IInspectable** deriva de **IUnknown**).

Na coclasse do código acima, implementamos o método **INotificationActivationCallback::Activate**, que é a função chamada quando o usuário clica no botão de retorno de chamada em uma notificação do sistema. Porém, antes que essa função possa ser chamada, é preciso criar uma instância da coclasse e esse é o trabalho da função **IClassFactory::CreateInstance**.

A coclasse que implementamos apenas é conhecida como o *ativador COM* para notificações e tem uma ID de classe (CLSID) na forma do identificador `callback_guid` (do tipo **GUID**), que pode ser vista acima. Usaremos esse identificador mais tarde, na forma de um atalho do menu Iniciar e uma entrada do Registro do Windows. O ativador COM CLSID e o caminho para seu servidor COM associado (que é o caminho para o arquivo executável que estamos criando aqui) é o mecanismo pelo qual uma notificação do sistema sabe qual classe usar para criar uma instância quando o botão de retorno de chamada é clicado (independentemente de o clique ocorrer na Central de Ações ou não).

## <a name="best-practices-for-implementing-com-methods"></a>Práticas recomendadas para implementação de métodos COM

As técnicas para o tratamento de erros e para o gerenciamento de recursos podem andar de mãos dadas. É mais conveniente e prático usar exceções que códigos de erro. E se você implementar a linguagem resource-acquisition-is-initialization (RAII), será possível evitar a verificação explícita de códigos de erro e o lançamento explícito de recursos. Essas verificações explícitas tornam seu código mais complicado que o necessário, e fornecem muitos lugares para os bugs se esconderem. Em vez disso, use RAII e exceções throw/catch. Dessa forma, as suas alocações de recursos se tornam à prova de exceções e seu código fica simples.

No entanto, você não deve permitir que as exceções escapem das suas implementações de método COM. Você pode garantir o uso do especificador `noexcept` em seus métodos de COM. É possível lançar as exceções em qualquer lugar no gráfico de chamadas do método, desde que você os trate antes de sair do seu método. Se você usar `noexcept`, mas depois permitir que uma exceção escape do seu método, o aplicativo será encerrado.

## <a name="add-helper-types-and-functions"></a>Adicionar os tipos e funções auxiliares

Nesta etapa, vamos adicionar algumas funções e tipos auxiliares usados pelo restante do código. Portanto, imediatamente antes de `main`, adicione o seguinte.

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implementar as funções restantes e a função de ponto de entrada wmain

Exclua a função `main` e, no lugar dela, cole esta listagem de códigos, que inclui um código para registrar sua coclasse e, em seguida, fornecer uma notificação de sistema capaz de retornar chamadas para seu aplicativo.

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
    ::Sleep(50); // Give the callback chance to display.
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    winrt::init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>Como testar o aplicativo de exemplo

Compile o aplicativo e execute-o pelo menos uma vez como um administrador para fazer com que o código de registro e de outras configurações sejam executados. Uma maneira de fazer isso é executar o Visual Studio como administrador e executar o aplicativo do Visual Studio. Clique com o botão direito do mouse no Visual Studio na barra de tarefas para exibir a lista de atalhos, clique com o botão direito do mouse no Visual Studio na lista de atalhos e clique em **Executar como administrador**. Concorde com o prompt e abra o projeto. Ao executar o aplicativo, é exibida uma mensagem indicando se o aplicativo está sendo executado como administrador. Caso não esteja, o registro e outras configurações não serão executados. Esse registro e outras configurações devem ser executados pelo menos uma vez para que o aplicativo funcione corretamente.

Se você não estiver executando o aplicativo como administrador, pressione "T" para fazer com que uma notificação do sistema seja exibida. Clique no botão **Retornar chamada de ToastAndCallback** diretamente na notificação do sistema exibida ou na Central de Ações e o aplicativo será iniciado, a coclasse será instanciada e o método **INotificationActivationCallback::Activate** será executado.

## <a name="in-process-com-server"></a>Servidor COM em processamento

O aplicativo de exemplo *ToastAndCallback* acima funciona como um servidor COM local (ou fora do processo). Isso é indicado pela chave [LocalServer32](/windows/desktop/com/localserver32) do Registro do Windows , usada para registrar a CLSID da sua coclasse. Um servidor COM local hospeda sua(s) coclasse(s) em um binário executável (um `.exe`).

Como alternativa (e o que é mais provável), opte por hospedar sua(s) coclasse(s) em uma biblioteca de vínculo dinâmico (uma `.dll`). Um servidor COM na forma de uma DLL é conhecido como um servidor COM em processamento e é indicado pelas CLSIDs que estão sendo registradas ao usar a chave [InprocServer32](/windows/desktop/com/inprocserver32) do Registro do Windows.

### <a name="create-a-dynamic-link-library-dll-project"></a>Criar um projeto de biblioteca de vínculo dinâmico (DLL)

Você pode começar a tarefa de criação de um servidor COM em processamento criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Visual C++**  > **Área de Trabalho do Windows** > **Biblioteca de Vínculo Dinâmico (DLL)** .

Para adicionar o suporte a C++/WinRT para o novo projeto, siga as etapas descritas em [Modificar um projeto de aplicativo de Área de Trabalho do Windows para adicionar suporte ao C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implementar a coclasse, a fábrica de classes e as exportações de servidor em processamento

Abra `dllmain.cpp` e adicione a ele a listagem de códigos exibida abaixo.

Se você já tem uma DLL que implementa as classes do Windows Runtime para C++/WinRT, já tem a função **DllCanUnloadNow** mostrada abaixo. Se quiser adicionar coclasses à DLL, use a função **DllGetClassObject**.

Se você não tem um código da [Biblioteca de Modelos C++ do Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) com o qual deseja permanecer compatível, remova a WRL do código mostrado.

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            return winrt::make<MyCoclass>()->QueryInterface(riid, ppvObject);
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

### <a name="support-for-weak-references"></a>Suporte para referências fracas

Confira também [Referências fracas em C++/WinRT](weak-references.md#weak-references-in-cwinrt).

O C++/ WinRT (especificamente, o modelo de struct básico [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)) implementa [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) se seu tipo implementa [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (ou qualquer interface derivada de **IInspectable**).

Por esse motivo **IWeakReferenceSource** e [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) são projetados para tipos do Windows Runtime. Dessa forma, é possível ativar o suporte de referência fraca para sua coclasse simplesmente adicionando **winrt::Windows::Foundation::IInspectable** (ou uma interface derivada de **IInspectable**) à sua implementação.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>APIs importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interface IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [Modelo de struct winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Consumir componentes COM com C++/WinRT](consume-com.md)
* [Enviar uma notificação do sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
