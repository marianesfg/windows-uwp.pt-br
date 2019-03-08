---
title: Reputação
description: Saiba como o Xbox Live usa o serviço de reputação a fim de incentivar a jogabilidade positiva.
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, reputação, plataforma social
ms.localizationpriority: medium
ms.openlocfilehash: 4e7763294458f19509d5b797a6fa10e03678abee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641811"
---
# <a name="reputation"></a>Reputação

Reputação é uma estatística de usuário, assim como qualquer outro e está incluída em chamadas para recuperar todas as estatísticas de um usuário e usá-los no monitoramento e emissão de relatórios. A reputação em si é representada pela **reputação classe**. O **ReputationService classe**representa o serviço de reputação. Os URIs correspondentes são descritas em **URIs de reputação**.

> [!IMPORTANT]
> Estatísticas de reputação são globais em serviço, não associado a um título específico. A ID (SCID) de configuração de serviço para o serviço é o SCID global usado para acessar as estatísticas de reputação.


## <a name="features-of-the-reputation-service"></a>Recursos do serviço de reputação de

O serviço de reputação:

-   Lida com comentários e reclamações, da mesma maneira. As entidades enviar comentários sobre um usuário, e esses comentários afetam reputação do usuário. Os comentários, em seguida, podem ser encaminhado para a equipe de imposição para uma ação adicional.
-   Permite aos usuários enviar comentários sobre os outros usuários. Títulos podem enviar automaticamente comentários.
-   Permite acesso direto à API de títulos. Um usuário pode enviar comentários diretamente de dentro de um jogo e de dentro do contexto da área de jogo em que o usuário está atualmente.
-   Identificadores de baixa reputação como afetando o que os usuários são capazes de fazer no Xbox Live e dentro de títulos. Assim que os usuários devem ficar atento à sua reputação e act mais adequadamente durante o jogo online.
-   Permite que comentários positivos, bem como comentários negativos. Os usuários que ajudam a comunidade do Xbox ou community de um título podem ser recompensados por seus esforços e eles podem até mesmo receber privilégios especiais.
-   Deriva de uma única geral reputação, representada pela **Reputation.OverallReputation propriedade**. Ele é derivado dos seguintes tipos de reputação:

    -   Fair play. Representado pela **Reputation.FairplayReputation propriedade**.
    -   Comunicações. Representado pela **Reputation.CommunicationsReputation propriedade**.
    -   Conteúdo gerado pelo usuário (UGC). Representado pela **Reputation.UserGeneratedContentReputation propriedade**.

Ver **ResetReputation (JSON)** para obter mais informações.


## <a name="usage-flow-for-the-reputation-service"></a>Fluxo de uso para o serviço de reputação

O fluxo geral para o serviço de reputação tem duas fases: reputação e filtrados de reputação de cruzamento de emissão de relatórios.


## <a name="reporting-reputation"></a>Reputação de emissão de relatórios

Quando foi relatado suficiente feedback negativo para um usuário específico, o título define o **Reputation.OverallReputation propriedade** para indicar uma reputação ruim para o usuário (atributo JSON OverallReputationIsBad). Considerando a hora sem reclamações, lentamente melhora a reputação do usuário e é possível que um usuário com uma reputação ruim de uma vez obter uma boa reputação novamente.


## <a name="reputation-filtered-matchmaking"></a>Filtrado de reputação de cruzamento

Para emparelhamento filtrado de reputação, especificado por requisito Xbox (XR), o título recupera o jogador **Reputation.OverallReputation propriedade**. Esse valor é uma pontuação que indica o estado de reputação de geral do jogador.

> [!NOTE]
> Se o título está procurando o atributo OverallReputationIsBad em um arquivo JSON e não o encontra, é seguro pressupor que o usuário tem uma boa reputação. Isso pode acontecer com novas contas, até que os comentários para o usuário foi processado. Os jogadores com comentários de outros usuários sempre terão as estatísticas de reputação e um valor para o **Reputation.OverallReputation** propriedade.


## <a name="reputation-as-an-indicator-of-behavior"></a>Reputação como um indicador de comportamento

Reputação fornece um indicador de como o usuário se comporta no sistema. Por exemplo, é a pessoa que um jogador amigável ou não? Comentários de outros membros da sessão determina a reputação de um jogador. Cada usuário tem uma pontuação de reputação que viaja com essa pessoa em qualquer lugar no Xbox One. Ele é exposto em destaque em locais de redes sociais que amigos podem ver, para que eles podem aplicar pressão de par para um player para se comportar melhor. Por exemplo:

-   É no cartão de perfil de cada usuário. Qualquer pessoa no sistema pode examinar o perfil do usuário com um único clique.
-   Ele é mostrado em jogos com vários participantes. Quando os usuários jogam juntos on-line, reputação do grupo é igual da reputação do player com a mais baixa reputação no grupo. O grupo só é comparado com outras pessoas com reputação semelhante. Outros jogadores usam reputação para decidir qual tipo de players estão em jogo e decida se deseja ingressar o jogo.


## <a name="key-elements-of-reputation"></a>Principais elementos de reputação

Um título pode determinar se um usuário tiver atingido um Evite Me nível para fair play, comunicações ou categorias de conteúdo gerado pelo usuário (UGC). O usuário atingiu evitar Me para a categoria associada se qualquer uma das seguintes propriedades for definida como 1:

-   **Propriedade Reputation.OverallReputation**. O atributo JSON associado é OverallReputationIsBad.
-   **Propriedade Reputation.FairplayReputation**. O atributo JSON associado é FairplayReputationIsBad.
-   **Propriedade Reputation.CommunicationsReputation**. O atributo JSON associado é CommsReputationIsBad.
-   **Propriedade Reputation.UserGeneratedContentReputation**. O atributo JSON associado é UserContentReputationIsBad.


