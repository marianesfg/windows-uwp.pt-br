---
title: Contrato de correspondência
description: Descreve os estágios UX de jogadores em andamento através de uma experiência de torneio.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arena, torneio, experiência do usuário
ms.localizationpriority: medium
ms.openlocfilehash: edc81ff7c692c4789855d341917c54acf4d24594
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598351"
---
# <a name="match-engagement"></a>Contrato de correspondência

Depois que um jogador é registrado e o check-in de um torneio, ele estará pronto para reproduzir. Um participante pode estar em um dos quatro estágios. Cada estágio contém um conjunto exclusivo de critérios, a orientação dos usuários por meio da experiência do jogo torneio. Os quatro estágios são:

1.  Pronto (Arena da interface do usuário ou do jogo)
2.  Executar (no jogo)
3.  Resultados (Arena da interface do usuário ou do jogo)
4.  End (Arena da interface do usuário ou do jogo)

> [!NOTE]  
> Reprodução é o único estágio dos quatro que não há suporte para a interface do usuário Arena. No mínimo, o título deve redirecionar participantes na interface do usuário do Arena no final de cada correspondência (quando o estágio de reprodução está acima), para que eles possam exibir os resultados.

###### <a name="diagram-the-four-stages-of-participant-progression-after-check-in"></a>Diagrama: Os quatro estágios de progressão de participante, após o check-in.

![Diagrama de fluxo de envolvimento de correspondência](../../images/arena/arena-ux-match-flow.png)

![Barra de fluxo de envolvimento de correspondência](../../images/arena/arena-ux-match-flow-bar.png)

## <a name="1-ready-waiting-for-match"></a>1. Pronto ("aguardando por correspondência")

![Aguardando o diagrama de correspondência](../../images/arena/arena-ux-flow-ready.png)

### <a name="user-impact"></a>Impacto do usuário

Nesse estágio, o jogador espera executar em uma correspondência, mas o adversário ainda não é conhecido. Esse estágio é disparado quando um torneio foi iniciado e o participante está aguardando a sua sessão de correspondência começar. Esse período de espera pode ser porque:

* O período de check-in ainda está aberto.
* A próxima rodada ainda não começou.
* O participante tem um bye atual de ida e volta.

### <a name="when-a-bye-is-needed"></a>Quando um bye é necessária

Um bye pode acontecer em qualquer rodada de um torneio, mesmo que podemos dispersar a propagação para tentar evitar isso.

Sempre que uma eliminação de único ou duplo eliminação torneio envolve um número de jogadores ou equipes que não seja uma potência de 2 (ou seja, 4, 8, 16, 32, 64, 128 ou 256), bytes devem ser concedidos. Um bye simplesmente significa que uma equipe precisa participar na primeira rodada de torneio e em vez disso, passe para a segunda rodada livre.

### <a name="discovery"></a>Descoberta

![Captura de tela de descoberta de equipe](../../images/arena/arena-ux-team-discovery.png)

No estágio de pronto, os participantes Saiba mais sobre seu status nesses locais:

* Arena UI: Página de detalhes do torneio
* No jogo: Lista de torneio, promoção de Lobby de correspondência

> [!TIP]  
> **Recomendações de experiência do usuário**  
> Indica que o jogador está aguardando sua próxima correspondência no evento. Exemplos: "Corresponde à pendente" ou "Você recebeu um bye" notificação.  
>
> Se houver suporte neste estágio, fornecem um método para retornar à página de detalhes do torneio.  
>
> Fornece o contexto do evento: Nome do torneio, estado, estilo, descrição, inicie tempo/término tempo, etc.


###### <a name="ui-example-in-game-multiplayer-lobby"></a>Exemplo de interface do usuário: Lobby de jogos com vários participantes

![Captura de tela do torneio lobby](../../images/arena/arena-ux-tournament-lobby.png)

Sempre apresentam o estado do torneio e o motivo em uma posição consistente, sempre que uma correspondência de torneio será promovida ou detalhes de correspondência são apresentados.

Isso é crítico diretrizes para jogadores quando eles devem decidir:

* Para deixar o jogo para saber mais.
* Quais as próximas etapas são necessárias para avançar enquanto estiver em um torneio.

