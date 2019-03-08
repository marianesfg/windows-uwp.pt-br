---
title: Perguntas frequentes e solução de problemas do sistema Multijogador 2015
description: Perguntas frequentes sobre o Xbox Live Multiplayer 2015 e solução de problemas.
ms.assetid: 75823f10-b342-4e20-b885-e5ad4392bc3d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes
ms.localizationpriority: medium
ms.openlocfilehash: 171d80f4fc925d95d80043f40bb387b045a4fe23
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625521"
---
# <a name="multiplayer-2015-faq-and-troubleshooting"></a>Perguntas frequentes e solução de problemas do sistema Multijogador 2015

-   Estou desenvolvendo um novo título. Quais elementos de API com vários participantes deve usar?
-   Como acessar a nova API com vários participantes de um serviço?
-   Meu título pode assinar as alterações de mais de uma sessão?
-   Será um usuário removido imediatamente se ele/ela perder uma conexão de rede ou desativa o console?
-   Como posso descobrir quais SCID, um modelo de sessão e uma área restrita usar?
-   Há um exemplo de um corpo de solicitação pode comparar com aquele para o título da minha?
-   Estou recebendo um código HTTP/403 ao chamar MPSD.
-   Estou recebendo um código HTTP/404 ao chamar MPSD.
-   Por que estou recebendo um código HTTP/404 ao chamar **MultiplayerService.WriteSessionByHandleAsync** ou **MultiplayerService.GetCurrentSessionByHandleAsync**?
-   Estou recebendo um código HTTP/412 ao chamar MPSD.
-   Estou recebendo um HTTP/400, 405, 409, 503, código etc. ao chamar MPSD.
-   O que pode ou deve alterar nos modelos de sessão para o título da minha?
-   Estou recebendo um erro que está dizendo que minha sessão não foi inicializado.
-   Minha sessão não está sendo criado e estou recebendo um código HTTP/204.
-   Quando eu deve sondar MPSD?
-   O que acontece se um player que foi reservado ou convidado para a sessão não uni-lo?
-   Por que uma sessão criada não seria encontrada por emparelhamento?
-   O que é a principal diferença entre a maneira partes são usadas corretamente pelo Multiplayer 2015 e o que eles foram usados no Multiplayer 2014?
-   Se uma sessão de jogo tem slots player aberto e dá suporte à junção em andamento, por que os usuários não seria capazes de encontrar a sessão depois que ele for iniciado?
-   Se uma sessão de jogo estiver aberta, pode um usuário que acaba de ingressar um jogo simplesmente ingressar na sessão e iniciar a reprodução sem precisar aguardar a reserva?
-   Quando as sessões de jogos grandes estão jogando no título da minha, por que todos os membros da sessão não estiverem vendo a torrada convite jogo?
-   Eu estou vendo comportamento inconsistente de jogo e recebeu informações de ativação de protocolo, fazendo referência a recursos de jogos de terceiros.
-   Por que estou vendo a sintaxe para documentos de sessão v105 em meu rastreamentos apesar de eu ter configurado um modelo de sessão v107?


### <a name="i-am-developing-a-new-title-which-multiplayer-api-elements-should-i-use"></a>Estou desenvolvendo um novo título. Quais elementos de API com vários participantes deve usar?

Funcionalidade com vários participantes de 2014 continuará a ser aplicado para títulos existentes, mas os elementos de API associados maior probabilidade de ser substituído. É altamente recomendável o uso de vários jogadores de 2015, quando preparar seus clientes para lançamento no 2015.


### <a name="how-can-i-access-the-new-multiplayer-api-from-a-service"></a>Como acessar a nova API com vários participantes de um serviço?

Ver [chamando MPSD](multiplayer-session-directory.md).


### <a name="can-my-title-subscribe-to-changes-for-more-than-one-session"></a>Meu título pode assinar as alterações de mais de uma sessão?

Sim, um título pode assinar para receber alterações em até 10 sessões por conexão.


### <a name="will-a-user-be-removed-immediately-if-heshe-loses-a-network-connection-or-turns-off-the-console"></a>Será um usuário removido imediatamente se ele/ela perder uma conexão de rede ou desativa o console?

A conexão de soquete da web permite MPSD rapidamente detectar a desconexão do cliente e definir o cliente como inativo. Membros de sessão são removidos, assim que o tempo limite de remoção inativo expirar. Para obter mais informações, consulte [tempos limite de sessão](mpsd-session-details.md).


### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>Como posso descobrir quais SCID, um modelo de sessão e uma área restrita usar?