## <a name="good-game-reports"></a>Bons relatórios de jogo

Além dos usuários para ações incorretas de emissão de relatórios, os usuários podem também enviar uns aos outros bons relatórios de jogos, que podem ser criados em títulos como votação para o player mais valioso.


## <a name="feedback-history"></a>Histórico de comentários

Informações de relatórios de histórico de comentários, como a hora de quando o usuário foi relatado pela última vez e qual foi a pessoa relatados para, por exemplo, "recentemente recebeu comentários sobre o estilo de comunicação."


## <a name="xbox-requirement-for-reputation"></a>Requisito do Xbox para reputação

Requisito (XR Xbox) 068, para que haja correspondência filtragem por reputação, requer a separação de jogadores com baixa reputação do que aquelas com alta reputação. Isso é feito examinando os **Reputation.OverallReputation** valor de um jogador e atributo de OverallReputationIsBad do jogador em estatísticas de usuário.

O título pode recuperar a reputação de um usuário, passando "OverallReputation" para o *statisticName* parâmetro do **método UserStatisticsService.GetSingleUserStatisticsAsync (cadeia de caracteres, cadeia de caracteres, cadeia de caracteres)**. As chamadas de método do WinRT **obter (/users/xuid({xuid})/scids/{scid}/stats)** conforme mostrado no seguinte corpo JSON.

```json
    {
      "requestedusers": [
        "2533274792693551"
      ],
      "requestedscids": [
        {
          "scid": "7492baca-c1b4-440d-a391-b7ef364a8d40",
          "requestedstats": [
            "OverallReputationIsBad",
            "FairplayReputationIsBad",
            "CommsReputationIsBad",
            "UserContentReputationIsBad"
          ]
        }
      ]
    }
```

Quando os comentários de um usuário de outros jogadores atinge um nível baixo, OverallReputationIsBad é definido como 1 para o usuário. As pessoas para quem **Reputation.OverallReputation** é 1 só deve ser correspondidas com outras pessoas tendo **OverallReputation** definido como 1. Por padrão, quando as pessoas inserem uma correspondência, elas geralmente não precisam lidar com as pessoas com baixa reputação. Títulos, opcionalmente, podem permitir que um player com uma boa reputação de aceitar para corresponder com baixa reputação jogadores.

Para obter a versão mais recente do XRs que se aplicam à reputação, consulte [Xbox requisitos](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx) na rede de desenvolvedor de jogos (GDN).


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>Criando usuários com má reputação geral para teste

Para teste, os usuários com a reputação muito ruim podem ser definidos para testar a filtragem de implementação para um título de correspondência. Para definir um valor específico para um usuário, o título de teste ou a ferramenta chama **POST (/users/xuid({xuid})/resetreputation)** com um arquivo JSON que define os valores de reputação específicas do usuário. Ver **ResetReputation (JSON)** para obter mais informações.

Por exemplo, o seguinte corpo JSON define reputação de fair play de um usuário para um valor muito baixo:

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

Parceiros podem fazer essa chamada para todas as áreas restritas, exceto para varejo. Essa solicitação define as pontuações de reputação de base do usuário e pesos do player para comentários positivos serão todos serão zerados. A reputação real do usuário após esta chamada consiste nessas pontuações base mais bônus do Embaixador do jogador, mais bônus de seguidores do jogador. Esse mecanismo cria um usuário com uma pontuação baixa e **Reputation.OverallReputation** definido como 1 para testar o XR de filtro de correspondência. O título pode obter a pontuação de reputação do usuário para o usuário do serviço de estatísticas do usuário, conforme descrito na seção "Requisitos de reputação do Xbox" deste tópico.


## <a name="resetting-a-users-reputation-to-the-defaults"></a>Redefinindo a reputação de um usuário com as configurações padrão

O título define o atributo OverallReputationIsBad para indicar que o usuário tem uma boa reputação. Ele chama **POST (/ usuários/me/resetreputation)** e define todos os valores para 75. Uma única chamada para **/users/xuid({xuid})/deleteuserdata** pode ser usado para redefinir vários IDs de usuário do Xbox. O corpo da solicitação é:

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>Enviar comentários de títulos

Títulos podem enviar comentários de fair play sobre players de correspondências. Isso é feito diretamente do título ou do serviço de título em lotes. O título pode usar o **ReputationService.SubmitReputationFeedbackAsync método** ou métodos REST de direcionar o seguinte:

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| POSTAGEM do cliente          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| Serviço (lote) de POST | https://reputation.xboxlive.com:10443/users/batchfeedback   |

Ver **comentários (JSON)** para um exemplo de corpo JSON para o envio e para obter definições dos campos permitidos.


## <a name="types-of-feedback-allowed"></a>Tipos de permissão de comentários

As categorias de comentários que podem ser publicados são definidas no **comentários (JSON)**.


## <a name="when-titles-should-send-feedback"></a>Quando títulos devem enviar comentários

Um título deve enviar feedback negativo quando um evento afeta negativamente a experiência do jogador em um jogo. Como regra geral, o título deve enviar comentários apenas um por tipo, por ida reproduzida. Por exemplo, o título deve:

1.  Envie um item de comentário FairPlayKillsTeammates somente de uma pessoa por rodada eliminar 3 ou mais membros da equipe, em vez de enviar um evento sempre que a pessoa elimina uma colega de equipe.
2.  Envie item de comentário FairplayQuitter quando alguém propositadamente fecha uma correspondência no início.
3.  Envie item de comentário FairplayUnsporting, uma vez a cada corrida, quando um usuário está orientando com versões anteriores em um jogo de corrida.
