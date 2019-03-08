---
title: Problemas comuns na migração com vários participantes de 2015
description: Saiba mais sobre problemas comuns que você pode encontrar ao adaptar seu título 2014 com vários participantes para vários jogadores de 2015.
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a04370ee2f45534c88467700b9523c5a4ad11094
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615811"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>Problemas comuns ao adaptar seu título 2014 com vários participantes para vários participantes de 2015

Este tópico descreve problemas que você deve considerar ao adaptar os títulos de 2014 com vários participantes para vários jogadores de 2015.


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>Alterações de configuração a marca para vários jogadores de 2015

Esta seção descreve as alterações a serem observadas ao configurar suas sessões e modelos para vários jogadores de 2015. Para obter exemplos de itens específicos discutidos, consulte [modelos de sessão MPSD](multiplayer-session-directory.md).

### <a name="add-a-capability-for-active-member-connection"></a>Adicionar um recurso para o membro ativo de Conexão

A capacidade de connectionRequiredForActiveMembers permite a detecção de desconexão e sessão alterar recursos de assinatura de vários jogadores de 2015. Adicione esse recurso para o objeto de /constants/system/capabilities para todos os modelos de sessão.


### <a name="add-a-system-constant-for-invite-protocol"></a>Adicione uma constante de sistema para o protocolo de convite

A constante de sistema inviteProtocol permite que o destinatário de um convite para receber uma notificação do sistema quando o cargo do remetente chama o **método MultiplayerService.SendInvitesAsync** ou o **SystemUI.ShowSendGameInvitesAsync Método (IUser, IMultiplayerSessionReference, String)**. Adicionar essa constante, definido como "game", para o objeto de /constants/system para todos os modelos de sessões para o qual o título convida os jogadores.


## <a name="runtime-considerations-for-2015-multiplayer"></a>Considerações de tempo de execução para vários jogadores de 2015

Títulos de 2015 com vários participantes devem:   Sempre chamar o **MultiplayerService.EnableMultiplayerSubscriptions método** antes de entrar a área para múltiplos jogadores do código do título. Essa chamada permite que ambas as assinaturas às alterações de sessão e detecção de desconectar.
-   Certifique-se de usar o mesmo **XboxLiveContext classe** objeto para todas as chamadas pelo mesmo usuário. O contexto contém o estado relacionado ao gerenciamento da conexão usada para assinaturas com vários participantes e desconectar-se a detecção.
-   Se houver vários usuários locais, use um separado **XboxLiveContext** objeto para cada usuário.


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>Migrando de um modelo de sessão do contrato versão 104/105 para 107

A última versão de contrato de modelo de sessão é 107, com algumas alterações no esquema usado para MPSD. Modelos que você tenha escrito para a versão de contrato de modelo de sessão 104/105 devem ser atualizados se forem publicados novamente para usar a versão 107. Este tópico resume as alterações a serem feitas na migração de seus modelos para a versão mais recente. Os modelos em si são descritos em [modelos de sessão MPSD](multiplayer-session-directory.md).

| Importante                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Funcionalidade que é definida por meio de um modelo não pode ser alterada por meio de gravações para MPSD. Para alterar os valores, você deve criar e enviar um novo modelo com as alterações necessárias. Todos os itens que não são definidos por meio de um modelo podem ser alterados por meio de gravações para a MPSD. |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>Alterações para o objeto /constants/system/managedInitialization

O objeto /constants/system/managedInitialization foi renomeado para /constants/system/memberInitialization. Aqui estão as alterações a serem feitas para os pares de nome/valor para esse objeto: autoEvaluate é renomeado para externalEvaluation. Suas alterações de polaridade, exceto que false continua sendo o padrão.
-   O valor padrão de membersNeededToStart muda de 2 para 1.
-   O valor padrão de joinTimeout muda de 5 segundos para 10 segundos.
-   O valor padrão de measurementTimeout muda de 5 segundos para 30 segundos.


### <a name="changes-to-the-constantssystemtimeouts-object"></a>Alterações para o objeto /constants/system/timeouts

O objeto /constants/system/timeouts é removido e os tempos limite são renomeados e realocados em /constants/system. Aqui estão as alterações a serem feitas para os pares de nome/valor para esse objeto:   O tempo limite de reservada se torna reservedRemovalTimeout.
-   O tempo limite da inativo se torna inactiveRemovalTimeout. Seu novo padrão é 0 (horas).
-   O tempo limite de pronto se torna readyRemovalTimeout.
-   O tempo limite de sessionEmpty torna-se sessionEmptyTimeout.

Detalhes sobre os tempos limite é apresentados [tempos limite de sessão](mpsd-session-details.md).


### <a name="change-to-the-game-play-capability"></a>Altere para o recurso de jogo

O jogo recurso habilita recente jogadores e emissão de relatórios de reputação. Agora você deve especificar o campo /constants/system/capabilities/gameplay como true em sessões que representam a reprodução do jogo real, em vez de sessões de auxiliar, por exemplo, corresponderem e lobby sessões.


## <a name="see-also"></a>Consulte também

[Modelos de sessão MPSD](mpsd-session-details.md)