Se você não estava envolvido no registro inicial do seu título, quando essas informações foi criadas, você não tem acesso a essas informações. Entre em contato com seu gerente de conta de desenvolvedor, que pode obter esses dados para você.


### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-to-the-one-for-my-title"></a>Há um exemplo de um corpo de solicitação pode comparar com aquele para o título da minha?

Ver a estrutura de solicitação no **MultiplayerSessionRequest (JSON)**


### <a name="i-am-getting-an-http403-code-when-calling-mpsd"></a>Estou recebendo um código HTTP/403 ao chamar MPSD.

Isso geralmente é uma permissão ou problema de escopo. Coletar rastreamentos do Fiddler para obter mais informações e, em seguida, verifique as mensagens retornadas como parte do corpo HttpResponse para 403 mensagens comuns:

-   A configuração de serviço solicitado não pode ser acessada.
    1.  Confirme que você está usando uma conta que tenha acesso à área restrita.
    2.  Confirme que você está na área restrita correta.
     3.  Se você estiver usando a autenticação de certificado e recebendo esse erro, entre em contato com seu gerente de conta de desenvolvedor.   A sessão solicitada não pode ser acessada. O usuário chamador deve ter o privilégio para múltiplos jogadores e sessões privadas só podem ser lidos por membros da sessão.

    Você não tem a capacidade de ver a sessão, possivelmente porque você está tentando acessar uma sessão que tem uma visibilidade de privado. Se a visibilidade está definida como privado, somente os membros da sessão em questão podem ler o documento de sessão.

| Observação                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Um usuário deve ter uma conta de ouro para registrar uma nova sessão. Sem privilégios de conta de ouro para um usuário de reprodução, as solicitações para registrar uma nova sessão retornam HTTP/403. |

-   O corpo da solicitação não pode conter referências de membro existente, a menos que a autenticação de entidade de segurança inclui um servidor.

    Você não pode ingressar outro usuário em uma sessão em nome do usuário. Você pode convidar apenas. Definir o índice "reservar\_&lt;número&gt;" convidar um player.


### <a name="i-am-getting-an-http404-code-when-calling-mpsd"></a>Estou recebendo um código HTTP/404 ao chamar MPSD.

Colete rastreamentos do Fiddler para obter mais informações e, em seguida, faça o seguinte:

1.  Verifique a mensagem retornada como parte do corpo do HttpResponse para mensagens de 404 comuns:
    -   A configuração de serviço solicitado é inválido ou não foi configurado para sessões. Certifique-se de que o SCID que está sendo usada está correta.
    -   A sessão solicitada não foi encontrada. Certifique-se de que a sessão é criada e se o modelo de sessão está correto antes de recuperá-los. Você pode criar uma sessão com uma chamada PUT.

2.  Verifique se o URI que você está usando.
3.  Reinicie o console ou tente com um novo usuário.
4.  Verifique os fóruns de desenvolvedores de jogos para o código de erro ou outras soluções.
5.  Confirme que a sessão não foi excluída como resultado estava vazio. As sessões podem se tornar vazias porque os usuários o tempo limite. Quando isso acontece, é com frequência porque todos os membros da sessão estão em um estado para o qual um tempo limite é aplicado, como "pronto" ou "inativos". Ver [estados de sessão do usuário](mpsd-session-details.md) para obter detalhes de estado do usuário.


### <a name="why-am-i-getting-an-http404-code-when-calling-multiplayerservicewritesessionbyhandleasync-or-multiplayerservicegetcurrentsessionbyhandleasync"></a>Por que estou recebendo um código HTTP/404 ao chamar **MultiplayerService.WriteSessionByHandleAsync** ou **MultiplayerService.GetCurrentSessionByHandleAsync**?

Se o título está acessando uma sessão MPSD pelo identificador em resposta a uma ativação de protocolo de associação que contém uma ID de identificador, a ID do identificador na ativação de protocolo pode estar obsoleto para um dos seguintes motivos:

-   O modo de exibição da interface do usuário no shell do Xbox da qual o título iniciou a junção pode ser desatualizado. Alguns modos de exibição da interface do usuário, por exemplo, os cartões de perfil do usuário, não atualiza automaticamente identificadores de junção enquanto eles estiverem abertos. Para evitar o recebimento do código/404 de HTTP, o título deve fechar e reabrir o modo de exibição para atualizar os dados antes de se juntar.
-   O usuário que está ingressando o título pode ter alternado atividade sessões depois que o título selecionado a operação de junção do shell do Xbox da interface do usuário. Esse motivo para o código HTTP/404 deve ser raro.

Em ambos os casos, seu código do título deve mostrar o usuário ingressando em uma mensagem de erro indicando que houve falha na junção.