### <a name="under-the-hood"></a>Nos bastidores
Se neste estágio for compatível com o título, vá para o estágio de reprodução quando a correspondência está pronta.

 
## <a name="2-playing"></a>2. Reproduzindo

![Reproduzir o diagrama de correspondência](../../images/arena/arena-ux-flow-play.png)

### <a name="user-impact"></a>Impacto do usuário

Correspondência-up do player próxima é conhecida e a sessão para múltiplos jogadores foi criada. Durante esse estágio, o título move os jogadores por meio do fluxo normal de jogo (telas de instalação de correspondência, lobby jogo e assim por diante). Este é o estágio que os usuários atingem quando a correspondência está pronta.

### <a name="discovery"></a>Descoberta

Há duas maneiras de um participante de torneio de alerta e habilitá-los ingressar em uma correspondência de torneio de fora de um jogo:

* **Notificação do Xbox** – quando a notificação será exibida, o participante pode conter os ![botão nexus](../../images/xbox-nexus.png) (botão de Nexus) para iniciar o jogo e insira uma correspondência.

  ![Toast pronto de correspondência](../../images/arena/arena-ux-match-ready-toast.png)

* Página de detalhes do torneio da arena – o participante pode selecionar **reproduzir** para iniciar o jogo e insira uma correspondência
 
> [!TIP]  
> **Recomendação de experiência do usuário:** Confirme se a entrada de correspondência.  
> Mostra interface do usuário que informa a e/ou confirma que o participante está prestes a entrar diretamente em um corredor de correspondência.

###### <a name="ui-example-match-entry-confirmation-dialog-box"></a>Exemplo de interface do usuário: caixa de diálogo de confirmação de entrada corresponde

![Caixa de diálogo de confirmação de entrada correspondência](../../images/arena/arena-ux-confirm-match-entry.png)

Exiba os nessa interface do usuário depois que o participante de opta por inserir um torneio de um local fora do jogo. Faça isso depois lançou um jogo e antes de entrar no corredor de correspondência.

> [!TIP]  
> **Recomendação de experiência do usuário:** Oferece uma opção para cancelar.

Os participantes podem decidir há outras tarefas que eles precisam ser concluídas para se preparar para o torneio. É importante dar a eles a flexibilidade de mudar sua ideia. O título pode oferecer opções adicionais:

* Mantenha-se no jogo. (O participante é obtido ao menu principal).
* Retornar a área da interface do usuário.
* Se o participante de optar por deixar o jogo, é recomendável que seu título informar o participante do tempo restante antes que o limite de perder tempo limite foi atingido.

###### <a name="ui-example-match-entry-confirmation--leave-game-warning"></a>Exemplo de interface do usuário: corresponde à confirmação de entrada – deixe o aviso de jogo

![Diálogo de confirmação de perder de entrada de correspondência](../../images/arena/arena-ux-confirm-match-cancel.png)

### <a name="playing-a-match"></a>Reproduzir uma correspondência

A hora de início para a correspondência é definida no modelo de sessão. Ele é definido como uma data e hora no formato UTC padrão — por exemplo, `2009-06-15T13:45:30.0900000Z`. O organizador do torneio pode criar a sessão como muito antes da hora de início quanto desejar, incluindo simultaneamente com a hora de início. Correspondência são enviadas notificações para os participantes de correspondência na hora de início, para que os títulos de nunca ser ativados antes da hora de início. Em geral, trate a sessão Arena da mesma forma que você trataria a sua sessão de vários jogadores do título. No entanto, há algumas diferenças:

* O título não pode alterar a lista da sessão de jogo.
* Lá, não é o processo normal de habilitação de jogadores com velocidades de conexão de um problema.
* O título deve participar de arbitragem.

### <a name="match-lobby-details"></a>Detalhes de lobby de correspondência

Um corredor de correspondência do torneio deve incluir detalhes que deixam claro que se trata de um torneio, e que ele é diferente de uma correspondência para múltiplos jogadores padrão. São exemplos de detalhes, medição de tempo, dependência de equipe e Arredonda consecutivas e estágios. Quando a correspondência for relativa, vá para o estágio 3 (aguardando resultados) ou retornar o jogador na interface do usuário Arena.

###### <a name="ui-example-match-lobby-details"></a>Exemplo de interface do usuário: Detalhes de Lobby de correspondência

