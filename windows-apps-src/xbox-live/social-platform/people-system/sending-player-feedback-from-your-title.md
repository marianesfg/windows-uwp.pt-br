---
title: Enviar comentários do Player do título
description: Saiba como seu título pode ajudar a promover experiências positivas de jogadores por meio do envio de comentários do player para o serviço de reputação do Xbox Live.
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, reputação, comentários do player
ms.localizationpriority: medium
ms.openlocfilehash: 3e8d82fc9b195f174bf7a46a8d21fb20b2df9551
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592231"
---
# <a name="sending-player-feedback-from-your-title"></a>Enviar comentários do player do título
A maioria dos membros do Xbox Live é incrível, mas há uma pequena porcentagem de "Maçãs ruim" que prejudicam a experiências de jogo de outras pessoas. Podemos identificar essas porcentagens pequeno de usuários por meio de usuário e os comentários enviados do título. Nós ajudamos a proteger o restante da população, garantindo que esses "maçãs ruim" tenham uma experiência de com vários participantes limitada em que eles não consegue interferir em jogos de jogadores BOM. Xbox depende intensamente dos usuários para relatar a outros usuários para manter o sistema precisos, mas títulos no Xbox One podem participar diretamente e drasticamente ajudar a melhorar a precisão da população de reputação de usuário.

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>Etapas para enviar comentários do título ou o serviço de título
1. Adicionar momentos de comentários ao título ou o serviço de título
2. Determinar o tipo correto de comentários
3. Chamar APIs de comentários de reputação com comentários

### <a name="adding-feedback-moments-to-title-or-title-service"></a>Adicionando comentários momentos de títulos ou serviço de título
Todos os jogadores tiveram experiências ruins com colegas de equipe que sabotar suas próprias lado, jogadores que espera-se apenas ao redor em vez de reproduzir ativamente ou cheaters quem arruinar seus jogos. Xbox Live permite que os usuários desses participantes problemáticos de relatório diretamente, mas os comentários do usuário não não perfeito. Títulos podem facilmente determinar coisas simples, como players ocioso no jogo e encerrando antecipadamente e, às vezes, até mesmo podem determinar quando alguém vale. O título pode enviar comentários em uma ampla variedade de momentos de comentários, que ajudará a melhorar a experiência de todos os jogadores BOM.

### <a name="determining-the-correct-feedback-type"></a>Determinando o tipo correto de comentários
O sistema de reputação tem muitos tipos de comentários que se destinam para capturar as várias maneiras que um usuário poderá justificar comentários. Eles são totalmente listados na tabela 1 abaixo. A maioria deles é negativa, mas é possível melhorar a reputação de um usuário com comentários positivos também.

Sistema do Xbox da interface do usuário fornece uma maneira de jogadores enviar comentários sobre outros usuários no jogo. Esses comentários do usuário para não significa muito peso, já que os usuários estão sujeitos a griefing entre si quando eles perdem. Títulos podem complementar esse sistema de interface do usuário, fornecer uma interface para os usuários diretamente enviar comentários sobre o outro, mas em vez disso, Preferimos que títulos enviem comentários em nome do título em si por meio de comentários de parceiros. Comentários de parceiros são altamente confiável.

## <a name="example-partner-feedback-usage-scenarios"></a>Cenários de uso de comentários de parceiro de exemplo

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>Encerrando um jogo no meio de uma correspondência de usuário
Um player está perdendo um jogo e usa o menu do jogo para encerrar o jogo, abandonar seus colegas. Quando um título detecta esse comportamento eles relatam o comportamento usando **FairPlayQuitter.**

### <a name="idling-after-match-found-in-game"></a>Quando ocioso após a correspondência encontrada no jogo
É a correspondência com outros jogadores para reproduzir um player, mas consistentemente significa ocioso no jogo, em vez de ajudar a equipe. O título pode relatar o comportamento de player usando **FairplayIdler.**

### <a name="killing-teammates-in-game"></a>Eliminando colegas de equipe no jogo
Um player em um jogo está constantemente matando colegas de equipe por diversão. Quando um título detecta que um usuário consistentemente eliminando equipe ele pode relatar o jogador usando **FairPlayKillsTeammates.**

### <a name="title-has-community-kickvote-feature"></a>Título tem o recurso de inicialização/voto de comunidade
Um player tem sido votado por jogadores na rodada a ser removido da sessão para o comportamento ruim. Se o título remove esse jogador da sessão, ele pode relatar o usuário com **FairPlayKicked.**

