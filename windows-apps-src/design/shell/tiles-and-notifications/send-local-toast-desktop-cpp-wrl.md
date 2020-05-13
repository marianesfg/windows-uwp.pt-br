---
Description: Saiba como os aplicativos do Win32 C++ WRL podem enviar notificações do sistema local e manipular o usuário clicando no sistema de notificação.
title: Enviar uma notificação do sistema local a partir de aplicativos C++ WRL da área de trabalho
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP, Win32, desktop, notificações do sistema, enviar um sistema de notificação, enviar notificação local, desktop Bridge, msix, pacote esparso, C++, CPP, CPlusPlus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 3e103c41de7bf169629085fd259e23e17804360d
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234663"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>Enviar uma notificação do sistema local a partir de aplicativos C++ WRL da área de trabalho

Os aplicativos da área de trabalho (incluindo aplicativos [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) empacotados, aplicativos que usam [pacotes esparsos](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obter a identidade do pacote e aplicativos Win32 não empacotados clássicos) podem enviar notificações de sistema interativas, assim como os aplicativos do Windows. No entanto, há algumas etapas especiais para aplicativos de desktop devido aos diferentes esquemas de ativação e a possível falta de identidade de pacote se você não estiver usando o MSIX ou um pacote esparso.

> [!IMPORTANT]
> Se você estiver criando um aplicativo UWP, consulte a [documentação da UWP](send-local-toast.md). Para outros idiomas da área de trabalho, consulte [Desktop C#](send-local-toast-desktop.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Etapa 1: habilitar o SDK do Windows 10

Se você ainda não ativou o SDK do Windows 10 para seu aplicativo Win32, você deve fazer isso primeiro. Existem algumas etapas chave...

1. Adicionar `runtimeobject.lib` às **Dependências adicionais**
2. Focar no novo SDK do Windows 10

Clique com o botão direito do mouse no projeto e selecione **Propriedades**.

Na parte superior do menu **Configuração**, selecione **Todas as configurações** para que a alteração a seguir seja aplicada à Depuração e à Versão.

Em **Vinculador -> Entrada**, adicione `runtimeobject.lib` às **Dependências adicionais**.

Em seguida, em **Geral**, certifique-se de que a **Versão do SDK do Windows** está definida como 10.0 ou posterior (não Windows 8.1).


## <a name="step-2-copy-compat-library-code"></a>Etapa 2: copiar o código da biblioteca de compatibilidade

Copie o arquivo [DesktopNotificationManagerCompat.h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) e [DesktopNotificationManagerCompat.cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) do GitHub para o projeto. A biblioteca de compatibilidade abstrai grande parte da complexidade das notificações da área de trabalho. As instruções a seguir exigem a biblioteca de compatibilidade.

Se você estiver usando cabeçalhos pré-compilados, certifique-se de `#include "stdafx.h"` como a primeira linha do arquivo DesktopNotificationManagerCompat.cpp.


## <a name="step-3-include-the-header-files-and-namespaces"></a>Etapa 3: incluir os arquivos de cabeçalho e namespaces

Inclua o arquivo de cabeçalho da biblioteca compatível e os arquivos de cabeçalho e namespaces relacionados ao uso das APIs do sistema do Windows.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>Etapa 4: implementar o ativador

Você deve implementar um manipulador para ativação do sistema de forma que, quando o usuário clicar no seu sistema de notificação, seu aplicativo possa fazer algo. Isso é necessário para a notificação do sistema persistir na Central de Ações (pois a notificação do sistema pode ser clicada dias depois quando o aplicativo é fechado). Essa classe pode ser colocada em qualquer lugar do projeto.

Implemente a interface de **INotificationActivationCallback** como visto abaixo, incluindo uma UUID e também chame **CoCreatableClass** para sinalizar sua classe como criável para COM. Para a UUID, crie uma GUID exclusiva usando um dos muitos geradores de GUID online. Essa CLSID (identificador de classe) de GUID é como a Central de Ações sabe qual classe ativar para COM.

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```


## <a name="step-5-register-with-notification-platform"></a>Etapa 5: registrar-se na plataforma de notificação

Em seguida, você deve se registrar na plataforma de notificação. Há diferentes etapas, dependendo se você estiver usando pacotes MSIX/esparsos ou Win32 clássico. Em caso de suporte para ambos, você deve executar as duas etapas (no entanto, não há necessidade de bifurcar o código; a nossa biblioteca processa isso para você!).


### <a name="msixsparse-package"></a>Pacote MSIX/esparso

Se você estiver usando o [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) ou um [pacote esparso](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) (ou se você der suporte a ambos), em seu **Package. appxmanifest**, adicione:

1. Declaração para **xmlns:com**
2. Declaração para **xmlns:desktop**
3. No atributo **IgnorableNamespaces**, **com** e **desktop**
4. **com:Extension** do ativador COM usando a GUID da etapa 4. Certifique-se de incluir `Arguments="-ToastActivated"` para saber que a inicialização foi de uma notificação do sistema
5. **desktop:Extension** para **windows.toastNotificationActivation** a fim de declarar a CLSID do ativador de notificação do sistema (a GUID da etapa 4).

**Package. appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>Win32 clássico

Se estiver usando o Win32 clássico (ou se ambos forem compatíveis), você deve declarar a ID de modelo de usuário do aplicativo (AUMID) e a CLSID do ativador da notificação do sistema (a GUID da etapa 4) no atalho do aplicativo na tela inicial do sistema.

Escolha uma AUMID exclusiva para identificar o aplicativo Win32. Em geral, isso ocorre na forma de [CompanyName].[AppName], mas você quer garantir que isso seja exclusivo em todos os aplicativos (fique à vontade para adicionar alguns dígitos no final).

#### <a name="step-51-wix-installer"></a>Etapa 5.1: instalador do WiX

Se estiver usando o WiX como instalador, edite o arquivo **Product.wxs** para adicionar duas propriedades de atalho ao atalho do menu Iniciar, como mostrado abaixo. Certifique-se de que a GUID da etapa 4 esteja encapsulada em `{}` como visto abaixo.

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Para usar as notificações, você deve instalar o aplicativo por meio do instalador uma vez antes da depuração normal para que o atalho de Iniciar com a AUMID e a CLSID esteja presente. Depois que o atalho de Iniciar estiver presente, você pode depurar usando F5 no Visual Studio.


#### <a name="step-52-register-aumid-and-com-server"></a>Etapa 5.2: registrar a AUMID e o servidor COM

Em seguida, independentemente do instalador, no código de inicialização do aplicativo (antes de chamar quaisquer APIs de notificação), chame o método **RegisterAumidAndComServer**, especificando a classe de ativador de notificação da etapa 4 e a AUMID usada acima.

```cpp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

Se você der suporte ao pacote MSIX/esparso e ao Win32 clássico, sinta-se à vontade para chamar esse método independentemente. Se você estiver executando em um pacote MSIX ou esparso, esse método simplesmente retornará imediatamente. Não há necessidade de bifurcar o código.

Esse método permite que você chame as APIs de compatibilidade para enviar e gerenciar notificações sem precisar fornecer constantemente a AUMID. Além disso, insere a chave de registro LocalServer32 do servidor COM.


## <a name="step-6-register-com-activator"></a>Etapa 6: registrar o ativador COM

Para o pacote MSIX/esparso e os aplicativos Win32 clássicos, você deve registrar seu tipo de ativação de notificação para que possa lidar com ativações do sistema.

No código de inicialização do aplicativo, chame o seguinte método **RegisterActivator**. É necessário chamá-lo para que você receba quaisquer ativações de notificação do sistema.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>Etapa 7: enviar uma notificação

O envio de notificações é idêntico ao dos aplicativos UWP, exceto que você usará **DesktopNotificationManagerCompat** para criar um **ToastNotifier**. A biblioteca de compatibilidade lida automaticamente com a diferença entre o pacote MSIX/esparso e o Win32 clássico, para que você não precise bifurcar seu código. Para o Win32 clássico, a biblioteca de compatibilidade armazena em cache a AUMID fornecida ao chamar **RegisterAumidAndComServer** para que você não precisa se preocupar sobre quando deve fornecer ou não a AUMID.

Certifique-se de usar a associação **ToastGeneric** conforme visto abaixo, pois os modelos de notificação do sistema herdados do Windows 8.1 não ativam o ativador de notificação COM criado na etapa 4.

> [!IMPORTANT]
> As imagens http têm suporte apenas em aplicativos de pacote MSIX/esparsos que têm o recurso de Internet em seu manifesto. Os aplicativos do Win32 clássicos não oferecem suporte às imagens http; você deve baixar a imagem para os dados de aplicativo local e referenciá-los localmente.

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc.Get(), &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> Os aplicativos clássicos do Win32 não podem usar modelos de notificação do sistema herdados (como ToastText02). A ativação dos modelos herdados falha quando CLSID COM é especificada. Você deve usar os modelos ToastGeneric do Windows 10 conforme visto acima.


## <a name="step-8-handling-activation"></a>Etapa 8: manipular a ativação

Quando o usuário clica no sistema ou nos botões na notificação do sistema, o método **Activate** da classe **NotificationActivator** é invocado.

No método Activate, você pode analisar os argumentos especificados na notificação do sistema e obter a entrada que o usuário digitou ou selecionou e, em seguida, ativar o aplicativo adequadamente.

> [!NOTE]
> O método **Activate** é chamado em um thread separado no principal.

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

Para oferecer a inicialização correta do suporte enquanto o aplicativo é fechado, na função WinMain, você deseja determinar se está sendo iniciado em uma notificação do sistema ou não. Se for iniciado a partir de uma notificação do sistema, haverá um arg de inicialização de "-ToastActivated". Quando vocês virem isso, você deve parar de executar qualquer código de ativação de inicialização normal e permitir que o **NotificationActivator** processe as janelas de lançamento, se necessário.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>Sequência de ativação de eventos

A sequência de ativação é a seguinte...

Se o aplicativo já estiver sendo executado:

1. **Activate** no **NotificationActivator** é chamada

Se o aplicativo não estiver sendo executado:

1. O EXE do aplicativo é iniciado e você obtém um argumentos da linha de comando de "-ToastActivated"
2. **Activate** no **NotificationActivator** é chamada


### <a name="foreground-vs-background-activation"></a>Ativação em primeiro plano e segundo plano
Para aplicativos da área de trabalho, a ativação em primeiro plano e em segundo plano é processada de forma idêntica: o ativador COM é chamado. O código do aplicativo deve decidir se deseja mostrar uma janela ou apenas executar algum trabalho e sair. Portanto, especificar um **activationType** de **segundo plano** no conteúdo da notificação do sistema não muda o comportamento.


## <a name="step-9-remove-and-manage-notifications"></a>Etapa 9: remover e gerenciar notificações

A remoção e o gerenciamento de notificações é idêntico ao dos aplicativos UWP. No entanto, recomendamos que você use a biblioteca de compatibilidade para obter uma **DesktopNotificationHistoryCompat** para que você não precisa se preocupar em fornecer a AUMID se estiver usando o Win32 clássico.

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>Etapa 10: implantação e depuração

Para implantar e depurar seu aplicativo de pacote MSIX/esparso, consulte [Executar, depurar e testar um aplicativo de desktop empacotado](/windows/uwp/porting/desktop-to-uwp-debug).

Para implantar e depurar seu aplicativo para Win32 clássico, você deve instalar o aplicativo pelo instalador uma vez antes da depuração normal para que o atalho no menu Iniciar com a AUMID e a CLSID esteja presente. Depois que o atalho de Iniciar estiver presente, você pode depurar usando F5 no Visual Studio.

Caso suas notificações simplesmente não apareçam no aplicativo para Win32 clássico (e nenhuma exceção for lançada), isso provavelmente significa que o atalho de Iniciar não está presente (instalação do aplicativo pelo instalador) ou a AUMID usada no código não corresponde à AUMID do atalho do menu Iniciar.

Se as notificações aparecem, mas não continuam na Central de Ações (desaparecem após ignorar o pop-up), isso significa que você ainda não implementou o ativador COM corretamente.

Se você tiver instalado o pacote MSIX/esparso e o aplicativo Win32 clássico, observe que o aplicativo de pacote MSIX/esparso substituirá o aplicativo Win32 clássico ao lidar com ativações do sistema. Isso significa que as notificações do aplicativo Win32 clássico ainda iniciarão o aplicativo de pacote MSIX/esparso quando clicado. A desinstalação do aplicativo de pacote MSIX/esparso reverterá as ativações de volta para o aplicativo Win32 clássico.

Se você receber `HRESULT 0x800401f0 CoInitialize has not been called.`, certifique-se de chamar `CoInitialize(nullptr)` no aplicativo antes de chamar as APIs.

Se você receber `HRESULT 0x8000000e A method was called at an unexpected time.` enquanto chama as APIs de compatibilidade, isso provavelmente significa que você não pôde chamar os métodos de registro necessários (ou se um aplicativo de pacote MSIX/esparso, você não está executando seu aplicativo no contexto MSIX/esparso).

Caso receba vários erros de compilação `unresolved external symbol`, você provavelmente esqueceu de adicionar `runtimeobject.lib` às **Dependências adicionais** na etapa 1 (ou apenas adicionou à configuração de Depuração e não a configuração de Versão).


## <a name="handling-older-versions-of-windows"></a>Processamento de versões mais antigas do Windows

Se houver suporte para o Windows 8.1 ou anterior, você deve verificar no tempo de execução se está executando no Windows 10 antes de chamar quaisquer APIs de **DesktopNotificationManagerCompat** ou ao enviar quaisquer notificações do sistema ToastGeneric.

O Windows 8 introduziu as notificações do sistema, mas usava os [modelos de notificação do sistema herdados](https://docs.microsoft.com/previous-versions/windows/apps/hh761494(v=win.10)), como ToastText01. A ativação era manipulada pelo evento **Ativado** na memória na classe **ToastNotification** pois as notificações do sistema eram apenas breves pop-ups que não continuaram. O Windows 10 introduziu as [notificações do sistema interativas ToastGeneric](adaptive-interactive-toasts.md), assim como a Central de Ações em que as notificações são mantidas por vários dias. A introdução da Central de Ações exigiu a introdução de um ativador COM para que a notificação do sistema possa ser ativada alguns dias após sua criação.

| Sistema operacional | ToastGeneric | Ativador COM | Modelos de notificação do sistema herdados |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | Com suporte | Com suporte | Suportado (mas não ativa o servidor COM) |
| Windows 8.1/8 | N/D | N/D | Suportado |
| Windows 7 e anterior | N/D | N/D | N/D |

Para verificar se você está executando no Windows 10, inclua o cabeçalho `<VersionHelpers.h>` e verifique o método **IsWindows10OrGreater**. Se isso retorna true, continue chamando todos os métodos descritos nesta documentação. 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>Problemas conhecidos

**CORRIGIDO: o aplicativo não é concentrado depois de clicar na notificação do sistema**: nas versões 15063 e anteriores, os direitos de primeiro plano não estavam sendo transferidos para o aplicativo ao ativar o servidor COM. Portanto,o aplicativo simplesmente piscava ao tentar movê-lo para o primeiro plano. Não havia uma solução alternativa para esse problema. Corrigimos isso nas versões 16299 e posteriores.


## <a name="resources"></a>Recursos

* [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notificações do sistema a partir de aplicativos da área de trabalho](toast-desktop-apps.md)
* [Conteúdo e documentação sobre notificações do sistema](adaptive-interactive-toasts.md)