### <a name="i-am-getting-an-http412-code-when-calling-mpsd"></a>Estou recebendo um código HTTP/412 ao chamar MPSD.

A solicitação a seguir retorna 412/HTTP, se a sessão já existe:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-None-Match: *


A solicitação a seguir retorna 412/HTTP, se a etag da sessão não coincidir com o cabeçalho If-Match:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D


Ver [sincronização de atualizações de sessão](multiplayer-session-directory.md) para obter mais informações.


### <a name="i-am-getting-an-http400-405-409-503-etc-code-when-calling-mpsd"></a>Estou recebendo um HTTP/400, 405, 409, 503, código etc. ao chamar MPSD.

Coletar rastreamentos do Fiddler para obter mais informações e, em seguida, verificar mensagens retornadas como parte do corpo HttpResponse. Isso deve fornecer informações suficientes para identificar e corrigir o erro, ou você pode pesquisar os fóruns de desenvolvedores para resoluções.

Você também pode obter o corpo da resposta se você estiver usando XSAPI, conforme descrito em [solução de problemas a API de serviços do Xbox Live](../../using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md). Como alternativa, seu código pode usar o **XboxLiveContextSettings.EnableServiceCallRoutedEvents** propriedade para enviar a saída para o título da interface do usuário.


### <a name="what-can-or-should-i-change-in-the-session-templates-for-my-title"></a>O que pode ou deve alterar nos modelos de sessão para o título da minha?

Modelos de sessão são padrões para as sessões e você não pode substituir as constantes já definidas nos modelos. No entanto, você pode adicionar novas propriedades aos modelos. Ver [modelos de sessão MPSD](multiplayer-session-directory.md) para obter mais detalhes.


### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>Estou recebendo um erro que está dizendo que minha sessão não foi inicializado.

Erro de resposta de exemplo:

400 - \[ResponseBody\]: Esta sessão está configurada para exigir que membros de pelo menos 2 iniciar a inicialização gerenciada.

A sessão não pode ser criada porque não há reservas de membro de sessão com o campo "inicializar" é definido como true são incluídas na solicitação. Seu código pode definir esse campo para um membro usando o *initializeRequested* parâmetro para o **MultiplayerSession.AddMemberReservation** método ou o **MultiplayerSession.Join** método.

Quando a inicialização é especificada em seu modelo de sessão, certifique-se de que "inicializar": "true" é definido para o suficiente, as reservas de membro para passar para que haja correspondência QoS. Para obter mais informações, consulte [inicialização da sessão de destino e QoS](smartmatch-matchmaking.md).


### <a name="my-session-is-not-being-created-and-im-getting-an-http204-code"></a>Minha sessão não está sendo criado e estou recebendo um código HTTP/204.

Esse código de status indica que nenhum usuário foram adicionado à sessão quando você o criou. Se não houver nenhum usuário da sessão quando ela é criada e o tempo limite da sessão vazia é zero (padrão), a sessão não será criada.

Certifique-se de que você inclua pelo menos um player durante a criação de uma sessão. Para cenários de servidor dedicado, obtenha um player de quem está tentando criar uma correspondência ou quem precisa criar uma correspondência e tornar esse jogador o player inicial na correspondência. Como alternativa, você pode alterar ou remover o tempo limite da sessão vazia. Para obter mais informações, consulte [tempos limite de sessão](mpsd-session-details.md).


### <a name="when-should-i-poll-mpsd"></a>Quando eu deve sondar MPSD?

Os títulos devem evitar a sondagem MPSD. Se um título deve descobrir alterações às sessões MPSD, ele deve inscrever-se para eventos de alteração de sessão. Para obter mais informações, consulte [como: Assine para receber notificações de alteração de sessão MPSD](multiplayer-how-tos.md).


### <a name="what-happens-if-a-player-who-was-reserved-or-invited-to-the-session-does-not-join-it"></a>O que acontece se um player que foi reservado ou convidado para a sessão não uni-lo?

Ela depende ou não o título está em execução quando o player é notificado de que a sessão de jogo está pronto. Se o jogador está no título e não aceita a notificação de sessão de jogo do título da interface do usuário, cabe o título para remover o player da sessão de jogos com o **MultiplayerSession.Leave** método.

Se o título é restrito ou não em execução, o shell fornece uma notificação que informa o player que um slot está disponível. Se o jogador recusa ou ignora as notificações do sistema, MPSD remove essa pessoa a sessão de jogo.


### <a name="why-would-a-created-session-not-be-found-by-matchmaking"></a>Por que uma sessão criada não seria encontrada por emparelhamento?

