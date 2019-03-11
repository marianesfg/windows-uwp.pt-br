---
title: Fluxos de atualizado de convites de jogos com vários participantes
description: Fornece fluxos atualizados para implementar o Xbox Live convida o jogo com vários participantes.
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, com vários participantes de 2015
ms.localizationpriority: medium
ms.openlocfilehash: 1092c84521271e996db0b89630d22f7e51727bfa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596881"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>Fluxos de atualizado de convites de jogos com vários participantes

Como resultado de comentários do Xbox One beta, o fluxo de experiência do usuário para o jogo com vários participantes convida foi alterado a partir do Xbox uma recuperação atualização 24, lançada em 6 de novembro de 2013. Essa é uma alteração para o **experiência do usuário (UX) apenas** e não afetará nenhum comportamento ou a funcionalidade da perspectiva de um título de jogo. Os desenvolvedores de título, não serão necessário fazer qualquer alteração no código.

## <a name="summary-of-changes"></a>Resumo das alterações

-   Toast o convite inicial foi alterado de "Adicionar minha festa" para "&lt;título do jogo&gt; vamos mexer" e agora tem um botão que permite aos usuários iniciar o jogo e ir diretamente para o jogo.

-   O aplicativo de terceiros não é ajustado por padrão, quando o usuário escolhe o novo "&lt;título do jogo&gt; vamos mexer" opção. Essa alteração também é feita para que o usuário pode ir diretamente para o jogo.

-   Uma nova notificação do sistema foi adicionada no lado do remetente que diz "adicionando \[ *número* \] amigos ao jogo". Isso deixa claro que os convites foram enviadas quando uma sessão de jogo está associada com a parte de um usuário.

Os fluxos de experiência do usuário detalhadas são descritos nos exemplos a seguir. Cada tabela mostra um fluxo de exemplo para dois usuários, David e Laura. Esses fluxos são exibidos em cada coluna e ocorrem em paralelo. O <b style="background-color: #FFFF00">texto realçado</b> mostra os ajustes que foram feitos de fluxos de experiência do usuário anteriores.

## <a name="inviting-users-from-within-a-game"></a>Convidar usuários de dentro de um jogo

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
David é no corredor com vários participantes para um jogo que ele está em execução.<p><br>David escolhe <b>convidar um amigo</b>.<p><br>David seleciona Laura.<p><br>Toast é exibida indicando que um convite foi enviado. | Laura está jogando diferentes.
    </td>
    <td style="border-bottom:solid 1px #fff">
Laura está jogando diferentes.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Toast pop-up que indica um convite do David, e <b style="background-color: #FFFF00">exibe o nome do jogo e o ícone</b>. O aplicativo de terceiros não auto-snap (.) <p><br>
No Centro de notificação, <b style="background-color: #FFFF00">Laura pode escolher a inicialização e aceitar o convite</b>, <b>aceitar o convite</b>, ou <b style="background-color: #FFFF00">Convide de declínio</b>.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">Caso 1: Laura seleciona lançamento e aceitar o convite</b> (Isso é uma nova opção)</td>
  </tr>
  <tr>
    <td>
Toast é exibida indicando que Laura ingressou terceiros de David.             
    <p><br>
David inicia o jogo de lobby com vários participantes.                              
    <p><br>
    <b style="background-color: #FFFF00">Toast é exibida indicando que um jogo convite foi enviado para Laura.</b>
    </td>
    <td>
O jogo inicia e o aplicativo de terceiros não se ajusta.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>Caso 2: Laura seleciona Aceitar convite</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">Laura se junta ao grupo.</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">Toast é exibida indicando que um jogo convite foi enviado para Laura.</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>Toast é exibida indicando que um jogo foi encontrado para a parte.
    <p><br>
No Centro de notificação, Laura pode escolher: <ul>
    <li>   <b>Aceite convite para jogo:</b> Lançamentos de jogos.
    <li>   <b>Recuse convite para jogo:</b> Não há jogos inicia. Laura ainda está em parte e receberá subsequentes convites do jogos.         
    <li>   <b style="background-color: #FFFF00">Deixe a terceiros: Não há jogos inicia. Laura é removida da parte.</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>Fluxo de convite no jogo: Em uma parte e títulos de alternância

