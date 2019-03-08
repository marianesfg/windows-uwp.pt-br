---
title: Joinability e visibilidade de sessão de jogo
description: Descreve a sessão de jogo Xbox Live e visibilidade de jogo de terceiros e joinability.
ms.assetid: 39b6dac1-0c6b-4dc1-9fe0-3cb7c471fbab
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 65da0d60080cd790b84ce46f8598c2f6f638ff05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619711"
---
# <a name="game-session-and-game-party-visibility-and-joinability"></a>Sessão de jogo e visibilidade de jogo de terceiros e joinability

No Xbox One, o *visibilidade* e *joinability* configurações para sessões de jogos e partes do jogos, respectivamente, controlar o acesso para experiências com vários participantes. Para fornecer uma excelente experiência de usuário para ingressar na sessão e para os jogadores que está convidando em partes e sessões de jogos, os desenvolvedores de título precisam entender essas configurações. Este white paper analisa as diferenças entre a visibilidade e joinability e aborda as configurações específicas, recomendamos que os títulos usam fornecer aos seus clientes o fluxo de melhor usuário com vários participantes.

## <a name="game-session-visibility"></a>Visibilidade da sessão de jogo


Acesso de sessão do diretório de sessão com vários participantes (MPSD) é retido por duas configurações: sessão *visibilidade* e a sessão *joinability*. Essas configurações podem ser usadas para limitar o acesso de sessão no nível do MPSD.

O primeiro aspecto do controle de acesso é a visibilidade de sessão. Visibilidade de sessão é uma constante que é definida no momento da criação da sessão. Ele é normalmente definido no modelo de sessão e determina quais tipos de usuários que leu e acesso de gravação a uma sessão. Os valores para a configuração de visibilidade são:

-   *Aberto —* todos os usuários podem ler e gravar para a sessão.

-   *Visible —* todos os usuários podem ler, mas apenas unidas e reservado players podem escrever para a sessão.

-   *Privada —* apenas unidas e reservado jogadores possam ler ou gravar para a sessão.

> **Observação:** Definindo a visibilidade privada pode exigir repetições na **JOIN ()** chamar pelo player convidado. Se um convite é enviado por meio da plataforma da interface do usuário, ele pode ser recebido por outro jogador antes desse jogador foi reservada na sessão. Essa condição de corrida pode fazer com que um **JOIN ()** chamar para que um jogador convidado falhar porque sessões privadas ou visíveis exigem uma reserva para o player de junção.
>
> Se nenhuma reserva existir, é gerado um erro de acesso na sessão (HTTP 403). O player de convidado, em seguida, teria que repetir a **JOIN ()** operação, mas as tentativas não devem ser tentada com mais frequência do que a cada cinco segundos. É recomendável que títulos de definir a visibilidade de sessão para abrir para evitar condições de corrida durante o fluxo de associação de um player.

## <a name="game-session-joinability"></a>Joinability de sessão de jogo


O segundo aspecto para controle de acesso é joinability. Ele pode ser definido dinamicamente durante o tempo de vida de sessão e determina quais tipos de usuários podem ingressar em uma sessão. Os valores de joinability de sessão são:

-   *Nenhum* (valor padrão) — não há nenhuma restrição sobre quem pode ingressar na sessão.

-   *Local*— somente usuários locais poderão ingressar na sessão.

-   *Seguido —* somente usuários locais e os usuários que são seguidos por outros membros da sessão podem ingressar na sessão sem reserva.

Um host de sessão ou um arbitrador pode criar uma sessão privada por meio da configuração joinability: fazendo joinability local ou seguido será restringir o acesso a uma sessão de jogo e torná-la particular.

Além disso, o host de sessão ou um arbitrador deve manter o controle da joinability de uma sessão, para que a sessão mais antigo convites podem ser rejeitadas no nível do host se necessário. Por exemplo, se qualquer players convidados não chegaram para ingressar em uma sessão e/ou terceiros até que a sessão já está cheia, o host ou um arbitrador pode instruir qualquer junção jogadores que a sessão foi bloqueada e eles precisam sair da sessão e/ou de terceiros automaticamente.

