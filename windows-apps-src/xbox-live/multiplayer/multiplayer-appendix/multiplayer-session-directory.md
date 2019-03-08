---
title: Diretório de sessão para múltiplos jogadores
description: Saiba mais sobre o diretório de sessão para múltiplos jogadores em tempo real do Xbox (MPSD).
ms.assetid: a9b029ea-4cc1-485a-8253-e1c74184f35e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, mpsd, diretório de sessão para múltiplos jogadores.
ms.localizationpriority: medium
ms.openlocfilehash: 1681fe59533ebaf0db197efb95ca46b828846f5d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662541"
---
# <a name="multiplayer-session-directory-mpsd"></a>Diretório de sessão multijogador (MPSD)

Este artigo contém as seguintes seções:

* Visão geral MPSD
* MPSD alterar o tratamento de notificação e desconectar de detecção
* Identificadores MPSD para sessões
* Sincronização de atualizações de sessão
* Chamar MPSD
* Modelos de sessão MPSD
* Gerenciador de sessão para múltiplos jogadores

## <a name="session-overview"></a>Visão geral da sessão

### <a name="what-is-mpsd"></a>O que é MPSD?

Diretório de sessão para múltiplos jogadores (MPSD) é um serviço funcionando na nuvem Xbox Live que centraliza os metadados do sistema com vários participantes de um jogo em vários clientes. Ele é encapsulado pela **MultiplayerService classe**.

MPSD permite que os títulos compartilhar as informações básicas necessárias para se conectar a um grupo de usuários. Isso garante que a funcionalidade de sessão é sincronizado e consistente. Ele coordena com o sistema de operacional de shell e console no envio/aceitando convites e no que estão sendo unidas por meio de cartão de jogador.


### <a name="mpsd-sessions"></a>Sessões MPSD

Uma sessão MPSD é representada pela **MultiplayerSession classe** quanto o cenário em que uma ou mais pessoas usam um jogo. Uma sessão é armazenada por MPSD como um documento JSON seguro que residem na nuvem do Xbox Live. Especificamente, uma sessão MPSD tem as seguintes características:   É criada e gerenciada por títulos.

-   Tem um URI exclusivo. Para obter mais informações, consulte **URIs do diretório de sessão**.
-   Permite a conectividade entre usuários, chamados de membros da sessão.
-   Armazena dados úteis para possibilitar o jogo executar, por exemplo, atributos por membro, configurações do jogo, informações e as informações de servidor de jogo de inicialização.

MPSD dá suporte a diversas variações de sessão para uso na configuração de jogos com vários participantes. Cada sessão contém identificadores de usuário do Xbox jogadores (XUIDs) e proteger os dados de endereço de associação de dispositivo.

-   Sessão de jogo, usado como o padrão para o jogo. Uma sessão de jogo pode ser peer-to-peer, ponto a ponto para o host, servidores de mesmo nível ou híbrida desses tipos.
-   Sessão de tíquete, uma sessão de auxiliar usada para controlar o estado de uma correspondência durante a compatibilidade. Ele geralmente também é uma sessão de lobby e, às vezes, pode ser uma sessão de jogo. Ver [SmartMatch cruzamento](smartmatch-matchmaking.md).
-   Sessão de destino, uma sessão de auxiliar criada durante o relacionamento de pessoas para representar a jogabilidade correspondente. Ele também quase sempre é uma sessão de jogo. Ver [SmartMatch cruzamento](smartmatch-matchmaking.md).
-   Sessão lobby, uma sessão de auxiliar usada para acomodar os jogadores convidados que estão aguardando para ingressar em uma sessão de jogo. Muitos títulos criar uma sessão de lobby e uma sessão de jogo. Para obter mais informações, consulte **players de gerenciamento em seu título**.

## <a name="mpsd-change-notification-handling-and-disconnect-detection"></a>MPSD alterar o tratamento de notificação e desconectar de detecção

MPSD permite que os clientes se conectem a ele usando o soquete da web em tempo real de atividade do serviço. A conexão é usada para:

