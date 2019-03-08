---
description: C + + c++ /CLI WinRT pode ajudá-lo a criar componentes COM clássicos, exatamente como ele ajuda a criar classes de tempo de execução do Windows.
title: Criar componentes COM com C++/WinRT
ms.date: 09/06/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, autor, COM, o componente
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e6b77f8be6c75070336ad48f0c6471fc0a824a4c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616561"
---
# <a name="author-com-components-with-cwinrt"></a>Criar componentes COM com C++/WinRT

[C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) pode ajudar você a criar clássica (COM Component Object Model) componentes (ou coclasses), exatamente como ele ajuda a criar classes de tempo de execução do Windows. Aqui está uma ilustração simple que você pode testar se você colar o código para o `pch.h` e `main.cpp` de uma nova **aplicativo de Console do Windows (C + + c++ /CLI WinRT)** projeto.

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

Consulte também [consumir componentes com C + + c++ /CLI WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Um exemplo mais realista e interessante

O restante deste tópico orienta a criação de um projeto de aplicativo de console mínimo que usa C + + c++ /CLI WinRT para implementar uma coclass básico (componente COM ou classe COM) e a fábrica de classes. O aplicativo de exemplo mostra como fornecer uma notificação do sistema com um botão de retorno de chamada e coclass (que implementa o **INotificationActivationCallback** interface COM) permite que o aplicativo ser iniciado e chamado de volta quando o usuário clica nesse botão na torrada.

Obter mais informações sobre a área de recurso de notificação do sistema podem ser encontradas em [enviar uma notificação do sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Nenhum dos exemplos de código nesta seção da documentação, usar c++ /CLI WinRT, no entanto, portanto, recomendamos que você prefere o código mostrado neste tópico.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Criar um projeto de aplicativo de Console do Windows (ToastAndCallback)

Comece criando um novo projeto no Microsoft Visual Studio. Criar uma **Visual C++** > **área de trabalho do Windows** > **aplicativo de Console do Windows (C + + c++ /CLI WinRT)** de projeto e nomeie-  *ToastAndCallback*.

Abra `pch.h`e adicione `#include <unknwn.h>` antes do inclui para qualquer C + + c++ /CLI cabeçalhos do WinRT.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Abra `main.cpp`e remova as usando diretivas que gera o modelo de projeto. Em seu lugar, cole o código a seguir (o que nos dá a bibliotecas, cabeçalhos e os nomes de tipo que precisamos).

```cppwinrt
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
```

## <a name="implement-the-coclass-and-class-factory"></a>Implementar a fábrica de classe coclass

No C + + c++ /CLI WinRT, você implementa coclasses e fábricas de classes, derivando de [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) struct de base. Imediatamente após as três usando diretivas mostradas acima (e antes de `main`), cole este código para implementar seu componente de ativador de notificação COM notificação do sistema.

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

A implementação da coclass acima segue o mesmo padrão que é demonstrado [APIs de autor com C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Portanto, você pode usar a mesma técnica para implementar interfaces COM, bem como interfaces de tempo de execução do Windows. Componentes COM e classes de tempo de execução do Windows expõe seus recursos por meio de interfaces. Cada interface COM, por fim, deriva de [ **interface IUnknown** ](https://msdn.microsoft.com/library/windows/desktop/ms680509) interface. O tempo de execução do Windows é baseado em COM&mdash;derivam de uma distinção, sendo que o tempo de execução do Windows interfaces, por fim, o [ **interface IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (e **IInspectable**  deriva **IUnknown**).

A coclass no código acima, implementamos o **INotificationActivationCallback::Activate** método, que é a função que é chamada quando o usuário clica no botão de retorno de chamada em uma notificação do sistema. Mas antes que essa função pode ser chamada, uma instância da coclass precisa ser criado e que é o trabalho do **IClassFactory** função.

A coclass que implementamos apenas é conhecida como o *ativador COM* para notificações e ele tem sua id de classe (CLSID) na forma dos `callback_guid` identificador (do tipo **GUID**) que você pode ver acima. Usaremos esse identificador mais tarde, na forma de um atalho no menu Iniciar e uma entrada de registro do Windows. O ativador COM CLSID e o caminho para seu servidor COM associado (que é o caminho para o executável que estamos criando aqui) é o mecanismo pelo qual uma notificação do sistema sabe o que a classe para criar uma instância de quando seu retorno de chamada botão é clicado (se a notificação é clicada na Central de ações ou não).

## <a name="best-practices-for-implementing-com-methods"></a>Práticas recomendadas para implementar os métodos COM

Técnicas para manipulação de erros e para gerenciamento de recursos podem ir em mãos. É mais conveniente e prático usar exceções que códigos de erro. E se você empregar o idioma do recurso-aquisição-for-inicialização (RAII), em seguida, você pode evitar verificar explicitamente os códigos de erro e, em seguida, liberando recursos explicitamente. Essas verificações explícitas tornam seu código mais complicado que o necessário, e ele fornece bugs muitos locais para ocultar. Em vez disso, use RAII e throw/catch exceções. Dessa forma, suas alocações de recursos são à prova de exceções, e seu código é simple.

No entanto, você não deve permitir exceções escapar suas implementações de método COM. Você pode garantir que, usando o `noexcept` especificador em seus métodos de COM. É okey para exceções seja lançada em qualquer lugar no grafo de chamadas do método, desde que tratá-las antes de sair de seu método. Se você usar `noexcept`, mas você, em seguida, permitir que uma exceção escapar o seu método, em seguida, o aplicativo será encerrado.

## <a name="add-helper-types-and-functions"></a>Adicionar funções e tipos auxiliares

Nesta etapa, vamos adicionar alguns tipos auxiliares e funções que faz o resto do código usam. Portanto, antes de `main`, adicione o seguinte.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implementar as funções restantes e a função de ponto de entrada de wmain

O modelo de projeto gera um `main` função para você. Excluir que `main` de função e, em seguida, em seu lugar, cole este código de listagem, que inclui código para registrar seu coclass e, em seguida, para entregar uma notificação do sistema capaz de chamar novamente o seu aplicativo.

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
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (Administrator)." : L".") << std::endl;

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

Compilar o aplicativo e, em seguida, executá-lo a pelo menos uma vez como administrador para fazer com que o registro e outra configuração, o código seja executado. Se você estiver executando como administrador, depois pressione ' t ' para fazer com que uma notificação do sistema a ser exibido. Você pode clicar na **ToastAndCallback de retorno de chamada** botão diretamente do pop-up notificação do sistema ou da Central de ações e seu aplicativo será iniciado, a coclass instanciada e o  **INotificationActivationCallback::Activate** método executado.

## <a name="in-process-com-server"></a>Servidor COM em processo

O *ToastAndCallback* aplicativo de exemplo acima funciona como um servidor COM local (ou out-of-process). Isso é indicado com o [LocalServer32](/windows/desktop/com/localserver32) chave do registro do Windows que você usa para registrar o CLSID do seu coclass. Um servidor COM local hospeda seus coclass(es) dentro de um executável binário (um `.exe`).

Como alternativa (e possivelmente mais provável), você pode optar por hospedar seus coclass(es) dentro de uma biblioteca de vínculo dinâmico (uma `.dll`). Um servidor COM na forma de uma DLL é conhecido como um servidor de COM em processo e ele é indicado por CLSIDs que está sendo registrados usando o [InprocServer32](/windows/desktop/com/inprocserver32) chave do registro do Windows.

### <a name="create-a-dynamic-link-library-dll-project"></a>Criar um projeto de biblioteca de vínculo dinâmico (DLL)

Você pode começar a tarefa de criação de um servidor de COM em processo, criando um novo projeto no Microsoft Visual Studio. Criar uma **Visual C++** > **área de trabalho do Windows** > **biblioteca de vínculo dinâmico (DLL)** projeto.

Adicionar C + + c++ /CLI WinRT suporte para o novo projeto, siga as etapas descritas em [modificar um projeto de aplicativo de área de trabalho do Windows para adicionar o C + + c++ /CLI WinRT suporte](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implementar o coclass, fábrica de classes e exportações de servidor em processo

Abra `dllmain.cpp`e adicionar a ele a listagem de código mostrada abaixo.

Se você já tiver uma DLL que implementa C + + c++ /CLI classes de tempo de execução do Windows WinRT, você terá a **DllCanUnloadNow** função mostrada abaixo. Se você deseja adicionar coclasses para essa DLL, você pode adicionar o **DllGetClassObject** função.

Se não tiver [biblioteca de modelos em C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) código que você deseja permanecer compatível com, você poderá remover a WRL partes do código mostrado.

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
            *ppvObject = winrt::make<MyCoclass>().get();
            return S_OK;
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

Consulte também [faz referência fraca em C + + c++ /CLI WinRT](weak-references.md#weak-references-in-cwinrt).

C + + c++ /CLI WinRT (especificamente, o [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) modelo struct de base) implementa [ **IWeakReferenceSource** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) para você se seu Digite implements [ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (ou qualquer interface que deriva **IInspectable**).

Isso ocorre porque **IWeakReferenceSource** e [ **IWeakReference** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) são projetados para tipos de tempo de execução do Windows. Dessa forma, você pode ativar o suporte de referência fraca para seu coclass simplesmente adicionando **winrt::Windows::Foundation::IInspectable** (ou uma interface que deriva **IInspectable**) para sua implementação.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>APIs Importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [WinRT::Implements struct modelo](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar APIs com C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Consumir componentes COM C + + c++ /CLI WinRT](consume-com.md)
* [Enviar uma notificação do sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
