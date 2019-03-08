---
title: Estatísticas dos jogadores
description: Saiba como definir as estatísticas do player no Xbox Live.
ms.assetid: 5ec7cec6-4296-483d-960d-2f025af6896e
ms.date: 07/30/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, estatísticas de player, placares de líderes
ms.localizationpriority: medium
ms.openlocfilehash: e8b88f3db93f98789ada3c3b3dfe74195925657e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597301"
---
# <a name="player-stats"></a>Estatísticas de Player

Conforme descrito em [visão geral da plataforma de dados](../data-platform/data-platform.md), estatísticas são partes importantes de informações que você deseja acompanhar sobre um player. Por exemplo *capturas de cabeça* ou *tempo de visão geral mais rápido*. Estatísticas são usadas para gerar os placares de líderes em vários cenários que permitirá que os jogadores a ser comparado a suas habilidades com seus amigos e esforço e cada jogador na comunidade de um título. Configurado stats aparecem em um título [jogo Hub](../data-platform/designing-xbox-live-experiences.md) placar de líderes em que um jogador verá como de classificação em relação a seus amigos que também reproduziu o título. As estatísticas que aparecem em Hub de um título de jogo são chamadas **recurso Stats**, também conhecido como **Hero Stats**. Além de aparecendo no Hub do jogo, desde a atualização do Xbox One 1806 estatísticas em destaque podem também periodicamente aparecer em blocos fixados de conteúdo que os usuários podem adicionar ao modo de exibição Home. Isso permite o acesso rápido à placares de líderes social para diretamente da exibição da página inicial. É importante lembrar que as estatísticas em destaque são um dos vários itens possíveis exibidos em blocos fixados de conteúdo, portanto, os usuários talvez não sempre vê-los se os serviços de conteúdo da Microsoft determinam que outro item de conteúdo pode ser mais atraente.

## <a name="stats-2013-and-2017"></a>Estatísticas de 2013 e 2017

Atualmente, há duas implementações para estatísticas para Xbox Live, estatísticas de 2013 e estatísticas de 2017. Ambas estão disponíveis para ID@Xbox e parceiros desenvolvedores gerenciados. Os desenvolvedores de programa de criadores do Xbox Live só podem usar estatísticas 2017 e portanto, podem ignorar estatísticas de 2013.

Os desenvolvedores de programa de criadores do Xbox Live podem pular para a [documento estatísticas 2017](stats2017.md).

Essas duas implementações operam em princípios fundamentalmente diferentes. Ao usar as estatísticas de 2013, você envia **eventos** para o serviço Xbox Live que contém certas informações sobre uma ação que um usuário executou. As informações nesses **eventos** é usado para atualizar estatísticas de forma adequada. Estatísticas de 2013 o serviço controlar e atualizar todos os seus valores de estatísticas, tornando-o a fonte de verdade para valores de estatística para um player ou o grupo de jogadores. Para contraste, no Stats 2017 você enviará o real valor stat em si para que o servidor a ser usado. 2017 de estatísticas, que o servidor faz pouca ou nenhuma validação no valor enviado para ele e então, cabe a seu título para controlar os valores corretos de stat e ser a fonte de verdade para valores de estatística. É recomendável que você acompanhe e armazena suas estatísticas na nuvem com o [plataforma de armazenamento do Xbox Live](../storage-platform/storage-platform.md) se você decidir implementar 2017 de estatísticas. Você pode pensar em 2017 estatísticas como um serviço de relatório. Enviar o stat correto para o seu jogo para o servidor, stat será, em seguida, ficam no servidor e esperar para ser exibido na solicitação ou atualizado.

## <a name="a-brief-history"></a>Um breve histórico

Com o lançamento do Xbox One, Xbox Live introduziu um novo stats controlada por evento modelo, estatísticas de 2013 do Player. Isso ofereceu uma variedade de benefícios – um único evento de um jogo pode atualizar dados para vários recursos Xbox Live, como placares de líderes e conquistas; Configuração do Xbox Live reside no servidor, em vez de no cliente; e muito mais. Nos anos após o lançamento do Xbox One, ouvimos de perto os comentários de desenvolvedor de jogos e os desenvolvedores solicitadas consistentemente um serviço de estatísticas mais simplificado que seria permitir que eles ignorem a complexidade que vem com um evento controlado por sistema, bem como permitir eles usem alguma estatística que já estavam usando de métodos de acompanhamento. Com base nos comentários do desenvolvedor foi criada uma nova versão de simplificada de estatísticas que seriam colocar o controle da lógica de estatísticas de novamente nas mãos do desenvolvedor. Que o sistema é Stats 2017, um serviço que simplesmente pega o valor passado para ele pelo título oferecendo aos desenvolvedores controle a lógica de como um valor de stat é determinado.