-   Enviar notificações breves (shoulder taps) quando ocorrem alterações de sessão, com base em assinaturas de evento que títulos iniciar.
-   Detecte desconexões de usuário.
-   Definir os usuários como inativa e, subsequentemente, removê-los da sessão, com base na detecção de desconexão.


### <a name="making-user-connections"></a>Estabelecendo conexões de usuário

A biblioteca XSAPI gerencia a conexão entre o cliente e MPSD. O título primeiro chama o **MultiplayerService.EnableMultiplayerSubscriptions método**. Esse método informa XSAPI que o cliente pretende usar uma conexão de atividade em tempo real para fins de com vários participantes. Em seguida, quando o título faz sua primeira chamada para o **método MultiplayerService.WriteSessionAsync** ou o **MultiplayerService.WriteSessionByHandleAsync método**, com o usuário atual definido como ativo estado, uma conexão é criado e vinculado a MPSD.

| Observação                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------------------------|
| Para habilitar as notificações de sessão e desconexões de detectar o modelo de sessão tem que definir o connectionRequiredForActiveMembers como true. |

| Observação                                                                                                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Em versões anteriores da XSAPI, títulos de chamada a **RealTimeActivityService.ConnectAsync método** para criar conexões de usuário para o serviço de atividade em tempo real. Para Multiplayer 2015, este método não faz nada, e a conexão é criada sob demanda. |

### <a name="subscribing-for-session-changes"></a>Inscrever-se para alterações de sessão

MPSD usa um toque"the shoulder" como uma notificação leve que algo interessante foi alterado. O título deve recuperar o recurso modificado para determinar a natureza exata da alteração. Com as assinaturas habilitadas, o título pode assinar para shoulder toca nas alterações de sessão com uma chamada para o **MultiplayerSession.SetSessionChangeSubscription método**. Consulte [como: Assine para receber notificações de alteração de sessão MPSD](multiplayer-how-tos.md).


### <a name="handling-shoulder-taps"></a>Tratando os ombro toca

Quando uma alteração em uma sessão corresponde a assinatura do título para a sessão, MPSD notifica o título de alteração usando o **RealTimeActivityService.MultiplayerSessionChanged evento**. O título deve recuperar a sessão e comparar a versão recuperada da sessão com o modo de exibição em cache anterior e execute ações adequadamente.


### <a name="handling-notifications-of-connection-state-changes"></a>Manipulando notificações de alterações de estado de Conexão

O título também pode ser notificado sobre alterações na integridade do que a conexão para MPSD. Essas alterações de sinal de dois eventos: * * RealTimeActivityService.MultiplayerSubscriptionsLost evento * * – acionado quando a conexão do título para MPSD usando o serviço de atividade em tempo real é perdida. Quando esse evento ocorre, o título deve desligar o com vários participantes.
-   * * Evento RealTimeActivityService.ConnectionStateChanged * * – disparado após uma alteração temporária na integridade de conexão do título para o serviço de atividade em tempo real. O título não é necessário realizar nenhuma ação após o recebimento desse evento, mas pode ser útil usar o evento para fins de diagnóstico.


### <a name="disconnecting-clients"></a>Desconexão de clientes

Desconectar de clientes para o título do MPSD quando o título desabilita as notificações com uma chamada para o **RealTimeActivityService.DisableMultiplayerSubscriptions método**. Logo após esta chamada, o **MultiplayerSubscriptionsLost** evento é acionado, indicando que um cliente foi desconectado de MPSD.

| Observação                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Em versões anteriores de vários jogadores, chamado de títulos a * * método RealTimeActivityService.Disconnect * * para se desconectar do serviço de atividade em tempo real. Para Multiplayer 2015, esse método não fará nada. A desconexão ocorre automaticamente após **DisableMultiplayerSubscriptions** é chamado, se não existem usuários de que a conexão de soquete da web, por exemplo, uma assinatura do serviço de atividade em tempo real para a presença. |


### <a name="disconnect-detection"></a>Desconectar de detecção

