---
author: stevewhims
description: C++ c++ WinRT pode ajudá-lo a criar componentes COM clássicos, assim como ele ajuda a criar classes de tempo de execução do Windows.
title: Criar componentes COM C++ c++ WinRT
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, padrão, c++, cpp, winrt, projeção, autor, COM, componente
ms.localizationpriority: medium
ms.openlocfilehash: 729cfae39f302ae6b5bae275d9e28a39f3d9503b
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4126517"
---
# <a name="author-com-components-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Criar componentes COM [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

C++ c++ WinRT pode ajudar você a criar aplicativos clássicos (COM Component Object Model) componentes (ou coclasses), assim como ele ajuda a criar classes de tempo de execução do Windows. Aqui está uma ilustração muito simple, que você pode testar se colá-lo para o `main.cpp` de uma nova **aplicativo de Console do Windows (C++ c++ WinRT)** projeto.

```cppwinrt
// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;

int main()
{
    init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist>
    {
        HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
}
```

Consulte também [componentes de consumir COM C++ c++ WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Um exemplo mais realista e interessante

O restante deste tópico explica como criar um projeto de aplicativo de console mínimo que usa C++ c++ WinRT para implementar uma fábrica coclass e classe básica. O aplicativo de exemplo mostra como fornecer uma notificação do sistema com um botão de retorno de chamada nele, e o coclass (que implementa a interface **INotificationActivationCallback** COM) permite que o aplicativo ser iniciado e chamado quando o usuário clicar nesse botão na notificação.

Obter mais informações sobre a área de recurso de notificação do sistema podem ser encontradas em [Enviar uma notificação do sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Nenhum dos exemplos de código nesta seção da documentação usar C++ c++ WinRT, porém, portanto, recomendamos que você preferir o código mostrado neste tópico.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Criar um projeto de aplicativo de Console do Windows (ToastAndCallback)

Comece criando um novo projeto no Microsoft Visual Studio. Criar um **Visual C++** > **Windows Desktop** > **aplicativo de Console do Windows (C++ c++ WinRT)** projeto e nomeie-o *ToastAndCallback*.

Abra `main.cpp`e remover as usando-diretivas que gera o modelo de projeto. Em seu lugar, cole o seguinte código (o que nos dá a bibliotecas, cabeçalhos e nomes de tipo que precisamos).

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

## <a name="implement-the-coclass-and-class-factory"></a>Implementar a fábrica coclass e de classe

No C++ c++ WinRT, você implementa coclasses e fábricas de classe, derivando struct [**WinRT:: Implements**](/uwp/cpp-ref-for-winrt/implements) base. Imediatamente após as três usando-diretivas mostradas acima (e antes de `main`), cole este código para implementar o componente de ativador de notificação COM notificação do sistema.

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

A implementação do coclass acima segue o mesmo padrão que é demonstrado [criar APIs com C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Observe que você pode usar essa técnica não apenas para interfaces do Windows Runtime (qualquer interface que, por fim, é derivado de [**IInspectable**](https://msdn.microsoft.com/library/br205821)), mas também para implementar interfaces COM (qualquer interface que, por fim, é derivado de [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)).

Coclass no código acima, implementamos o método **INotificationActivationCallback::Activate** , que é a função que é chamada quando o usuário clica no botão de retorno de chamada em uma notificação do sistema. Mas, antes que essa função pode ser chamada, uma instância do coclass precisa ser criado, e esse é o trabalho da função **IClassFactory** .

O coclass que implementamos apenas é conhecido como o *ativador COM* para notificações e tem seu id de classe (CLSID) na forma do `callback_guid` identificador (do tipo **GUID**) que você consulte acima. Vamos usar esse identificador mais tarde, na forma de um atalho do menu Iniciar e uma entrada de registro do Windows. O CLSID do ativador COM e o caminho para o servidor COM associado (que é o caminho para o executável que estamos criando aqui) é o mecanismo pelo qual uma notificação do sistema sabe qual classe para criar uma instância de quando o botão de retorno de chamada é clicado (se o notificação é clicada na Central de ações ou não).

## <a name="best-practices-for-implementing-com-methods"></a>Práticas recomendadas para implementar métodos COM

Técnicas para o tratamento de erros e para gerenciamento de recursos podem ir mão em conjunto. É mais conveniente e prático usar exceções de códigos de erro. E se você implante o idioma de (RAII)-aquisição-for-inicialização de recursos, em seguida, você pode evitar explicitamente verificando códigos de erro e, em seguida, explicitamente, liberando recursos. Essas verificações explícitas tornam seu código mais complicado que o necessário e dá bugs muitos locais para ocultar. Em vez disso, use RAII e jogue/catch exceções. Dessa forma, seus alocações de recursos são prova de exceções, e seu código é simple.

No entanto, você não deve permitir exceções escapar suas implementações de método COM. Você pode garantir que usando o `noexcept` especificador em seus métodos COM. É okey para exceções seja lançada em qualquer lugar no gráfico de chamada do seu método, desde que você manipulá-los antes do método é encerrado. Se você usar `noexcept`, mas você, em seguida, permitir que uma exceção no seu método de escape, seu aplicativo será encerrado.

## <a name="add-helper-types-and-functions"></a>Adicionar funções e tipos auxiliares

Nesta etapa, vamos adicionar alguns tipos auxiliares e funções que o restante do código faz usam de. Portanto, antes de `main`, adicione o seguinte.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implementar as funções restantes e a função de ponto de entrada WMA

O modelo de projeto gera um `main` função para você. Exclua `main` função e, em seu lugar cole este código de listagem, que inclui código para registrar seu coclass, e, em seguida, para fornecer uma notificação do sistema capaz de chamar novamente seu aplicativo.

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
    init_apartment();

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

Criar o aplicativo e, em seguida, executá-lo a pelo menos uma vez como administrador para fazer com que o registro e outra instalação, o código para ser executado. Ou não está executando como administrador, então pressione pré-lançados ' para fazer com que uma notificação do sistema a ser exibido. Clique no botão de **retorno de chamada ToastAndCallback** tanto diretamente da notificação do sistema que estalos para cima ou da Central de ações e seu aplicativo serão iniciados, o coclass instanciado e o INotificationActivationCallback ** :: Ativar** método executado.

## <a name="important-apis"></a>APIs Importantes
* [Interface IInspectable](https://msdn.microsoft.com/library/br205821)
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Modelo de estrutura winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Consumir componentes COM C++ c++ WinRT](consume-com.md)
* [Enviar uma notificação do sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