![Tela de detalhes de lobby de correspondência](../../images/arena/arena-ux-match-lobby-details.png)

**Um** -reforçar que se trata de uma correspondência de "Torneio" ou "Arena".  
**B** -detalhes do torneio  
   1. Nome do torneio (no máximo 128 caracteres)  
   2. Estilo, de modo de jogo  
   3. Round ou número de estágio  

**C** - pronto para cima / participantes de botão Reproduzir – podem ter inserido um torneio, mas exigem tempo extra antes de iniciar a sessão de correspondência. Por exemplo, uma correspondência pode exigir a instalação, como seleção de caractere. Essa funcionalidade pode informar os outros membros da equipe quando estiver prontos.  
**1!d** -temporizador de contagem regressiva
  * O temporizador é baseado na **limite de tempo limite de perder** (definido pelo jogo).
  * Isso avisa que as equipes que, se os requisitos mínimos de equipe não forem atendidos neste momento, o jogo é perdido e o wins de equipe ativo.  

**E** - difusão lembrete  
**F** -nome da equipe + participantes

 
### <a name="under-the-hood"></a>Nos bastidores

#### <a name="protocol-activation"></a>Ativação de protocolos

A interface do usuário Arena inicia as coisas, enviando participantes diretamente em um título, quando um usuário está pronto para reproduzir uma correspondência — por exemplo, quando aceitando a torrada "correspondência pronto" enviadas por Arena. Ativação de protocolo pode ocorrer em duas vezes: como o título é iniciado ou quando ele já está em execução. Em ambos os casos, a ativação é semelhante ao que acontece quando um título é ativado em resposta a um usuário que aceita um convite para o jogo.

* **Se o título está sendo iniciado** - - as **Activated** evento é acionado pela primeira vez quando o título é iniciado. Se essa ativação inicial for uma ativação de protocolo da Arena, isso significa que o título foi iniciado por um usuário tentar reproduzir em um torneio. O título deve ignorar o mais depressa possível para a correspondência, ignorando as telas de entrada e o menu principal. A ID de usuário de Xbox (XUID) do usuário participante é fornecido na URI de ativação e já deve estar conectado.

* **Se o título já estava em execução** - - as **ativação** também podem acionar eventos com uma ativação de protocolo da Arena enquanto o título já está em execução. Se o título está ocioso quando ele recebe a ativação de protocolo, ele deve ir imediatamente para a correspondência de torneio. É seguro fazer isso porque o evento de ativação é disparado somente por uma ação explícita do usuário (atuando em uma notificação do sistema que leva para a correspondência, ou pular para a correspondência da interface do usuário Arena). Se o título recebe a ativação de protocolo durante um jogo, ele deve dar ao usuário a opção de deixar o jogo (primeiro salvá-lo, se necessário) e insira a correspondência de torneio da maneira mais vantajoso possível.

* **Se um participante ignora a notificação do Xbox** – é recomendável que um título Arena habilitado sempre consultar Arena na inicialização para ver se o usuário está em uma correspondência ativa. Se assim, o título deve apresentar "confirmar a entrada de correspondência" interface do usuário, para informar ao usuário que eles têm uma correspondência e perguntam se deseja inseri-la.  

  Isso garante que os participantes fazer a transição para uma correspondência de torneio corretamente, caso eles perderem a torrada de correspondência e simplesmente iniciar o jogo (esperando entrar em sua correspondência). Ou, caso o jogo trava e o participante de reiniciar apenas o título para voltar à sua correspondência.

> [!NOTE]  
> O título deve verificar o XUID sobre a ativação de protocolo URI. Se ele não coincidir com o jogador atual do título, o título deve também alternar contextos de usuário.

#### <a name="forfeit-time-out"></a>Tempo limite de perder

Pelo menos um player deve estar ativo na sessão antes do tempo de perder, que é hora de início mais o tempo limite de perder (consulte o diagrama reprodução). Se ninguém ingressou a sessão como ativa antes da hora de perder, a correspondência é cancelada e ambas as equipes recebem uma perda por meio de arbitragem Arena.
 
## <a name="3-results"></a>3. Resultados

![Resultados do diagrama de correspondência](../../images/arena/arena-ux-flow-results.png)