MPSD usa seu desconectar o recurso de detecção para saber rapidamente quando um usuário se desconecta de maneira brusca. Uma desconexão anormais pode ocorrer quando houver falha na rede de um jogador, ou quando a falha de um título. MPSD altera o estado de desconectado do player do Active Directory como inativa e notifica outros membros da sessão da alteração conforme apropriado, com base nas assinaturas dos membros para a sessão.

## <a name="mpsd-handles-to-sessions"></a>Identificadores MPSD para sessões

Um identificador de sessão MPSD é uma referência abstrata e imutável para uma sessão que também pode conter dados tipados adicionais. Ele é semelhante a um identificador de arquivo. Todos os identificadores de tem um identificador de GUID (ID) e uma referência de sessão completa consiste em configuração do serviço de ID (SCID), o modelo de sessão e o nome da sessão. Um identificador não pode ser atualizado, mas pode ser criado, leia e excluído.

| Observação                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Um identificador pode apontar para uma sessão que não existe. Criar um identificador usando um nome de sessão inexistente não faz com que uma nova sessão a ser criado. |


### <a name="handle-types"></a>Tipos de identificador

2015 multiplayer suporta os tipos de identificador descritos nesta seção.

#### <a name="invite-handle"></a>Identificador de convidar

Um identificador de convite representa um convite (convite) para uma pessoa específica. Dados específicos do tipo incluem pessoa do código-fonte, a pessoa de destino e cadeia de caracteres de contexto que descreve o convite, por exemplo, um modo de jogo específico.

Um identificador de convite concede acesso de leitura / gravação a uma sessão aberta. Se a sessão for fechada, o identificador concede acesso de sessão somente leitura.

| Observação                                                |
|------------------------------------------------------------------|
| MPSD pode criar um convite, mesmo se a sessão está cheio ou fechado. |


#### <a name="activity-handle"></a>Identificador de atividade

Um identificador de atividade indica que um usuário está fazendo no momento. A atividade do usuário é representada pela **MultiplayerActivityDetails classe**.

| Observação                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Um identificador de atividade também pode ser explicitamente excluído, mas somente para o título de proprietário para um usuário específico. Essa exclusão é feita usando o **MultiplayerService.ClearActivityAsync método**. |


### <a name="creating-an-invite-handle"></a>Criando um identificador de convite

Para criar um identificador de convite, as chamadas de título do **MultiplayerService.SendInvitesAsync método**. O método envia um convite para os usuários especificados na forma de uma interface do usuário do sistema que os destinatários podem atuar em aceitar o convite.


### <a name="creating-an-activity-handle"></a>Criando um identificador de atividade

Para criar um identificador de atividade, as chamadas de título do **MultiplayerService.SetActivityAsync método**. MPSD define a nova ID do identificador como atividade de associados do membro de sessão. Se houver uma atividade associada anterior, MPSD exclui o identificador correspondente. Quando o membro ativo se torna inativo ou sai da sessão, MPSD exclui o identificador da atividade associada.


### <a name="using-handles"></a>Usando identificadores

O título usa identificadores de quando um usuário aceita um convite (identificador de convite) e quando um usuário ingressa na atividade atual de um amigo (identificador de atividade). Em ambos os casos, o título deve:

1.  Obter a ID do identificador do título parâmetros de ativação.
2.  Criar um objeto de sessão local MPSD e ingresse-a como ativa.
3.  Grave a sessão, passando o identificador apropriado.

## <a name="synchronization-of-session-updates"></a>Sincronização de atualizações de sessão

Como uma sessão é um recurso compartilhado que pode ser criado ou atualizado por qualquer um dos seus membros, há o potencial para gravações em conflito. Isso pode levar a resultados inesperados, por exemplo, um título inadvertidamente pode substituir as alterações feitas por outro título. A abordagem do MPSD para resolver esses conflitos é dar suporte à simultaneidade otimista e um padrão de leitura, gravação e alteração.

Sincronização de atualizações de sessão por MPSD use dois padrões de implementação de alto nível relacionados:

