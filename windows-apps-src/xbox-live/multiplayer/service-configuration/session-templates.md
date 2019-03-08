---
title: Modelos de sessão multijogador
description: Saiba mais sobre modelos de sessão para múltiplos jogadores Xbox Live.
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox one, para múltiplos jogadores, modelo de sessão
ms.localizationpriority: medium
ms.openlocfilehash: 0bbe4f6a3afe2d39fb18b4d4bad13e2aa91d246e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599431"
---
# <a name="multiplayer-session-templates"></a>Modelos de sessão multijogador

Este tópico fornece uma visão geral dos modelos de sessão para múltiplos jogadores e fornece vários exemplos de modelos que você pode copiar e modificar de suas sessões com vários participantes.

Um modelo de sessão para múltiplos jogadores é um plano que é usado para criar uma sessão para múltiplos jogadores. Todas as sessões devem ser criadas com base em um modelo predefinido. Um modelo define constantes que serão o mesmo para qualquer sessão que é criado a partir do modelo. Depois que um jogo cria uma sessão de um modelo, o jogo pode adicionar e modificar dados adicionais para a sessão, mas não é possível modificar as constantes que foram definidas no modelo.

 Para obter mais informações, consulte [visão geral da sessão](../multiplayer-appendix/mpsd-session-details.md).

A lista de modelos de sessão que se aplicam a um identificador de configuração de serviço (SCID), bem como o conteúdo dos modelos de sessão específica, pode ser recuperada do diretório de sessão com vários participantes (MPSD).


## <a name="about-session-templates"></a>Sobre modelos de sessão

Um modelo de sessão usa o mesmo formato que uma solicitação HTTP PUT para criar ou modificar uma sessão. A diferença é que o modelo é limitado às constantes (não há membros, servidores ou propriedades). As constantes podem ser constantes qualquer sessão, incluindo uma seção personalizada e a gama completa de constantes de sistema.

### <a name="session-template-versions"></a>Versões de modelo de sessão

Os modelos de sessão definidos neste tópico são construídos usando a versão do modelo de contrato 107. Ao usá-los para criar um novo modelo, certifique-se de que 107 for inserida como a versão do contrato.

Se você usar XSAPI e assistir as solicitações resultantes no depurador, você poderá observar que as solicitações de usam a versão do modelo de contrato 105. MPSD efetivamente "atualizações" essas solicitações para a versão 107 em tempo de execução.

> **Observação:** É permitido usar uma versão de contrato diferente na solicitação da que é usada no modelo de sessão.

Se necessário, você pode alterar um modelo de sessão da versão 104/105 para versão 107. Para obter instruções, consulte Migrando as instruções em [comuns problemas quando adaptar seus títulos para 2015 Multiplayer](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md).


## <a name="session-template-default-values"></a>Valores de padrão de modelo de sessão

Cada sessão criada de um modelo de sessão é iniciada como uma cópia do modelo. Valores que não inclui o modelo podem ser fornecidos no momento da criação da sessão. Valores padrão são fornecidos em alguns casos, quando nenhum outro valor é definido. Por exemplo, o conjunto padrão de tempos limite para a versão do contrato 107 é:

```json
    {
      "constants": {
        "system": {
          "reservedRemovalTimeout": 30000,
          "inactiveRemovalTimeout": 0,
          "readyRemovalTimeout": 180000,
          "sessionEmptyTimeout": 0
        }
      }
    }
```
Você pode forçar um valor para permanecer não definidas pela especificação de null. Isso substitui qualquer configuração padrão e impede que o valor que está sendo definida no momento da criação da sessão. Por exemplo, para remover o vazio tempo limite da sessão, permitindo que as sessões continuar indefinidamente, até mesmo sem nenhum membro, adicione o seguinte para o modelo de sessão:
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **Importante:** Constantes que são definidas por meio de um modelo não podem ser alterados por meio de gravações para MPSD. Para alterar os valores, você deve criar e enviar um novo modelo com as alterações necessárias.


## <a name="example-session-templates"></a>Modelos de sessão de exemplo
Esta seção mostra vários exemplos de modelos de sessão para diferentes finalidades e topologias de rede. Examine cada modelo antes de escolher aquele mais adequado para seu cliente. Você pode copiar e colar o modelo em sua configuração de serviço, potencialmente depois de fazer as alterações necessárias.

### <a name="standard-lobby-session"></a>Sessão lobby padrão
Você pode usar o modelo a seguir como um modelo de início para criar uma sessão de lobby para o seu jogo:

* Alterar o `maxMembersCount` valor para o número máximo de players que você deseja dar suporte em sua sessão do corredor.  
* Se o título não dá suporte a jogadores de diferentes plataformas (como um Xbox One e um PC) juntos em execução, você pode remover o `crossPlay` elemento.  
* Você pode alterar outros valores, mas os valores a seguir são bons valores para começar se você não tiver certeza de que você precisa.


```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            },
        },
        "custom": {}
    }
}
```

### <a name="standard-game-session-without-matchmaking"></a>Sessão de jogo padrão sem emparelhamento
Você pode usar o modelo a seguir como um modelo de início para criar uma sessão de jogo para o seu jogo, se o seu jogo não inclui o relacionamento de pessoas anônimo e não requer mais de 100 membros.