No Xbox One, simplesmente criando uma sessão não é suficiente para o emparelhamento localizar a nova sessão. Você deve criar um tíquete de correspondência para iniciar a sessão para o serviço de cruzamento de publicidade. Ver [SmartMatch cruzamento](smartmatch-matchmaking.md).


### <a name="what-is-the-key-difference-between-the-way-parties-are-properly-used-by-2015-multiplayer-and-the-way-they-were-used-in-2014-multiplayer"></a>O que é a principal diferença entre a maneira partes são usadas corretamente pelo Multiplayer 2015 e o que eles foram usados no Multiplayer 2014?

Em 2015 com vários participantes, a API para múltiplos jogadores define não parte de jogo de nível de sistema, somente as partes de bate-papo. Em vez de usar partes do jogos, o título usa sessões para controlar o ingresso, convites e recursos relacionados ao. Para vários jogadores de 2014, a API com vários participantes no Xbox One usado em destaque o conceito de jogo de terceiros (**terceiros** classe), que implementa efetivamente um corredor de junção de nível de sistema em vez de jogo convida.


### <a name="if-a-game-session-has-open-player-slots-and-supports-join-in-progress-why-would-users-not-be-able-to-find-the-session-once-it-has-started"></a>Se uma sessão de jogo tem slots player aberto e dá suporte à junção em andamento, por que os usuários não seria capazes de encontrar a sessão depois que ele for iniciado?

Quando uma sessão de jogo for iniciado, ele não é anunciado no serviço de cruzamento. Se a slots de player ficarem disponíveis dentro da sessão e o arbitrador (host) deseja atrair novos jogadores, o arbitrador deve criar um novo tíquete de correspondência para a sessão em andamento com o **MatchmakingService.CreateMatchTicketAsync** método. O tíquete anuncia a sessão novamente e descobre mais jogadores. Ver [SmartMatch cruzamento](smartmatch-matchmaking.md).


### <a name="if-a-game-session-is-open-can-a-user-who-has-just-joined-a-game-simply-join-the-session-and-start-playing-without-having-to-wait-for-the-reservation"></a>Se uma sessão de jogo estiver aberta, pode um usuário que acaba de ingressar um jogo simplesmente ingressar na sessão e iniciar a reprodução sem precisar aguardar a reserva?

Sim. Isso é especialmente útil em casos em que o seu título usa várias sessões para acompanhar os subgrupos de jogadores em uma sessão do jogo. O ingresso de usuário pode ingressar na sessão que representa seu grupo e, em seguida, preciso ingressar na sessão de jogo maior.


### <a name="when-large-game-sessions-are-playing-in-my-title-why-arent-all-session-members-seeing-the-game-invite-toast"></a>Quando as sessões de jogos grandes estão jogando no título da minha, por que todos os membros da sessão não estiverem vendo a torrada convite jogo?

Quando seu título adiciona um usuário à sessão por meio do ingresso, sempre define o título do **MultiplayerSessionMember.InitializeRequested** propriedade como true. Isso informa ao MPSD para aguardar o restante dos membros a sessão antes de passar o jogo fora do estágio de inicialização. Caso contrário, os usuários têm uma janela muito pequena na qual se unir e deixará de notificações do sistema e as notificações de alterações de sessão.


### <a name="i-am-seeing-inconsistent-game-behavior-and-have-received-protocol-activation-information-referencing-game-party-features"></a>Eu estou vendo comportamento inconsistente de jogo e recebeu informações de ativação de protocolo, fazendo referência a recursos de jogos de terceiros.

Isso indica que você está misturando Multiplayer 2014 e 2015 de funcionalidade com vários participantes. A API para 2015 Multiplayer nunca deve ser usada com o código escrito para vários jogadores de 2014.


### <a name="why-am-i-seeing-the-syntax-for-v105-session-documents-in-my-traces-although-i-have-configured-a-v107-session-template"></a>Por que estou vendo a sintaxe para documentos de sessão v105 em meu rastreamentos apesar de eu ter configurado um modelo de sessão v107?

O MPSD executa a conversão automática entre versões do documento de sessão com base em solicitação do cliente. No momento, todas as APIs de serviço Xbox usam v105 para solicitações para o MPSD. Isso pode resultar em uma sintaxe diferente entre os modelos de sessão v107 e v105 rastreia, mas não tem nenhum outro impacto funcional. Modelos de sessão devem ser configurados como v107.

© 2015 Microsoft Corporation. Todos os direitos reservados.
Enviar comentários na <https://forums.xboxlive.com/spaces/22/index.html>.
Versão: 2.0.90731.0
