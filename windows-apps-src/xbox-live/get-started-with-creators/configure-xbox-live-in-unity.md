---
title: Configurar o Xbox Live no Unity
description: Saiba como usar o plug-in do Xbox Live Unity para configurar o Xbox Live em seu jogo do Unity.
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, Unity, configurar
localizationpriority: medium
ms.openlocfilehash: d464fc54d322db9da91870bd3ca7cbc29957b379
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596721"
---
# <a name="configure-xbox-live-in-unity"></a>Configurar o Xbox Live no Unity

> [!NOTE]
> O plug-in do Xbox Live Unity é recomendado somente para [programa de criadores do Xbox Live](../developer-program-overview.md) membros, pois, atualmente, não há suporte para realizações ou com vários participantes.

Com o [Xbox Live Unity plug-in do](https://github.com/Microsoft/xbox-live-unity-plugin), adicionando suporte a Xbox Live para um jogo em Unity é fácil, dando a você mais tempo para se concentrar em usando o Xbox Live de maneiras que melhor se adequar às seu título.

Este tópico percorrer o processo de configurar o plug-in Xbox Live no Unity.

## <a name="prerequisites"></a>Pré-requisitos

Antes de poder usar o Xbox Live no Unity, você precisará do seguinte:

1. Uma  **[conta do Xbox Live](https://support.xbox.com/browse/my-account/manage-account/Create%20account)**.
1. Registro na  **[programa de desenvolvedores do Partner Center](https://developer.microsoft.com/store/register)**.
2. **[Atualização de aniversário do Windows 10](https://microsoft.com/windows)**  ou posterior
3. **[Unity](https://store.unity.com/)**  versões **5.5.4p5** (ou mais recente), **2017.1p5** (ou mais recente), ou **2017.2.0f3** (ou mais recente) com **[Ferramentas do Microsoft Visual Studio para Unity](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)** e **.NET da Windows Store back-end de script**.
4. **[Visual Studio 2015](https://www.visualstudio.com/)**  ou **[Visual Studio 2017 15.3.3](https://www.visualstudio.com/)** (ou mais recente) com o **ferramentas de desenvolvimento de aplicativo Universal do Windows**.
5. **[As extensões da plataforma SDK do Xbox Live](https://aka.ms/xblextsdk)**.


> [!NOTE]
> Se você quiser usar o back-end de script de IL2CPP com o Xbox Live, você precisará 2017.2.0p2 Unity ou mais recente e a versão de plug-in do Xbox Live Unity "versão de Preview 1802" ou superior.


## <a name="import-the-unity-plugin"></a>Importar o plug-in do Unity

Para importar o plug-in no seu projeto do Unity novo ou existente, siga estas etapas:

1. Navegue até a guia de versão do Plugin do Xbox Live Unity na [ https://github.com/Microsoft/xbox-live-unity-plugin/releases ](https://github.com/Microsoft/xbox-live-unity-plugin/releases).
2. Baixe **XboxLive.unitypackage**.
3. No Unity, clique em **ativos** > **Importar pacote** > **pacote personalizado** e navegue até **XboxLive.unitypackage**.

![Importação bem-sucedida](../images/unity/get-started-with-creators/importXBL_Small.gif)

### <a name="optional-configure-the-plugin-to-work-in-the-unity-editor-net-46-or-il2cpp-only"></a>(Opcional) Configurar o plug-in para trabalhar no Editor do Unity (.NET 4.6 ou IL2CPP apenas)

> [!NOTE]
> O suporte para alterar a versão de tempo de execução de scripts no Unity requer a versão de Plugin do Xbox Live Unity "versão 1711" ou superior para o .NET 4.6 e a versão "versão de Preview 1802" ou superior para IL2CPP.

Há três configurações que podem ser configuradas no Unity para definir como seu código seja compilado:

1. O **back-end de script** é o compilador que é usado. O Unity suporta dois diferentes scripts back-ends para plataforma Universal do Windows: .NET e IL2CPP.
2. O **versão de tempo de execução de scripts** é a versão do tempo de execução script que executa o Editor do Unity.
3. O **nível de compatibilidade de API** é a superfície de API que você compilará seu jogo em relação a.

A tabela a seguir mostra a matriz de suporte de script atual para o plug-in Xbox Live Unity:

| Script de back-end     | Versão de tempo de execução de script | Com suporte     | Versão do Unity mínima necessária |
|-------------------    |-------------------        |-----------    |------------------------------- |
| IL2CPP                | O .NET 3.5 equivalente       | Não            | N/D                            |
| Il2CPP                | O .NET 4.6 equivalente       | Sim           | 2017.2.0p2                     |
| .NET                  | O .NET 3.5 equivalente       | Sim           | Mesmo que os pré-requisitos          |
| .NET                  | O .NET 4.6 equivalente       | Sim           | Mesmo que os pré-requisitos          |

Adicionamos suporte de tempo de execução de script adicional para o Xbox Live Unity plug-in, começando com a versão "versão 1711". Por padrão, o plug-in é configurado para executar no editor do Unity com o .NET de back-end de script e scripts de versão de tempo de execução do .NET 3.5. Se seu projeto estiver usando a versão de tempo de execução de script do .NET 4.6, você precisará configurar o plug-in para funcionar corretamente no editor:

1. No Explorador de projeto do Unity, navegue até **Xbox Live\Libs\UnityEditor\NET46** e selecione todas as DLLs na pasta.
2. Na janela Inspetor, verifique **Editor** sob **plataformas incluem**.
3. No Explorador de projeto do Unity, navegue até **Xbox Live\Libs\UnityEditor\NET35** e selecione todas as DLLs na pasta.
4. Na janela Inspetor, desmarque a opção **Editor** sob **plataformas incluem**.

![alterar o tempo de execução do script](../images/unity/get-started-with-creators/changeScriptingRuntime.gif)

> [!IMPORTANT]
> Essas etapas precisará ser revertida se você alterar a versão de tempo de execução de scripts em seu projeto de volta para 3.5.

## <a name="set-visual-studio-as-the-ide-in-unity"></a>Definir o Visual Studio como o IDE no Unity

Visual Studio é necessário para criar uma [plataforma Universal do Windows (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) jogo. Você pode definir seu IDE no Unity para Visual Studio entrando no **edite** > **preferências** > **ferramentas externas** e definindo o **Editor de Script externo** para o Visual Studio.

![definir a ferramenta externa do VS](../images/unity/get-started-with-creators/setVSExternalTool_Small.gif)

## <a name="unity-plugin-file-structure"></a>Estrutura de arquivo de plug-in do Unity

Estrutura do arquivo do Unity do plug-in é dividida nas seguintes partes:

* __Xbox Live__ contém os ativos de plug-in real que são incluídos no publicado **unitypackage**.
    * __Editor__ contém scripts que fornecem a configuração básica da interface do usuário do Unity e processa os projetos durante a compilação.
    * __Exemplos__ contém um conjunto de arquivos de cena simples que mostram como usar os pré-fabricados vários e conectá-los.
    * __Imagens__ é um pequeno conjunto de imagens que são usadas pelos pré-fabricados.
    * __As bibliotecas__ é onde as bibliotecas do Xbox Live são armazenadas.
    * __Pré-fabricados__ contém vários [pré-fabricado Unity](https://docs.unity3d.com/Manual/Prefabs.html) objetos que implementam a funcionalidade do Xbox Live.
    * __Scripts__ contém todos os arquivos de código que chama as APIs do Xbox Live a partir de pré-fabricados. Isso é um ótimo lugar para procurar exemplos sobre como chamar corretamente as APIs do Xbox Live.
    * __Tools\AssociationWizard__ contém o Xbox Live associação assistente, usado para efetuar pull de configuração de aplicativo do [Partner Center](https://developer.microsoft.com/windows) para uso dentro do Unity.

## <a name="enable-xbox-live"></a>Habilitar o Xbox Live

Para o título interagir com o Xbox Live, você precisará configurar a configuração inicial do Xbox Live. Você pode ser feito com facilidade e dentro do Unity usando o Assistente de associação do Xbox Live:

1. No **Xbox Live** menu, selecione **configuração**.
2. No **Xbox Live** janela, selecione **Xbox Live associação Assistente executar**.
3. No **associar seu título com o Windows Store** caixa de diálogo, clique em **próxima**e entre com sua conta no Partner Center.
4. Selecione o aplicativo que você deseja associar com este projeto e, em seguida, clique em **selecionar**. Se você não estiver visível lá, tente clicar **Refresh**. Como alternativa, você pode criar um novo aplicativo reservando um nome e clicando em **reserva**.
5. Você será solicitado a habilitar se você ainda não tiver o Xbox Live. Clique em **habilitar** para habilitar o Xbox Live em seu título.
6. Clique em **concluir** para salvar sua configuração.

Para chamar serviços do Xbox Live, sua área de trabalho deve estar no modo de desenvolvedor e definidos para a mesma área de segurança, pois seu título na configuração do Xbox Live. Você pode verificar tanto examinando os **Xbox Live configuração** janela no Unity:

1. **Configuração do modo de desenvolvedor** deve dizer **Enabled**. Se ele diz **desabilitados**, clique em **alterne para modo de desenvolvedor**.
2. **Configuração do título** > **Sandbox** deve ter a mesma ID **configuração do modo de desenvolvedor** > **modo de desenvolvedor**.

![XBL habilitado](../images/unity/unity-xbl-enabled.png)

Ver [áreas restritas do Xbox Live](../xbox-live-sandboxes.md) para obter informações sobre as áreas restritas.

## <a name="build-and-test-the-project"></a>Compilar e testar o projeto

Ao executar seu título no editor, você verá dados falsos quando você tenta usar a funcionalidade do Xbox Live. Por exemplo, se você [adicionar recursos de entrada](unity-prefabs-and-sign-in.md) à sua cena e tente entrar, você verá **usuário forjar** aparecem como seu nome de perfil, com um ícone de espaço reservado. Para entrar com um perfil real e testar a funcionalidade do Xbox Live em seu título, você precisará criar uma solução UWP e executá-lo no Visual Studio.  Você pode compilar o projeto UWP no Unity, seguindo estas etapas:

1. Abra o **configurações de Build** janela selecionando **arquivo** > **configurações de Build**.
2. Adicione todos os bastidores que você deseja incluir na compilação sob o **cenas em compilação** seção.
3. Alterne para o **plataforma Universal do Windows** selecionando **plataforma Universal do Windows** sob **plataforma** e clicando em **alternar plataforma**.
4. Definir **SDK** à **10.0.15063.0** ou maior.
5. Para habilitar a verificação de depuração de scripts **Unity C# projetos**.
6. Clique em **Build** e especifique o local do projeto.

![configurações de build](../images/unity/build_settings.JPG)

Depois que a compilação for concluída, será Unity tiver gerado um novo arquivo de solução UWP que você precisará executar no Visual Studio:

1. Na pasta que você especificou, abra  **&lt;ProjectName&gt;. sln** no Visual Studio.
2. Na barra de ferramentas na parte superior, selecione **x64** e implantar o **Máquina Local**.

Se você habilitou **depuração de script** quando você compilou a solução UWP do Unity, em seguida, seus scripts estará localizados na **(Windows Universal) do Assembly-CSharp** projeto.

![Usuário fictício: 123456789](../images/unity/get-started-with-creators/visualStudio.PNG)

> [!NOTE]
> Antes de usar sua compilação do Visual Studio para testar seu jogo com dados reais, siga [esta lista de verificação](test-visual-studio-build.md) para ajudar a garantir que o título será capaz de acessar o serviço Xbox Live.

> [!IMPORTANT]
> Como de pode 2018 agora é necessário que você faça uma atualização para o arquivo appxmanifest para testar o título da UWP corretamente no Visual Studio. Para fazer isso:
>
> 1. Pesquisar Gerenciador de soluções para o arquivo appxmanifest
> 2. Clique com botão direito no arquivo e escolha View Code.  
    Se a opção de modo de exibição de código não está disponível ou o arquivo Package. appxmanifest não tem uma extensão. Você precisará abrir o arquivo como um xml e continue com as etapas restantes.
> 3. Sob o `<Properties></Properties>` seção, adicione a seguinte linha: `<uap:SupportedUsers>multiple</uap:SupportedUsers>`.
> 4. Implante o jogo para o seu Xbox iniciando um build de depuração remoto do Visual Studio. Você pode encontrar instruções para configurar o título em um Xbox na [configurar seu UWP no ambiente de desenvolvimento do Xbox](../../xbox-apps/development-environment-setup.md) artigo.
>
> A parte da configuração alterada pode parecer que está sendo habilitado vários jogadores, mas ainda é necessário executar seu jogo em cenários de único player.

## <a name="try-out-the-examples"></a>Experimente os exemplos

Você está pronto para começar a usar o Xbox Live em seu projeto do Unity! Tente abrir cenas na **Xbox Live/Examples** pasta para ver o plug-in em ação e para obter exemplos de como usar a funcionalidade por conta própria. Executar os exemplos no editor de fornecer dados falsos, porém, se você compilar o projeto no Visual Studio e [associar sua conta do Xbox Live a área restrita](authorize-xbox-live-accounts.md), você pode entrar com seu nome de jogador.

Experimente o **SignInAndProfile** cena para entrar em sua Account da Microsoft, o **placar de líderes** cena para criar um placar de líderes e o **UpdateStat** cena para exibindo e Atualizando estatísticas.

## <a name="see-also"></a>Consulte também

* [Entrar no Xbox Live no Unity](unity-prefabs-and-sign-in.md)
* [Autorizar contas do Xbox Live](authorize-xbox-live-accounts.md)
