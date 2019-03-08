---
title: Constantes de modelo de sessão
description: Descreve as constantes de sistema definidas nos modelos de sessão para múltiplos jogadores Xbox Live.
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox one, para múltiplos jogadores, modelo de sessão
ms.localizationpriority: medium
ms.openlocfilehash: 5bb4c6f39bab8779b5675a7b9c81b883965676c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646901"
---
# <a name="session-template-constants"></a>Constantes de modelo de sessão

As tabelas a seguir descrevem os elementos predefinidos de um modelo de sessão para múltiplos jogadores, usando a versão do modelo de sessão 107.

## <a name="system"></a>sistema

constante de sistema  | Descrição | Valores válidos | valor padrão
--|-- | -- | --
version | A versão do modelo de sessão. | 1 - n | nenhuma
maxMembersCount | O número de slots de membro total da sessão com suporte para a atividade com vários participantes. | 1 - 100 para uma sessão normal, 101 + para uma sessão de grande | 100
visibilidade | O estado de visibilidade da sessão, que indica se outros usuários podem ver e/ou ingressar na sessão. | privado, visível, abra | abrir
inviteProtocol | Definir essa constante para "game" permite que os convidados receber uma notificação do sistema quando eles forem convidados para a sessão. | jogo, tournamentgame, bate-papo, gameparty | nenhuma
reservedRemovalTimeout  | O tempo limite de uma reserva de membro, em milissegundos. Um valor de 0 indica um tempo limite imediato. Se o tempo limite for nulo, ele será considerado infinito. | 0 - n, nulo | 30000
inactiveRemovalTimeout  | O tempo limite para um membro a ser considerado inativo, em milissegundos. Um valor de 0 indica um tempo limite imediato. Se o tempo limite for nulo, ele será considerado infinito. | 0 - n, nulo | 0
readyRemovalTimeout | O tempo limite para um membro a ser considerados prontos para, em milissegundos. Um valor de 0 indica um tempo limite imediato. Se o tempo limite for nulo, ele será considerado infinito. | 0 - n, nulo | 180000
sessionEmptyTimeout | O tempo limite para uma sessão vazia, em milissegundos. Um valor de 0 indica um tempo limite imediato. Se o tempo limite for nulo, ele será considerado infinito. | 0 - n, nulo | 0
[**capabilities**](#capabilities) | Especifica os recursos da sessão. Consulte a seção de recursos abaixo. | n/d | n/d
[**metrics**](#metrics) | Especifica um conjunto de qualidade de título definido de requisitos de serviço, como velocidade de latência e largura de banda, que os membros da sessão devem satisfazer.  | n/d | n/d
[**memberInitialization**](#memberInitialization) | Especifica os requisitos de inicialização e tempos limite são aplicados quando novos membros ingressarem na sessão. Consulte a seção de inicialização de membro abaixo. | n/d | n/d
[**peerToPeerRequirements**](#peerToPeerRequirements) | Especifica a qualidade da rede de requisitos de serviço para conexões de malha ponto a ponto. Consulte a seção de requisitos de ponto a ponto abaixo. |n/d | n/d
[**peerToHostRequirements**](#peerToHostRequirements) | Especifica a qualidade de rede dos requisitos de serviço para o par de conexões de host. Consulte o par abaixo da seção de requisitos de host. | n/d | n/d
[**measurementServerAddresses**](#measurementserveraddresses) | Especifica uma coleção dos data centers de possíveis que são usados para determinar as medidas de QoS. Consulte a seção measurementServerAddresses abaixo. | n/d | n/d
[**cloudComputePackage**](#cloudComputePackage) | ? | n/d | n/d
[**Arbitragem**](#arbitration) | Especifica os tempos limite para membros enviar os resultados de arbitragem em torneios. Consulte a seção cloudComputePackage abaixo. | n/d | n/d
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | Especifica uma lista de IDs que sempre devem ter acesso de leitura para a sessão do título. Consulte a seção broadcastViewerTitleIds abaixo. | n/d | n/d
[**ownershipPolicies**](#ownershipPolicies) | Especifica as políticas relacionadas à propriedade de sessão. Consulte a seção OwnershipPolicies abaixo. | n/d | n/d


## <a name="capabilities"></a>capabilities
Recursos são valores booleanos, opcionalmente, são definidos no modelo de sessão. Se não há recursos que são necessários, um objeto vazio 'capabilities' deve estar no modelo para evitar que recursos que está sendo especificado na criação da sessão, a menos que o título deseja recursos dinâmicos de sessão.

Recurso |  description | Valores válidos | valor padrão
-- | -- | -- | -- |
da rede | Indica se a sessão oferece suporte a conectividade ponto a ponto. Se esse valor for false, em seguida, a sessão não é possível habilitar as métricas e os membros da sessão não é possível definir suas SecureDeviceAddress. Não pode ser definida em sessões grandes. | verdadeiro, FALSO | false
suppressPresenceActivityCheck | Se for true, desativa as verificações de presença. | verdadeiro, FALSO | false
jogo | Indica se a sessão representa o jogo real, em vez de tempo de instalação/menu como um corredor ou o emparelhamento. Se for true, a sessão está no modo de jogo. | verdadeiro, FALSO | false
grande | Indica se a sessão é de grande (mais de 100 membros). Não há suporte para sessões grandes para uso com o Gerenciador de com vários participantes. | verdadeiro, FALSO | false
connectionRequiredForActiveMembers | Indica se uma conexão é necessária para um membro pode estar ativa. | verdadeiro, FALSO | false
cloudCompute | Permite que os clientes solicitem que uma instância de computação de nuvem ser alocado em nome da sessão. | verdadeiro, FALSO | false
autoPopulateServerCandidates | Calcular e definir 'serverConnectionStringCandidates' de 'serverMeasurements' automaticamente. Esse recurso não pode ser definido em sessões grandes. | verdadeiro, FALSO | false
userAuthorizationStyle | Indica se a sessão oferece suporte a chamadas de plataformas sem identidade forte de título. Esse recurso não pode ser definido em sessões grandes.</br></br>Definindo o `userAuthorizationStyle` capacidade `true` padrões a `readRestriction` e `joinRestriction`da sessão para `local` em vez de `none`. Isso significa que os títulos devem usar identificadores de pesquisa ou transferir as alças para ingressar em uma sessão de jogo.| verdadeiro, FALSO | false
crossplay | Indica que a sessão oferece suporte a play cruzada entre dispositivos de PC e Xbox One. | verdadeiro, FALSO | false
Difusão | Indica que a sessão representa uma difusão. O nome da sessão deve ser o xuid de emissora. Exige a funcionalidade "grande". | verdadeiro, FALSO | false
Equipe | Indica que a sessão representa uma equipe de torneio. Esse recurso não pode ser definido em 'grandes' ou 'jogabilidade' sessões. | verdadeiro, FALSO | false
Arbitragem | Indica que a sessão deve ser criada por uma entidade de serviço que adiciona a entrada do servidor 'arbitragem'. Não pode ser definida em sessões 'grandes', mas requer 'jogabilidade'. | verdadeiro, FALSO | false
hasOwners | Indica que a sessão tem uma política de segurança com base em determinados membros sejam proprietários. | verdadeiro, FALSO | false
pesquisável | Indica que a sessão pode ser uma sessão de destino de um identificador de pesquisa. Se o recurso 'userAuthorizationStyle' estiver definido, o recurso 'searchable' não é possível definir se o recurso 'hasOwners' não está definido. | verdadeiro, FALSO | false

Exemplo:

```json
"capabilities": {
    "connectivity": true,  
    "suppressPresenceActivityCheck": true,
    "gameplay": true,
    "large": true,
    "connectionRequiredForActiveMembers": true,
    "cloudCompute": true,
    "autoPopulateServerCandidates": true,
    "userAuthorizationStyle": true,
    "crossPlay": true,  
    "broadcast": true,  
    "team": true,   
    "arbitration": true,   
    "hasOwners": true,   
    "searchable": true  
},
```

## <a name="metrics"></a>Métricas
Se o `metrics` propriedades não forem especificadas, eles têm como padrão os valores que são necessários para atender a qualidade dos requisitos de serviço.  
Se eles forem especificados, os valores devem ser suficientes para atender a qualidade dos requisitos de serviço.
Esse elemento só será válido se a sessão tem o `connectivity` conjunto de funcionalidade.

Métrica | Descrição | Valores válidos | valor padrão
-- | -- | -- | --
latência | | verdadeiro, FALSO | Consulte a descrição
bandwidthDown | | verdadeiro, FALSO | Consulte a descrição
bandwidthUp | | verdadeiro, FALSO | Consulte a descrição
Personalizado | | verdadeiro, FALSO | Consulte a descrição

Exemplo:
```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true
},
```

## <a name="memberinitialization"></a>memberInitialization
Se um `memberInitialization` objeto estiver definido, a sessão espera que o sistema cliente ou o título para realizar a inicialização após a criação de sessão e/ou como novos membros ingressar na sessão.  
Os estágios de inicialização e tempos limite são rastreados automaticamente pela sessão, incluindo medidas de QoS se quaisquer métricas são definidas.  
Esses tempos limite substituir a reserva da sessão e os tempos limite de pronto para os membros que têm 'initializationEpisode' definido.  
Não pode ser especificado em sessões grandes.

Elemento  | Descrição | Valores válidos | valor padrão
-- | -- | -- | --
joinTimeout | Indica o número de milissegundos que o membro deve ingressar na sessão. Reservas de usuários que não conseguir ingressar são removidas.</br>**Observação:** A duração padrão é suficiente para a execução de títulos normal, mas pode levar à junção tempos limite, se um título está sendo depurado durante o fluxo MPSD. Para esses cenários, substituir e aumentar o valor padrão para a sessão.| 0 - n | 10000
measurementTimeout | Indica o número de milissegundos de um membro de sessão para carregar as medidas. Um membro que não puder ser carregado de medidas é marcado com um motivo da falha de "tempo limite".  | 0 - n | 30000
evaluationTimeout | Indica o número de milissegundos que uma avaliação externa deve carregar medidas. | 0 -n | 5000
externalEvaluation | Se for true, indica que o código do título realiza a avaliação de que uma junção com base nas medidas de QoS. O serviço com vários participantes não realiza nenhuma lógica de QoS e o título é responsável por Avançar o estágio de inicialização. Isso não é necessário normalmente com títulos. | verdadeiro, FALSO | false
membersNeededToStart | O número de membros necessário para iniciar a sessão, para inicialização episódio de zero somente. | 1 - maxMembersCount | 1

Exemplo:
```json
"memberInitialization": {
    "joinTimeout": 10000,  
    "measurementTimeout": 30000,  
    "evaluationTimeout": 5000,
    "externalEvaluation": false,
    "membersNeededToStart": 1
},
```


## <a name="peertopeerrequirements"></a>peerToPeerRequirements

requisitos de rede ponto a ponto | Descrição | valor padrão
-- | -- |--
latencyMaximum | A latência máxima, em milissegundos, entre quaisquer dois clientes. | 250
bandwidthMinimum | A largura de banda mínima em quilobits por segundo entre quaisquer dois clientes. | 10000

Exemplo:
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

emparelhar a requisitos de rede do host | Descrição | Valores válidos | valor padrão
-- | -- | -- | --
latencyMaximum | A latência máxima, em milissegundos, para o par de conexão de host. | | 250
bandwidthDownMinimum | A largura de banda mínima em quilobits por segundo para obter informações enviadas do host para o ponto a ponto. | | 100000
bandwidthUpMinimum | A largura de banda mínima em quilobits por segundo para obter informações enviadas do par para o host. | | 1000
hostSelectionMetric | Indica qual métrica é usada para selecionar o host. | bandwidthup, bandwidthdown, largura de banda e latência | latência

Exemplo:
```json
"peerToHostRequirements": {
    "latencyMaximum": 250,
    "bandwidthDownMinimum": 100000,
    "bandwidthUpMinimum": 1000,  
    "hostSelectionMetric": "bandwidthup"
},
```

## <a name="measurementserveraddresses"></a>measurementServerAddresses
O conjunto de possíveis cadeias de caracteres da conexão de servidor que deve ser avaliada. As cadeias de caracteres de conexão devem estar em letras minúsculas.
Não pode ser especificado em sessões grandes.

As cadeias de caracteres de conexão são definidas no seguinte formato:

`"<server name>" : {deviceAddress}`

Em que o endereço do dispositivo é descrito da seguinte maneira:

cadeia de caracteres de conexão de servidor | Descrição
-- | --
secureDeviceAddress | A base 64 codificados seguro de dispositivos de endereço do servidor

Exemplo:
```json
"measurementServerAddresses": {
    "server farm a": {
        "secureDeviceAddress": "r5Y="
    },
    "datacenter b": {
        "secureDeviceAddress": "rwY="
    }
},
```

## <a name="cloudcomputepackage"></a>cloudComputePackage
Especifica que as propriedades da nuvem de computação pacote para alocar. Requer que o `cloudCompute` capacidade está definida.

propriedade de computação de nuvem | Descrição
-- | -- | -- | --
titleId | Indica o título do pacote para alocar de computação de ID da nuvem.
gsiSet | Indica o conjunto GSI do pacote de computação de nuvem para alocar.
Variant | Indica a variante do pacote de computação de nuvem para alocar.

Exemplo:
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>Arbitragem
Especifica os tempos limite para o processo de arbitragem. Requer que o `arbitration` capacidade está definida. A hora de início de arbitragem é definida em uma sessão do */servers/arbitration/constants/system/startTime* elemento.

timeout | Descrição | Valores válidos | padrão
-- | -- | -- | --
forfeitTimeout | Indica o tempo, em milissegundos desde a hora de início de arbitragem que um TBD | 0 - n | 60000
arbitrationTimeout | Indica o tempo, em milissegundos desde a hora de início de arbitragem, que o resultado de arbitragem atinge o tempo limite. Esse valor não pode ser menor do que o `forfeitTimeout` valor | 0 - n | 300000

Exemplo:
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

Especifica uma matriz de IDs dos títulos que sempre devem ter acesso de leitura para a sessão de transmissão de título.

Exemplo:
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
Especifica como lidar com uma sessão quando o proprietário do último deixa a sessão. Requer que o `hasOwners` capacidade está definida.

política de propriedade | Descrição | Valores válidos | padrão
-- | -- | -- | --
Migração | Indica o comportamento que ocorre quando o proprietário do último sai da sessão. Se a política de migração é definida como "endsession", expirar a sessão. Se a política de migração é definida como "antiga", selecione o membro com a hora mais antigo de junção para se tornar o novo proprietário da sessão. | "mais antigo", "endsession" | "endsession"

Exemplo:
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```
