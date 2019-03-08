---
title: Entrar com pré-fabricado PlayerAuthentication
description: Visão geral do Unity plug-in PlayerAuthentication pré-fabricado
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity
ms.openlocfilehash: ea161ff801e2004569d88d53c78ae963e91b4ce6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605731"
---
# <a name="easy-sign-in-with-the-playerauthentication-prefab"></a>Fácil entrar com pré-fabricado PlayerAuthentication

Pré-fabricado PlayerAuthentication é a maneira mais fácil para adicionar o Xbox Live autenticação ao seu título. Leva apenas três etapas fáceis para ir de uma nova cena para uma página de entrada.

1. Arraste PlayerAuthentication pré-fabricado da cena
2. Arraste um pré-fabricado XboxLiveServices até a cena
3. Adicione um EventSystem para a cena (tecnicamente o PlayerAuthentication criará um para você se um EventSystem não estiver presente, mas adicioná-lo é um hábito.)

E isso é tudo. Agora você pode entrar um player em XboxLive em seu título ao clicar no pré-fabricado PlayerAuthentication na sua cena. Teste sua cena no Unity clicando no botão play fará com que seu pré-fabricado gerar dados fictícios, isso ocorre porque o player do Unity não pode se conectar ao serviço Xbox Live. Para ver uma entrada real, você precisará criar seu projeto para ser executado localmente no Visual Studio. Se seu título tiver sido configurado no Partner Center e você autorizou um gamertag/conta Microsoft para entrar no seu título e em seguida, você poderá entrar uma de suas contas autorizadas em uma compilação do Visual Studio.

Script do pré-fabricado PlayerAuthentication tem algumas configurações que você pode manipular no seu modo de exibição no Inspetor de.

![Captura de tela de Inspetor PlayerAuthentication](../images/unity/playerauthentication_prefab_inspector.JPG)

* Número de Player - determina o player que esteja vinculado ao painel de entrada
* Tema - altera o esquema de cores para o painel de entrada para quando um usuário está conectado ou desconectado. Essa configuração tem uma opção clara ou escura.
* Habilite entrada do controlador - esta caixa de seleção permite que os jogadores para usar um controlador do Xbox para entrada e saída usando PlayerAuthentication pré-fabricado.
* Número de joystick - determina o controlador que pode entrar no nosso out usando pré-fabricado.
* Botão de entrada - uma lista suspensa que permite que você escolha qual botão em um controlador do Xbox conecta um usuário.
* Saia do botão – uma lista suspensa que permite que você escolha qual botão em um controlador do Xbox sai de um usuário.

## <a name="multiplayer-scenario"></a>Cenário com vários participantes

Além de player única entrada, você também pode usar vários pré-fabricados de PlayerAuthentication implementar local com vários participantes em títulos de console Xbox One. Adicionando várias instâncias do pré-fabricado e alterando o atributo de número de Player de cada um, você pode entrar em vários usuários seu título.

> [!WARNING]
> Entrar várias gamertags não é permitido em computadores Windows 10. Para se conectar a vários usuários, você precisará testar seu jogo em um Xbox um único Console.

Criar uma cena que permite que vários participantes só é um pouco mais difícil usando pré-fabricado PlayerAuthentication.

1. Arraste uma instância de PlayerAuthentication pré-fabricado da cena
2. Verifique as **habilitar o controlador de entrada** caixa no Inspetor do pré-fabricado.
3. Certifique-se de que o **número de Player** e **número de Joystick** são definidos como 1.
4. Atribuir a **botão de sinal** no menu suspenso.
5. Atribuir a **botão de sinal Out** no menu suspenso.
6. Arraste uma *segundo* instância de um pré-fabricado PlayerAuthentication até a cena.
7. Verifique as **habilitar o controlador de entrada** caixa no Inspetor do pré-fabricado.
8. Certifique-se de que o **número de Player** e **número de Joystick** estiver definido como 2.
9. Atribuir a **botão de sinal** no menu suspenso.
10. Atribuir a **botão de sinal Out** no menu suspenso.
11. Arraste um pré-fabricado XboxLiveServices até a cena
12. Adicionar um EventSystem à cena

Verifique se o pré-fabricados estão funcionando corretamente, pressionar reproduzir no Player do Unity e clicando os pré-fabricados. Elas serão retornam dados falsos que são esperados como o Player do Unity não é possível conectar-se com o Xbox Live. Com duas instâncias do pré-fabricado PlayerAuthentication configurado para players diferentes e joysticks, você está pronto para criar seu jogo no Visual Studio isso ele pode adequadamente testado em um Console do Xbox. Depois que o seu jogo é compilado, abra o arquivo de solução no Visual Studio, você precisará habilitar o suporte a vários usuários para o seu jogo.
Para fazer isso:

1. Pesquisar Gerenciador de soluções para o arquivo appxmanifest
2. Clique com botão direito no arquivo e escolha View Code
3. Sob o `<Properties><\/Properties>` seção, adicione a seguinte linha: ' < uap:SupportedUsers > várias <\/uap:SupportedUsers >.
4. Implante o jogo para o seu Xbox iniciando um build de depuração remoto do Visual Studio. Você pode encontrar instruções para configurar o título em um Xbox na [configurar seu UWP no ambiente de desenvolvimento do Xbox](../../xbox-apps/development-environment-setup.md) artigo.

> [!NOTE]
> A parte da configuração alterada pode parecer que está sendo habilitado vários jogadores, mas ainda é necessário executar seu jogo em cenários de único player.

Depois que tiver o PlayerAuthentication pré-fabricado trabalhando, você pode aprender mais sobre o script entrar com o artigo [entrar com o Gerenciador de login no Unity](sign-in-manager.md).