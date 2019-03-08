---
title: Sistema de pessoas em tempo real do Xbox
description: Saiba mais sobre o sistema de pessoas do Xbox Live.
ms.assetid: f1881a52-8e65-4364-9937-d2b8b8476cbf
ms.date: 03/19/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, social, sistema de pessoas, amigos
ms.localizationpriority: medium
ms.openlocfilehash: 39fd9283e169efa04f9c61d1c5e1d206617ab0c8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660261"
---
# <a name="xbox-live-people-system"></a>Sistema de pessoas em tempo real do Xbox

Antes de começar a integração de serviços do Xbox Live social em seu título pode ser útil para você entender o sistema que você está trabalhando. Para esse fim, descreveremos resumidamente o funcionamento do sistema Xbox Live pessoas. O sistema de pessoas dita e mantém as relações entre jogadores no ecossistema do Xbox Live. O sistema de pessoas permite para jogadores reconhecer facilmente e organizar seus contatos no Xbox Live, independentemente de estarem amigos fechados ou online conhecidos. Você pode mostrar seu nome real para as pessoas você confia e ocultá-lo daqueles que você não fizer isso. Você pode reproduzir favoritos com seus amigos para que você sempre saberá o que eles são até os primeiros enquanto você estiver online. O sistema de pessoas do Xbox Live permite que você adicione seus amigos do BOM e aqueles que forneceu bom jogos tudo em um cofre, fácil de organizar o pacote.

## <a name="one-and-two-way-relationships"></a>Uma e duas relações de forma

O sistema de pessoas permite que você tenha uma e duas relações de maneira por meio do conceito de *seguir*. Antes de você apenas poderia *amigo* outros jogadores enviando um convite e, em seguida, aguardar que eles para aceitar a solicitação amigo, resultando em uma relação de duas maneiras em que cada seguir uns aos outros. Esse processo pode levar dias, dependendo da frequência com que os jogadores marcada suas solicitações de amigo. Com a adição de uma maneira de relações, você pode *siga* um usuário e ver suas atividades online imediatamente, em vez de esperar para que eles possam responder. Neste estágio, você está uma *seguidor*. A pessoa que você tiver *seguidos* receberá ainda recebem uma notificação e terá a oportunidade de acompanhar você de volta, uma ação que irão torná-lo *amigos* em uma relação bidirecional. Relações de uma maneira de reduzir o tempo de espera para interação com o benefício adicional de permitir que você acompanhe jogadores observados que transmitir aos visualizadores ou tem obtido um seguindo para a sua habilidade no uso de concorrência.

## <a name="gametags-and-real-names"></a>Nomes de Gametags e real

Criar e manter um mapa mental do qual reais de usuário é que o nome de jogador pode se tornar uma tarefa herculean quando você tem o potencial para os amigos ilimitados e relações a seguir. Que é por isso que podemos lhe dará a opção para revelar seu nome real a jogadores sabe pessoal para que eles podem reconhecê-lo mais facilmente. Mostrar os nomes reais é opcional e deve ser aceito pelo usuário. Os nomes reais nunca são mostrados para qualquer pessoa no Xbox Live, eles são apenas mostrados às pessoas que o usuário sabe o que são expressos por meio de conexões manualmente indicadas pelo usuário ou por meio de uma rede social, que o usuário associado com a conta. Os usuários sempre serão exibidos como seu nome de jogador ao interagir com os aleatórios estranhos na tomada de correspondência ou outras experiências; Além disso, há nenhuma opção apareça como seu nome real para estranhos.

Os usuários podem controlar como eles aparecem em jogos e podem decidir que eles desejam que outras pessoas sempre vê-los como seu nome de jogador em jogos, mesmo se alguns essas pessoas têm acesso ao seu nome real. Em que o tempo que os usuários opt-na funcionalidade que permite que seu nome verdadeiro Mostrar (somente para aqueles que conhecem), são apresentados com a opção para sempre aparecem como seu nome de jogador em jogos. Selecionar esta opção significa que o nome que aparece na tela acima suas cabeças em jogo sempre será o seu nome de jogador.

## <a name="privacy-and-people-i-know"></a>Privacidade e pessoas que conheço

Dos principais recursos do sistema as pessoas é que ele fornece para relações anônimas e não anônima; os usuários podem ter o nome real de amigos, bem como os aleatórias relações de Gamertag com usuários encontrado no Xbox Live. Isso significa que há dois conjuntos diferentes de direitos que devem ser suportados: um para permitir que amigos reais ver informações confidenciais e um segundo para permitir aos usuários adicionar relações de Gamertag aleatórias, enquanto safe sensação que eles não revelam nada confidenciais .
Além dessa, os usuários podem impor restrições em partes individuais de informações sobre si mesmos, como o seu perfil, o status online e a lista de amigos. Acesso a essas informações pode ser colocado em três buckets.

- Todas as pessoas: Isso torna as informações públicas para qualquer pessoa que se depare com esse usuário.
- Amigos: Isso torna as informações disponíveis somente para aqueles que o usuário tenha considerado um amigo.
- Bloquear: Isso torna as informações inacessíveis.

Consulte a lista completa de configurações de privacidade em [xbox.com](https://account.xbox.com/Settings).

## <a name="favorite-people"></a>Pessoas favoritas

O sistema de Xbox Live pessoas fornece um sistema simples de categoria de usuário final para sua própria lista de pessoas. Os usuários podem marcar qualquer pessoa na sua lista como um favorito e as pessoas aparecerão primeiro e são uma prioridade mais alta entre os recursos. Favoritos mostrará primeiros em títulos a qualquer momento que é mostrada uma lista de pessoas. Não há nenhum limite para o número de um usuário pode ter de favoritos. Os usuários podem alternar qualquer pessoa na sua lista como um favorito, independentemente se a pessoa é um nome real ou relação Gamertag sem implicações de privacidade. Pessoas favoritas são um subconjunto da lista de pessoas do usuário e não se comporta como uma lista separada de pessoas.