## <a name="how-stats-are-handled-in-2013-and-2017"></a>Como as estatísticas são tratadas em 2013 e 2017

Vamos dar uma olhada em como configurar e atualizar estatísticas para 2013 e 2017. Vamos supor que vamos fazer estatísticas do alguns genérico rpg e deseja manter o controle de monstros eliminados.

Em 2013 Stats seu título enviaria uma *evento* que contém informações sobre uma ação executada por um player. Neste evento a ação será derrotará um orc enquanto o player tinha uma gumes equipada. Algumas das informações contidas neste evento podem ser que uma ação slay foi executada, a slayed coisa era um orc, o tipo de combater era melee e as armas usada foi uma gumes. Estatísticas 2013 executará essas informações por meio de um número de regras configuradas por você, o desenvolvedor, no [Portal de desenvolvedor do Xbox (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) ou [Partner Center](https://partner.microsoft.com/dashboard), e a atualização de estatísticas, também é configurada por você, com base em o evento. Em 2013 estatísticas o serviço será manter o controle de qual deve ser o valor para slaying estatísticas. O orc slay com evento gumes foi possível atualizar várias estatísticas, como, número de acaba com, número de orcs slain e número de elimina gumes.

Estatísticas de 2017, você enviará os valores reais para estatísticas. Para o orc slay com exemplo gumes, seu título manter o controle do número de orcs slain individualmente, acaba com a espada e acaba com geral e enviar o serviço o número atualizado para cada estatística. O serviço tem as verificações de validação mínima para certificar-se de que você está enviando um número que faz sentido, portanto, é absolutamente até seu título enviar-na estatística correta. Embora você possa usar o serviço de estatísticas de 2017 para recuperar os valores de estatísticas no início de uma sessão de jogo, você não deve usar o serviço de estatísticas de 2017 para confirmar se o valor de uma estatística enquanto a sessão está em andamento.

Abaixo você pode ver uma ilustração de como funcionam os dois tipos de estatísticas.
![Ilustração de diferença de estatísticas](../images/stats/Stats2013-7DiagramColored.jpg)

## <a name="a-few-more-notes"></a>Algumas observações mais

Para ID@Xbox e desenvolvedores de parceiro gerenciado lá estão algumas diferenças mais, você deve estar atento ao decidir entre 2013 Stats e estatísticas de 2017 do título.

Estatísticas de 2013 permite mais modos de exibição de placar de líderes.
Estatísticas de 2013 permite que mais facilmente fazer uma busca detalhada nos dados meta das estatísticas. Em nosso orc destruir a estatística geral foi mantendo o controle do número de acaba com de exemplo. Estatísticas de 2013 permite fazer uma busca detalhada em suas estatísticas para o que foi interrompido e com quais armas, bem como quaisquer outro kill definindo as informações que você pode configurar.

Estatísticas de 2013 tem um stat nativo de minutos jogados. Estatísticas de 2013 permite que acesso ao tempo de reprodução de um jogador no jogo sem configurar a estatística. Qualquer estatística de reprodução tem que ser acompanhado pelo seu título em estatísticas de 2017.

Estatísticas de 2013 tem uma frequência de atualização quase em tempo real.
O sistema de eventos de estatísticas 2013 atualiza suas estatísticas quase instantaneamente. Em estatísticas de 2017, há um tempo de espera de cinco minutos antes de estatísticas atualizadas são liberadas para o serviço. O desenvolvedor pode liberar as estatísticas manualmente, mas ainda será limitado a um mínimo de 30 segundos entre liberações.

Estatísticas de 2013 pode dar suporte a outros serviços do Xbox Live.
Com estatísticas de 2013, você pode usar estatísticas para desbloquear as conquistas e tomar decisões de emparelhamento. Estatísticas de 2017 só é usado para produzir os placares de líderes.

## <a name="further-reading"></a>Leitura adicional

Para obter uma explicação mais detalhada das estatísticas de 2013, você pode ler o [apenas a documentação sobre estatísticas de 2013 do parceiro](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/user-stats).
Para uma explicação mais detalhada sobre estatísticas de 2017, leia as [Stats 2017 documentação](stats2017.md).