-   Arbiter atualiza as partes compartilhadas da sessão. Se sua implementação envolve um arbitrador único, você pode evitar usando atualizações sincronizadas para a maioria das operações de gravação. O título pode evitar a sincronização de (1) qualquer atualização que o arbitrador torna a partes compartilhadas da sessão, a menos que elas estão relacionadas à comunicação de identidade do arbiter e (2) qualquer atualização para um título para a área de membro dentro da sessão.

| Observação                                                                                                                                                                                                                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Embora os tipos de atualização acima não é necessário para sincronização, ainda é importante sincronizar as atualizações para o * * MultiplayerSessionProperties.HostDeviceToken propriedade * *. Essa propriedade é usada para comunicar-se a identidade do arbiter, por exemplo, como parte da migração do arbiter. |

-   Todos os clientes atualizam partes compartilhadas da sessão. Nesse caso, todas as atualizações para as partes compartilhadas da sessão devem ser sincronizadas. No entanto, os títulos ainda podem gravar em suas próprias áreas de membro sem sincronização.


### <a name="session-update-synchronization-using-the-multiplayer-winrt-api"></a>Sincronização de atualização de sessão usando o API do WinRT para múltiplos jogadores

Os seguintes métodos de API do WinRT para múltiplos jogadores implementam a simultaneidade otimista:

-   **Método MultiplayerService.WriteSessionAsync**
-   **Método MultiplayerService.WriteSessionByHandleAsync**
-   **Método MultiplayerService.TryWriteSessionAsync**
-   **Método MultiplayerService.TryWriteSessionByHandleAsync**

Cada método de gravação aceita uma **MultiplayerSessionWriteMode enumeração** valor. Passando o valor SynchronizedUpdate faz uso de simultaneidade otimista para atualizações.

Outros valores na enumeração ajudam a resolver possíveis conflitos após a criação inicial de uma sessão. Qualquer gravação a uma parte da sessão MPSD que potencialmente pode ser gravada por outro título deve usar uma atualização sincronizada. No entanto, nem todas as gravações devem ser protegidas.

Se seu título tenta gravar o objeto de sessão local MPSD usando um dos métodos a sessão de gravação e recebe um código de status HTTP/412, ele deve atualizar a cópia local emitindo um **método MultiplayerService.GetCurrentSessionAsync**chamada para obter a versão mais recente do servidor da sessão antes de tentar a gravação novamente. Caso contrário, o documento de sessão local continua a conter os dados ruins e as chamadas para gravar a sessão continuarem a falhar.

| Observação                                                                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Se o título está usando uma da **TryWrite\***  métodos, a sessão atualizada é retornada no caso de um código de status HTTP/412. Esse comportamento evita a necessidade de chamar **GetCurrentSessionAsync**. |

Quando o título chama um dos métodos a sessão de gravação, uma versão atualizada da sessão pode ser retornada. Se for, o título deve alternar sua cópia armazenada em cache local para essa nova versão de uma forma thread-safe.


### <a name="session-update-synchronization-using-the-multiplayer-rest-api"></a>Sincronização de atualização de sessão usando a API REST para múltiplos jogadores

MPSD dá suporte à simultaneidade otimista em atualizações de sessão por meio da funcionalidade REST usando o cabeçalho "if-match" de HTTP com as configurações de ETag e o padrão de leitura, gravação e alteração. A ETag passada na solicitação de gravação deve ser aquela à qual o MPSD retorna com a solicitação de leitura anterior.

## <a name="calling-mpsd"></a>Chamar MPSD

Há duas maneiras para o título acessar MPSD para usar o sistema com vários participantes e o relacionamento de pessoas:
-   Recomendado. Use a API do WinRT para múltiplos jogadores, que contém classes que atuam como wrappers para a funcionalidade RESTful. Ver **Microsoft.Xbox.Services.Multiplayer Namespace**. Para emparelhamento SmartMatch, use o cruzamento de API do WinRT, representado pela **Microsoft.Xbox.Services.Matchmaking Namespace**.
-   Use chamadas diretas de HTTP padrão para o com vários participantes e emparelhamento APIs REST, incluído na **referência do Xbox Live dos serviços RESTful**. Os URIs aplicáveis são descritas em **URIs do diretório de sessão** (para vários jogadores) e **URIs de cruzamento** (para o relacionamento de pessoas). Objetos relacionados do JSON são descritos na **referência de objeto do objeto notação JSON (JavaScript)**.