Este é o estágio quando um player concluiu a correspondência e os resultados foram relatados ser arbitrário. Neste ponto, é ainda não foi determinado se a equipe está ainda em torneio. Enquanto isso, a experiência de tela "game final" deve continuar a relatar os resultados da sessão (assim como em qualquer correspondência com vários participantes).

Os resultados do torneio — nenhum (não há jogos) do concurso, Win, perda, Draw, classificação ou não mostrar — seguirá.

É possível para este estágio deve ser ignorada se o resultado esteja disponível imediatamente.

### <a name="reporting-results"></a>Resultados do relatório

Os resultados da correspondência são relatados de volta a Arena e o organizador do torneio por meio da sessão usando um recurso chamado *arbitragem*. Arbitragem é uma estrutura para usar uma sessão de correspondência para executar com segurança uma correspondência e um resultado de relatório.

Quando uma sessão de correspondência começa, ele é considerado uma sessão arbitrário. Ele tem uma linha do tempo fixa, que impõe a estrutura de arbitragem.
 
### <a name="arbitration-time-out"></a>Tempo limite de arbitragem

O tempo limite de arbitragem é a quantidade máxima de tempo, após a correspondência de início tempo, o que os participantes têm para reproduzir e resultados de relatório. Esse valor é definido no modelo de sessão para eventos de execução do organizador de torneio e no modo de jogo Multiplayer Arena para UGT e eventos de execução de franquia, portanto, pode ser tanto tempo quanto seu título precisa.

O título pode relatar os resultados a qualquer momento entre a hora de início e a hora de arbitragem. Arbitragem ocorrer a qualquer momento entre a hora de perder e a hora de arbitragem, depois de cada membro ativo da sessão enviou os resultados. Por exemplo, se apenas um membro está ativo na sessão no tempo a perder, eles podem (e deve) lançar um resultado e arbitragem ocorrerá. Não importa quantos resultados estão disponíveis em tempo de arbitragem, arbitragem ocorrerá se ele já não tiver.

Se um usuário em uma sessão que já foram arbitrários (porque a arbitragem tempo limite expirar, um servidor de jogo arbitra a sessão ou o usuário se associe atrasado), o título deve finalizar a correspondência e exibir o resultado arbitrário para o usuário.

Resultados de arbitragem sempre incluem o resultado para cada equipe. Quando a pontuação do jogador um individuais é relatada, ele inclui não apenas o resultado da sua equipe, mas o conjunto completo de resultados para cada equipe.
Arbitragem começa automaticamente assim que o primeiro cliente relata os resultados com o Xbox Live. Ele termina assim que os resultados de relatório de todos os clientes. Pode haver casos em que os participantes reproduzir maiores que o tempo de arbitragem especificado no modelo de sessão. Isso resulta em um risco de que uma correspondência é forçada a terminar antes que os participantes são prontos.

> [!TIP]  
> **Recomendação de experiência do usuário:** Forneça um período de tempo fixo de sessão de correspondência.
>
> Limites de tempo predefinidos em sessões de correspondência reduzirá o risco de problemas que ocorrem por tempo limite de arbitragem.

Há duas opções para impedir que uma correspondência final antes de terminar de participantes de reprodução:

* Defina um período de tempo mais amplo para o tempo limite de arbitragem.
* Se um período de tempo fixo está em conflito com estilo de jogo do seu título, tente a solução a seguir:
   1.   Determinar que os resultados não foi relatados, e que o tempo limite de arbitragem está prestes a expirar (por exemplo, cinco minutos estão à esquerda).
   2.   Exibir interface do usuário que alerta os participantes seu jogo está prestes a terminar em X minutos. Essa opção pode ser implementada para todos os torneios e se o tempo limite de arbitragem é mais amplo, a maioria dos participantes nunca o verá.

### <a name="returning-to-the-arena-ui-by-using-a-dedicated-a-button-press"></a>Retornando à interface do usuário Arena usando um dedicado um pressionamento de botão

Se o título não superfície resultados do torneio ou progressão de estágio no jogo, é recomendável que o participante de ter uma maneira de retornar à interface do usuário que oferecem essas informações. Isso pode ser a interface do usuário Arena ou um aplicativo do organizador de torneio de terceiros. Idealmente, essa funcionalidade seria exibido após a correspondência está acima ou, possivelmente em resposta à solicitação de um jogador para abandonar a correspondência em andamento. Isso pode ser feito de uma tela de relatório do fim do jogo.