<table>
  <tr>
    <td>
Jogar um jogo em conjunto.
    <p><br>
David alterna para um jogo diferente e vai para o menu com vários participantes.
     <p><br>
A interface do usuário do Xbox sistema solicita David se ele gostaria de mudar sua parte para o novo jogo, ao qual ele pode responder <b>Yes</b> ou <b>não</b>.
    </td>
    <td style="text-align:top">
Jogar um jogo em conjunto.
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Caso 1: Sim</b>
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
Terceiros é fornecido para o novo título.
    <p><br>
Lobby para múltiplos jogadores, David será iniciado o jogo.
    <p><br>
    <b style="background-color: #FFFF00">Toast é exibida indicando que foi enviado um convite para jogo para Laura.
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>Toast é exibida indicando que um jogo foi encontrado para a parte.
    <p><br>
Do Centro de notificação, Laura pode escolher: <ul>
     <li><b>Aceitar convite jogo</b>: Novos lançamentos de jogos <li><b>Recuse convite para jogo:</b> Nenhum jogo for iniciado, mas Laura está ainda em parte e irá receber convites de jogos subsequentes.
     <li><b style="background-color: #FFFF00"><b>Deixe a terceiros:</b> Não há lançamentos de jogos e Laura é removido da parte.</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Caso 2: Não
    </td>
  </tr>
  <tr>
    <td>
Parte não é fornecido para o novo título.
David é reproduzido no modo de vários participantes sem membros de sua parte.
David ainda está em parte.
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>Convidar fluxo em casa

<table>
  <tr>
    <td>
David está navegando <b>página inicial</b>e em seu <b>amigos</b> lista, ele vê que Laura está online.
    <p><br>
David escolhe convidar Laura para sua parte. Toast é exibida indicando que o convite é enviado. O aplicativo de terceiros é ajustado para David.
    </td>
    <td>
Laura é jogar um jogo.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Toast é exibida indicando que a David convidou Laura para sua parte.
    <p><br>
No Centro de notificação, Laura tem a opção de <b>aceite o convite</b>.
    <p><br>
Quando Laura aceita, o aplicativo de terceiros se ajusta.                                                                         </td>
  </tr>
  <tr>
    <td>
Toast é exibida indicando que Laura ingressou o terceiros.
    <p><br>
David e Laura discutem qual jogo que desejam reproduzir. David inicia o jogo e entra no modo de jogo com vários participantes.
    <p><br>
Jogo ou fornecerá uma opção para convidar amigos ou pull automaticamente os membros de terceiros.
    <p><br>
    <b style="background-color: #FFFF00">Toast é exibida indicando que um jogo convite foi enviado.</b>
    </td>
    <td>
Toast é exibida indicando que foi encontrado um jogo do participante.
    <p><br>
No Centro de notificação, Laura pode: <ul>
    <li>   <b>Aceite convite para jogo:</b> Lançamentos de jogos <li>   <b>Recuse convite para jogo:</b> Não há lançamentos de jogos, Laura é ainda estiver na parte e receberá convites subsequentes.
    <li>   <b style="background-color: #FFFF00">Deixe a terceiros: Nenhum jogo for iniciado, Laura é removido da parte.</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>Mais informações sobre a torrada jogo convite enviado

O **convidar jogo enviadas** toast só aparecerá na primeira vez em que uma sessão de jogo é estabelecida com membros de entidade remota. Solicitações subsequentes, enviadas aos membros da parte remota não gerará esta notificação do sistema. Isso impede que o usuário recebam com repetido **jogo convidar enviado** notificações do sistema se o título faz várias chamadas para **PullReservedPlayersAsync**.

**Observação** a prática recomendada é adicionar todos desejado amigos como reservado e, em seguida, chame **PullReservedPlayersAsync** apenas uma vez.