### <a name="using-the-multiplayer-winrt-api-to-call-mpsd"></a>Usando o API do WinRT para vários participantes para chamar MPSD

A maneira recomendada para chamar MPSD é usar a API do WinRT com vários participantes e a compatibilidade de API do WinRT.

| Observação                                                                                             |
|---------------------------------------------------------------------------------------------------------------|
| Os exemplos XDK são escritos usando o com vários participantes e emparelhamento APIs do WinRT e os outros elementos de XSAPI. |

Uso de código de wrapper para a funcionalidade subjacente de REST permite que uma abordagem mais tradicional para usando métodos da API do lado do cliente sem ter que lidar com o tráfego HTTP para cada chamada. Um binário e uma fonte para XSAPI são fornecidos com o XDK. Você pode usar o binário diretamente, ou, em seguida, modifique a fonte e incorporá-la em seu título, conforme necessário.


### <a name="using-the-multiplayer-rest-api-to-interact-with-mpsd"></a>Usando a API REST de com vários participantes para interagir com MPSD

O título, ou seu serviço, pode usar as chamadas HTTP padrão para a API REST com vários participantes e o API REST de emparelhamento. Ao usar a funcionalidade REST diretamente, os problemas de chamador EXCLUAM, COLOCARAM, lançar e OBTÉM chamadas em relação ao diretório de sessão URIs para a maioria das ações. Em uma solicitação PUT, o corpo da solicitação é mesclado na sessão existente. Se não houver nenhuma sessão existente, o corpo da solicitação é usado para criar uma nova sessão, juntamente com o modelo de sessão armazenado no [Portal de desenvolvedor do Xbox (XDP)](https://xdp.xboxlive.com) ou [Partner Center](https://partner.microsoft.com/dashboard). Todos os campos são opcionais, e apenas deltas devem ser especificados. Portanto, {} é uma solicitação PUT válida com zero deltas.

Para executar uma solicitação PUT hipotética que retorna o resultado da mesclagem sem afetar a cópia do servidor oficial da sessão, você pode acrescentar a cadeia de consulta "? nocommit = true" para a solicitação PUT.

As solicitações e respostas para os métodos de API REST de relacionamento de pessoas e com vários participantes são documentos JSON. Para uma estrutura de solicitação de sessão para múltiplos jogadores, consulte **MultiplayerSessionRequest (JSON)**. Uma estrutura de resposta associada é mostrada na **MultiplayerSession (JSON)**. A estrutura de resposta como uma lista vinculada de quadros os membros da sessão e preenche outras propriedades somente leitura da sessão e seus membros.


### <a name="querying-for-sessions-and-session-templates-rest"></a>Consultar para sessões e modelos de sessão (REST)

Os títulos podem consultar informações de sessão na configuração do serviço e os níveis de modelo de sessão. Este tópico discute consultas que usam a API REST com vários participantes.


#### <a name="query-for-basic-session-information"></a>Consultar informações de sessão básica

Você pode definir as consultas para obter informações de sessão básica usando o diretório de sessões e URIs de emparelhamento. O resultado de uma consulta é uma matriz JSON de referências, com alguns dados de sessão incluídas embutidas de sessão. Por padrão, uma consulta recupera até 100 sessões não privado.

| Observação                                                          |
|----------------------------------------------------------------------------|
| Cada consulta deve incluir um filtro de palavra-chave, um filtro XUID ou ambos. |


#### <a name="query-for-session-templates"></a>Consulta para os modelos de sessão

Para recuperar a lista de modelos de sessão para o SCID, bem como os detalhes de um modelo de sessão específica, use o método GET para um dos URIs a seguir:
-   **/serviceconfigs/{scid}/sessiontemplates**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}**