### <a name="helpful-player-community-vote"></a>Voto do Player úteis da comunidade
Após algumas rodadas de um jogo, o título oferece uma opção para escolher uma pessoa que ajudou a mais. Quando um título vê essa ação pode relatar o comportamento usando **PositiveHelpfulPlayer.**

### <a name="high-quality-ugc-user-generated-content"></a>Alta qualidade UGC (conteúdo gerado pelo usuário)
Um título tem uma cena em que um player pode escolher o melhor design para um veículo. Quando um título vê essa ação pode relatar o comportamento usando **PositiveHighQualityUGC.**

### <a name="skilled-player"></a>Player qualificado
Após algumas rodadas de um jogo, o título oferece uma opção para escolher uma pessoa que é MVP do que era o jogador melhor. Quando um título vê consistenly um jogador ganha ele pode relatar o status de MVP de comportamento usando **PositiveSkilledPlayer.**

### <a name="user-reports-questionable-ugc-in-title"></a>Usuário UGC questionável no título de relatórios
Um título tem uma cena em que um jogador pode exibir designs do veículo. Um player observa um design ofensivo e deseja relatá-lo. Quando um título vê esta ação, ela poderá relatar o invasor usando **UserContentInappropriateUGC.**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>Título deseja solicitar uma revisão de vetar XBL de um Player em seu título
Gerente da comunidade de um título observou um player de baixa reputação que consistentemente está causando o problema no seu jogo. Um título pode solicitar uma política de XBL e imposição equipe de revisão usando **FairPlayUserBanRequest.**

## <a name="complete-behavior-feedback-options"></a>Opções de comentários do comportamento de conclusão
A tabela a seguir lista os tipos de comentários que você pode usar para enviar comentários do usuário em nome de seu título. O serviço de reputação é flexível e pode adicionar facilmente a novos tipos de comentários se você acredita que elas não atendem às necessidades do seu título. Informe seu gerente de conta se você gostaria de ver um novo tipo de comentários adicionado.

Tabela 1: Os comentários de parceiro de vários tipos do serviço de reputação oferece suporte.

**Tipos de comentários do Fairplay**               | **Descrição**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | Relatar um player que está intencionalmente matando seus próprios membros da equipe
FairplayCheater                           | Relatório de um jogador que certamente vale
FairplayTampering                         | Relatar um player que certamente tem meddled com o disco de jogo ou violados caso contrário, o hardware ou software de jogos
FairplayUserBanRequest                    | Um player que você acha que recebeu uma suspensão de relatório
FairplayConsoleBanRequest                 | Relatório de um console que você acha que deve ser impedido de se conectar ao Xbox Live
FairplayUnsporting                        | Relatar um player que é certamente empenhado em conduta unsportsmanlike
FairplayIdler                             | Um player que insere para múltiplos jogadores corresponde mas não é possível reproduzir ativamente de relatório
FairplayLeaderboardCheater                | Um player que certamente tem trapaceamos apareçam alta em um placar de líderes de relatório
**Tipos de comentários de comunicações**         |
CommsInappropriateVideo                   | Relatar um player que está sendo inadequado em chat com vídeo
**Tipos de comentários sobre o conteúdo gerado pelo usuário** |
UserContentInappropriateUGC               | Uma parte inadequada do conteúdo criado por um player no seu título de relatório
UserContentReviewRequest                  | Uma parte do conteúdo de relatório proativamente para que a equipe XBLPET examinará
UserContentReviewRequestBroadcast         | Relatar uma difusão proativamente para que a equipe XBLPET examinará
UserContentReviewRequestGameDVR           | Relatar um clipe GameDVR proativamente para que a equipe XBLPET examinará
UserContentReviewRequestScreenshot        | Uma captura de tela de relatório proativamente para que a equipe XBLPET examinará
**Comentários positivos**                     |
PositiveSkilledPlayer                     | Se os usuários podem votar para determinar um MVP, um player habilidoso quando determinados que o jogador merece comentários positivos de relatório
PositiveHelpfulPlayer                     | Se um jogo fornece a interface do usuário para um player relatar que o outro era útil, relatar o player úteis
PositiveHighQualityUGC                    | Se um jogo fornece o conteúdo de interface do usuário para um player complementar o relatório de conteúdo, de outro usuário positivamente

## <a name="feedback-api-calls"></a>Chamadas de API de comentários
Títulos podem usar duas estratégias para chamar o serviço de reputação. O método preferencial é para usuários de relatório na agregação de um serviço de parceiro, usando um token de serviço para autenticação. Títulos também podem informar os usuários diretamente do cliente. A API do cliente tem a tecnologia de antifraude interna que requer vários relatórios em um usuário para ser considerado válido. Ambas as APIs são em lote e podem relatar vários usuários simultaneamente.

