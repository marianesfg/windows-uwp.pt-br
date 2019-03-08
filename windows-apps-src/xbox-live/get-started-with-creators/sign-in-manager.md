---
title: Entrar com a SignInManager no Unity
description: Visão geral do Gerenciador de plug-in entrar Unity
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity
ms.openlocfilehash: e6d066fe7792912f8918cb139d45ff05d105feaa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641721"
---
# <a name="scripting-sign-in"></a>Entrada de script

Para adicionar entrada a seus próprios objetos de jogo personalizados, que será necessário gerar o script em um GameObject. Digamos que pré-fabricado PlayerAuthentication não se ajustar seu jogo e você gostaria de ter seu próprio painel de entrada, este artigo o guiará durante as etapas básicas de adicionar lógica de sinal para o título da.

## <a name="sign-in-with-the-signinmanager"></a>Entrar com o SignInManager.

O plug-in Xbox Live Unity contém um script para o `SignInManager` sob o caminho do arquivo **ativos >> XboxLive >> Scripts >> SignInManager.cs**. O Gerenciador é uma classe Singleton que pode ser chamada formulário em qualquer lugar no seu título consultando o título *instância* da `SignInManager`. Isso *instância* não precisam ser inicializadas e você pode utilizá-lo assim que seu jogo começa. Você pode acessar todos os as suas propriedades públicas e funções consultando a *instância* como `SignInManager.Instance`.

O `SignInManager` contém todo o código necessário para gerenciar a autenticação para seu título, isso inclui entrada, saída, e obter informações sobre quais usuários está conectado como qual player.

### <a name="calls-and-results"></a>Chamadas e os resultados

O `SignInManager` tem três funções de rotina conjunta async `SignInPlayer(int playerNumber)`, `SignOutPlayer(int playerNumber)`, e `SwitchUser(int playerNumber)`, esse evento de gatilho de funções para coletar os resultados da chamada e aja de forma adequada. Você pode adicionar funções correspondentes ao seu script e atribuí-los para o `SignInManager.Instance`da lista de retorno de chamada. As funções de evento são `OnPlayerSignIn(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnPlayerSignOut(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnAnyPlayerSignIn(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, e `OnAnyPlayerSignOut(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`. Cada um em das funções de evento escuta o evento descrito em seu nome. Você pode adicionar suas próprias funções à lista de retorno de chamada do gerente em seu título `Start()` função com o código a seguir.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

void Start () {
    try
    {
        SignInManager.Instance.OnPlayerSignOut(this.playerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.OnPlayerSignIn(this.playerNumber, this.OnPlayerSignIn);
    }
    catch (Exception ex)
    {
        Debug.LogWarning(ex.Message);
    }

}
```

Este trecho de código adiciona ouvintes de entrada e saídas para o player associado playerNumber deste GameObject. Este GameObject `OnPlayerSignIn` função será chamada quando o `SignInManager` detecta uma tentativa de logon foi concluída e seu `OnPlayerSignOut` função será chamada quando o SignInManager detecta uma saída. As funções de evento no seu GameObject devem ter um tipo de retorno e parâmetros para corresponder ao tipo de função chamado pela SignInManager. Tanto a `OnPlayerSignIn` e `OnPlayerSignOut` são funções void que precisam de uma `XboxLiveUser`, `XboxLiveAuthStatus`e uma cadeia de caracteres como seus parâmetros. O shell de suas funções pode parecer com o seguinte:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}
```

Em ambas as funções, verifique as `XboxLiveAuthStatus` para certificar-se de que sua chamada para o `SignInManager.Instance` foi bem-sucedida. Em uma chamada bem-sucedida a `XboxLiveUser` será o `XboxLiveUser`, que foi assinado no nosso horizontalmente em `SignInManager`. Quando a chamada for malsucedida o `errorMessage` cadeia de caracteres conterá detalhes sobre o motivo da falha.

Adicionar algumas linhas de código para verificar se há uma chamada bem-sucedida resultaria em código semelhante ao seguinte:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if(authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = xboxLiveUserParam; //store the xboxLiveUser SignedIn
        this.signedIn = true;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage); //Log the error message in case of unsuccessful call. 
        }
    }
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if (authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = null;
        this.signedIn = false;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage);
        }
    }
}
```

Chamando entrar e capturar o evento resultante para o resultado, você pode manipular a entrada e saída para seu título.

## <a name="get-signed-in-player-information"></a>Façam logon no Player de informações