Observe que apenas os novos valores especificados do modelo de sessão lobby padrão são os seguintes:
* `constants.system.inviteProtocol : "game"`
* `constants.system.capabilities.gameplay : true`

```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "inviteProtocol": "game",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "gameplay" : true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            }
        },
        "custom": {}
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>Adicione emparelhamento para um modelo de sessão de jogo, permitindo que o serviço com vários participantes lidar com qualidade de serviço verifica.

Você pode adicionar o seguinte `memberInitialization` elemento json ao seu modelo de jogo para adicionar suporte para que haja correspondência.

Quando você cria seu hopper SmartMatch, use este modelo como o modelo de sessão de destino para o hopper.

```json
{
   "constants": {
        "system": {
            "memberInitialization": {
               "joinTimeout": 20000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        }
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>Adicione o emparelhamento para um modelo de sessão de jogo, onde a qualidade de serviço verifica são tratados por um datacenter gerenciado do título.



```json
{
   "constants": {
        "system": {
            "peerToHostRequirements": {  
                "latencyMaximum": 250,
                "bandwidthDownMinimum": 256,
                "bandwidthUpMinimum": 256,
                "hostSelectionMetric": "latency"
            },
            "memberInitialization": {
               "joinTimeout": 15000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}
```

### <a name="basic-session-template-for-client-server-game-session"></a>Modelo de sessão básica para a sessão de jogo de cliente-servidor

Você pode usar este modelo para um título que não faz a comunicação ponto a ponto e não usa o Xbox Live Compute, mas em vez disso, tem clientes a se conectar a um servidor hospedado de terceiros.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true,
          },
        },
        "custom": {}
      }
    }
```

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>Modelo da sessão de tíquete lobby/SmartMatch das redes baseadas em ponto a ponto

Use este modelo para a criação de uma sessão de corredor ou uma sessão de tíquete SmartMatch que só deve ser usado para enviar um grupo de jogadores em emparelhamento. É o modelo não deve ser usado para configurar uma sessão de jogo. Ele destina para uso por clientes que usam uma topologia de rede ponto a ponto ou host do par.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 10,
          "visibility": "open",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
          },
          "memberInitialization": {
            "membersNeededToStart": 1
          },
        },
        "custom": {}
      }
    }
```

### <a name="quality-of-service-qos-templates"></a>Qualidade dos modelos de serviço (QoS)

Se o cliente está usando para que haja correspondência e avaliando o QoS, você deve adicionar algumas constantes para o modelo de sessão para informar MPSD para coordenar com o cliente para gerenciar usuários de ingressar na sessão. Essa coordenação valida a qualidade do estado de conexão antes de informar aos usuários que o jogo está pronto para começar. No caso de jogos de cliente-servidor, a coordenação valida a qualidade da conexão antes de um grupo de jogadores insere o relacionamento de pessoas.

#### <a name="peer-to-host-game-session-template-with-qos"></a>Modelo de sessão de jogo de host do par com QoS

O exemplo a seguir apresenta um modelo de sessão de jogo de host do par com QoS.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectivity": true,
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true
          },
          "memberInitialization": {
            "membersNeededToStart": 2
          },
          "peerToHostRequirements": {
            "latencyMaximum": 350,
            "bandwidthDownMinimum": 1000,
            "bandwidthUpMinimum": 1000,
            "hostSelectionMetric": "latency"
          }
        },
        "custom": { }
      }
    }
```

#### <a name="peer-to-peer-game-session-template-with-qos"></a>Modelo de sessão de jogo peer-to-peer com QoS

O exemplo a seguir é um exemplo de um modelo de sessão de jogo peer-to-peer com QoS.
```json
    {
    "constants": {
      "system": {
        "version": 1,
        "maxMembersCount": 12,
        "visibility": "open",
        "inviteProtocol": "game",
        "capabilities": {
          "connectivity": true,
          "connectionRequiredForActiveMembers": true,
          "gameplay" : true
        },
        "memberInitialization": {
          "membersNeededToStart": 2
        },
        "peerToPeerRequirements": {
          "latencyMaximum": 250,
          "bandwidthMinimum": 10000
        }
      },
      "custom": { }
     }
    }
```

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>Modelo de sessão do cliente-servidor (Xbox Live Compute) lobby/compatibilidade com o QoS

Use este modelo para criar uma sessão de corredor ou uma sessão de emparelhamento usando QoS. Este modelo não se destina a ser usado para configurar uma sessão de jogo.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "memberInitialization": {
            "membersNeededToStart": 1
          }
        },
        "custom": {}
      }
    }
```

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>Modelo de sessão para crossplay entre Xbox One e Windows 10

Use este modelo para habilitar crossplay para múltiplos jogadores entre Xbox One e Windows 10. A capacidade de userAuthorizationStyle permite acesso ao Windows 10. O recurso opcional crossPlay significa que seu título dá suporte a interações, como convites e junção em andamento entre plataformas.
```json
    {
      "constants": {
        "system": {
          "capabilities": {
            "crossPlay": true,
            "userAuthorizationStyle": true
          },
        },
        "custom": {}
      }
    }
```