> [!TIP]  
> **Recomendação de experiência do usuário**  
> Dedica um botão do controlador como uma maneira rápida para jogadores retornar para o Hub Arena na interface Xbox.  
>
> ![Botão de controlador de hub arena](../../images/arena/arena-ux-arena-hub-button.png)

### <a name="redirect-participants-at-the-end-of-a-match"></a>Participantes de redirecionamento no final de uma correspondência

Quando a correspondência completa de participantes, é importante que seu título oferecer orientação clara sobre as próximas etapas. Isso inclui um método para examinar o torneio round ou resultados de estágio (se não apresentado no jogo).

> [!TIP]  
> **Recomendação de experiência do usuário:** Use uma sobreposição de pop-up.
>
> Essa interface do usuário aparece quando os resultados do torneio estão em, e a experiência do fim do jogo é concluída.

### <a name="recommended-ui-data"></a>Dados de recomendado da interface do usuário:

* Nome do torneio
* Próximo colchete
* Detalhes de round ou estágio Avançar
* Posição da equipe
* Resultados arbitrário
* Link profundo do torneio detalhes
* Opção de permanecer no jogo

### <a name="under-the-hood"></a>Nos bastidores

Depois de invocar a interface do usuário Arena, o título deve continuar executando, possivelmente na mesma tela, aguardando outro evento de ativação de protocolo. Em seguida, se o jogador tem outra correspondência para reproduzir, o título será pronto para começar. Quando o jogador vai de correspondência para correspondência, alternando entre o título e a Arena da interface do usuário, ele poderá agilizar a experiência do usuário.

###### <a name="ui-example-tournament-arbitrated-resultswinning-team"></a>Exemplo de interface do usuário: Torneio arbitrário resultados — equipe vencedora

![Vencido tela](../../images/arena/arena-ux-won-game-redirect.png)

###### <a name="ui-example-end-of-tournament-resultsmatch-ended"></a>Exemplo de interface do usuário: Resultados do final do torneio — correspondência terminou 

![Você perdeu uma tela](../../images/arena/arena-ux-lost-game-redirect.png)

## <a name="4-end"></a>4. Fim

![Final do diagrama de correspondência](../../images/arena/arena-ux-flow-end.png)

O estágio final de um fluxo de torneio ocorre quando o torneio em si foi encerrada e os resultados finais são relatados.

Os participantes serão informados sobre o que o torneio está acima ou que o jogador foi eliminado.

### <a name="discovery"></a>Descoberta

A interface do usuário Arena exibe os resultados e celebra os vencedores finais:

* Página de detalhes do torneio
* Resultados compartilhados pelos participantes em seu feed de atividades player ou clubes
* Resultados do torneio lançados para o perfil de um jogador

O título pode surgir resultados de torneio no jogo:

* Após a experiência do fim do jogo na fase final, o participante é concluído.
* Listado em **torneios meu**, em um recurso de torneio procurar.


###### <a name="ui-example-end-of-tournament-resultstournament-ended"></a>Exemplo de interface do usuário: Fim dos resultados do torneio — torneio terminou

![Torneio pela tela](../../images/arena/arena-ux-tournament-completed.png)

* Se houver suporte neste estágio, fornecem uma opção para retornar para a Arena da interface do usuário ou aplicativo do organizador de torneio.
* Exiba o nome do evento.
* Exiba o nome da equipe, se a equipe tem mais de um membro.
* Exiba a equipe aguardando no evento, se disponível. Se nenhuma posição está disponível (ou, opcionalmente, além de pé), o título deve indicar que a participação do player no evento chegou ao fim.
* O título da sugere as próximas etapas (por exemplo, "Assistir a transmissões ao vivo", "Localizar outro torneio,", "Mantenha-se no jogo").
* O título da fornece um motivo por que o envolvimento da equipe terminou:
  * Rejeitado — a equipe não conseguiu atender as qualificações para inserir o torneio.
  * Eliminado — a equipe perdeu a correspondência de torneio.
  * Removidos — a equipe foi removida de um torneio de Active Directory.
  * Concluído — a equipe concluiu a fase de final do torneio.