O título pode usar as seguintes APIs do Xbox Live para enviar comentários do player de reputação:

Idioma | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

Como alternativa, o título pode usar os seguintes métodos REST diretos:

API          | URL                                                      | Requisitos de autenticação
------------ | -------------------------------------------------------- | ---------------------------------------
Serviço de POSTAGEM | https://reputation.xboxlive.com/users/batchfeedback      | S-token com declarações do parceiro e da área restrita
POSTAGEM do cliente  | https://reputation.xboxlive.com/users/batchtitlefeedback | Xtoken com declarações título e a área restrita

## <a name="feedback-object"></a>Objeto de comentários
O objeto de comentários tem a seguinte especificação para a versão mais recente, 101. Ambas as APIs de esperar que um lote de objetos a seguir.

Membro       | Tipo   | Descrição
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | objeto | Um objeto que descreve a sessão MPSD este comentário se relaciona ao, ou nulo.
feedbackType | cadeia de caracteres | O tipo de comentários. Os valores possíveis são definidos na enumeração ReputationFeedbackType
textReason   | cadeia de caracteres | O texto fornecido pelo usuário que o remetente é adicionado para explicar o motivo pelo qual os comentários foi enviado. Isso é muito importante para nossa equipe de imposição de política.
evidenceId   | cadeia de caracteres | A ID de um recurso que pode ser usado como evidências dos comentários que estão sendo enviadas, por exemplo, um arquivo de vídeo registrada durante a execução do jogo ou um item de Feed de atividades.
titleID      | String | A ID do título do título jogado; necessário somente se não estiver presente no token.
targetXuid   | String | XUID do player que você está relatando.

Exemplo:

```json
POST https://reputation.xboxlive.com/users/batchtitlefeedback
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId" : null,
            "sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Title56932",
           },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Title detected this player killing team members 19 times",
            "evidenceId": null
        }
    ]
}
```

## <a name="feedback-qa"></a>Perguntas e respostas de comentários

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>P: Pode enviar uma dica para o sistema para ajudar com as pessoas que podem estar olhando para o relatório de player?
R: Sim, e é muito útil! Use o parâmetro "textReason" para ajudar o executor de que, por fim, veremos os comentários enviados. Por exemplo, para uma prática, você pode adicionar um motivo de texto que diz "Não recebemos nenhuma entrada do usuário deste player após os primeiros cinco segundos do jogo". Esse motivo de texto pode ser muito valiosa para os agentes de imposição XBLPET, portanto, certifique-se de que o motivo de texto é útil e descritivo.

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>P: Eu deve se preocupar sobre a frequência com que posso enviar comentários em um usuário?
R: Títulos devem chamar o serviço de reputação quando eles são confiantes de que um usuário recebeu comentários. O sistema tem várias captura de segurança para evitar que títulos e os usuários sejam capazes de excesso de impacto aos usuários.

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>P: Posso ajustar o peso dos comentários que estão sendo enviados?
R: Não, o serviço de reputação determina o peso dos comentários. Estamos sempre está ajustando os pesos para tornar o sistema melhor.

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>P: Posso desfazer comentários que enviei em um usuário?
R: Não, os comentários são final. Se você acredita que seu título tem um bug e está enviando comentários incorretos, fale conosco e nós vai colocar na lista negra seu título até que você corrija o bug.

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>P: Posso ver os comentários enviados para o título da minha dos usuários?
R: Atualmente, essa informação não está disponível de autoatendimento. Você pode pedir a sua conta de Gerenciador e podemos ter dados por título para que possamos divulgar. Em breve, gostaríamos de fazer isso diretamente disponíveis para você e também mostram a mistura de usuários com baixa reputação usando seu título.

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>P: Quando eu enviar no console ou como fazer uma solicitação de revisão de veto de usuário, eu sei o que aconteceu?
R: Atualmente, as informações para a revisão são enviadas à equipe de diretiva XBL e imposição, mas no momento, você não são atualizadas sobre a revisão de vetar.

### <a name="q-is-there-a-reputation-score-per-title"></a>P: Há uma pontuação de reputação por título?
R: Não. Atualmente, há sub pontuações de reputação de fairplay, comunicações e conteúdo gerado pelo usuário, mas não por título. Poderemos adicionar que esse recurso no futuro se há uma demanda suficiente, então, informe seu gerente de conta se você quiser solicitar esse recurso.