**Observação:** Joinability de sessão de jogo não é refletida na interface do usuário de aplicativo de terceiros. Mesmo se joinability é restrito, o aplicativo de terceiros convidar da interface do usuário e **ShowSendInvitesAsync** permitirá que a sessão de jogo convites.

## <a name="game-party-joinability"></a>Joinability de jogo de terceiros


Além de controlar a sessão MPSD, o título também tem controle sobre o estado do jogo de terceiros joinability. Essa configuração pode ser usada para limitar o acesso de terceiros jogo no nível de terceiros.

Joinability de terceiros pode ser alterado dinamicamente depois que a parte do jogo é criada. O estado do joinability de uma parte do jogo é refletido na parte convidar da interface do usuário. Ele pode ser definido como:

-   *Convite único —* essa configuração exige um convite para ingressar a parte do jogo.

-   *Permite junções por amigos* (valor padrão)*—* essa configuração requer uma relação de amigo de um player unir a parte de jogo (para ingressar em um terceiro, um membro de terceiros precisa seguir o player de junção).

Joinability pode ser usado para criar um participante de jogo de convite único. Para restringir o acesso a um participante e exigem que os jogadores tem recebido um convite para ingressar, joinability deve ser definido como "convidar apenas".

**Observação:** Joinability de terceiros não influencia joinability de sessão, mas o joinability de terceiros é refletida na interface do usuário de aplicativo de terceiros. Os jogadores podem alterar o joinability de terceiros no aplicativo de terceiros manualmente fora de um título.

## <a name="recommendations"></a>Recomendações


As seguintes recomendações se aplicam os cenários mais comuns do título. Títulos devem seguir essas configurações, se possível, e eles devem usar lógica no título para fazer a determinação final e autoritativa sobre se um novo player é admitido em sessão ou não.

### <a name="recommended-game-session-visibility-open"></a>Recomendado a visibilidade da sessão de jogo: abrir

Sessões abertas de jogos não exigem reservas de player, simplificando assim o fluxo de convites. O host de sessão ou um arbitrador não reserva jogadores no documento de sessão após um convite foi enviado. Em vez disso, o host de sessão ou um arbitrador só rastreia players convidados localmente. Isso permite que os participantes para se conectar ao host imediatamente e determinar se eles devem ingressar em uma sessão, são rejeitados ou devem aguardar (se há suporte para os jogadores de espera). O host ou o arbitrador é a autoridade ultimate e responde e instrui o novo membro para permanecer em ou sair da sessão.

**Observação:** Isso exigirá que o jogador convidado iniciar um título e conectar-se ao host ou arbitrador antes que a decisão final foi feita. É aceitável para exibir uma mensagem de erro para o usuário se uma sessão estiver cheio ou um convite foi rejeitado.

> **Observação:** Para estabelecer uma conexão para o host ou o arbitrador, é necessário um endereço de dispositivo seguro. O **HostDeviceToken** na sessão deve ser usado para indicar qual membro de sessão é o host atual ou o arbitrador de uma sessão e qual endereço de dispositivo proteger um player de convidado deve se conectar.

### <a name="recommended-game-session-joinability-default-not-set"></a>Recomendado joinability da sessão de jogo: padrão (*nenastaveno*)

Atualmente, joinability de sessão de jogo não influencia a interface do usuário do aplicativo de terceiros e não será exposto ao usuário. Para simplificar o fluxo de título, jogo joinability deve permanecer no valor padrão, e toda a autoridade de junção deve ser tratada por meio do arbiter ou host.

### <a name="recommended-game-party-joinability"></a>Recomendado joinability de jogo de terceiros

Joinability de jogo de terceiros é dependente do tipo de sessão que um título é a tentativa de fornecer a um usuário. Os dois cenários são:

-   Abra o jogo — para um jogo aberto, joinability o jogo de terceiros deve ser deixado com o valor padrão: Permite junções por amigos. Isso permite que os amigos unir o jogo de terceiros (e, por extensão a sessão de jogo) sem um convite.

-   Jogo privado — para um jogo particular ou fechado, joinability o jogo de terceiros deve ser definido como apenas com convite. Isso restringe outros jogadores de entrada na festa (e, por extensão a sessão de jogo) sem um convite.

