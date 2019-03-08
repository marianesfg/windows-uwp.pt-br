---
title: Feedback (JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
description: " Feedback (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1cb4ecc12219d54af1c4ab420671f2bbfa81f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644761"
---
# <a name="feedback-json"></a>Feedback (JSON)
Contém informações de comentários sobre um player.
<a id="ID4EN"></a>


## <a name="feedback"></a>Privacidade Jurídica

O objeto de comentários tem a seguinte especificação.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| sessionRef| objeto | Um objeto que descreve a sessão MPSD este comentário se relaciona ao, ou nulo. |
| feedbackType| cadeia de caracteres | O tipo de comentários. Os valores possíveis são definidos na <b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>. |
| textReason| cadeia de caracteres| O texto fornecido pelo usuário que o remetente é adicionado para explicar o motivo pelo qual os comentários foi enviado. |
| voiceReasonId| cadeia de caracteres| A ID de um arquivo de voz fornecido pelo usuário do Kinect o remetente adicionado para explicar o motivo pelo qual os comentários foi enviada (Base-64). |
| evidenceId| cadeia de caracteres| A ID de um recurso que pode ser usado como evidências dos comentários que estão sendo enviadas, por exemplo, um arquivo de vídeo registrada durante o jogo. |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>Tipos de comentários

A coluna "Enviado pela" indica quem pode enviar os comentários.

   * "Usuário" significa que ele pode ser enviado pelo console do usando um XToken para autenticação, portanto, a API pode aceitar **SubmitFeedback**.
   * "Parceiro" significa que ela pode ser enviada por um parceiro usando um certificado de declarações, portanto, a API pode aceitar **SubmitBatchFeedback**.
   * "Privacidade" significa que apenas o serviço de privacidade SLS pode enviar os comentários.
   * "None" significa que os comentários é gerado internamente pelo serviço de reputação de SLS para auditoria e não podem ser enviado por qualquer chamador.

| Tipo| Enviado por| Observações|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| Usuário| Os usuários enviar comentários para relatório inadequado comunicações por voz de dentro de um título e do painel Xbox. |
| CommsInappropriateVideo| Usuário, parceiro| Usuários e parceiros enviam comentários para informar o vídeo inadequado de dentro de um título e do painel Xbox. |
| CommsMuted| Privacidade| Quando um usuário retira a outro jogador, privacidade envia seus comentários para o serviço de reputação. |
| CommsPhishing| Usuário| Os usuários enviar esses comentários para relatar uma mensagem de phishing. |
| CommsPictureMessage| Usuário| O serviço de caixa de entrada chama o serviço de reputação, que atualiza a reputação do remetente com base em comunicação de uma imagem e relata os comentários à equipe de imposição. |
| CommsSpam| Usuário| Os usuários enviar esses comentários para relatar uma mensagem de spam. |
| CommsTextMessage| Usuário| O serviço de caixa de entrada chama o serviço de reputação, que atualiza a reputação do remetente e relata os comentários à equipe de imposição. **Observação:** A UI Inbox deve ter um botão para permitir que os usuários sinalizar uma mensagem. |
  | CommsVoiceMessage | Usuário | O serviço de caixa de entrada chama o serviço de reputação, que atualiza a reputação do remetente com base em comunicação de uma mensagem de voz e relata os comentários à equipe de imposição.  |
  | FairPlayBlock | Privacidade | Privacidade envia seus comentários para o serviço de reputação quando um usuário bloqueie o outro jogador.  |
  | FairPlayCheater | Usuário, parceiro | Os títulos que determinam que um usuário é trapaça podem enviar esse comentário sem intervenção do usuário.  |
  | FairPlayConsoleBanRequest | Parceiro | Um parceiro envia esses comentários como uma recomendação proibir o uso de um console do Xbox Live.  |
  | FairPlayIdler | Usuário, parceiro | Títulos que determinam se um usuário significa ocioso proposital em um jogo, geralmente round após rodada, podem enviar esse comentário sem intervenção do usuário.  |
  | FairPlayKicked | Usuário, parceiro | Títulos que detectam que um usuário é votado fora de um jogo (iniciado) podem enviar esse comentário sem intervenção do usuário.  |
  | FairPlayKillsTeammates | Usuário, parceiro | Títulos que podem determinar automaticamente quando um player killls seu colega de equipe pode enviar esse comentário sem intervenção do usuário.  |
  | FairPlayQuitter | Usuário, parceiro | Os títulos que determinam que um usuário de fechar um jogo no início podem enviar esse comentário sem intervenção do usuário.  |
  | FairPlayTampering | Usuário, parceiro | Os títulos que determinam que um usuário for violado por conteúdo no disco podem enviar esse comentário sem intervenção do usuário.  |
  | FairPlayUnblock | Privacidade | Privacidade envia esses comentários para o serviço de reputação quando um usuário desbloqueia outro jogador.  |
  | FairPlayUserBanRequest | Parceiro | Um parceiro envia esses comentários como uma recomendação proibir o uso de um usuário do Xbox Live.  |
  | InternalAmbassadorScoreUpdated | Nenhuma | Este é um tipo de comentários interno não para uso por chamadores.  |
  | InternalReputationReset | Nenhuma | Este é um tipo de comentários interno não para uso por chamadores.  |
  | InternalReputationUpdated | Nenhuma | Este é um tipo de comentários interno não para uso por chamadores.  |
  | PositiveHelpfulPlayer | Usuário, parceiro | Os usuários e parceiros enviam esses comentários para enviar informações positivas sobre útil players colegas de dentro de jogos, fóruns e assim por diante.  |
  | PositiveHighQualityUGC | Usuário, parceiro | Os usuários e parceiros enviam esses comentários para indicar que os títulos devem permitir que os usuários enviem comentários positivos em UGC compartilhada de dentro do jogo, por exemplo, ajuste as configurações da Forza.  |
  | PositiveSkilledPlayer | Usuário, parceiro | Os usuários e parceiros enviam esses comentários para indicar que os títulos podem permitir que usuários votar em um MVP no final de uma sessão MPSD.  |
  | UserContentGamerpic | Usuário | Os usuários enviar esses comentários para relatar uma imagem de jogador inadequada diretamente do cartão jogador.  |
  | UserContentGamertag | Usuário | Os usuários enviar esses comentários para relatar uma marca de jogador inadequada diretamente do cartão jogador.  |
  | UserContentInappropriateUGC | Usuário, parceiro | Os usuários e parceiros enviam esses comentários para indicar que os títulos devem permitir que os usuários sinalizar inadequado UGC compartilhada de dentro do jogo, por exemplo, Lata trabalhos no Forza.  |
  | UserContentPersonalInfo | Usuário | Os usuários enviar esses comentários para relatar uma biografia e outras informações pessoais diretamente do cartão jogador.  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
{
"sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Halo556932",
  },
  "feedbackType": "CommsAbusiveVoice",
  "textReason": "He called me a doodoo-head!",
  "voiceReasonId": null,
  "evidenceId": null
}

```


<a id="ID4EOEAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EQEAC"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
