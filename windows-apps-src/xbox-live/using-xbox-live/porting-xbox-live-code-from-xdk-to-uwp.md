---
title: Portabilidade de código Xbox Live do XDK para UWP
description: Saiba como portar código do Xbox Live da plataforma do Kit de desenvolvimento do Xbox (XDK) para Universal Windows Platform (UWP).
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, xdk, portabilidade
ms.localizationpriority: medium
ms.openlocfilehash: c6e8a6ebe716f1e062940066184e9f734441371b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590811"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>Portabilidade de código Xbox Live Kit do desenvolvedor do Xbox (XDK) à plataforma Universal do Windows (UWP)

## <a name="introduction"></a>Introdução

Este artigo destina-se a ajudar os desenvolvedores que usaram um Xbox XDK para começar a migrar seu código do Xbox Live para o Windows 10 Universal Windows Platform (UWP).

Parte da migração inclui a mudança do XSAPI 1.0 (Xbox Live API dos serviços, incluídos no Xbox um XDK até agosto de 2015) XSAPI 2.0 (incluído no Xbox XDK um a partir de novembro de 2015 e também está disponível no SDK do Xbox Live. A funcionalidade dessas APIs sejam virtualmente idênticos, mas há algumas diferenças importantes da implementação.

Outros tópicos abordados neste artigo incluem Preparando seu computador de desenvolvimento do Windows e a instalação de outras APIs normalmente necessárias ao usar os serviços Xbox Live, como API do Secure Sockets, bem como a API de armazenamento conectado para o gerenciamento baseado em nuvem jogo salva.

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-partner-center-and-xdp"></a>Definindo e configurando seu projeto no Partner Center e XDP

Um título UWP que usa o Xbox Live services precisa ser configurado no [Partner Center](https://partner.microsoft.com/dashboard). Para obter informações mais recentes, consulte [adicionando o Xbox Live para um projeto novo ou existente do UWP](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) no Xbox Live guia de programação incluídas com o [Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Os tópicos nessa página incluem estas etapas para usar serviços do Xbox Live em seu título:

-   Crie o projeto de aplicativo UWP no Partner Center.

-   Use XDP para configurar seu projeto para uso do Xbox Live.

-   Vincule seu produto Partner Center ao seu produto XDP.

-   Crie contas de desenvolvedor no XDP (necessário ao executar o título do Xbox Live em sua área restrita).

Se os títulos de suporte para vários participantes, algumas configurações adicionais podem ser necessário em seus modelos de sessão para múltiplos jogadores. Todos os títulos do Windows 10 que usam o Xbox Live para múltiplos jogadores e gravar em um MPSD (documento de sessão para múltiplos jogadores) exigem o novo campo na lista de "recursos" são encontrados em seus modelos de sessão: ```userAuthorizationStyle: true```.

### <a name="enabling-cross-play"></a>Habilitando cross-play

Se você for oferecer suporte a "cross-play" (uma Xbox Live configuração compartilhada entre os jogos Xbox One e PC, permitindo que vários dispositivos jogos para múltiplos jogadores), você também precisará adicionar essa funcionalidade a seus modelos de sessão: **crossPlay: true**.

Para obter informações adicionais sobre o suporte a seus requisitos de configuração e cross-play no XDP, consulte "Ingestão XDK e UWP Play entre títulos em XDP" no Xbox Live guia de programação.

Além disso, para algumas considerações programáticas, consulte a seção posterior [com suporte para múltiplos jogadores play cruzada entre o PC e o Xbox One](#_Supporting_multiplayer_cross-play).

## <a name="setting-up-your-windows-development-environment"></a>Como configurar seu ambiente de desenvolvimento do Windows

1.  [Baixe o versão mais recente **Xbox Live SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) e extraia localmente.

2.  [Instalar o **Xbox Live extensões do SDK da plataforma** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) se precisar de API do Secure Sockets e/ou API de salvar o jogo (também conhecido como armazenamento conectado) para UWP.

3.  Adicione suporte do Xbox Live em seu projeto de aplicativo Windows Universal no Visual Studio. Você pode adicionar a fonte de completa ou fazer referência aos binários instalando o pacote do NuGet no projeto do Visual Studio. Pacotes estão disponíveis para C++ e o WinRT. Para obter mais detalhes, consulte [adicionando o Xbox Live para um projeto novo ou existente do UWP](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  Configure seu computador de desenvolvimento para usar sua área restrita. Há um script de linha de comando no diretório de ferramentas do Xbox Live SDK que você pode usar em um prompt de comando de administrador (por exemplo: SwitchSandbox.cmd XDKS.1).

  **Observação** para voltar para a área restrita de varejo, você pode excluir a chave do registro que modifica o script, ou você pode alternar para a área restrita da chamada de varejo.

1.  Adicione uma conta de desenvolvedor para seu computador de desenvolvimento. Uma conta de desenvolvedor que criou no XDP é necessária para interagir com serviços do Xbox Live em tempo de execução quando você estiver desenvolvendo em sua área de segurança atribuída ou executando exemplos. Para adicionar uma ou mais contas para Windows:

    1.  Abra **configurações** (atalho: Tecla Windows + I).

    2.  Abra **contas**.

    3.  Sobre o **Your Account** , clique em **adicionar uma conta da Microsoft**.

    4.  Insira o email da conta de desenvolvedor e a senha.

### <a name="appxmanifest-changes"></a>Alterações de AppxManifest

As alterações mais comuns entre as versões do Xbox e UWP do arquivo appxmanifest. XML são:

1. Identidade do pacote é importante na UWP, até mesmo durante o desenvolvimento. O nome de identidade e o publicador *deve corresponder ao* que foi definido no Partner Center para seu aplicativo UWP.

1. Uma seção de dependência de pacote é necessária. Por exemplo:

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  Consulte para outras seções do manifesto do aplicativo que têm requisitos específicos para um manifesto de aplicativo UWP de exemplo (por exemplo, um dos exemplos UWP incluídos com o Xbox Live SDK ou um projeto de aplicativo do Windows Universal padrão criado no Visual Studio) UWP, como ```<VisualElements>```.

1.  Título e SCID são definidos no arquivo xboxservices.config (consulte a [próxima seção](#_Define_your_title)) em vez de na categoria de extensão "xbox.live".

1.  A categoria de extensão "xbox.system.resources" não é necessária.

1.  Soquetes seguros são definidos no arquivo networkmanifest.xml (consulte [Secure sockets](#_Secure_sockets)) em vez de na categoria "windows.xbox.networking".

1.  Uma categoria de extensão "windows.protocol" deve ser definida para receber convites do Xbox Live em seu título da UWP (consulte [de envio e recebimento de convites](#_Sending_and_receiving)).

1.  Se você usar a API GameChat, você desejará adicionar a funcionalidade do dispositivo microfone dentro a ```<Capabilities>``` elemento. Por exemplo:

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>Definir seu título e SCID para o Xbox Live SDK em um arquivo de configuração

O Xbox Live SDK precisa saber o título da ID e SCID, que não estão mais incluídas no appxmanifest. XML para títulos UWP. Em vez disso, você pode criar um arquivo de texto chamado **xboxservices.config** em seu projeto no diretório raiz e adicione os campos a seguir, substituindo os valores com as informações para o título:

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Todos os valores dentro de xboxservices.config diferenciam maiusculas de minúsculas.

Inclua este arquivo de configuração como conteúdo em seu projeto para que ele esteja disponível na saída da compilação.

**Observação** esses valores estarão disponíveis por meio de programação em seu título usando a seguinte API:

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>Mapeamento de namespace de API

Tabela 1. Mapeamento de Namespace do XDK para UWP.

<table>
  <tr>
    <td></td>
    <td><b>Um XDK do Xbox</b></td><td><b>UWP</b></td>
    <td><b>API está disponível com...</b></td>
  </tr>
  <tr>
    <td>Serviços do Xbox API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::Services (<i>no change</i>)</td>
    <td>Xbox Live SDK (use NuGet binário ou origem)</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat (*nenhuma alteração*) Microsoft::Xbox::ChatAudio </td>
    <td>
Xbox Live SDK (use o NuGet binário) </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>As extensões da plataforma SDK do Xbox Live</td>
  </tr>
  <tr>
    <td>Armazenamento Conectado</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>As extensões da plataforma SDK do Xbox Live</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>Assinaturas com vários participantes e manipulação de eventos

Uma das alterações significativas do XSAPI 1.0 para 2.0 XSAPI que títulos mais com vários participantes encontrará é a mudança dos vários métodos e eventos a partir de **RealTimeActivityService** para o **MultiplayerService**.

Por exemplo:

-   **EnableMultiplayerSubscriptions()\***  método

-   **DisableMultiplayerSubscriptions()** método

-   **MultiplayerSessionChanged** evento

-   **MultiplayerSubscriptionLost** evento

-   **MultiplayerSubscriptionsEnabled** propriedade

**Observação importante implementação** mesmo que você talvez não estejam explicitamente usando mais nada na **RealTimeActivityService** depois de mover esses eventos e métodos para o **MultiplayerService**, você ainda deve chamar **xblContext -&gt;RealTimeActivityService -&gt;Activate()** antes de chamar **EnableMultiplayerSubscriptions()** porque as assinaturas com vários participantes requerem que o serviço RTA.

## <a name="whats-handled-differently-in-uwp"></a>O que é tratado diferentemente na UWP

A seguir é uma lista de alto nível das seções de código que provavelmente terá as diferenças entre o XDK e UWP, como encontrado no novo [NetRumble exemplo](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) (que inclui versões XDK e UWP):

-   Acessando informações de ID e SCID título

-   Ativação de pré-inicialização (nova para UWP)

-   Suspender/retomar tratamento PLM

-   Execução estendida (nova para UWP)

-   Xbox **usuário** objeto e as diferenças de tratamento de usuário

    -   Manipulação de entrada e saída

    -   Controlador de emparelhamento (somente manipulado no Xbox)

    -   Tratamento de Gamepad

-   Verificando privilégios para múltiplos jogadores

-   Suporte para múltiplos jogadores play cruzada entre o PC e o Xbox One

-   Envio de convites de jogos

    -   Capacidade de abrir o aplicativo de terceiros de jogos - n/d na UWP

    -   Capacidade de enumerar os membros de terceiros de jogos - n/d na UWP

-   Mostrando o perfil de jogador

-   Alterações de superfície de API de soquete seguras

-   Iniciação de medição de QoS e tratamento de resultado

-   Gravar eventos de jogos

-   GameChat: objeto ChatUser, configurações e eventos

-   Conectado a alterações de superfície da API de armazenamento

-   Eventos de PIX (somente no Xbox; não abordadas neste white paper)

-   Algumas diferenças de renderização

As seções a seguir entram em mais detalhes sobre muitas dessas diferenças.

### <a name="accessing-title-id-and-scid-info"></a>Acessando informações de ID e SCID título

Na UWP, a ID de título e a ID de configuração de serviço são acessados por meio da propriedade AppConfig em uma instância de um **XboxLiveContext**.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**Observação** em XDK do, você pode obter esses IDs usando essas novas propriedades ou propriedades estáticas no antigas **Windows::Xbox::Services::XboxLiveConfiguration**.

### <a name="prelaunch-activation"></a>Ativação de pré-lançamento

Títulos usados no Windows 10 podem ser pré-inicializado quando o usuário faz logon. Para lidar com isso, o título deve ter código que verifica os argumentos de inicialização para **PreLaunchActivated**. Por exemplo, você provavelmente não deseja carregar todos os seus recursos durante a esse tipo de ativação. Para obter mais informações, consulte o artigo do MSDN [identificador de aplicativo pré-inicialização](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx).

### <a name="suspendresume-plm-handling"></a>Suspender/retomar tratamento PLM

Suspender e retomar e PLM em geral, funcionam da mesma forma em um aplicativo Windows Universal à forma como eles funcionam no Xbox One; No entanto, há algumas diferenças importantes para ter em mente:

-   Não há nenhuma **Constrained** estado no PC — esse é um conceito exclusivo Xbox One.

-   Suspendendo começa imediatamente quando o título está minimizado. para uma solução alternativa para isso, consulte a seção [estendido de execução](#_Extended_execution).

-   O tempo é diferente: você tem 5 segundos seja suspenso em um PC, em vez de 1 segundo no console.

Outra consideração importante se você usar o armazenamento conectado é o novo **ContainersChangedSinceLastSync** propriedade na versão UWP dessa API. Ao manipular um evento de retomada, você pode verificar essa propriedade para ver se todos os contêineres alterados na nuvem, enquanto o título foi suspenso. Isso pode acontecer se o jogador suspenso o jogo em um único PC, reproduzida em outro lugar e, em seguida, é retornado para o primeiro PC. Se você tivesse dados lidos desses contêineres na memória antes de você tinha suspenso, você provavelmente desejará lê-los novamente para ver o que alterou e lidar com as alterações de acordo.

Para obter mais informações sobre como lidar com o PLM em um aplicativo UWP no Windows 10, consulte o artigo do MSDN [Iniciar, retomar e tarefas em segundo plano](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx).

Você também pode encontrar o [PLM para Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) white paper sobre GDN útil, pois ele foi gravado com jogos em mente, e a maioria dos conceitos para lidar com o ciclo de vida do aplicativo ainda se aplica em um PC.

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>Execução estendida

Aplicativo em um computador minimizando um UWP normalmente resulta em ele começar imediatamente a suspender. Com o uso estendido de execução, você tem a oportunidade de atrasar a esse processo. Exemplo de implementação:

```cpp
using namespace Windows::ApplicationModel::ExtendedExecution;
//If this goes out of scope the request is nullified
ExtendedExecutionSession^ session;
void App::RequestExtension()
{
       if (!session)
       {
              session = ref new ExtendedExecutionSession();
       }
       session->Reason = ExtendedExecutionReason::Unspecified;
       session->Description = "foo";
       session->Revoked += ref new TypedEventHandler<Platform::Object^, ExtendedExecutionRevokedEventArgs^>(this, &App::ExtensionRevokedHandler);
       IAsyncOperation<ExtendedExecutionResult>^ request = session->RequestExtensionAsync();
       //At this point the request has been made. When the IAsyncOperation request completes, verify that the ExtendedExecutionResult == Allowed and you will not suspend for up to 10 minutes while minimized.
}

void App::ExtensionRevokedHandler(Platform::Object^ obj, ExtendedExecutionRevokedEventArgs^ args)
{
       if (args->Reason == Windows::ApplicationModel::ExtendedExecutionRevokedReason::Resumed)
       {
              //Request the extension again in preparation for the next suspend.
RequestExtension();
       }
       //The app will either complete suspending if the extension was revoked by system policy or resume running if the user has switched back to the app.
}

```

Após o **ExtensionRevokedHandler** tiver sido chamado, uma nova extensão deve ser solicitado para suspensão potencial futura. O **ExtensionRevokedHandler** é chamado quando há pressão de memória no sistema, 10 minutos tenham passado ou o usuário alterna para o jogo, enquanto o jogo é minimizado. Portanto, **RequestExtension()** provavelmente deve ser chamado nos seguintes momentos:

-   Durante a inicialização.

-   No **ExtensionRevokedHandler** quando args -&gt;motivo = = retomado (o usuário com guias novamente enquanto o jogo foi minimizado antes de expirar o temporizador de 10 minutos).

-   No **OnResuming** manipulador (se o título foi suspenso devido à pressão de memória ou o timer de 10 minutos).

### <a name="handling-users-and-controllers"></a>Tratamento de usuários e controladores

No Windows, você trabalha com um usuário conectado por vez. O Xbox Live SDK, você primeiro cria uma **XboxLiveUser** do objeto, entrar com o Xbox Live e, em seguida, crie **XboxLiveContext** objetos do usuário.

Antes, no Xbox um XDK:

1.  Adquira um usuário (de interação de gamepad, por exemplo).
2.  Criar uma **XboxLiveContext** do que o usuário:
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  Lidar com uma **SignOut** eventos:
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  Lidar com o emparelhamento de gamepad/controlador usando:
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

Agora, para o UWP/Xbox Live SDK:

1.  Criar uma **XboxLiveUser**:

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  Tente entrar usando a última conta da Microsoft usados por eles, sem se preocupá-los com interface do usuário:

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  Se você obtiver **SignInResult::Success** cria o resultado dessa operação assíncrona, o **XboxLiveContext**, e, em seguida, você terminar:

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  Se, em vez disso, você obtém **SignInResult::UserInteractionRequired**, você precisa chamar o método de logon interativo que abrirá a interface do usuário do sistema:

  ```
  xblUser->SignInAsync();
  ```

1.  A partir daqui, você pode receber **SignInResult::UserCancel**, caso em que você não tiver um usuário conectado e você deve considerar o fornecimento de uma opção de menu para que ele tente entrar novamente.

  **Observação** ao fornecer opções de menu, ele é uma boa ideia para dar-lhes a opção para alternar para outra conta da Microsoft:

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  Depois de ter um usuário conectado, talvez você queira conectar a **XboxLiveUser::SignOutCompleted** eventos para que você possa reagir ao usuário sair:

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  Não há nenhum controlador de emparelhamento para lidar com no Windows 10.

Este é um exemplo simplificado para C++ / WinRT. Para obter um exemplo mais detalhado, consulte "Xbox Live autenticação no Windows 10" no Xbox Live guia de programação. Você também pode encontrar o exemplo mais amplo em "Adicionando o Xbox Live para um novo projeto UWP" útil.

### <a name="checking-multiplayer-privileges"></a>Verificando privilégios para múltiplos jogadores

O equivalente a **CheckPrivilegeAsync()** ainda não está disponível no SDK do Xbox Live. Por enquanto, você precisará pesquisar o privilégio necessário na lista de cadeia de caracteres retornada pela **privilégios** propriedade para um **XboxLiveUser**. Por exemplo, para verificar os privilégios para múltiplos jogadores, procure o privilégio "254." Usando a documentação do XDK, você pode encontrar uma lista de todos os privilégios Xbox Live na **Windows::Xbox::ApplicationModel::Store::KnownPrivileges** enumeração.

Para obter uma discussão sobre esse tópico, consulte o postagem do fórum [xsapi & usuário privilégios](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html).

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>Suporte para múltiplos jogadores entre-play entre Xbox One e UWP do PC

Além dos requisitos de modelo de nova sessão no XDP (consulte [definindo e configurando seu projeto no Partner Center e XDP](#_Setting_up_and)), entre-play vem com novas restrições na capacidade de associação de sessão. Você não poderá mais usar "None" como uma restrição de associação de sessão. Você deve usar "Seguidos" ou "Local" (a restrição padrão é "Local").

Além disso, a junção e restrições de leitura padrão "Local" devido ao exigida **userAuthorizationStyle** capacidade para vários jogadores do Windows 10.

Este artigo do fórum, [é possível criar uma sessão para múltiplos jogadores pública](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html), contém informações adicionais.

Mais informações e exemplos podem ser encontrados nos fluxogramas de desenvolvedor para múltiplos jogadores atualizado, o exemplo com vários participantes habilitado entre-play NetRumble, ou do Gerenciador de conta de desenvolvedor (mãe).

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>Enviar e receber convites

A API para exibir a interface do usuário para o envio de convites agora está **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()**. Passar em uma sessão -&gt; **SessionReference** objeto da sua sessão de atividade (normalmente sua lobby). Opcionalmente, você pode passar em um segundo parâmetro que referências personalizado convidar ID de cadeia de caracteres foi definido em sua configuração de serviço no XDP. A cadeia de caracteres que você defina lá será exibido na notificação do sistema enviada para os jogadores convidados. Observe que o que você está passando em como um parâmetro para esse método é o número de ID, e ele deve ser formatado corretamente para o serviço. Por exemplo, a ID de cadeia de caracteres "1" deve ser passado como "/ / / 1".

Se você quiser enviar convites diretamente por meio do serviço com vários participantes (ou seja, sem mostrar qualquer interface do usuário), você ainda pode usar o outro método de convite, **Microsoft:: Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()** de que o usuário **XboxLiveContext**.

Para permitir convites chegam ao Windows protocolo-ativar seu título, você precisará adicionar essa extensão para o **&lt;aplicativo&gt;** elemento em que o appxmanifest:

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Em seguida, você pode manipular o invite, como fez anteriormente no Xbox One quando sua **CoreApplication** obtém uma **Activated** evento e a ativação de tipo é um **ActivationKind::Protocol**.

### <a name="showing-the-gamer-profile-card"></a>Mostrando o jogador cartão de perfil

Para o pop-up cartão de perfil jogador na UWP, use **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()**, passando o XUID para o usuário de destino.

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>Soquetes seguros

A API de soquete seguro está incluída no separada [Xbox Live extensões do SDK da plataforma](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Consulte este fórum lançar para uso da API: [Configurando SecureDeviceAssociation para várias plataformas](https://forums.xboxlive.com/answers/45722/view.html).

**Observação** para UWP, o **SocketDescriptions** seção foi movido para fora o appxmanifest e em seu próprio [networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt). O formato dentro de &lt;SocketDescriptions&gt; elemento é praticamente idêntico, mas sem os **mx:** prefixo.

Para reproduzir cruzada entre Xbox e Windows 10, ser *-se de que* que tudo o que está definido *identicamente* entre os dois tipos diferentes de manifestos (Package. appxmanifest para Xbox One e networkmanifest.xml para Windows 10). O nome de soquete, protocolo, etc. devem corresponder *exatamente*.

Também entre-Play, você precisará definir os seguintes quatro usos SDA dentro de ```<AllowedUsages>``` elemento no *ambos* Xbox um Package. appxmanifest e networkmanifest.xml o Windows 10:

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>Medidas de QoS com vários participantes

A alteração do namespace na API do Sockets seguro, além de alguns dos valores e nomes de objeto foram alterados, muito. O mapeamento para o status de medição usado normalmente é encontrado na tabela a seguir.

Tabela 2. Normalmente usado mapeamento de status de medição.

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | TimedOut                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| Êxito                            | Êxito                                  |

As etapas envolvidas na *medindo* QoS (qualidade de serviço) e *processar os resultados* são em princípio iguais ao comparar as versões de API do XDK e UWP. No entanto, devido a alterações de nome e algumas alterações de design, o código resultante tem uma aparência diferente em alguns locais.

Para medir o QoS para o **XDK**, você criou uma coleção de endereços de seguro de dispositivos e uma coleção de métricas e passados na **MeasureQualityOfServiceAsync()** método.

Para medir o QoS para **UWP**, você cria um novo **XboxLiveQualityOfServiceMeasurement()** de objeto, chame **append ()** ao seu **métricas** e **DeviceAddresses** propriedades e do, em seguida, chamar o objeto **MeasureAsync()** método.

Por exemplo:

```cpp
auto qosMeasurement = ref new Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurement();
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageInboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageOutboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageLatencyInMilliseconds);
qosMeasurement->NumberOfProbesToAttempt = myDefaultQosProbeCount;
qosMeasurement->TimeoutInMilliseconds = myDefaultQosMeasurementTimeout;

// Add secure addresses for each session member
for (const auto& member : session->GetMembers())
{
    if (!member->IsCurrentUser)
    {
        auto sda = member->SecureDeviceAddressBase64;

        if (!sda->IsEmpty())
        {
qosMeasurement->DeviceAddresses->Append(Windows::Networking::XboxLive::XboxLiveDeviceAddress::CreateFromSnapshotBase64(sda));
        }
    }
}

if (qosMeasurement->DeviceAddresses->Size > 0)
{
    qosMeasurement->MeasureAsync();
}

```

Para obter mais exemplos, consulte o **MatchmakingSession::MeasureQualityOfService()** e **MatchmakingSession::ProcessQosMeasurements()** funções no exemplo NetRumble.

### <a name="writing-game-events"></a>Gravar eventos de jogos

Enviar eventos de jogos que são configurados na configuração de serviço do seu título tem uma API diferente na UWP. O Xbox Live SDK usa o **EventsService** e um modelo de recipiente de propriedades.

Por exemplo:

```cpp
auto properties = ref new Windows::Foundation::Collections::PropertySet();
properties->Insert("RoundId", m_roundId);
properties->Insert("SectionId", safe_cast<Platform::Object^>(0));
properties->Insert("MultiplayerCorrelationId", m_multiplayerCorrelationId);
properties->Insert("GameplayModeId", safe_cast<Platform::Object^>(0));
properties->Insert("MatchTypeId", safe_cast<Platform::Object^>(0));
properties->Insert("DifficultyLevelId", safe_cast<Platform::Object^>(0));

auto measurements = ref new Windows::Foundation::Collections::PropertySet();

xblContext->EventsService->WriteInGameEvent("MultiplayerRoundStart", properties, measurements);

```

Para obter mais informações, consulte a documentação do Xbox Live SDK.

**Dica** você pode usar o **xcetool.exe** fornecido com o Xbox Live SDK (localizado no diretório de ferramentas) para converter o arquivo events.man que você baixou do XDP em um arquivo de cabeçalho. h. Use o '-x' opção para gerar esse cabeçalho de C++ usando o novo esquema de recipiente de propriedades de v2. Esse cabeçalho contém funções de C++ que você pode chamar para todos os seus eventos configurados; Por exemplo, **EventWriteMultiplayerRoundStart()**. Se você preferir usar uma interface de WinRT, você ainda pode consultar esse arquivo de cabeçalho para ver como as propriedades e medidas são construídas para cada um dos seus eventos.

### <a name="game-chat"></a>Jogo bate-papo

GameChat na UWP está incluído com o Xbox Live SDK como um pacote do NuGet binário. Consulte as instruções no Xbox Live guia de programação para saber como adicionar este pacote NuGet ao seu projeto.

Uso básico é praticamente idêntico entre o XDK e as versões UWP. Algumas diferenças na API incluem:

1.  O **User::AudioDeviceAdded** evento não precisa ser conectado por um título UWP. O dispositivo de identificadores de biblioteca de bate-papo subjacente adiciona e remove.

2.  **ChatUser** agora é chamado **GameChatUser**.

3.  **Microsoft::Xbox::GameChat** namespace permanece a mesma, mas o **Windows::Xbox::Chat** namespace tornou **Microsoft::Xbox::ChatAudio**.

4.  **AddLocalUserToChatChannelAsync()** leva um XUID ou **ChatAudio::IChatUser ^** em vez de uma **XboxUser**.

5.  **RemoveLocalUserFromChatChannelAsync()** exige um **ChatAudio::IChatUser ^** em vez de uma **XboxUser**. Você pode obter um **IChatUser** de uma **GameChatUser**-&gt;**usuário**.

### <a name="connected-storage"></a>Armazenamento conectado

A API de armazenamento conectado é fornecida no separada [Xbox Live extensões do SDK da plataforma](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx). Documentação está incluída nos documentos do Xbox Live SDK.

O fluxo geral é o mesmo que no Xbox One, com a adição do **ContainersChangedSinceLastSync** propriedade na versão de UWP. Essa propriedade deve ser verificada quando seu título manipula um evento de retomada, depois de chamar **GetForUserAsync()** novamente ver o que mudou contêineres na nuvem ao seu título foi suspenso. Se você tiver dados carregados na memória de um dos contêineres que foi alterada, você provavelmente deseja ler os dados novamente para ver o que alterou e lidar com as alterações de forma adequada.

Outras diferenças notáveis na versão UWP incluem:

1.  Alteração do Namespace do **Windows::Xbox::Storage** à **Windows::Gaming::XboxLive::Storage**.

2.  **ConnectedStorageSpace** é renomeado **GameSaveProvider**.

3.  **Windows::System::User** é usado em **GetForUserAsync()** em vez de uma **XboxUser**, e o SCID agora é necessário.

4.  Nenhum armazenamento local de "computador" (ou seja, **GetForMachineAsync()** foi removido). Considere o uso de **Windows::Storage::ApplicationData** em vez disso, para seu não roaming, local salvar dados.

5.  Async resultados são retornados em um sem exceções \*objeto do tipo de resultado (por exemplo, **GameSaveProviderGetResult**); deste você pode verificar o **Status** propriedade, e se não houver nenhum erro, ler o objeto retornado do **valor** propriedade.

6.  **Enum ConnectedStorageErrorStatus** é renomeado **GameSaveErrorStatus** e é retornado no **Status** propriedade de um resultado. Existem todos os valores antigos e alguns novos foram adicionados:

-   Anular

-   ObjectExpired

-   OK

-   UserHasNoXboxLiveInfo

Consulte o exemplo GameSave ou a amostra NetRumble por exemplo uso.

**Observação** Gamesaveutil.exe equivale a xbstorage.exe (o utilitário de linha de comando do desenvolvedor incluído com o XDK). Após instalar o SDK de extensões do Xbox Live plataforma, esse utilitário pode ser encontrado aqui: C:\\(x86) de arquivos de programa\\Windows Kits\\10\\SDKs de extensão\\XboxLive\\1.0\\Bin\\x64

## <a name="summary"></a>Resumo

As alterações de API e os novos requisitos descritos neste white paper são aqueles que você provavelmente encontrará ao portar o código existente de jogo do Xbox um XDK para o novo UWP. Foi dada ênfase especial para o aplicativo e configuração do ambiente, bem como áreas de recurso relacionadas aos serviços Xbox Live, como o armazenamento com vários participantes e conectado. Para obter mais informações, siga os links fornecidos neste artigo e nas seguintes referências e não deixe de visitar a seção "Windows 10" a [fóruns para desenvolvedores](https://forums.xboxlive.com) para obter mais ajuda, respostas e notícias.

## <a name="references"></a>Referências

-   [Portabilidade do Xbox One para Windows 10](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [White Papers do Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [Exemplos](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
