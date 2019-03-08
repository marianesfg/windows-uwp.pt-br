---
title: Autenticação para projetos da UWP
author: aablackm
description: Saiba como conectar usuários Xbox Live em um título de plataforma Universal do Windows (UWP).
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/19/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, autenticação, entrar
ms.localizationpriority: medium
ms.openlocfilehash: 5473b7ede7731d7d07b7e5bfd72857fdb64f1c89
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628381"
---
# <a name="authentication-for-uwp-projects"></a>Autenticação para projetos da UWP

Para tirar proveito dos recursos do Xbox Live em jogos, um usuário precisa criar um perfil do Xbox Live para se identificar na comunidade do Xbox Live.  Serviços do Xbox Live manter o controle de jogo relacionado atividades usando o perfil Xbox Live, como o gamertag do usuário e o jogador imagem, que são amigos de jogos do usuário, o que o usuário de jogos tem desempenhado, quais realizações desbloqueou o usuário, onde o usuário se encontra Placar de líderes para um determinado, jogo, etc.

Quando um usuário quiser acessar serviços do Xbox Live em um jogo específico em um dispositivo específico, o usuário precisa autenticar primeiro.  O jogo pode chamar APIs do Xbox Live para iniciar o processo de autenticação.  Em alguns casos, o usuário será apresentado uma interface para fornecer informações adicionais, como inserir o nome de usuário e a senha da Account da Microsoft para usar, dando consentimento de permissão ao jogo, resolvendo problemas de conta, aceitando novos termos de uso, etc.

Uma vez autenticado, o usuário está associado com o dispositivo até que eles explicitamente saia do Xbox Live do aplicativo do Xbox.  Apenas um player tem permissão para ser autenticado em um dispositivo de não console por vez (para todos os jogos Xbox Live);  para um novo player seja autenticado em um dispositivo de não console, o player autenticado existente deve entrar primeiro.

## <a name="steps-to-sign-in"></a>Etapas para entrar

Em um alto nível, você pode usar as APIs do Xbox Live, seguindo estas etapas:

1. Criar um objeto de XboxLiveUser para representar o usuário
2. Entrar no modo silencioso Xbox Live na inicialização
3. Tente entrar com uma experiência do usuário se necessário
4. Criar um contexto Xbox Live, com base no usuário de interação
5. Usar o contexto do Xbox Live para acessar serviços do Xbox Live
6. Quando o usuário ou sair do jogo sinais horizontalmente, liberar o objeto XboxLiveUser e o objeto de XboxLiveContext definindo-os como nulo

### <a name="creating-an-xboxliveuser-object"></a>Criar um objeto XboxLiveUser

A maioria das atividades do Xbox Live está relacionada ao usuário Xbox Live.  Como desenvolvedor de jogos, você precisa primeiro criar um objeto de XboxLiveUser para representar o usuário local.

