---
title: Unity para UWP com back-end IL2CPP
description: Adicionar suporte a Xbox Live para o Unity para UWP com o back-end script IL2CPP para ID@Xbox e gerenciados de parceiros
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, o Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622911"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Adicionar suporte a Xbox Live para o Unity para UWP com o back-end script IL2CPP para ID@Xbox e gerenciados de parceiros

## <a name="overview"></a>Visão geral

Suporte de tempo de execução do Windows para IL2CPP no Unity

Com o lançamento do Unity 5.6f3 o mecanismo incluiu um novo recurso que permite aos desenvolvedores usar componentes de tempo de execução do Windows (WinRT) diretamente no script, incluindo-os diretamente no projeto do jogo. Até que os 5.6 desenvolvedores precisou de um plug-in ou dll para oferecer suporte a qualquer recurso de plataforma (incluindo Xbox Live SDK) do script de jogo em UWP. Essa nova camada de projeção elimina a necessidade de plug-in e introduz um fluxo de trabalho novo e simplificado compatível somente com jogos que escolher IL2CPP script back-end.

Para obter mais informações sobre como começar, consulte a documentação do Unity: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Etapas

**1) instalar o Unity**

Instalar o Unity 5.6 ou posterior e verifique se você tem o **back-end o script do Windows Store Il2CPP** selecionado durante a instalação

**2) instalar ferramentas do Visual Studio para Unity versão 3.1 e acima para o IntelliSense dá suporte ao usar WinMDs** para o Visual Studio 2015, isso pode ser encontrado em https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Para o Visual Studio 2017, é possível adicionar o componente dentro do instalador do Visual Studio 2017.

**3) abra um projeto novo ou existente do Unity**

**4) alternar a plataforma para a plataforma Universal do Windows no menu de configurações de Build do Unity**

**5) habilitar IL2CPP script back-end nas configurações do player do Unity e definir a compatibilidade de API para o .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**6) importar a versão mais recente do pacote de ativos do Xbox Live WinRT Unity** isso pode ser encontrado em https://github.com/Microsoft/xbox-live-api/releases

**7) adicionar e anexar um novo C\# script a um objeto do Unity.**

Por exemplo, clique em um objeto do Unity como a câmera"Main" e clique em "Adicionar componente" \| "Novo Script de" \| C\# Script \| e nomeie-a como "XboxLiveScript". Qualquer objeto do jogo fará.

**8) abra o script no Visual Studio (com o VSTU 3.1 + instalado)**

Você observará dois projetos, abra o seu script jogo XboxLiveTest.cs no projeto "Player" gerado pelo VSTU

![](../images/unity/unity-il2cpp-2.png)

Isso é um projeto especial gerado para UWP e inclui referências para os arquivos de winmd que você tenha colocado no seus ativos.
Ele também será definir o "#if ENABLE_WINMD_SUPPORT" definir para você, portanto, IntelliSense e realce de sintaxe funcionará corretamente.

**9) adicione o seguinte código do Xbox Live para o arquivo de origem XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new   Microsoft.Xbox.Services.System.XboxLiveUser();

    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
#endif
    string debugText = "";
    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;
        UIDispatcher = cw.Dispatcher;
        SignIn();
#endif
    }
    // Update is called once per frame
    void Update()
    {
    }
    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }
#if ENABLE_WINMD_SUPPORT
    async void SignIn()
    {
        Microsoft.Xbox.Services.System.SignInResult result = await m_user.SignInAsync(UIDispatcher);
        if (result.Status == Microsoft.Xbox.Services.System.SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }

    }
#endif
}

