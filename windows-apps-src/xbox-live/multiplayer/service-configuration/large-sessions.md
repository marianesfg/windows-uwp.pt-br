---
title: Sessões grandes
description: Saiba como usar sessões grandes com a plataforma para múltiplos jogadores Xbox Live.
ms.date: 07/11/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, sessão para múltiplos jogadores, grande, jogadores recentes
ms.localizationpriority: medium
ms.openlocfilehash: dcd7b27bc0aea8b8406e7eccb420140cf32c1ed5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618541"
---
# <a name="large-sessions"></a>Sessões grandes

Se você precisar de uma sessão para múltiplos jogadores que pode lidar com mais de 100 membros, você precisará usar o que é conhecido como uma sessão grande. Esse cenário é mais comum para jogos (MMO) online com e difunde (onde a maioria dos membros é spectators), mas podem ter aplicativos para outros estilos de jogos também.

Em algumas circunstâncias, você também poderá usar sessões grandes até mesmo ao lidar com os grupos menores de jogadores. Se você quiser vários participantes para estar na mesma sessão, mas não necessariamente estar cientes uns dos outros se eles não encontram uns aos outros em jogo, você pode usar a propriedade "encontrar" de sessões grandes.

Sessões grandes não são atualmente suportadas pelo [Xbox integrado com vários participantes (XIM)](../xbox-integrated-multiplayer.md) ou pelo [Manager com vários participantes (MPM)](../multiplayer-manager.md), portanto, você deve usar as APIs de 2015 com vários participantes para usar chamadas diretas para o com vários participantes Diretório de serviço (MPSD).

Sessões grandes são tratadas ligeiramente diferente que sessões regulares:

* Contém menos informações que sessões regulares, mas são mais eficientes.
* Dá suporte a até 10.000 membros.
* Não é possível inscrever-se a uma sessão grande.
* Não há nenhum inclusão automática nas listas recentes de player para membros de uma sessão de grande.

## <a name="recent-players"></a>Jogadores recentes

Um dos recursos do Xbox Live é que quando os jogadores do Xbox Live jogos com vários participantes com novas pessoas, após o jogo poderão ver esses players em seu painel na **jogadores recentes** lista. Se um jogador tinha uma ótima experiência com um novo player em um jogo, eles querem jogar novamente com eles ou adicioná-los como um amigo. Se eles tinham uma experiência ruim com um player, que desejam evitá-los no futuro, e/ou o comportamento ruim de relatório depois que o jogo.

Com sessões regulares, Xbox Live adiciona automaticamente os jogadores na mesma sessão à lista de jogadores recentes. Se você usar sessões grandes, no entanto, você deve executar algumas etapas adicionais para garantir que as listas recente do player são preenchidas corretamente.

## <a name="set-up-a-large-session"></a>Configurar uma sessão grande

Para configurar sessões tão grandes, adicione `“large”: true` para a seção de recursos no modelo de sessão. Permite definir o `maxMembersCount` até 10.000. Um modelo de sessão, como a abaixo devem funcionar:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 2000,
            "visibility": "open",
            "capabilities": {
                "gameplay": true,
                "large": true
            },
            "timeouts": {
                "inactive": 0,
                "ready": 180000,
                "sessionEmpty": 0
            }
        },
        "custom": { }
    }
}
```

## <a name="working-with-large-sessions"></a>Trabalhando com grandes sessões

Ao gravar sessões grandes MPSD, é recomendável que você não quiser para exceder 10 gravações por segundo. Normalmente, essa é uma sessão de player de 1000 com uma gravação a cada 2 minutos, em média por player (por exemplo, ingresso/saída).

Outras propriedades não devem ser mantidas nas sessões grandes.

### <a name="associating-players-from-the-same-large-session"></a>Associando os jogadores da mesma sessão grande

Quando você recupera uma sessão grande de MPSD, a lista de membros não voltar com a resposta e, na verdade, não é possível obter a lista completa. Em vez disso, se o chamador estiver na sessão, seu registro de membro será o único na coleção "membros", rotulado como "me" (assim como na solicitação).

Isso significa que membros de clientes só poderá atualizar suas próprias entradas na sessão e se baseia no servidor a fim de fornecer um identificador comum que Xbox Live pode usar para associar os jogadores reproduzida juntos.

Há duas maneiras para indicar que as pessoas em uma sessão reproduzida juntos (para a atualização reputação e status de jogadores recente).

#### <a name="1-persistent-groups"></a>1. Grupos persistentes

Se um grupo de pessoas permanecer juntos em uma base contínua, potencialmente com pessoas que estão indo e dele, você pode dar ao grupo um nome (por exemplo, um guid – seguindo as mesmas regras de nomenclatura para sessões regulares).  Conforme cada membro e a partir do grupo, eles devem adicionar ou remover o nome do grupo para sua própria propriedade de "grupos", que é uma matriz de cadeias de caracteres:

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "groups": [ "boffins-posse" ]
                }
            }
        }
    }
}
```

#### <a name="2-brief-encounters"></a>2. Encontra breve

Se duas pessoas com uma breve encontram ocasional, o jogo em vez disso, pode usar a matriz de "encontra". Forneça cada encontrem um nome e, depois de encontrar, participantes de ambos os (ou todos) seriam gravar o nome em sua própria propriedade "encontrar":

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "encounters": [ "trade.0c7bbbbf-1e49-40a1-a354-0a9a9e23d26a" ]
                }
            }
        }
    }
}
```

Você pode usar o mesmo nome para "grupos" e "encontrar" – por exemplo, se um jogador "negocia com" um grupo, as pessoas no grupo de não precisará fazer nada (supondo que eles adicionados anteriormente o nome do grupo "grupos") e a pessoa que tivesse a encontrar seria u carregar o nome do grupo em sua lista de "encontrar". Isso fará com que o indivíduo ver todos os membros do grupo de jogadores recentes e vice-versa.

Contagem de encontra como tendo sido um membro do grupo por 30 segundos. Como a encontra é considerada eventos únicos, a matriz de "encontrar" é sempre imediatamente processada e, em seguida, limpo da sessão.  Ele nunca será exibido em uma resposta.  (A matriz de "grupos" pen drives em torno de até alterado ou removido, ou o membro deixa a sessão).