C++:

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C++/CX (WinRT):

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C# (WinRT):

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **windowsSystemUser** o objeto de usuário de sistema do windows a ser usado para associar ao usuário em tempo real do xbox. Pode ser nullptr se o aplicativo for application(SUA) um único usuário.
  * Para obter mais informações sobre Application(SUA) de usuário único e Multi Application(MUA) de usuário, verifique se [Introdução aos aplicativos de vários usuários](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * Para obter mais informações sobre como obter Windows::System::User ^ do Windows, verifique se [recuperando o usuário do sistema windows na UWP](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>Entrar no modo silencioso Xbox Live na inicialização ###

Seu jogo deve iniciar autenticar o usuário com o Xbox Live mais cedo possível depois de iniciar, até mesmo antes de apresentar a interface do usuário, para buscar previamente os dados de serviços do Xbox Live.

Para autenticar o usuário local silenciosamente, chamar

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C++/CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  Thread Dispatcher é usado para comunicação entre threads. Embora a API de logon silenciosa não vai mostrar qualquer interface do usuário, o XSAPI ainda precisa o dispatcher do thread da interface do usuário para obter informações sobre a localidade do seu appx. Você pode obter estático dispatcher do thread de interface do usuário chamando Windows::UI::Core::CoreWindow::GetForCurrentThread() -> Dispatcher no thread da interface do usuário. Ou, se você tiver certeza de que essa API está sendo chamado no thread da interface do usuário, você pode passar nullptr (por exemplo, em JS UWA).


Há 3 possíveis resultados da tentativa de logon silenciosa

* **Sucesso**

  Se o dispositivo estiver online, isso significa que o usuário autenticado com êxito com o Xbox Live e fomos capazes de obter um token válido.

  Se o dispositivo estiver offline, isso significa que o usuário foi autenticado anteriormente no Xbox Live com êxito e não foi explicitamente conectado-out desse título.  Observe nesse caso, há nenhuma garantia de que o título tem acesso a um token válido, só é garantido que a identidade do usuário é conhecida e foi verificada.    A identidade do usuário é conhecida como o título por meio de sua id de usuário do xbox (xuid) e o gamertag.

* **UserInteractionRequired**

  Isso significa que o tempo de execução não foi possível entrar o usuário silenciosamente.  O jogo deve chamar `xbox_live_user::sign_in` que invoca o provedor de identidade do Xbox para mostrar o fluxo de experiência do usuário necessário para que o usuário/entrada-inscrição.  Problemas comuns são:

  * Usuário não tem uma Account da Microsoft
  * Usuário não tiver definido uma Account preferencial para jogos da Microsoft
  * Microsoft Account selecionada não tem um perfil do Xbox Live
  * Usuário deve aceitar o consentimento de Account da Microsoft

* **Outros erros**

  O tempo de execução não conseguiu entrar por outros motivos.  Normalmente, esses problemas não são acionáveis pelo jogo ou o usuário. Ao usar c + + API, você precisará verificar o erro verificando .err() de <> xbox_live_result; no WinRT, você precisará capturar Platform:: Exception ^.

### <a name="attempt-to-sign-in-with-ux-if-required"></a>Tente entrar com uma experiência do usuário se necessário ###

Seu jogo deve autenticar o usuário com o Xbox Live com a experiência do usuário habilitada quando entrar silenciosa não foi bem-sucedida, e você estiver pronto para apresentar a interface do usuário.

Para autenticar o usuário local com a experiência do usuário, chame

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C++/CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  Thread Dispatcher é usado para comunicação entre threads. Entrada na API requer que o dispatcher de interface do usuário para que ele pode mostrar o sinal na interface do usuário e obter informações sobre a localidade do seu appx. Você pode obter estático dispatcher do thread de interface do usuário chamando Windows::UI::Core::CoreWindow::GetForCurrentThread() -> Dispatcher no thread da interface do usuário. Ou, se você tiver certeza de que essa API está sendo chamado no thread da interface do usuário, você pode passar nullptr (por exemplo, em JS UWA).

Há 3 possíveis resultados da tentativa de logon com a experiência do usuário:

* **Sucesso**

  Se o dispositivo estiver online, isso significa que o usuário autenticado com êxito com o Xbox Live e fomos capazes de obter um token válido.

  Se o dispositivo estiver offline, isso significa que o usuário foi autenticado anteriormente no Xbox Live com êxito e não foi explicitamente conectado-out desse título.  Observe nesse caso, há nenhuma garantia de que o título tem acesso a um token válido, só é garantido que a identidade do usuário é conhecida e foi verificada.    A identidade do usuário é conhecida para a id de usuário do título xbox (xuid) e o gamertag.

* **UserCancel**

  Isso significa que o usuário cancelou a operação de entrada antes da conclusão.  Quando isso acontece, o jogo não deverá repetir automaticamente entrar com a experiência do usuário.  Em vez disso, ela deve apresentar a experiência do usuário do jogo que permite ao usuário repetir a operação de entrada.  (Por exemplo, um botão de entrada)

* **Outros erros**

  O tempo de execução não conseguiu entrar por outros motivos.  Normalmente, esses problemas não são acionáveis pelo jogo ou o usuário. Ao usar c + + API, você precisará verificar o erro verificando .err() de <> xbox_live_result; no WinRT, você precisará capturar Platform:: Exception ^.

## <a name="sign-in-code-examples"></a>Exemplos de código de entrada

### <a name="c"></a>C++

```cpp

#include "xsapi\services.h" // contains the xbox::services::system namespace

using namespace xbox::services::system; // contains definitions necessary for sign-in

void SignInSample::SignIn()
{
    //1. Create an xbox_live_user object
    m_user = std::make_shared<xbox::services::system::xbox_live_user>(); // m_user declared in header file

    //2. Sign-In silently to Xbox Live at startup
    m_user->signin_silently()
        .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> result)
    {
        if (!result.err())
        {
            auto rPayload = result.payload();
            switch (rPayload.status())
            {
            case xbox::services::system::sign_in_status::success:
                // sign-in successful
                signIn = true;
                break;
            case xbox::services::system::sign_in_status::user_interaction_required:
                // 3. Attempt to sign-in with UX if required
                m_user->signin(Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher)
                    .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> loudResult) // use task_continuation_context::use_current() to make the continuation task running in current apartment 
                {
                    if (!loudResult.err())
                    {
                        auto resPayload = loudResult.payload();
                        switch (resPayload.status())
                        {
                        case xbox::services::system::sign_in_status::success:
                            // sign-in successful
                            signIn = true;
                            break;
                        case xbox::services::system::sign_in_status::user_cancel:
                            // user cancelled sign in 
                            // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                            break;
                        }
                    }
                    else
                    {
                        //login has failed at this point
                    }
                }, concurrency::task_continuation_context::use_current());
                break;
            }
        }
    });
    if (signIn)
    {
        // 4. Create an Xbox Live context based on the interacting user
        m_xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(m_user); // m_xboxLiveContext declared in header file

        // add sign out event handler
        AddSignOut();
    }
}

void SignInSample::AddSignOut()
{
    xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp

using System.Diagnostics;
using Microsoft.Xbox.Services.System;
using Microsoft.Xbox.Services;

public async Task SignIn()
{
    bool signedIn = false;

    // Get a list of the active Windows users.
    IReadOnlyList<Windows.System.User> users = await Windows.System.User.FindAllAsync();

    // Acquire the CoreDispatcher which will be required for SignInSilentlyAsync and SignInAsync.
    Windows.UI.Core.CoreDispatcher UIDispatcher = Windows.UI.Xaml.Window.Current.CoreWindow.Dispatcher; 

    try
    {
        // 1. Create an XboxLiveUser object to represent the user
        XboxLiveUser primaryUser = new XboxLiveUser(users[0]);

        // 2. Sign-in silently to Xbox Live
        SignInResult signInSilentResult = await primaryUser.SignInSilentlyAsync(UIDispatcher);
        switch (signInSilentResult.Status)
        {
            case SignInStatus.Success:
                signedIn = true;
                break;
            case SignInStatus.UserInteractionRequired:
                //3. Attempt to sign-in with UX if required
                SignInResult signInLoud = await primaryUser.SignInAsync(UIDispatcher);
                switch(signInLoud.Status)
                {
                    case SignInStatus.Success:
                        signedIn = true;
                        break;
                    case SignInStatus.UserCancel:
                        // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                        break;
                    default:
                        break;
                }
                break;
            default:
                break;
        }
        if(signedIn)
        {
            // 4. Create an Xbox Live context based on the interacting user
            Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(user);

            //add the sign out event handler
            XboxLiveUser.SignOutCompleted += OnSignOut;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e.Message);
    }

}

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        primaryUser = null;
        xboxLiveContext = null;
    }
```

## <a name="sign-out"></a>Sair

### <a name="handling-user-sign-out-completed-event"></a>Evento concluído logout do usuário do tratamento

O usuário será saída de um título, caso ocorra um destes procedimentos:

1. O usuário conectado-out do Xbox App (Windows 10) ou do shell do console (Xbox One). Saindo afetará todos os aplicativos do Xbox Live habilitado instalados para este usuário.
2. O usuário alternado para outra Account da Microsoft
3. O usuário conectado ao mesmo título de um dispositivo diferente

Nesses casos, o título receberão um evento a partir de `xbox_live_user::add_sign_out_completed_handler` ou `XboxLiveUser::SignOutCompleted` manipuladores.  O jogo deve tratar o evento concluído de saída adequadamente:

1. O jogo deverá exibir indicação visual para o usuário que ele tenha assinado-out do Xbox Live.
2. O jogo não é possível chamar qualquer serviço Xbox Live APIs no manipulador de eventos, porque o usuário tem já assinado-out e nenhum token de autorização está disponível.

## <a name="sign-out-handler-code-samples"></a>Saia de exemplos de código do manipulador

### <a name="c"></a>C++

```cpp

xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>Determinando se o dispositivo estiver offline

Entrada APIs ainda serão bem-sucedidas quando offline se o usuário tiver se conectado uma vez e o último logon na conta será retornado.  

Se nenhum usuário tiver sido conectado antes, entrar offline não serão obtido.

Se o título pode ser executado offline (campanha modo, etc.), o título pode permitir que o usuário tocar e andamento jogo registro por meio da API de WriteInGameEvent e a API de armazenamento conectados, ambos funcionam corretamente enquanto o dispositivo está offline.

Se o título não pode ser executado offline (jogo com vários participantes ou jogo baseado em servidor, etc.) o título deve chamar a API de GetNetworkConnectivityLevel para descobrir se o dispositivo está offline e informar o usuário sobre os status e possíveis soluções (por exemplo, ' é necessário conectar-se à Internet para continuar... ').

## <a name="online-status-code-samples"></a>Exemplos de código de Status Online

### <a name="c"></a>C++

```cpp

using namespace Windows::Networking::Connectivity;

//Retrieve the ConnectionProfile
ConnectionProfile^ InternetConnectionProfile = NetworkInformation::GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile->GetNetworkConnectivityLevel();

switch (connectionLevel)
{
case NetworkConnectivityLevel::InternetAccess:
    // User is connected to the internet.
    break;
case NetworkConnectivityLevel::ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
     // display error message for user.
    LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
    break;
default:
    LogConnectivityLine("Game Offline: No internet access.");
    break;
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
using Windows.Networking.Connectivity;

//Retrieve the ConnectionProfile
string connectionProfileInfo = string.Empty;
ConnectionProfile InternetConnectionProfile = NetworkInformation.GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile.GetNetworkConnectivityLevel();

switch(connectionLevel)
    {
        case NetworkConnectivityLevel.InternetAccess:
            // User is connected to the internet.
            break;
        case NetworkConnectivityLevel.ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
            // display error message for user.
            LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
            break;
        default:
            LogConnectivityLine("Game Offline: No internet access.");
            break;
    }
```