**Observação**: Os jogadores podem alterar manualmente o joinability de uma sessão de jogo por meio do aplicativo de terceiros.

Para ambos os tipos de jogos, o host ou um arbitrador ainda deve permanecer a autoridade mais importante para determinar quem é aceito em uma sessão. Isso resolve quaisquer condições de corrida que podem ocorrer de alternar o fluxo do jogo de aberto para privado. Se o host do arbiter/rejeitar um player, a instância de título rejeitadas deve remover o player da sessão e mostrar uma interface do usuário do jogo para também permitir que um player deixar a parte.

### <a name="maximum-session-size"></a>Tamanho máximo de sessão

O tamanho máximo de membro para uma sessão pode ser usado para limitar o número de jogadores ingressar em uma sessão de jogo. No entanto, esse limite pode causar um ainda aparecerá como completo durante determinadas desconectar cenários no ponto aberto. Por exemplo, se um jogador for desconectado como resultado de uma falha de energia ou de rede, o atraso não será imediatamente refletido na sessão. O membro será definido para inativo assim que o serviço de presença detecta a desconexão (até 20 segundos) e o player será removido tenha expirado o tempo limite de inativo.

Em comparação, uma malha ponto a ponto que usa uma pulsação para detectar uma desconexão geralmente está ciente de uma desconexão dentro de duas a três segundos e foi possível abrir o slot de jogador imediatamente. No entanto, o arbitrador ou o host não é possível remover outros membros.

Para resolver esse problema, o tamanho máximo de membro de uma sessão sempre deve ser definido maior do que o número máximo de players que realmente suporta um título. Por exemplo, se o número do player é 8, o título deve definir o tamanho máximo de sessão a 12. Dessa forma novos jogadores podem associação mesmo se os antigos jogadores têm não ainda atingiu o tempo limite. O host ou um arbitrador determina se a sessão está completa e, em seguida, define uma propriedade de sessão personalizado que será determina se os novos jogadores ainda podem ingressar (**IsFull** : "true"). Isso permite que uma verificação rápida de unir os jogadores se a sessão for permite junções.

O arbitrador ou o host também deve manter uma propriedade personalizada que indica quais membros, por índice, tem atingiu o tempo limite (por exemplo, **TimedOutPlayers** : “3, 5, 9”). Isso permite que os participantes de novo para identificar corretamente os membros da sessão atual.

### <a name="notifying-waiting-players"></a>Notificar os jogadores de espera

Um título pode dar suporte a jogadores de espera por gerenciar uma lista de player em fila, além da parte do jogo. Quando um player conecta-se ao host e a sessão de jogo está cheio, o player é adicionado à lista de espera interna no host. O jogador na fila não ingressar na sessão de jogo, mas permanece na parte do jogo. Para minimizar o tráfego de rede, o player de espera se desconecta o host neste momento.

Quando um slot na sessão do jogo é aberto para o player de espera, o arbitrador ou o host adiciona uma reserva para o player chamando o **AddMemberReservation** método. Neste ponto o player espera ainda não tiver sido ciente dessa reserva, portanto o arbitrador ou o host tem que chamar o **PullReservedPlayersAsync** método. Isso faz com que uma interface do usuário do sistema ou **GameSessionReady** notificação para todos os jogadores reservadas, dependendo da configuração de notificação do sistema e o estado de título de foco. O novo player reconecta-se ao host quando essa notificação é recebida e une a sessão.

Como alternativa, um player pode permanecer conectado ao host e ingressar na sessão quando o host alertas o player que um slot está disponível. No entanto, nesse fluxo não pode ser exibida uma notificação do sistema de interface do usuário fora do título.

**Observação:** Os jogadores serão removidos automaticamente da parte jogo se o título é suspenso e/ou encerrado e receberão nenhuma outra notificações.

<a name="client-multiplayer-flow"></a>Fluxo de cliente com vários participantes
-----------------------
![](../../images/whitepapers/gamesessionvisibility_image1.png)
![](../../images/whitepapers/gamesessionvisibility_image2.png)