Além de entrar jogadores no serviço de SignInManager controla de todos os usuários conectados. No PC, isso será limitado a um único conectado no player, e o Xbox é limitado a 16. Você pode verificar como próximo do limite que você está comparando o resultado de `SignInManager.Instance.GetCurrentNumberOfPlayers()` como o resultado da `SignInManager.Instance.GetMaximumNumberOfPlayers()`. O SignInManager tem um dicionário de jogadores assinado indexados por esse jogador *playerNumber*. Você pode usar isso para recuperar algumas informações básicas sobre o player acessível de seus respectivos `XboxLiveUser`.

```csharp
if (SignInManager.Instance.GetPlayer(this.playerNumber).IsSignedIn) // If there is a player signed in for this gameObjects player number
            {
                this.displayedGamertag = SignInManager.Instance.GetPlayer(this.playerNumber).Gamertag; // Set that users gamertag to the gamertag displayed
            }
```

Esse pequeno trecho de código verifica para ver se há um player conectado ao slot de número de player para este GameObject e, em seguida, armazena gamertag que os usuários a ser exibido se estão conectados. Enquanto o `XboxLiveUser` contém assinado em usuários gamertag e ID de usuário do Xbox (xuid), você precisará chamar outros serviços, como o `SocialManager` para acessar as informações como gamerpic e jogos.

## <a name="destroying-your-sign-in-gameobject"></a>Destruindo o GameObject entrar

Ao destruir um objeto do jogo que usa um dos gerenciadores de plug-in de Xbox Live, como o `SignInManager` ou o `SocialManager`, normalmente ao carregar uma nova cena, é importante remover qualquer adicionadas à lista de ouvintes de eventos para o Gerenciador de funções. No código de exemplo para este artigo é necessário remover as funções que adicionamos para os ouvintes de evento para entrada e saída. Removeríamos essas funções a partir o `SignInManager` no `OnDestroy()` função da nossa GameObject.

```csharp
private void OnDestroy()
{
    if (SignInManager.Instance != null)
    {
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignIn);
    }
```

Esse código removerá as funções de retorno de chamada de entrada e saída para o player associado a este GameObject.

## <a name="testing-you-code-in-visual-studio"></a>Testando o código no Visual Studio

Além de [as etapas necessárias para criar seu jogo no Visual Studio](configure-xbox-live-in-unity.md#build-and-test-the-project), listado na [configurar o título do Xbox Live para Unity](configure-xbox-live-in-unity.md) do artigo, há uma etapa adicional necessária para testar seu jogo corretamente no Visual Studio. Você precisará atualizar uma propriedade do arquivo appxmanifest. Para fazer isso:

1. Pesquisar Gerenciador de soluções para o arquivo appxmanifest
2. Clique com botão direito no arquivo e escolha View Code
3. Sob o `<Properties><\/Properties>` seção, adicione a seguinte linha: ' < uap:SupportedUsers > várias <\/uap:SupportedUsers >.
4. Implante o jogo para o seu Xbox iniciando um build de depuração remoto do Visual Studio. Você pode encontrar instruções para configurar o título em um Xbox na [configurar seu UWP no ambiente de desenvolvimento do Xbox](../../xbox-apps/development-environment-setup.md) artigo.

> [!NOTE]
> A parte da configuração alterada pode parecer que está sendo habilitado vários jogadores, mas ainda é necessário executar seu jogo em cenários de único player.

## <a name="policies-and-limitations"></a>As políticas e limitações

Há algumas políticas de entrada e as limitações do título que você talvez queira considerar ao desenvolver a sua experiência de entrada.

- Depois de entrar inicial do seu título, você deve manter pelo menos um player conectado. O `SignInManager` irá gerar um erro e falha da chamada, se você tentar sair o último usuário conectado. Também é importante observar que não é possível chamar `SignInManager.Instance.SwitchUser(int playerNumber)`, no último assinado no player conforme ele tenta sair o player antes de entrar em um novo player.

- PC pode apenas entrar um usuário por vez, Console possa entrar até 16 jogadores ao mesmo tempo.

- O título não tem permissão para desconectar um player do sistema operacional na verdade, devido a essa saída pode não funcionar conforme o esperado. O SignInManager pode desconectar um usuário de entrada e saída em que o título está preocupado, mas não puder sair qualquer pessoa da máquina em que o título é implantado.

- Vários entrada do usuário só está disponível em um Console Xbox.

- Contas de convidado não estão disponíveis para títulos de programa de criadores do Xbox Live.