```

**10) Verifique se você tem a capacidade de 'InternetClient' selecionada nas configurações de publicação encontradas nas configurações do player**

![](../images/unity/unity-il2cpp-3.png)

**11) compila o projeto no Unity.**

1.  Ir para arquivo \| configurações de compilação, clique em **plataforma Universal do Windows** e verifique se você clicar em **alternar plataforma**

2.  Clique em "Adicionar aberta nos bastidores" para adicionar a cena atual para a compilação

3.  Na caixa de combinação do SDK, escolha "Universal 10"

4.  Na caixa de combinação tipo de compilação da UWP, escolha "D3D", mas "XAML" também funcionará se você preferir.

5.  Clique em "Build" para o Unity gerar o projeto do Visual Studio de UWP que encapsula o jogo Unity em um aplicativo UWP. Quando for solicitada uma localização, crie uma nova pasta para evitar confusão, pois muitos dos novos arquivos serão criados. É recomendável chamar a pasta "Build" e, em seguida, selecione a pasta

**12) adicione a configuração do Xbox Live ao seu projeto**

Adicione o arquivo xboxservices.config:

![](../images/unity/unity-il2cpp-4.png)

Execute a página de doc chamada [adicionando o Xbox Live para um projeto novo ou existente do UWP](get-started-with-visual-studio-and-uwp.md)

> [!NOTE]
> Todos os valores dentro de xboxservices.config diferenciam maiusculas de minúsculas.

**13) compilar e executar o aplicativo UWP do Visual Studio**

Isso iniciará o aplicativo como um aplicativo UWP normal e permitir chamadas Xbox Live operar, pois elas exigem um contêiner de aplicativo UWP à função.

**14) recompile se você fizer alterações a qualquer coisa no Unity**  
Se você alterar qualquer coisa no Unity, você deve recompilar o projeto UWP.

Observe que o Unity substituirá o arquivo pfx ao recompilar que fará com que o Xbox Live entrar falhe, portanto, você deve atualizá-lo dentro do projeto do Unity para evitar esse problema.

Para fazer isso, vá para arquivo \| configurações de compilação, clique em "Configurações de Build" no **plataforma Universal do Windows** player e clique no botão PFX para substituir o PFX do arquivo com um obtido acima. Como alternativa, você pode excluir o arquivo PFX cada vez que você recompilar o projeto a partir do Unity.

## <a name="troubleshooting-common-issues"></a>Solução de problemas comuns

**1)** Unity se tiver que um script associado pode não ser carregado, certifique-se de que você etapa 3 para arrastar o WinMD para o painel de ativos de projeto do Unity

**2)** se o aplicativo falhar imediatamente na inicialização ou quando tentar executar essa linha de código:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Certifique-se de que você adicionou um arquivo de texto xboxservices.config ao projeto e em suas propriedades, o conjunto a "ação de compilação" como "Content" e o conjunto de "Copiar para diretório de saída" para "Copiar sempre".
Além disso, verifique se que ele contém JSON formatação correta com o TitleId no formato decimal, tais como:

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** se o aplicativo é iniciado, mas não consegue entrar, em seguida, verifique o seguinte:

a) sua máquina é definida como a sua área de segurança do desenvolvedor.  Use o script de SwitchSandbox.cmd na pasta \Tools do Xbox Live SDK para fazer isso.

b) você está entrando com uma conta do Xbox Live que tenha acesso à área restrita de desenvolvedor.  Contas normais de varejo do Xbox Live não tem acesso.  Você pode usar XDP ou Partner Center para criar contas de teste.

c) sua package.appxmanfiest em seu aplicativo UWP é definido como a identidade correta.  Você pode editá-la manualmente, mas a maneira mais fácil de corrigir esse problema é clique com botão direito no projeto no Visual Studio e escolha "Store" \| "Associar aplicativo com o Store".

d) o arquivo. pfx de estoque fornecido pelo Unity não terá a identidade correta para ou excluí-lo a partir do disco e remova a linha em. csproj que faz referência a ele, ou à direita, clique no projeto no Visual Studio e escolha "Store" \| "associar aplicativo com a Store "qual colocará para baixo de um arquivo. pfx apropriado.  Certifique-se, em seguida, para voltar para o Unity, clique em "Configurações de Build" na **plataforma Universal do Windows** player e clique no botão PFX para substituir o arquivo. pfx com aquela que você obteve na ação de "Associar aplicativo com o Store" do Visual Studio.
