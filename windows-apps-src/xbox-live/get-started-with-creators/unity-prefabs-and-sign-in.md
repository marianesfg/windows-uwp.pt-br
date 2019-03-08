---
title: XBL em pré-fabricados do Unity e entrar
description: Aborda os pré-fabricados sociais e exemplos de script para serviços sociais no Xbox Live
ms.date: 01/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity
ms.openlocfilehash: a893858dac11fa848c2601df2c1bd6292b72ac6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660031"
---
# <a name="unity-prefabs-and-scripted-sign-in"></a>Pré-fabricados do Unity e entrar com script

Este artigo o guiará pela adição de Xbox Live entrar para seus projetos do Unity. Há duas maneiras de conseguir entrar se você tiver baixado o [Xbox Live Unity plug-in do](https://github.com/Microsoft/xbox-live-unity-plugin). Você pode usar os pré-fabricados contidos o plug-in ou usar os scripts e bibliotecas incluídas para o script Xbox Live entrar em sua própria GameObjects personalizado.

> [!IMPORTANT]
> Este artigo se aplica a uma versão do plug-in antes de uma atualização feita em maio de 2018 (versão 1804). Se você instalou o Plugin do Xbox Live depois desse período, ou ainda não baixou-lo, você pode ter uma versão mais recente que apresente diferenças significativas como entrada é executada. Além disso, você descobrirá que as capturas de tela desse plug-in não correspondem da versão mais recente. Em vez disso, consulte a [pré-fabricado atualizado entrar para obter](playerauthentication-prefab-sign-in.md) , bem como [o artigo detalhando os métodos atualizados para o script entrar](sign-in-manager.md).

## <a name="before-you-begin"></a>Antes de começar

Antes de iniciar a adição de serviços do Xbox Live social ao jogo do Unity, há algumas etapas, que você precisará concluir antes de começar. Primeiro, certifique-se de que você tenha baixado e integrado a [plug-in do Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin). Em segundo lugar, você vai querer ter seu título reservado e publicado por meio de [Microsoft Development Center](https://developer.microsoft.com/en-us/games/uwp). Leia [criar um novo título do programa de criadores do Xbox Live](../get-started-with-creators/create-and-test-a-new-creators-title.md) para obter instruções sobre como publicar seu título.
Por fim, leia [configurar o Xbox Live no Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) para configurar corretamente o seu ambiente do Unity e configurar o título para usar os serviços do Xbox Live. Depois que seu projeto do Unity está configurado corretamente, é hora de aprender sobre as ferramentas que você pode usar em seu título Xbox Live habilitado, bem como as duas maneiras principais que você pode implementar o Xbox Live no Unity: pré-fabricados e scripts.

## <a name="prefabs"></a>Pré-fabricados

Unity tem um tipo de ativo pré-fabricado que permite que você armazene um GameObject completo com propriedades e os componentes. Pré-fabricado atua como um modelo do qual você pode criar novas instâncias de objetos na cena Unity.
[Saiba mais sobre a pré-fabricados pelo site do Unity](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage).

O plug-in do Xbox Live Unity fornece alguns pré-fabricados que você pode usar em seu projeto para utilizar recursos do Xbox Live. Os pré-fabricados descritos neste artigo permitirá que você entrar, [adicionar suporte a vários usuários](../get-started-with-creators/add-multi-user-support.md) a seu título ou exibir a lista de amigos de assinado no Xbox Live perfil. Você pode encontrar esses e outros pré-fabricados sob o **projeto** guia seguindo o caminho: **Ativos > Xbox Live > pré-fabricados**.

### <a name="the-userprofile-prefab"></a>Pré-fabricado UserProfile

Pré-fabricado social primeiro e mais importante é o **UserProfile** pré-fabricado. O **UserProfile** pré-fabricado tem tudo que é necessário para permitir que um sign-in do Xbox Live. Isso é muito importante, pois você deverá entrar um usuário antes de usar os serviços do Xbox Live. Pré-fabricado contém o botão de entrada e um GameObject para representar um registradas no player, seu nome de jogador, Gamerpic e jogos.

> [!NOTE]
> Para usar qualquer um de outros Xbox Live pré-fabricados, você deve incluir um **UserProfile** pré-fabricado ou manualmente invocar a API de entrada.

![Pré-fabricado UserProfile em ativos e hierarquia](../images/unity/unity-userprofile-views.png)

Se você expandir o **UserProfile** pré-fabricado no **Project** painel ou no **hierarquia** depois que ela foi adicionada a uma cena, você verá que o **UserProfile**  pré-fabricado contém dois GameObjects dentro dele. O primeiro objeto é o **SignInPanel** que contém a experiência de botão de entrada. O segundo objeto se a **ProfileInfo** que conterá as informações sobre o usuário quando eles entrarem. O **UserProfile** pré-fabricado é o que você usará para representar as informações de qualquer Xbox Live do usuário conectado localmente para seu título.

### <a name="the-xboxliveuser-prefab"></a>Pré-fabricado XboxLiveUser

O **UserProfile** pré-fabricado usa um pré-fabricado social em segundo lugar no seu código que chamou o **XboxLiveUser**. Use deste pré-fabricado não é imediatamente evidente pois ele não precisa ser adicionado para a hierarquia da cena, como ele pode simplesmente ser instanciado no código. O **XboxLiveUser** não tem representação visual, ele simplesmente contém os detalhes que pertencem ao usuário Xbox Live. Você precisará de uma instância das **XboxLiveUser** para cada instância das **UserProfile**. Isso é importante quando [adicionando suporte a vários usuários](../get-started-with-creators/add-multi-user-support.md) ao seu título. Além de manter informações sobre o usuário depois de entrar neste pré-fabricado também é um wrapper para o código usado para entrar um usuário do Xbox Live.

## <a name="sign-in-with-the-userprofile-prefab"></a>Entrar com pré-fabricado UserProfile

Os plug-in de Xbox Live Unity pré-fabricados existem para tornar certas tarefas de desenvolvimento muito mais fácil. Para habilitar o Xbox Live entrar para seu projeto do Unity simplesmente será preciso usar o **UserProfile** e **XboxLiveServices** pré-fabricados junto com um Unity **EventSystem**.

Arraste o **UserProfile** pré-fabricado em uma cena. O ideal é que o **UserProfile** deve ser colocado na tela inicial do menu do seu projeto.

![Arraste userprofile à hierarquia](../images/unity/drag-userprofile.gif)

Além de **UserProfile** pré-fabricado, você também precisará ter certeza de que o **XboxLiveServices** pré-fabricado está presente pelo menos a primeira cena do seu projeto.
O **XboxLiveServices** pré-fabricado permite que você alterne se determinados pré-fabricados registrará em log informações de depuração. Isso é útil para verificar no comportamento de pré-fabricado.

![verificar se há xboxliveservices pré-fabricado](../images/unity/check-for-xboxliveservices.gif)

Por fim, o **UserProfile** também requer um **EventSystem** seja executado corretamente. Isso pode ser adicionado clicando duas vezes em cena a entrar, em seguida, clicando em **GameObject--> interface do usuário--> EventSystem**.

![Adicionar o sistema de eventos](../images/unity/add_event_system.gif)

Se você inserir o modo de reprodução, o serviço irá entrar em um usuário automaticamente. No Unity, o Xbox Live SDK simula chamadas para o serviço Xbox Live e envia dados de volta falsos para trabalhar com. Para exibir os dados dinâmicos, você precisará compilar o projeto como um aplicativo UWP e executá-lo do Visual Studio. Ver [configurar o Xbox Live no Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) para obter mais informações. Quando você insere o modo do jogo no Unity, pré-fabricado será preenchido com os dados fictícios, que simulará informações como o nome de jogador, Gamerpic e jogos de um player. Essas são as informações que devem ser exibidas, o **UserProfile** pré-fabricado.

Uma entrada bem-sucedida se parecerá com o seguinte: ![play userprofile bem-sucedida](../images/unity/correct-user-profile-play.gif)

Se você não tiver configurado seu projeto para se conectar ao Xbox Live inserir corretamente o modo de reprodução desabilitar o botão entrar e exibir uma mensagem de erro.

O exemplo a seguir é um exemplo de um sign-in com falha devido a uma configuração de aplicativo do Xbox Live incorreta.
![Falha na reprodução do perfil de usuário](../images/unity/flawed-user-profile-play.gif)

## <a name="scripting-sign-in"></a>Entrada de script

Agora que você sabe como usar o **UserProfile** pré-fabricado seria melhor examinar o script subjacente que rege a funcionalidade do pré-fabricado. Se você examinar a **UserProfile** na **Inspetor** você verá que ele tem um **UserProfile.cs** script anexado a ele. Esse script tem tudo o que você precisa entrar em um usuário e carregar as informações de perfil que você deseja exibir na entrada. No entanto, em vez de examinar pré-fabricado inteiro (que pode ser atualizado ao longo do tempo), vamos examinar algumas linhas de exemplo de código para entender o que é necessário para entrar e um usuário do Xbox Live.

### <a name="the-xboxliveuser-class"></a>A classe XboxLiveUser

As chamadas necessárias para um usuário de entrada são encapsuladas no `XboxLiveUserInfo` classe. No **UserProfile.cs** script, você verá que há uma instância das `XboxLiveUserInfo` classe chamada `XboxLiveUser`. Nós usaremos o mesmo nome de variável em nosso código de exemplo. O `XboxLiveUserInfo` classe contém uma instância das `XboxLiveUser` classe chamada `User` como uma das suas variáveis de membro. O `XboxLiveUser` classe contém as funções de deve entrar a `XboxLiveUser`. Você usará a instância das `XboxLiveUser` classe `User` entrar usuários, bem como para obter informações que descrevem-los como seu nome de jogador, Gamerpic e jogos. Para fazer isso, você deve inicializar uma instância das `XboxLiveUserInfo` de classe e usar resultante `XboxLiveUserInfo.User` a chamada de entrada.

### <a name="initialize-the-xboxliveuser"></a>Inicializar o XboxLiveUser

Inicializar o usuário do Xbox Live é a primeira etapa antes de entrar, na verdade, Xbox Live. Isso é feito muito simples no código, usando o `XboxLiveUserInfo.Initialize()` função.
Em nosso código de exemplo, podemos usar o `XboxLiveUser` variável de membro como nosso `XboxLiveUserInfo` da instância e inicializar que será usada para entrar.

```csharp
    void ButtonClickTask()
    {
        this.StartCoroutine(this.InitializeXboxLiveUser());
    }

    public IEnumerator InitializeXboxLiveUserAndCallSignIn()
    {
        // Disable the sign-in button
        SignInButton.interactable = false;

        this.XboxLiveUser.Initialize();

        //Wait until the Xbox User has been initialized to call SignInAsync()
        yield return new WaitUntil(() => this.XboxLiveUser != null && this.XboxLiveUser.User != null);
        this.StartCoroutine(this.SignInAsync());
    }
```

Observando esse código de exemplo, você verá que o `XboxLiveUserInfo.Initialize()` função é chamada em resposta a um clique de botão. Completo **UserProfile.cs** script pré-fabricado tem código que permite a entrada automática no onde `XboxLiveUserInfo.Initialize()` for chamado sem interação.
O `XboxLiveUserInfo.Initialize()` função cria um novo `XboxLiveUserInfo.User` que nos permitirá chamar as funções de contido no `XboxLiveUserInfo.User` classe.

### <a name="call-sign-in"></a>Entrada de chamada

Depois que o XboxLiveUser tenha sido inicializado, é hora à chamada de entrada. Na **UserProfile.cs** entrar é chamado de `SignInAsync()` função de UserProfile.cs. No código de exemplo anterior é simplesmente esperar para que o `XboxLiveUser` a ser inicializada antes de chamar o `SignInAsync()` função.

> [!NOTE]
> É necessário aguardar a `XboxLiveUser` a ser inicializada antes de chamar entrar porque o `XboxLiveUser` contém o `XboxLiveUser.User` propriedade que é usada para chamada de entrada.

Na **UserProfile.cs** o `SignInAsync()` função contém duas entrar funções que podem ser usadas para a entrada do usuário. `XboxLiveUser.User.SignInSilentlyAsync()` e `XboxLiveUser.User.SignInAsync()` essas são as funções que o usuário entrar. O `SignInAsync()` função é um bom exemplo de como usar essas funções adequadamente. O exemplo de código a seguir mostra um método apropriado para chamar as duas funções de entrada:

```csharp
SignInStatus signInStatus;
TaskYieldInstruction<SignInResult> signInSilentlyTask = this.XboxLiveUser.User.SignInSilentlyAsync().AsCoroutine();
yield return signInSilentlyTask;

signInStatus = signInSilentlyTask.Result.Status;
if (signInSilentlyTask.Result.Status != SignInStatus.Success)
{
    TaskYieldInstruction<SignInResult> signInTask = this.XboxLiveUser.User.SignInAsync().AsCoroutine();
    yield return signInTask;

    signInStatus = signInTask.Result.Status;
}
```

Neste exemplo, os resultados das chamadas de entrada são armazenados na variável `signInStatus`. Isso nos permite verificar se a entrada foi bem-sucedida e aja de forma adequada. Neste exemplo, que a função primeiro tenta entrar no modo silencioso, em seguida, se a entrada sem confirmação falhar a função chama o sinal normal na função. Uma vez que uma chamada bem-sucedida para uma das funções de entrada que será ter entrado o usuário. Agora você pode usar o `XboxLiveUser.User` para obter e exibir detalhes sobre o usuário conectado. Dê uma olhada a `LoadProfileInfo()` funcionar **UserProfile.cs** para obter um exemplo de como usar o `XboxLiveUser.User` para exibir informações sobre um usuário conectado.

## <a name="build-and-test-sign-in"></a>Compilar e testar o logon

Ao executar seu título no editor, você verá dados falsos quando você tenta usar a funcionalidade do Xbox Live. Para entrar com um perfil real e testar a funcionalidade do Xbox Live em seu título, você precisará criar uma solução UWP e executá-lo no Visual Studio.  Você pode compilar o projeto UWP no Unity, seguindo estas etapas:

1. Abra o **configurações de Build** janela selecionando **arquivo** > **configurações de Build**.
2. Adicione todos os bastidores que você deseja incluir na compilação sob o **cenas em compilação** seção.
3. Alterne para o **plataforma Universal do Windows** selecionando **plataforma Universal do Windows** sob **plataforma** e clicando em **alternar plataforma**.
4. Definir **SDK** à **10.0.15063.0** ou maior.
5. Para habilitar a verificação de depuração de scripts **Unity C# projetos**.
6. Clique em **Build** e especifique o local do projeto.

Depois que a compilação for concluída, será Unity tiver gerado um novo arquivo de solução UWP que você precisará executar no Visual Studio:

1. Na pasta que você especificou, abra  **&lt;ProjectName&gt;. sln** no Visual Studio.
2. Na barra de ferramentas na parte superior, selecione **x64** e implantar o **Máquina Local**.

Se você habilitou **depuração de script** quando você compilou a solução UWP do Unity, em seguida, seus scripts estará localizados na **(Windows Universal) do Assembly-CSharp** projeto.

> [!NOTE]
> Antes de usar sua compilação do Visual Studio para testar seu jogo com dados reais, siga [esta lista de verificação](test-visual-studio-build.md) para ajudar a garantir que o título será capaz de acessar o serviço Xbox Live.

## <a name="troubleshooting"></a>Solução de problemas

Se você estiver tendo problemas ao entrar no Xbox Live tente ler [solução de problemas de Xbox Live entrar](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md).
