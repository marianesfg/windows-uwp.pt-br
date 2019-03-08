---
title: Organizar partida com o SmartMatch
description: Saiba mais sobre o serviço Xbox Live SmartMatch de cruzamento de jogadores correspondentes em um jogo com vários participantes.
ms.assetid: ba0c1ecb-e928-4e86-9162-8cb456b697ff
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox one, com vários participantes, para que haja correspondência, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 6bc5f15e6fdb7d2f393daef8d21c134579610a9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647291"
---
# <a name="smartmatch-matchmaking"></a>Organizar partida com o SmartMatch

Este artigo contém as seguintes seções
* Introdução ao SmartMatch
* Operações de tempo de execução SmartMatch
* Configurando SmartMatch do título
* Definindo regras de equipe durante a configuração de SmartMatch
* Inicialização da sessão de destino e QoS

## <a name="introduction-to-smartmatch"></a>Introdução ao SmartMatch

O XSAPI fornece um serviço de cruzamento, chamado SmartMatch, que é encapsulado pela [API do Gerenciador de vários jogadores](../multiplayer-manager/multiplayer-manager-api-overview.md).  Para o uso da API avançado, você pode consultar a **MatchmakingService classe**, mas se você tiver um cenário de relacionamento de pessoas não é possível implementar usando o Gerenciador de vários jogadores, forneça seus comentários por meio de sua mãe.  Independentemente de qual API você usar, as informações conceituais neste artigo se aplica.

