---
title: Adicionar suporte a vários usuários ao seu jogo do Unity
description: Adicionar suporte a vários usuários ao seu jogo do Unity usando o plug-in Xbox Live Unity
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity, multiusuário
ms.localizationpriority: medium
ms.openlocfilehash: 483a0e966be756de483bf7e2ab8458647397687b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613731"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>Adicionar suporte a vários usuários ao seu jogo do Unity
> [!IMPORTANT]
> O plug-in do Xbox Live Unity não oferece suporte realizações ou multiplayer online e é recomendado somente para [programa de criadores do Xbox Live](../developer-program-overview.md) membros.

Agora há suporte para suporte a vários usuários o Xbox Live Unity plug-in para cenários que envolvem várias empresas locais. Cada jogador pode usar sua própria conta do Xbox Live para estatísticas e entrar. Seu jogo também pode habilitar os convidados para os usuários que não possuem contas do Xbox Live para reproduzir. Esse recurso só tem suporte em consoles do Xbox.

Este tutorial o orientará pelo processo de adicionar suporte a vários usuários para o pré de-fabricados Live diferentes de Xbox.

> [!IMPORTANT]
> Somente há suporte para cenários de vários usuários nos consoles do Xbox. Essa funcionalidade não funciona no PC.

## <a name="prerequisites"></a>Pré-requisitos
Você precisará ter seguido as [guia de Introdução](configure-xbox-live-in-unity.md) tutorial para habilitar e configurar o Xbox Live.

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>Adicionando e entrar em vários usuários do Xbox Live

1. Dos **ativos** > **Xbox Live** > **pré-fabricados** pasta, arraste até sua cena tantos **XboxLiveUser** pré-fabricado instâncias pois haverá jogadores. Para este tutorial, haverá dois jogadores até duas **XboxLiveUser** instâncias serão adicionadas à cena. Nos referimos a eles como **XboxLiveUser** e **XboxLiveUser2** para sua conveniência.

2. Para adicionar os usuários e assiná-las em adequadamente, adicionar duas instâncias do **UserProfile** pré-fabricado à cena, uma para cada **XboxLiveUser**. Verifique se você tem uma instância da `XboxLiveServices` pré-fabricado na cena. Além disso, certifique-se de mover os dois **UserProfile** objetos separados na cena de diferenciá-los. Como esses pré-fabricados usam Eventsystem o Unity, certifique-se de que você tenha uma instância da `EventSystem` na cena.

    ![Hierarquia de suporte a vários usuários no Xbox Live projeto Tutorial de plug-in do Unity](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![Jogo cena de suporte a vários usuários em um projeto do Tutorial de plug-in do Xbox Live Unity](../images/unity/MUA-Tutorial-GameScene.png)

3. Atribuir uma instância das **XboxLiveUser** pré-fabricados têm em cena a cada um dos **UserProfile** objetos.

    ![Pré-fabricado UserProfile para suporte a vários usuários](../images/unity/user-profile-for-mua.png)

4. Uma vez que somente há suporte para aplicativos de vários usuários em dispositivos Xbox One, adição de suporte de controlador para o **UserProfile** objetos é necessária. Em cada **UserProfile** do objeto, há um campo chamado `InputControllerButton` onde você pode especificar o joystick e números, cada botão **UserProfile** deve escutar.

Para este tutorial, usaremos `joystick 1 button 0` para o **UserProfile** atribuído ao jogador 1 e `joystick 2 button 0` para o jogador 2 e a segunda **UserProfile** objeto do jogo. Isso atribuirá a `A` botão para interagir com **UserProfile** para os dois controladores.

> [!Note]
> Para obter mais detalhes sobre o suporte de controlador do Xbox no plug-in Xbox Live Unity, check-out: [Adicionar suporte de controlador para pré-fabricados do Xbox Live](add-controller-support-to-xbox-live-prefabs.md)

5. Execute a cena no editor e ocorrências executadas em cada um dos botões para verificar se tudo está configurada corretamente.

    ![Teste o suporte a vários usuários no Editor do Unity](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>Compilando e testando a UWP

1. Depois de seguir as etapas descritas na [títulos de criadores de desenvolver com o Unity](configure-xbox-live-in-unity.md) tutorial, abra a solução UWP exportada no Visual Studio.

2. Sob o projeto UWP do seu jogo, clique com botão direito do `package.appxmanifest.xml` do arquivo e escolha **Exibir código**.

3. Sob o `<Properties></Properties>` seção, adicione o seguinte que permite a entrada de vários usuário para o aplicativo: `<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. Para testar no Xbox, siga a documentação na [configurar seu UWP no ambiente de desenvolvimento do Xbox](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup) tutorial.

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>Usando os outros Xbox Live pré-fabricados com vários usuários

No **exemplos** pasta o plug-in, há um **MultiUserExample** cena que mostra como os pré-fabricados diferentes podem usar o **XboxLiveUser** instâncias para exibir específico informações para cada usuário.
