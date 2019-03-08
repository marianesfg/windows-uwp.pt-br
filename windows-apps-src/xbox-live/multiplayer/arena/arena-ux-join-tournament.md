---
title: Ingressar em um torneio
description: Saiba como criar a interface do usuário para ingresso torneios do seu jogo dos jogadores.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arena, torneio, experiência do usuário
ms.localizationpriority: medium
ms.openlocfilehash: 1cdcfc7243463a35eccfaeb13a3b9b92b616952b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613501"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>Ingressar em um torneio usando a interface do usuário Arena

As APIs da Arena pode fornecer seu título com dados sobre o registro, fazer check-in e informações da equipe, mas eles *não* fornecem a funcionalidade *executar* as tarefas relacionadas. O título é esperado para usar a interface do usuário Arena ou um organizador de torneio de terceiros (TO) ou criar seu próprios suporte de gerenciamento de torneio.

## <a name="xbox-arena-ui-team-formation"></a>Xbox Arena da interface do usuário: Formação da equipe

A interface do usuário Arena fornece um método para jogadores em formulário ou em uma junção de uma equipe. Não há nenhum requisito para o título.

###### <a name="ui-example-create-a-team"></a>Exemplo de interface do usuário: Criar uma equipe

![Formar uma tela de equipe](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>Ao formar uma equipe, um jogador pode:

* Envie convites para ingressar em um ou mais amigos ou clube.
* Localize os membros da equipe pelo lançamento de um anúncio LFG.
* Registrar ou cancelar o registro de uma equipe.
* Remova um membro de uma equipe.

## <a name="xbox-arena-ui-registration"></a>Xbox Arena da interface do usuário: Registro

A interface do usuário Arena fornece um método para jogadores registrar uma equipe. Não há nenhum requisito para o título.

###### <a name="ui-example-register-a-team"></a>Exemplo de interface do usuário: Registrar uma equipe

![Registrar uma tela de equipe](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>Ao se registrar para um torneio, um usuário pode:

* Cancelar o registro de um torneio *antes de* fechou o registro.
* Cancelar o registro de uma equipe no check-in ou quando o torneio está em andamento.
* Cancelar o registro de toda a equipe. *Observe que um usuário individual, deixando uma equipe cancela o registro de toda a equipe.*
* Registrar como um chefe.
* Entenda os requisitos de torneio e regras de participação antes do registro.
* Receba uma notificação de que o registro foi bem-sucedido.
* Ver o status de torneio alterar 'registrado' em tempo real.
* O colchete de visualização no momento em que um torneio começa.
* Consulte os jogadores que já se inscreveram para o torneio.
* Ser impedidos de se registrar se não cumprirem as qualificações do torneio.
* Ser impedidos de se registrar se um jogador foi proibido.

> [!div class="nextstepaction"]
> [Contrato de correspondência](arena-ux-match-engagement.md)