Para que haja correspondência SmartMatch agrupa players com base nas informações de usuário e a solicitação de emparelhamento para os usuários que desejam jogar juntos. Para que haja correspondência é baseada em servidor, que significa que os usuários forneçam uma solicitação para o serviço e que posteriormente sejam notificados quando uma correspondência for encontrada. Ao criar um título para Xbox One, você pode usar SmartMatch conforme descrito em [usando o emparelhamento de SmartMatch](using-smartmatch-matchmaking.md). Como alternativa, você pode seu próprio serviço de cruzamento, conforme descrito em [usando seu próprio serviço de cruzamento](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/multiplayer-and-networking/using-your-own-matchmaking-service). Observe que ao acessar esse link requer que você tenha um [Partner Center](https://partner.microsoft.com/dashboard) conta que está habilitada para o desenvolvimento do Xbox Live.

### <a name="about-smartmatch"></a>Sobre SmartMatch

O serviço de cruzamento trabalha junto com MPSD para simplificar o relacionamento de pessoas. Ele permite que os títulos facilmente fazer o cruzamento em segundo plano, por exemplo, enquanto o usuário estiver em execução jogador dentro do título.

Indivíduos ou grupos que deseja inserir o cruzamento criar uma sessão de tíquete de correspondência e, em seguida, solicitar o serviço de cruzamento para localizar outros jogadores com quem configurar uma correspondência. Isso resulta na criação de um temporário "correspondência tíquete" que reside dentro do serviço para que haja correspondência por um período de tempo.

O serviço de cruzamento escolhe sessões para reproduzir em conjunto com base na configuração, as estatísticas armazenadas para cada jogador e qualquer informação adicional fornecido no momento da solicitação de correspondência. O serviço, em seguida, cria uma sessão de destino de correspondência que contém todos os jogadores que tem sido correspondidos e notifica os títulos dos usuários da correspondência.

Quando a sessão de destino estiver pronta, títulos de executam verificações de QoS para confirmar que o grupo pode jogar juntos e, em seguida, começar play se tudo estiver bem. Durante a QoS processo e matchmade jogabilidade, títulos de manter o estado de sessão atualizados dentro de MPSD e recebem notificações de MPSD sobre alterações à sessão. Essas alterações incluem os usuários estão indo e vindo e muda para o arbitrador de sessão.


### <a name="match-ticket-session"></a>Sessão de tíquete de correspondência

Uma sessão de tíquete de correspondência representa os clientes para os jogadores que desejam fazer uma correspondência. Ele é criado com base em um jogo ou em um grupo de estranhos que estão em um corredor junto ou em outros grupos específicos de título de jogadores. Em alguns casos, a sessão de tíquete pode ser uma sessão já está em andamento que está procurando mais jogadores de jogo.


### <a name="match-ticket"></a>Tíquete de correspondência

Enviar uma sessão de tíquete para emparelhamento resulta na criação de um tíquete de correspondência que controla a tentativa de emparelhamento. Atributos no tíquete, por exemplo, o mapa de jogo ou jogador, juntamente com os atributos dos jogadores na sessão de tíquete, são usados para determinar a correspondência.
#### <a name="hoppers"></a>Hoppers

Hoppers são locais lógicos de onde os tíquetes de correspondência são coletados. Só os tíquetes dentro do mesmo hopper podem ser correspondidos. Um título pode ter vários hoppers. Por exemplo, um título pode criar um hopper para qual player habilidade é o item mais importante para correspondência. Ele pode usar outro hopper em que os jogadores são correspondidos somente se tiver adquirido o mesmo conteúdo que pode ser baixado.


#### <a name="hopper-rules"></a>Regras de Hopper

Regras de Hopper fornecem as definições dos critérios que o serviço de cruzamento usa para decidir sobre os jogadores para agrupar. Há dois tipos de regras:   DEVE regras - precisam ser atendidos para tíquetes de correspondência ser considerado compatível.
-   --DEVE regras um tíquete de correspondência de uma regra de correspondência é favorecido em relação a um que não.

Dentro de cada uma dessas categorias, há vários tipos específicos de regras. Para obter mais informações, consulte informações de configuração de operação de tempo de execução no **operações de tempo de execução SmartMatch**.


### <a name="match-target-session"></a>Sessão de destino da correspondência

Depois que um grupo correspondente foi encontrado, o serviço cria uma sessão de destino da correspondência e reservas de lugares para todos os jogadores de sessões de tíquete que são correspondidos juntos.


## <a name="smartmatch-runtime-operations"></a>Operações de tempo de execução SmartMatch



| Observação                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Para obter informações sobre como usar o emparelhamento SmartMatch em seu título, consulte [usando o emparelhamento de SmartMatch](using-smartmatch-matchmaking.md). |


### <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Criação de uma sessão de tíquete de correspondência e um tíquete de correspondência

Um cliente designado como o "scout" para que haja correspondência configura uma sessão de tíquete de correspondência para representar um grupo de jogadores que deseja inserir o relacionamento de pessoas. Todos os jogadores neste grupo de ingressar na sessão usando o método MultiplayerSession.Join. Depois que a sessão é preenchida com os jogadores, o título envia a sessão para o serviço de cruzamento usando o método MatchmakingService.CreateMatchTicketAsync, que cria um tíquete de correspondência que representa a sessão.

| Observação                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| O serviço de SmartMatch atualiza o campo /servers/matchmaking/properties/system/status da sessão de tíquete para "Pesquisar" durante a operação de CreateMatchTicketAsync. |

A resposta do método de criação do tíquete match é um objeto CreateMatchTicketResponse. Essa resposta contém a ID de tíquete de correspondência, um GUID que pode ser usado para cancelar o relacionamento de pessoas, excluindo o tíquete. Além disso, um tempo de espera médio para o hopper está incluído na resposta para usar na definição de expectativas de player.

| Observação                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A menos que haja um jogos stat dados específicos que precisam ser preservado, é recomendável que os títulos de usam o valor de PreserveSessionMode.Never ao chamar CreateMatchTicketAsync. Usando PreserveSessionMode.Never permite que mais rápida para que haja correspondência e remove a necessidade de ter opções de interface do usuário para a hospedagem de um jogo em vez de procurar por um jogo. |


### <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Definindo atributos para que haja correspondência na sessão e Players

Ao enviar a sessão de tíquete, o título define sessão e os atributos de player que o serviço de cruzamento usa para a sessão com outras sessões de grupo. Atributos podem ser especificados no nível de permissão ou no nível por membro.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Definindo atributos de compatibilidade no nível de permissão de correspondência

O título envia atributos no nível de permissão de correspondência no parâmetro do método MatchmakingService.CreateMatchTicketAsync ticketAttributesJson. Atributos especificados no nível do tíquete substituem os mesmos atributos especificados no nível por membro.


#### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Definindo atributos de compatibilidade no nível por membro

O título Especifica atributos por membro em cada membro dentro da sessão de tíquete de correspondência. Eles são definidos chamando o método MultiplayerSession.SetCurrentUserMemberCustomPropertyJson, usando um nome de propriedade de "matchAttrs". Essa chamada coloca os atributos em /members/ {index} / propriedades/personalizado/matchAttrs campo em cada jogador dentro da sessão de tíquete.

O processo de emparelhamento "nivela" cada por membro em um único atributo de nível de permissão, com base no método flatten especificado para o atributo na Xbox Live configuração da interface do usuário para o hopper. Isso pode ser configurado no [XDP](https://xdp.xboxlive.com) ou [Partner Center](https://partner.microsoft.com/dashboard).


### <a name="making-the-match"></a>Fazendo a correspondência

Com a sessão de tíquete e a correspondência tíquete configurada, as correspondências de serviço para que haja correspondência a sessão de tíquete representados com outras sessões de permissão que representa outros grupos e cria ou identifica uma sessão de destino da correspondência. O serviço também cria as reservas para os jogadores correspondentes na sessão de destino e marca as sessões de tíquete conforme correspondido. MPSD, em seguida, notifica o título dessa alteração para a sessão de tíquete.

Agora o título deve inicializar a sessão de destino para confirmar que suficiente players apareceram e executar a qualidade de serviço (QoS) verifica para garantir que eles possam se conectar entre si com êxito. Se a inicialização e/ou QoS falhar, o título da marca da sessão de tíquete para ser reenviadas para o relacionamento de pessoas para que o outro grupo que pode ser encontrado. Para obter mais informações sobre esse processo, consulte **inicialização da sessão de destino e QoS**.

Durante a atividade de correspondência, as seguintes alterações são feitas aos objetos JSON para a sessão:

-   campo de /Servers/matchmaking/Properties/System/status definido como "encontrado".
-   campo de /Servers/matchmaking/Properties/System/targetSessionRef definido para a sessão de destino.
-   /Members/ {index} / propriedades/personalizado/campo matchAttrs para cada sessão de tíquete é copiado para os membros / {index} / constantes/personalizado/matchmakingResult/playerAttrs campo.
-   Para cada jogador, do tíquete atributos copiados do campo no tíquete correspondência ticketAttributes /members/ {index} / constantes/personalizado/matchmakingResult/ticketAttrs campo.


### <a name="maintaining-the-match-ticket"></a>Manter o tíquete de correspondência

O serviço de cruzamento usa um instantâneo da sessão de tíquete no momento quando o tíquete de correspondência é criado para a sessão. Portanto, se qualquer players ingressar ou sair da sessão de tíquete, o título deve usar o serviço de cruzamento excluir e recriar o tíquete de correspondência.


### <a name="filling-spots-in-an-in-progress-game-session"></a>Preenchimento de pontos em uma sessão de jogo em andamento

Um título pode reutilizar uma sessão de jogo existente como uma sessão de tíquete de correspondência para localizar mais participantes para ingressar em um jogo que já está em andamento ou uma sessão de jogo após uma rodada de preenchimento e alguns jogadores à esquerda. Esse processo é conhecido como "aterramento".

É uma maneira de realizar aterramento é criar um tíquete de correspondência que será "preserve" a sessão em andamento, chamando **MatchmakingService.CreateMatchTicketAsync método** com o parâmetro preserveSession definido como PreserveSessionMode.Always. O serviço de cruzamento garante que a sessão existente preservada em todo o processo de emparelhamento e torna-se a sessão de destino resultante.

Há desvantagens para esse padrão, pois duas sessões com preserveSession definido como sempre não poderão corresponder entre si, pois eles não podem ser combinados. Isso pode resultar em cruzamento de aterramento muito lento. Para evitar esse cenário, um título só deve usar PreserveSessionMode.Always se ele requer a preservação do estado do jogo. Otherwis definido PreserveSessionMode.Never e migrar os jogadores para o novo targetSession quando uma correspondência for encontrada.


### <a name="deleting-the-match-ticket"></a>Excluindo o tíquete de correspondência

Para excluir o título do tíquete de correspondência chama **MatchmakingService.DeleteMatchTicketAsync método**. Na exclusão do tíquete de serviço de cruzamento de:

1.  Interrompe o relacionamento de pessoas para os usuários na sessão de tíquete.
2.  Atualizações de campo /servers/matchmaking/properties/system/status na sessão de tíquete para "cancelado".


### <a name="matchmaking-for-games-using-xbox-live-compute"></a>Para que haja correspondência para jogos usando computação em tempo real do Xbox

O exemplo a seguir demonstra a alto nível para que haja correspondência usando os serviços de computação ao vivo. Uma abordagem semelhante pode aplicar aos jogos hospedados nos recursos de terceiros.

1.  O cliente 'scout' cria uma sessão de tíquete para representar o grupo. Esta sessão contém uma lista de possíveis datacenters, localizados na configuração da sessão em /constants/system/measurementServerAddresses. Ele é resultado do modelo de sessão, se a lista de datacenter é estática ou do cliente no momento da criação de sessão após recebê-lo do serviço de computação ao vivo. Esta sessão contém também gsiSetId, gameVariantId e maxAllowedPlayers valores no objeto personalizado/targetSessionConstants/gameServerPlatform.
2.  Todos os outros clientes no grupo de ingressarem na sessão de tíquete.
3.  Todos os membros do grupo de baixar os valores de measurementServerAddresses do objeto /constants/system para a sessão de tíquete. Cada membro, em seguida, executar o ping de cada endereço usando a API da plataforma e carregar uma lista ordenada de centros de dados preferencial para a sessão, conforme definido em /members/ {index} / sistema/propriedades/serverMeasurements.

| Observação                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| O título pode definir e recuperar os valores de measurementServerAddresses da sessão usando **método MultiplayerSession.SetMeasurementServerAddresses** e o  **Propriedade MultiplayerSessionConstants.MeasurementServerAddressesJson**. |

4.  As chamadas de cliente 'scout' **MatchmakingService.CreateMatchTicketAsync método**, passando uma referência para a sessão de tíquete.

| Observação                                                                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Se os objetos de sessão de tíquete tiverem constantes incompatíveis, o método CreateMatchTicket poderão falhar. Isso pode ser evitado com a adição de uma necessidade de regra para o hopper e evitar a correspondência dos jogadores com constantes incompatíveis. |

5.  Se o **MatchTicketDetailsResponse.PreserveSession propriedade** é definido como nunca, o serviço de cruzamento copia as medidas do servidor de cada membro para a representação interna do tíquete antes de mesclar o coletados-los em uma única coleção para que o tíquete e armazenado como um atributo do tíquete "especiais".
6.  Se **MatchTicketDetailsResponse.PreserveSession propriedade** é definida como sempre, o servidor de medidas não são usadas. Em vez disso, o serviço de cruzamento o valor de /properties/system/matchmaking/serverConnectionString copiado para a sessão para a representação interna de tíquete, como uma coleção de serverMeasurements de tamanho 1 com latência zero.
7.  O serviço de cruzamento corresponde a sessão de tíquete com outras pessoas que representa outros grupos, colocar o servidor de coleções de medição em conta. Ele tenta corresponder o grupo com outros grupos que têm os mesmos datacenters preferenciais altamente.
8.  Depois que um grupo correspondente foi encontrado, o serviço de cruzamento cria ou identifica uma sessão de destino e adiciona todos os jogadores de sessões de tíquete que são correspondidas juntos. O serviço grava as medidas de servidor nivelado final para o grupo correspondente no /properties/system/serverConnectionStringCandidates. Em seguida, grava as medidas do servidor original para cada novo membro na sessão de destino /members/ {index} / constantes/sistema/matchmakingResult/serverMeasurements.
9.  Todos os clientes executam inicialização na sessão de destino, conforme discutido acima. No entanto, como os clientes se conectarem ao Live Compute, eles não fazem QoS uns com os outros para confirmar a conectividade.
10. Algumas ou todas as chamadas de clientes **GameServerPlatformService.AllocateClusterAsync método**. O primeiro deles dispara a alocação, enquanto os outros recebem somente uma confirmação. O método obtém a sessão de destino do MPSD e escolhe um dataceter com base nas preferências de datacenter na sessão de destino, bem como carga e outros dados de conhecimento específico de computação ao vivo.
11. Reproduzir todos os clientes.

## <a name="configuring-smartmatch-for-your-title"></a>Configurando SmartMatch do título


### <a name="configuration-of-smartmatch-matchmaking-runtime-operations"></a>Configuração das operações de tempo de execução para que haja correspondência SmartMatch

Todas as configurações de emparelhamento SmartMatch ocorre por meio de [Portal de desenvolvedor do Xbox (XDP)](https://xdp.xboxlive.com) ou [Partner Center](https://partner.microsoft.com/dashboard). Usa a configuração do ServiceConfiguration -&gt;seção com vários participantes e para que haja correspondência para um título.


#### <a name="matchmaking-session-template-configuration"></a>Configuração de modelo de sessão de emparelhamento

Conforme discutido em [SmartMatch cruzamento](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking), há dois tipos de sessão relacionadas ao relacionamento de pessoas, a sessão de tíquete de correspondência e a sessão de destino da correspondência. Basicamente, uma sessão de tíquete é a entrada para o serviço de cruzamento, enquanto a sessão de destino é a saída. Ao configurar modelos de sessão, você deve criar um modelo para cada tipo de sessão.

Para uma sessão de tíquete, você pode usar um modelo dedicado. Como alternativa, você pode reutilizar um modelo para uma sessão de corredor ou outra sessão que não se destina a ser usado para o jogo.

| Importante                                                                                      |
|-------------------------------------------------------------------------------------------------------------|
| A sessão de tíquete não deve ter a verificações de QoS habilitadas e não deve ser marcada com a capacidade de "jogo". |

Para uma sessão de destino, você deve usar um modelo que é destinado para matchmade jogo. Ele deve ter as configurações que permitem a verificações de QoS entre pares antes do início do jogo e devem ser marcadas com o recurso de "jogo".

Com a configuração da interface do usuário para XDP ou Partner Center, você pode mapear cada sessão para um ou mais hoppers, cada contém regras que determinam como as sessões são correspondidas juntos no que hopper. Para obter mais informações, consulte configuração básica do Hopper emparelhamento.


#### <a name="basic-hopper-configuration-for-matchmaking"></a>Configuração básica Hopper para emparelhamento

Esta seção define os campos usados para configurar campos hopper básico. Após essa configuração, você deve configurar as regras de hopper, conforme descrito na configuração das regras de Hopper.

![](../../images/multiplayer/session_template_hopper_edit.png)


###### <a name="name"></a>Nome

O nome do hopper que é usado ao enviar uma sessão de emparelhamento. Esse nome deve corresponder ao valor passado como um parâmetro para o **CreateMatchTicketAsync** método durante a criação do tíquete de correspondência.


###### <a name="minmax-group-size"></a>Tamanho do grupo de Min/Max

Os tamanhos mínimo e máximo para o grupo de player que deve ser criado de sessões no hopper. O serviço de cruzamento tenta criar um grupo correspondente que é tão grande quanto possível, até o tamanho máximo do grupo. No entanto, ele criar um grupo de correspondente se ele pode montar suficiente players para atender ao tamanho mínimo de grupo.


###### <a name="should-rule-expansion-cycles"></a>Deve ciclos de expansão de regra

Para uma deveria de regra, o serviço de cruzamento tenta aumentar o espaço de pesquisa e relaxar as regras de compatibilidade fornecido ao longo do tempo, se não houver correspondência bem-sucedida. Esse processo é executado por vários ciclos, como especificada usando o campo deve ciclos de expansão de regra. Após o último ciclo de expansão, as regras de deveria são descartadas, de modo que eles não impedem que tíquetes de correspondência. No entanto, eles ainda são usados para determinar a melhor correspondência se vários tickets estão disponíveis. Somente o número e tipos de QoS são expandidos antes que eles são descartados. Consulte também as regras de configuração de Hopper neste tópico.

Aumentar o valor de ciclos deve regra Explansion configuração fornece mais ciclos de deve a expansão de regra, mas também aumenta a duração de emparelhamento. O valor padrão é 3, que é geralmente suficiente para a maioria das configurações.

| Importante                                                                                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ciclos de expansão ocorrem em intervalos de tempo fixo de 5 segundos. Após o último ciclo de expansão, todas as regras de "Deve" são não é mais consideradas para o restante da tentativa de emparelhamento. |

##### <a name="ranked-hopper"></a>Hopper com classificação
Normalmente, SmartMatch impedirá players bloqueados que está sendo correspondido. Se uma classificação Hopper estiver marcada, essa lógica é ignorada para impedir que os jogadores utilizando esse sistema para evitar os representantes da maior habilidade.

#### <a name="configuration-of-hopper-rules"></a>Configuração de regras Hopper

Esta seção define os campos usados para configurar regras para um hopper.

###### <a name="common-rule-fields"></a>Campos de regra comuns

Os campos definidos nesta seção são comuns a todas as regras de hopper.

**Nome da regra**

O nome amigável exibido para a regra para fins de configuração.

**Tipo de regra**

O tipo de regra. As opções são deve e deveria. DEVEM ter regras a serem atendidos para compatibilidade com êxito. DEVEM regras pode ser reduzidas e/ou removidas para localizar uma correspondência com êxito. Consulte a configuração de expansão de regra deve para obter mais detalhes sobre esse processo.


**Tipo de dados**

O tipo de dados do atributo da regra de relacionamento de pessoas. Os valores possíveis são:   Número. Especifique um valor simple de numérico de 32 bits.
-   Cadeia de caracteres. Especifique uma cadeia de caracteres Unicode de até 128 caracteres.
-   coleção. Especifique uma matriz de cadeias de caracteres. Use esse valor para identificar o conteúdo para download (DLC), associação de Esquadrão ou preferência de função para os jogadores.
-   Qualidade de serviço. Especifique um tipo de dados personalizado para incluir a latência de dados de QoS no emparelhamento. Apenas uma dessas regras deve ser usada por hopper de emparelhamento.

| Observação |
|----------|
| Entre em contato com seu gerente de conta de desenvolvedor se esse limite é problemático para seu título. |

-   Valor total. Especifique um tipo de dados personalizado que soma os valores de cruzamento enviado. Você pode usar esse valor para garantir que a soma resultante é em um intervalo específico ou um valor exato.
-   Equipe. Especifique um tipo de dados personalizados para as equipes de jogadores incluídos nas solicitações de emparelhamento. Você pode usar esse valor para evitar dividir os jogadores dentro de um tíquete de única correspondência entre várias equipes.


###### <a name="data-type-specific-rule-fields"></a>Campos de regra específica de tipo de dados

Esta seção define os campos usados para definir regras que se aplicam para alguns tipos de dados, mas não para outras pessoas. A interface do usuário deve ser capaz de esclarecer quais tipos se aplicam a regras específicas de dados.

**Permitir caracteres curinga**

Um valor que indica se o atributo pode ser omitido na permissão de correspondência. Se ele for omitido, o tíquete torna-se compatível com qualquer outro tíquete, independentemente do valor para esse atributo.


**Origem do atributo**

A origem do valor de tipo de dados. Fontes possíveis são:
- Título fornecido. O valor de dados é enviado no tíquete correspondência.
-   Instância de usuário stat. O valor de dados é recuperado automaticamente do serviço UserStatistics.


**Nome do atributo**

O nome da fonte de valor de atributo. É o nome da propriedade na correspondência de permissão ou o nome de uma estatística de usuário.


**Valor Padrão**

O valor padrão para o tipo de dados, se nenhum valor for especificado ou está disponível para a solicitação de emparelhamento. O valor padrão não é aplicado quando o campo permitir curingas é selecionado e nenhum valor for especificado.


**Peso**

A importância da regra. O peso pode ser usado para indicar quais regras são priorizadas durante a expansão do relacionamento de pessoas e a regra. O valor de peso deve ser positivo e o padrão é 1.


**Método flatten**

Somente tipos de dados de número. Um valor que indica como os vários valores são combinados para atender a uma correspondência. Ele se aplica a vários valores de jogadores diferentes em um única correspondência tíquete e entre vários tíquetes. Os valores possíveis são:
-  Mín. / máx. Use o valor mínimo ou máximo de vários valores de tíquetes de correspondência diferentes.
-   Média. Use o valor médio de vários valores de tíquetes de correspondência diferentes.


**Máximo de comparação**

Somente tipos de dados de número. A diferença de numérica aceitável máxima entre dois valores comparados para atender a uma regra. Para uma deveria de regra, esse valor é o ponto de partida para a expansão de regra.


**A operação de definição**

Coleção somente tipos de dados. A operação a ser executada na correspondência com o grupo de conjunto de valores. As opções possíveis são:
- Interseção. Corresponde a duas coleções com base na quantidade de interseção entre elas. Essa configuração resulta em valores da coleção semelhantes ou idênticos.
-   Diferença. Corresponde a duas coleções com base na quantidade de diferença entre eles.
-   Preferência de função. Corresponder a coleções de acordo com as preferências para a função de um player em modos de jogo baseado em função.


**Interseção de destino**

Parte da configuração de operação definida. A diferença máxima para duas coleções antes que elas correspondam ou interseção mínimo.


**Topologia de rede**

Qualidade de serviço do tipo de dados somente. A topologia de rede que é usada para QoS. Os valores possíveis são ponto a ponto, ponto a ponto para o Host e cliente/servidor.


**Máximo de latência/dimensionamento máximo**  
Qualidade de serviço do tipo de dados somente. A latência máxima de compatibilidade bem-sucedida na topologia de rede especificado. Esse valor é tratado como um valor de colocação em escala (em vez de uma latência necessária) quando usar uma qualidade de cliente-servidor de serviço deve regra.


| Observação                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Além disso, as regras de reputação de padrão também são aplicadas a um hopper. Essas regras não podem ser removidas e são usadas para garantir que a manipulação correta de reputação durante o relacionamento de pessoas. |


**Permitir a espera para funções**

Tipo de dados de preferências de função de coleção somente. Especifica se o serviço de correspondência contém o tíquete de emparelhamento para preencher todas as funções disponíveis.


###### <a name="expansion-delta"></a>Delta de expansão

Valor que indica quanto para relaxar a regra enviada para cada geração de expansão. O delta de expansão é aplicado além do valor Max Diff. Consulte o exemplo 1 (regra de expansão) para obter detalhes.

Você também pode usar o delta de expansão para expandir vários valores numéricos em velocidades diferentes. Isso não é possível por meio de expansão ciclo configuração definindo porque ele se aplica a todas as regras. Em vez disso, a abordagem é usar a expansão decimal valores, por exemplo, 0.4. Uma expansão só ocorre quando um novo inteiro for atingido, o que permite a expansão diferentes velocidades de, mesmo para o mesmo número de ciclos de expansão.


###### <a name="qos-expansion-peer-to-peer-peer-to-host"></a>Expansão de QoS (Peer-to-Peer, ponto a ponto para o Host)
Para qualidade de expansão de tipo de serviço para jogos de ponto a ponto, o delta de expansão não pode ser configurado. Em vez disso, você deve usar uma das seguintes estratégias de expansão.

<table>

<tr>

<tr>
<td>
<b>1. MaxLatency < 256.</b><p/><p/>

Expansão é realizada no MaxLatency * ciclo de expansão.<p/>
Por exemplo, se o valor inicial é 200, 200 será usada no primeiro ciclo, 400 no ciclo de segundo e assim por diante.
</td>
</tr>

<tr>
<td>
<b>2. MaxLatency > ou igual a 256</b><p/><p/>

Expansão pode ser dimensionado linearmente de 50 para MaxLatency - 256.<p/>
Por exemplo, se o valor inicial é 556, o valor escalona linearmente de 50 a 300 sobre o número de ciclos. Ou seja, se seis ciclos tivesse sido escolhidos, os valores devem ser 50, 100, 150, 200, 250 e 300. Se cinco ciclos tivesse sido escolhidos, os valores devem ser 50, 112.5, 175, 237.5 e 300.
</td>
</tr>

</tr>
</table>

##### <a name="qos-expansion-client-server"></a>Expansão de QoS (cliente-servidor)
Ao usar servidores dedicados, a expansão se baseia em preferência relativa. Somente de maior preferência servidores serão considerados em ciclos de expansão no início. Ao longo do tempo, outros, menos servidores preferenciais será usado. Para garantir que a expansão apropriados, exigimos um valor semelhante a MaxLatency, chamado dimensionamento máximo. Esse máximo de dimensionamento deve ainda ser definido como o maior tempo de ping aceitável — no entanto, esse valor permite que uma escala relativa para os tempos de ping do servidor diferente fornecidos por um player, em vez de fornecer um requisito absoluto para tempos de ping. Observe que você possa excluir servidores com tempos de ping inaceitável intencionalmente, removendo-as da lista na solicitação.

#### <a name="example-1-rule-expansion"></a>Exemplo 1 (expansão de regra)

Nível do Player é usado para compatibilidade e players são correspondidos livremente, com base no grau de seus níveis. Os jogadores com o mínimo da diferença entre os níveis são preferenciais.

-   Regra de nível de Player
-   Tipo de regra: DEVE
-   Tipo de dados: Número
-   Máximo de comparação: 1
-   Delta de expansão: 2
-   Método flatten: Média

Por padrão, a diferença necessária entre os níveis de player é 1 ou menos. Se uma correspondência for encontrada dentro dessa diferença, os jogadores são correspondidos. Se nenhuma correspondência inicial for encontrada, o valor de nível de player será expandido por 2 para cada iteração (três iterações, por padrão). Esse cenário resulta em um comportamento para que haja correspondência para um player no nível 20, conforme mostrado na tabela a seguir.

| Etapa                    | Valor de nível para possíveis candidatos de correspondência | Distância de nível eficaz para correspondência com êxito |
|-----|
| Valor de enviado inicial | 19-21                                      | 1                                             |
| Ciclo de expansão 1       | 17-23                                      | 3                                             |
| Ciclo de expansão 2       | 15-25                                      | 5                                             |
| Ciclo de expansão 3       | 13-27                                      | 7                                             |

À medida que os ciclos de expansão continuam, a distância de nível em vigor para uma correspondência com êxito aumenta sem alterar o valor máximo Diff. Somente o valor de nível de jogador é reduzido.


#### <a name="example-2-collection-rule"></a>Exemplo 2 (regra de coleta)

O jogo libera os três tipos de DLC que estão disponíveis para os jogadores. Essa regra de cruzamento é aplicada ao cruzamento de jogo "DLC apenas", e um player deve possuir pelo menos um DLC para ser matchmade com outros jogadores.

-   Regra DLC do Player
-   Tipo de regra: DEVE
-   Tipo de dados: Coleção
-   Operação de definição: Interseção
-   Interseção de destino: 1

Os jogadores avaliar suas DLCs e enviar os valores mostrados na tabela a seguir em seus tíquetes de correspondência.

| Observação                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Na tabela a seguir, o valor de coleção indica a propriedade do DLC. Ele é definido como 1 se o DLC está disponível para o jogador e como 0 se não for. |

| Player   | Valor de coleção | Correspondência com os Players | Observação           |
|----------|------------------|----------------------|----------------|
| Jogador 1 | { "1", "1", "1"} | 3                    | Possui todos os DLCs  |
| 2 de Player | { "0", "0", "0"} | Nenhuma                 | Não possui nenhum DLC    |
| 3 de Player | { "1", "0", "0"} | 1                    | Possui primeiro DLC |

Se a interseção de destino no exemplo é definida como 2, 1 e 3 de jogadores não haverá correspondência, como a interseção entre elas é apenas 1.

#### <a name="example-3-avoid-previous-players"></a>Exemplo 3 (Evite anteriores players)
O título prefere evitar um jogo com o player executados mais recentemente.
* Tipo de regra: DEVE
* Tipo de dados: Coleção
* Operação de definição: Diferença
* Interseção de destino: 0

## <a name="defining-team-rules-during-smartmatch-configuration"></a>Definindo regras de equipe durante a configuração de SmartMatch

### <a name="configuring-team-rules"></a>Configurando regras de equipe

Para configurar a regra de equipe, comece criando um em sua plataforma de configuração escolhido (XDP ou Partner Center). Preencha os tamanhos de equipe espera que seu jogo para criar a partir os tíquetes correspondidos nesse hopper. Por exemplo, se seu jogo espera 4v4, você deve criar duas entradas, esperando um tamanho máximo de 4 cada e um nome diferente. Há um mínimo tamanho da equipe também – use essa opção se um jogo que pode ser executado com menos jogadores em uma equipe. Caso contrário, o mínimo e máximo devem ser o mesmo valor.


#### <a name="using-team-rules"></a>Usando regras de equipe

Depois que a regra de equipe estiver configurada, tíquetes dentro o hopper serão impedidos de correspondência se não houver nenhuma maneira de ajustar seus grupos em equipes sem causar uma divisão. A regra gravará a alocação resultante da equipe a sessão de destino, em membros/constantes/personalizado/matchmakingresult/initialTeam. Observe que isso é simplesmente uma alocação sugerida – o título pode achar que reorganizar os jogadores pode criar um jogo melhor, enquanto ainda impede que tíquetes dividir em equipes diferentes.

Se uma permissão é criada para um jogo que está em andamento, a regra de equipe exige informações adicionais. Suponha, por exemplo, que 8 players estão em uma queda de jogadores de jogos, quando dois 4v4 ou estão desconectadas. O título que preencha esses pontos de acesso vazios, mas ele não é possível reorganizar as equipes, enquanto o jogo está sendo jogado.

Tentativas de preencher jogos em andamento são representadas por tíquetes com o campo PreserveSession definido como "sempre". Em casos como esse, porque as equipes de já tem sido alocadas para os jogadores, o título deve especificar a alocação de equipe atual, portanto, correspondência saberá quantos espaços são abertos em cada equipe. Para fornecer os nomes das equipes de cada jogador está ativada, cada jogador grava seu nome de equipe para a sessão de jogo, sob membros/me/propriedades/sistema/grupos. Este campo é um JArray.

Depois que as propriedades acima são gravadas para a sessão de jogo, um jogador cria um tíquete de sessão, em uma tentativa de localizar mais jogadores. Quando o tíquete é atendido, correspondência novamente gravará a equipe sugerida de qualquer jogadores que participam em membros/constantes/personalizado/matchmakingresult/initialTeam


###### <a name="preferring-even-teams"></a>A preferência pelo mesma equipes

Além disso, correspondências serão feitas com as equipes maiores pela primeira vez. Isso significa que em um hopper 4v4 hipotético, tíquetes de 4 jogadores corresponderá primeiro junto, até que nenhum tíquete de 4 permanecem. Tíquetes de 3, em seguida, continuam, puxando singletons conforme eles precisam e assim por diante. Dessa maneira, tíquetes de tamanho similar geralmente serão reproduzido em relação uns aos outros, se eles estiverem presentes e não foi impedido por outras regras. Observe que isso oferece a precedência de regra de equipe muito forte sobre outras regras. Por exemplo, suponha que você tem uma população limitada, consistindo de um tíquete de tamanho 4 com alta skill(A), um tíquete de tamanho 4 com skill(B) baixa e quatro tíquetes de tamanho 1 com skill(C-F) alta. A regra com a equipe fará com que a correspondência de preferir a correspondência A e B, em vez de A, C, D, E e F.


###### <a name="should-variant"></a>Deve Variant

Devem regra impede que o tíquete de divisão em todas as gerações e fornece a classificação de equipes até mesmo prefer. O deve regra é idêntica até a última geração – depois de lá, tíquetes podem ser divididos, embora a classificação de equipes até mesmo prefer ainda estará ativa.

## <a name="target-session-initialization-and-qos"></a>Inicialização da sessão de destino e QoS

Um grupo de jogadores é correspondido em uma sessão de destino por emparelhamento SmartMatch, conforme descrito em [SmartMatch cruzamento](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking). O título deve tomar medidas para confirmar que o suficiente players ingressaram que eles podem se conectar com êxito um ao outro se eles precisarem. Esse processo é conhecido como inicialização da sessão de destino.

Para jogos usando topologias de rede ponto a ponto, um aspecto importante da inicialização da sessão de destino é medida de QoS e avaliação. Operações associadas são a medida de latência e largura de banda entre os consoles Xbox One (ou entre servidores e consoles) e a avaliação das medidas resultantes para determinar se a conexão de rede entre os nós é boa.

O fluxograma a seguir ilustra como implementar a inicialização da sessão de destino e as operações de QoS.

![](../../images/multiplayer/Multiplayer_2015_Matchmaking_QoS.png)

### <a name="managed-initialization"></a>Inicialização gerenciada

MPSD dá suporte a um recurso chamado "gerenciado por inicialização" por meio do qual ele coordena o processo de inicialização da sessão de destino envolvidos em uma sessão de clientes. MPSD automaticamente rastreia os estágios de inicialização e nos limites associados para a sessão e avalia a conectividade entre clientes, se necessário. Inicialização gerenciada é representada pela **MultiplayerManagedInitialization classe**.

| Observação                                                                                                                  |
|------------------------------------------------------------------------------------------------------------------------------------|
| Ele é altamente recomendável para seu título usando emparelhamento SmartMatch entrem aproveitar o MPSD gerenciado o recurso de inicialização. |


#### <a name="managed-initialization-episodes-and-stages"></a>Episódios de inicialização gerenciado e estágios

Uma sessão de destino passa pela qualquer momento para que haja correspondência adiciona novos jogadores à sessão de inicialização gerenciada. SmartMatch adiciona membros da sessão como estado de usuário reservado, o que significa que cada membro ocupa um slot, mas ainda não ingressou a sessão. Cada grupo de novos jogadores dispara um novo episódio de inicialização.

Após a conclusão da inicialização, cada jogador for bem-sucedida ou falha o processo. Um que tiver êxito na inicialização pode executar usando a sessão de destino. Um player que não deve ser reenviado para o emparelhamento a ser correspondido em outra sessão. Para casos em que uma sessão é enviada para o emparelhamento com o *preserveSession* parâmetro definido como sempre, os membros já existentes da sessão não são submetidos a inicialização, como MPSD pressupõe que eles esteja configurado corretamente.

Cada episódio de inicialização gerenciado consiste em um desses estágios:

-   Unindo - os membros da sessão escrever próprios à sessão para mover seu estado de usuário de reservado para o Active Directory e carregar dados básicos, como o endereço de seguro de dispositivos.
-   Medida – para topologias baseadas em ponto a ponto, os membros da sessão medem QoS umas às outras e carreguem os resultados para a sessão.
-   Avaliação – MPSD avalia os resultados dos últimos dois estágios e determina se a sessão e/ou os membros têm inicializado com êxito.

O código do título opera na sessão para Avançar a cada usuário (e, portanto, a sessão) pelas fases ingressando e medição. Ele, em seguida, inicie play ou volte para o emparelhamento após a fase da avaliação teve êxito ou falhou.


### <a name="configuring-the-target-session-for-initialization"></a>Configurar a sessão de destino para a inicialização

O título pode configurar o processo de inicialização gerenciado usando constantes na sessão de destino que está sendo inicializada. Essas constantes são definidas em /constants/system no modelo de sessão com a versão 107, a versão recomendada do modelo. Dois tipos de definições de configuração podem ser feitos: configurações que configuram o processo de inicialização gerenciado como um todo e configurações que configurar os requisitos de QoS. Ver [modelos de sessão MPSD](multiplayer-session-directory.md) para obter exemplos de modelos de sessão para cenários comuns de título.

| Observação                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------|
| Se os requisitos de QoS não estão definidos na configuração de inicialização da sessão de destino durante a inicialização, o estágio de medida será ignorado. |


#### <a name="configuring-managed-initialization-as-a-whole"></a>Configurar gerenciados inicialização como um todo

Abaixo estão os campos a serem definidas para controlar a inicialização gerenciada geral. Eles fazem parte do objeto /constants/system/memberInitialization:
- joinTimeout. Especifica quanto tempo MPSD aguarda para cada membro ingressar, depois que o episódio de inicialização tiver começado. Padrão é 10 segundos.
-   measurementTimeout. Especifica quanto tempo MPSD aguarda para cada membro carregar resultados de medição de QoS, após o início do estágio de medição. O padrão é 30000 segundos.
-   membersNeededToStart. Especifica o número de membros que deve ter êxito na inicialização para o primeiro episódio de inicialização seja bem-sucedida. O padrão é 1.

| Observação                                              |
|----------------------------------------------------------------|
| Se esse limite não for atendido, todos os membros da falham na inicialização. |


#### <a name="configuring-qos-requirements"></a>Configurar requisitos de QoS

QoS é necessário somente durante a inicialização se o título usa uma topologia ponto a ponto ou host do par. Cada topologia é mapeado para uma constante de topologia específica em /constants/sistema /.
###### <a name="configuring-qos-requirements-for-peer-to-peer-topology"></a>Configurar requisitos de QoS para a topologia ponto a ponto

| Observação                                                                                                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| É raro para títulos tornar as configurações dos requisitos de QoS para a topologia ponto a ponto. Essas configurações são muito restritivas e causam problemas para os jogadores com conversões de endereço de rede estritas (NATs). |

Requisitos de QOS da topologia ponto a ponto são definidos no objeto peerToPeerRequirements. Cada cliente deve ser capaz de se conectar a todos os outros clientes. O objeto tem os seguintes campos pertinentes:
-   latencyMaximum. Especifica a latência máxima entre quaisquer dois clientes.
-   bandwidthMinimum. Especifica a largura de banda mínima entre quaisquer dois clientes.


###### <a name="configuring-qos-requirements-for-peer-to-host-topology"></a>Configurar requisitos de QoS para a topologia de host do par

Requisitos de QOS de topologia de host do par são definidos no objeto peerToHostRequirements. Cada cliente deve ser capaz de se conectar a um único host comuns. Se esse objeto é configurado e inicialização for bem sucedida, MPSD criará uma lista inicial de clientes que são possíveis hosts, conhecidos como os "candidatos a host". Aqui estão os campos para definir:
-   latencyMaximum. Especifica a latência máxima entre cada par e o host.
-   bandwidthDownMinimum. Especifica a largura de banda mínima downstream entre cada par e o host.
-   bandwidthUpMinimum. Especifica a largura de banda upstream mínima entre cada par e o host.
-   hostSelectionMetric. Especifica a métrica usada para selecionar o host.

## <a name="see-also"></a>Consulte também

[Modelos de sessão MPSD](multiplayer-session-directory.md)

[Operações de tempo de execução SmartMatch](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking)