#### <a name="query-for-session-state"></a>Consultar o estado de sessão

Para consultar o estado de sessão, use o método GET para um desses URIs de:
-   **/serviceconfigs/{scid}/sessions**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions**

## <a name="multiplayer-session-explorer"></a>Gerenciador de sessão para múltiplos jogadores

Gerenciador de sessão com vários participantes é uma ferramenta interna do MPSD para procurar as sessões, modelos de sessão e cadeias de caracteres de localização. A ferramenta destina-se somente a ser usado para as áreas restritas de desenvolvimento.


### <a name="accessing-multiplayer-session-explorer"></a>Acessando o Gerenciador de sessão para múltiplos jogadores

| Observação                                                                                                      |
|------------------------------------------------------------------------------------------------------------------------|
| Para usar a ferramenta, você deve estar conectado. A procura é limitada às sessões que o usuário conectado como um membro. |

Para acessar o Gerenciador de sessão com vários participantes, abra o Internet Explorer em seu Xbox One, pressione a **modo de exibição** botão e, em seguida, insira <https://sessiondirectory.xboxlive.com/debug> no **endereço** campo.

| Observação                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Você receberá um código de status 404 de HTTP/se você tentar acessar a ferramenta na área restrita de varejo. Para obter mais informações sobre esse código, consulte [códigos de Status de sessão com vários participantes](multiplayer-session-status-codes.md). |


### <a name="using-multiplayer-session-explorer"></a>Usando o Gerenciador de sessão para múltiplos jogadores


#### <a name="open-the-main-page"></a>Abra a principal página

1.  Acesse a página principal da ferramenta. Ele exibe o contexto de segurança (usuário conectado e área restrita) e uma lista de IDs (SCIDs) de configuração de serviço na área restrita.
2.  Pressione a **Menu** botão Fixar esta página para página inicial para que você não precise digitar o URI de cada vez.


#### <a name="display-available-sessions-and-templates"></a>Exibir modelos e sessões disponíveis

1.  Clique em um SCID na ferramenta para exibir uma lista de sessões em que SCID dos quais o usuário conectado é membro.
2.  Nessa mesma página, você pode clicar no SCID e exibir os modelos de sessão e cadeias de caracteres de localização na configuração do serviço para o SCID. Esses itens são ingeridos por meio [XDP](https://xdp.xboxlive.com) ou [Partner Center](https://partner.microsoft.com/dashboard).


#### <a name="display-the-full-contents-of-a-session"></a>Exibir o conteúdo completo de uma sessão

No Gerenciador de sessão com vários participantes, clique em um nome de sessão para exibir todo o conteúdo da sessão correspondente.

A sessão, conforme mostrado pelo MPSD pode diferir da resposta para um método GET padrão para o URI da sessão por alguns motivos:

-   A chamada GET pode estar usando uma versão mais antiga do contrato no cabeçalho X-Xbl-versão de contrato. Gerenciador de sessão sempre exibe a sessão usando a versão mais recente do contrato.
-   Quando uma sessão é solicitada normalmente por meio de GET, transformações e efeitos colaterais podem ser disparados, por exemplo, expirado o tempo limite. Gerenciador de sessão exibe um instantâneo da sessão, conforme eles são armazenados, sem executar qualquer lógica, transformações ou efeitos colaterais.
-   Uma vez que o campo de objeto JSON nextTimer é calculado ao mesmo tempo que os efeitos colaterais, ele não está presente em sessões MPSD.

## <a name="see-also"></a>Consulte também

[Visão geral da sessão](mpsd-session-details.md)

[Códigos de Status de sessão para múltiplos jogadores](multiplayer-session-status-codes.md)

[Como: Atualizar uma sessão para múltiplos jogadores](multiplayer-how-tos.md)

[Como: Ingressar em uma sessão MPSD a ativação de um título](multiplayer-how-tos.md)

[Como: Assine para receber notificações de alteração de sessão MPSD](multiplayer-how-tos.md)

[Para que haja correspondência SmartMatch](smartmatch-matchmaking.md)