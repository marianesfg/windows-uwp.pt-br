---
title: Guia de integração do título arena
description: Saiba como criar um título no suporte para a plataforma do Xbox Live Arena.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arena, torneio
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659881"
---
# <a name="arena-title-integration-guide"></a>Guia de integração do título arena

## <a name="introduction"></a>Introdução

A plataforma da Arena para Xbox permite organizadores de torneio (TOs) para criar e operar torneios para títulos que dão suporte a Arena. Dá suporte a arena tanto um Xbox Live para que permite que o publicador e o usuário executar torneios, como guias de instruções de terceiros que se integram com Arena. O estilo do torneio, como eliminação simples ou Suíça, é determinado pelo campo para.

Este tópico orienta você, como um desenvolvedor de título, por meio de como implementar o suporte a Arena em seu título. Depois que um jogo for Arena habilitado, ele funcionará com qualquer formato com suporte do torneio selecionado pelo campo para. PARA orquestra correspondências para o título de acordo com a estrutura do torneio. O título da Arena habilitado, em seguida, coloca os usuários certos em cada correspondência no jogo e relata os resultados de correspondência de volta para o campo para.

## <a name="overview"></a>Visão geral

Xbox Arena usa conceitos familiares para desenvolvimento de jogos para múltiplos jogadores do Xbox. Se você não estiver familiarizado com o sistema com vários participantes do Xbox, é recomendável que você examine essa documentação antes de continuar. O processo é semelhante de um usuário que aceita um convite para um jogo com vários participantes. Quando for o momento de um usuário reproduzir uma correspondência em um torneio Arena, uma notificação do sistema é exibida para informar ao usuário. Aceitando a torrada faz com que o título receber um evento de ativação de protocolo Arena padrão. Se o título não estiver sendo executado, ele é iniciado no momento.

A correspondência de torneio em si é coordenada por meio de uma sessão de diretório de sessão com vários participantes (MPSD). No caso do setor, a sessão é criada pela plataforma Arena em vez de por seu título. Isso é feito usando um modelo de sessão e configuração adicional de jogo que foram criados por você como o Editor de título. Os participantes do torneio correspondência serão já ingressaram à sessão. Como o Editor de título, você deve configurar o modelo de sessão usado para Arena para dar suporte a arbitragem para relatórios de resultados. Todos os requisitos para a sessão são incluídos neste documento. O título também é gratuito para criar quaisquer sessões adicionais necessárias.

O título da usa a sessão para configurar os resultados de correspondência e relatório. Respondendo a ativação de protocolo e interagir com a sessão MPSD são o nível mínimo de integração exigida pelo títulos Arena. Navegação e se registrar para torneios é suportada por meio da interface do usuário a Arena do Xbox. Opcionalmente, títulos podem obter informações adicionais sobre o torneio do serviço Hub de torneios, permitindo uma experiência mais rica de no título. Por exemplo, usando o Hub de torneios, seu título pode exibir contexto, como os nomes de equipe e descobrir novas sessões de correspondência para um usuário. Esse fluxo de dados é representado pelas setas verdes no diagrama a seguir e é descrito detalhadamente na [experiência de requisitos e práticas recomendadas](#experience-requirements-and-best-practices) seção. As setas de esmaecimento indicam que a referência da equipe para a sessão é alterado ao longo do tempo, quando o usuário move a correspondência de correspondência dentro do torneio.

![](../../images/arena/tournament-flow.png)


A ativação de protocolo da Arena URI contém informações sobre o torneio, a sessão para a correspondência e um link profundo que seu título pode invocar quando a correspondência for relativa. O link profundo retorna o usuário na interface do usuário do Xbox Arena. Esses componentes do URI são descritas mais detalhadamente a [ativação de protocolo](#1protocol-activation) seção [requisitos básicos para integração da Arena](#basic-requirements-for-arena-integration).

## <a name="basic-requirements-for-arena-integration"></a>Requisitos básicos para integração da Arena

Esta seção fornece orientação técnica e detalhes para a integração com os requisitos mínimos para dar suporte a Arena do seu título. Esse estilo de integração utiliza o fluxo de dados representado pelas setas laranjas no diagrama de visão geral a seguir.

![](../../images/arena/arena-data-flow.png)

O título é ativado de protocolo de interface do usuário a Arena do Xbox. Isso pode se originar de uma notificação do sistema, na página de detalhes do torneio, ou qualquer outro ponto de entrada para a correspondência. As etapas que ocorrem quando um usuário participa de uma correspondência são:

1.  O título é ativado de protocolo por interface do usuário a Arena do Xbox.
2.  O título da usa os parâmetros de ativação para executar uma única correspondência.
3.  Quando a correspondência for relativa, seu título relata os resultados para a sessão de jogo MPSD.
4.  O título da dá ao usuário a opção para retornar para a interface do usuário Arena.

As seções a seguir percorrem cada uma dessas etapas em detalhes.

### <a name="1--protocol-activation"></a>1.  Ativação de protocolos

A interface do usuário Arena inicia as coisas, ativando protocolo seu título quando uma correspondência está pronta para o usuário. Há dois casos: o título pode ser iniciado neste momento, ou ele pode já estar em execução. Ambos os casos são semelhantes ao que acontece quando um título é ativado em resposta a um usuário que aceita um convite para um jogo com vários participantes.

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>Se o título é iniciado no momento da ativação de protocolo

O **Activated** evento é acionado pela primeira vez quando o título é iniciado. Se essa ativação inicial é uma ativação de protocolo da Arena, o título foi iniciado por um usuário tentar reproduzir em um torneio. Ter seu título assim que possível pular para a correspondência, ignorando as telas de menu e entrar. A ID de usuário de Xbox (XUID) do usuário participante é fornecido na URI de ativação e já devem conseguir entrar.

#### <a name="if-your-title-is-already-running"></a>Se seu título já está em execução

O **Activated** também podem acionar eventos com uma ativação de protocolo da Arena enquanto seu título já está em execução. O **ativação** evento é disparado apenas por uma ação explícita do usuário (atuando em uma notificação do sistema que indica a correspondência, ou pular para a correspondência da interface do usuário a Arena do Xbox). Se o título está aguardando em uma tela de menu, colocá-lo a ir imediatamente para a correspondência de torneio. Se seu título recebe a ativação de protocolo durante um jogo, colocá-lo a dar ao usuário a opção de deixar o jogo e insira a correspondência de torneio da maneira mais vantajoso possível. Dar ao usuário uma chance de salvar seu jogo, se necessário.

Ativações de protocolo são entregues ao seu título por meio de `CoreApplicationView.Activated` eventos. Se o `IActivatedEventArgs.Kind` dos argumentos do evento estiver definida como `Protocol`, a ativação é uma ativação de protocolo e os argumentos podem ser convertidos para o `ProtocolActivatedEventArgs` classe, em que o URI de ativação de protocolo está disponível por meio do `Uri` propriedade.

Ter seu título verificar o XUID sobre a ativação de protocolo URI. Se o jogador atual não corresponder, o título também precisará alternar contextos de usuário.

#### <a name="the-protocol-activation-uri"></a>O URI de ativação de protocolo

O modelo para o URI é:

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

Para títulos de ERA no console, o esquema de ativação é um pouco diferente:

```
ms-xbl-{titleIdHex}://
```

Isso é o mesmo para convites. Para essa finalidade, a ID do título deve ser oito caracteres hexadecimais, portanto, ele incluirá zeros à esquerda se necessário.

Primeiro verifique se o `Uri.Host` (o nome imediatamente após o separador "://") é "torneio". Isso é como as ativações de protocolo da Arena são diferenciadas dos ativações de outros recursos, como convites de jogos.

Os argumentos de cadeia de caracteres de consulta será codificado de URL e diferenciam maiusculas de minúsculas. O título não deve confiar na ordem de parâmetros, e ele deve ignorar os parâmetros não reconhecidos.


Paramater(s) | Descrição
--- | ---
**action** | A única ação com suporte é "joinGame". Se o **ação** está ausente ou outro valor seja especificado para ele, ignorar a ativação.
**joinerXuid** | O **joinerXuid** é o XUID do usuário conectado que está tentando reproduzir em uma correspondência de torneio. O título deve permitir alternar para o contexto desse usuário. Se o **joinerXuid** não está assinado, o título deve solicitar ao usuário a fazer isso abrindo o seletor de conta. XUIDs sempre são expressos em decimais.
**organizerId, tournamentId** | O **organizerId** e **tournamentId**, quando combinados, o identificador exclusivo para o torneio associado a correspondência do formulário. Use esse identificador para recuperar as informações mais detalhadas sobre o torneio do Hub de torneios, se você escolher para exibi-lo em seu título.
**teamId** | O **teamId** é um identificador exclusivo para a equipe, no contexto do torneio que o usuário (especificado pelo **joinerXuid** parâmetro) é um membro de. Como o **organizerId** e **tournamentId** parâmetros, você pode usar o **teamId** opcionalmente recuperar informações sobre a equipe do Hub de torneios.
**scid, templateName, name** | Juntos, eles identificam a sessão. Estes são os mesmos três parâmetros no caminho de URI MPSD à sessão:</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`.</br></br>No XSAPI, eles são os três parâmetros para o `multiplayer_session_reference `construtor.
**returnUri, returnPfn** | O **returnUri** é um URI de ativação de protocolo para retornar o usuário na interface do usuário do Xbox Arena. O **returnPfn** parâmetro pode ou não estar presente. Se for, ele é o nome da família de produtos (PFN) para o aplicativo que foi criado para lidar com o **returnUri**. Para o código de exemplo que mostra como usar esses valores, consulte [retornando para a interface do usuário do Xbox Arena](#4returning-to-the-xbox-arena-ui).

### <a name="2--playing-the-match"></a>2.  Reproduzindo a correspondência

Quando a sessão MPSD é criada pelo organizador torneio, o usuário é definido como um membro inativo da sessão em questão. O título da imediatamente deve definir o player para Active Directory usando `multiplayer_session::join()`. Isso indica ao Xbox Live e outros usuários na correspondência de que o usuário está no seu título e pronto para reproduzir.

A hora de início para a correspondência é na sessão no `/servers/arbitration/constants/system/startTime` como um valor de data e hora no formato UTC padrão (por exemplo, "2009-06-15T13:45:30.0900000Z"). (Hora de início também está disponível no XSAPI como `multiplayer_session_arbitration_server::arbitration_start_time()`, a partir do XDK 1703). PARA pode criar a sessão como muito antes da hora de início quanto desejar (incluindo simultaneamente com a hora de início). Correspondência são enviadas notificações para os participantes de correspondência na hora de início, para que os títulos de nunca ser ativados antes da hora de início. A hora de início é também o horário mais cedo, no qual MPSD permitirá que os resultados a serem relatados a correspondência.

O título da examina cada membro `/member/{index}/constants/system/team` propriedade (`multiplayer_session_member::team_id()`) para descobrir qual equipe cada usuário está em.

O título também usa a sessão para determinar as configurações de correspondência, como o mapa e o modo. Essas configurações específicas de título podem ser definidas no modelo de sessão ou como parte da definição do torneio como constantes personalizadas. (Para obter mais informações, consulte [Configurando um título para Arena](#configuring-a-title-for-arena).) Aqui está um exemplo:

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

Você pode recuperar essas configurações de sessão como um objeto de notação JSON (JavaScript Object) usando o `multiplayer_session_constants::session_custom_constants_json()` API.

Em geral, o título deve tratar a sessão Arena da mesma forma que ele trataria a sua própria sessão MPSD. Por exemplo, ele pode criar identificadores e assine para receber notificações de RTA. Mas há algumas diferenças. Por exemplo, o título não pode alterar a lista da sessão de jogo ou usar os recursos de qualidade de serviço (QoS) da sessão, e ele deve participar de arbitragem. Os detalhes completos da sessão são fornecidos neste documento.

### <a name="3--reporting-results"></a>3.  Resultados do relatório

Os resultados da correspondência são relatados novamente a Arena e para por meio da sessão usando um recurso chamado arbitragem. Arbitragem é uma estrutura para usar uma sessão para executar com segurança uma correspondência e um resultado de relatório.

A sessão fornecida para o título na etapa de ativação de protocolo será uma sessão arbitrário, o que significa que ele tem uma linha do tempo fixa que impõe a estrutura de arbitragem. Este diagrama mostra a linha do tempo de arbitragem.

![](../../images/arena/arbitration-timeline.png)

Pelo menos um player deve estar ativo na sessão antes do tempo de perder, que é a hora de início (`multiplayer_session_arbitration_server::arbitration_start_time()`) mais o tempo limite de perder. O tempo limite de perder é na sessão como o número de milissegundos no `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`). Se ninguém ingressou a sessão como ativa antes da hora de perder, a correspondência será cancelada.

O tempo limite de arbitragem é em milissegundos no `/constants/system/arbitration/arbitrationTimeout` (`multiplayer_session_constants::arbitration_timeout()`). Tempo de arbitragem é a hora de início mais o valor de tempo limite de arbitragem e representa o tempo pelo qual os participantes devem ter concluído a correspondência e reportaram resultados. Esse valor é definido no modelo de sessão ou modo de jogo pelo editor. Defini-lo para permitir tanto tempo quanto seu título precisa para a correspondência seja concluída.

O título pode relatar os resultados a qualquer momento entre a hora de início e a hora de arbitragem. Arbitragem ocorrer a qualquer momento entre a hora de perder e a hora de arbitragem, depois de cada membro ativo da sessão enviou os resultados. Por exemplo, se apenas um membro está ativo na sessão no tempo a perder, eles podem (e deve) lançar um resultado e arbitragem ocorrerá. Não importa quantos resultados estão disponíveis em tempo de arbitragem, arbitragem ocorrerá se ele já não tiver. Se nenhum resultado é enviado quando o tempo de arbitragem é atingido, todos os participantes na correspondência recebem uma perda.

Também é possível que um servidor de jogo forçar a arbitragem a qualquer momento, simplesmente escrevendo um resultado arbitrário.

Se um usuário estiver em uma sessão que já tenha sido arbitrário (ou porque o tempo limite de arbitragem expirou, um servidor de jogo arbitra a sessão ou o usuário se associe atrasado), seu título termina a correspondência e exibe o resultado arbitrário para o usuário.

Resultados de arbitragem sempre incluem o resultado para cada equipe. Quando um player individual grava um resultado para a sessão, ele inclui não apenas o resultado da sua equipe, mas o conjunto completo de resultados para cada equipe.

Os resultados em JSON tem esta aparência.

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

Neste exemplo, a equipe de IDs é "vermelha" e "azul". A equipe vermelha chegou no primeiro e a equipe azul em segundo lugar. Os possíveis valores para **resultado** são:

* "win"
* "perda"
* "desenhar"
* "noshow"

Se **resultado** for algo diferente de "classificação", a classificação for omitida para essa equipe.

Usuários individuais gravar os resultados da correspondência `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`). O exemplo a seguir ilustra como um título pode construir e enviar um conjunto de resultados, supondo que o título não é a equipe com base (ou seja, todos os jogadores são nas equipes de um).

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

Após a ocorrência de arbitragem MPSD coloca os resultados finais na `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`), juntamente com algumas outras propriedades como mostrado aqui.

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

As possibilidades de **resultState** incluem:

* "concluído": Todos os jogadores active relatou um resultado.
* "partialresults": Algumas, mas nem todos os jogadores ativos relatou um resultado antes do tempo limite de arbitragem.
* "/noresults": Nenhum dos jogadores active relatou um resultado antes do tempo limite de arbitragem.
* "cancelado": Nenhum jogadores ativos chegaram antes do tempo limite de perder.

No caso de "/noresults" e "cancelado", os resultados são omitidos.

O **resultSource** é "arbitragem" quando MPSD executa o arbitramento de acordo com os resultados relatados por jogadores individuais. Um servidor de jogo pode gravar /servers/arbitration/properties/system em si, ignorando a arbitragem. Nesse caso, ele indica um **resultSource** de "servidor". Também é possível que um servidor de jogo reescrever (ou seja, corrigir ou ajuste) os resultados, na qual o caso conjuntos **resultSource** "ajustada".

O **resultConfidenceLevel** é um inteiro entre 0 e 100 que indica o nível de consenso no resultado. 100 significa que todos os jogadores concordam. 0 significa que não havia nenhum consenso e um resultado foi escolhido essencialmente aleatoriamente daquelas enviadas. Quando um servidor de jogo define os resultados de arbitragem, ele define o **resultConfidenceLevel**-normalmente como 100, a menos que, por algum motivo, mesmo o servidor do jogo em si não estiver certo sobre (por exemplo, se ele está relatando os resultados de seu próprio por jogadores votação procedimento). Defina as **resultConfidenceLevel** também quando os resultados são ajustados para refletir a confiança nos resultados da ajustado.

Quando o **resultSource** é "arbitragem" (e o **resultState** é "concluído" ou "partialresults"), os resultados fornecidos são uma cópia exata de pelo menos um dos resultados relatados pelo player.

### <a name="4--returning-to-the-xbox-arena-ui"></a>4.  Retornando a Arena do Xbox da interface do usuário

Quando a correspondência é pela (ou possivelmente em resposta à solicitação de um jogador para abandonar a correspondência em andamento), apresentam uma opção para que o jogador retornar para a interface do usuário do Xbox Arena de onde eles associados a correspondência. Isso pode ser feito de uma tela de resultados de pós- correspondência de ou qualquer outra interface de usuário do fim do jogo.

Para retornar para a interface do usuário Arena, invocar o **returnUri** que foi passado do URI por ativação de protocolo usando o `Windows::System::Launcher` classe. Certifique-se de incluir o contexto do usuário.

A API de inicialização é exposta ligeiramente diferente para jogos ERA que os jogos de plataforma Universal do Windows (UWP). A versão da ERA da API não permite que o PFN seja fornecido, portanto, o PFN pode ser ignorado, mesmo se estiver presente, sobre a ativação do URI.

Aqui está o código de exemplo para uma ÉPOCA retornar o usuário na interface do usuário Arena.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

Jogos UWP podem definir as **TargetApplicationPackageFamilyName** propriedade em **LauncherOptions** para o PFN de retorno, se a ativação de protocolo URI fornecido. Isso garante que o aplicativo específico é iniciado e que o usuário é levado para a Store, se o aplicativo já não estiver instalado.

Aqui está o código de exemplo para um aplicativo UWP retornar o usuário na interface do usuário Arena.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

Depois de invocar a interface do usuário Arena, seu título continua em execução, possivelmente nessa mesma tela, aguardando outro evento de ativação de protocolo. Dessa forma, se o jogador tem outra correspondência para reproduzir, o título fica pronto para começar. Isso acelera a experiência do usuário quando o jogador vai de correspondência para correspondência, alternando entre o título e a interface do usuário Arena do Xbox.

### <a name="handling-errors-and-edge-cases"></a>Tratamento de erros e casos de borda

O fluxo na seção anterior descreve o caminho final por meio da execução de uma correspondência. Há muitos lugares onde você poderá ver um comportamento inesperado ou problemas podem surgir. Esta seção aborda um número de erros em potencial ou casos de borda e fornece recomendações para resolvê-los no seu título.

#### <a name="game-session-not-found"></a>Sessão de jogo não encontrado

Se o título é iniciado com uma referência de sessão para uma correspondência, e o cliente, em seguida, Falha ao receber um erro ao solicitar essa sessão de MPSD, apresente um erro para o usuário indicando que eles são incapazes de unir a correspondência.

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>Usuário tenta unir uma correspondência que foi reproduzida

Se um usuário tentar ingressar em uma correspondência após os resultados tenham sido relatados, bloquear esse cliente de iniciar uma nova correspondência e apresentar ao usuário um erro. Você pode verificar se os resultados foram relatados por Iterando através dos membros da sessão para ver se qualquer possui um `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) definido como "concluído".

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>Clientes do jogo não é possível estabelecer uma conexão p2p

Se seu título usa conexões de 1:1 (p2p) entre os clientes para vários jogadores e não tem suporte para retransmissões, pode haver instâncias em que os usuários que são correspondidos juntos são não é possível formar uma conexão p2p.

Se o cliente é capaz de recuperar a sessão para a correspondência, mas não é possível se conectar a outros clientes, relatar um resultado de "desenhar" para cada equipe sob `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`). Isso indica ao Xbox Live que a correspondência não tiver sido feita. Ele também evita que explorações de onde os usuários podem Avançar um torneio, forçando uma falha de conexão p2p.

#### <a name="game-client-disconnects-mid-match"></a>Jogo cliente se desconecta a correspondência intermediário

Um dos cenários mais comuns de erro é quando os clientes se desconectar enquanto está sendo reproduzida uma correspondência. Dependendo do estado da correspondência e sua implementação, há algumas opções para lidar com esse caso:

* Se a correspondência de torneio é aquela versus um, ou se todos os membros de uma equipe se desconectar de outros clientes e/ou em seu serviço de jogo, recomendamos que você relate "perda" da equipe que não está mais conectado. Isso impede que o caso em que os usuários podem forçar uma desconexão de uma correspondência que eles estão perdendo para impedir que o resultado que está sendo relatado.
* Se a correspondência entre as equipes com vários membros, e um subconjunto dos clientes para uma equipe desconectar Mid-correspondência, você pode escolher finalizar a correspondência nesse ponto ou permitir que ele continue com os usuários restantes. Se você optar por continuar a correspondência, você também pode permitir que os usuários desconectados reingressar a correspondência.

#### <a name="full-teams-not-present"></a>Equipes completos não está presentes

Se seu título dá suporte a correspondências com as equipes de duas ou mais, você poderá encontrar casos em que alguns membros de uma equipe falharem para iniciar seu título para a sua correspondência. Nesse caso, você pode optar por permitir que os membros da equipe para reproduzir sem seu membro de equipe e, opcionalmente, permitir que esse membro da equipe para a correspondência em andamento.

Como alternativa, você pode impor um resultado. Se você fizer isso, aguarde até o momento de forfeitTimeout para dar uma oportunidade para unir a correspondência antes de impor o resultado de todos os participantes. Isso permite que você ajuste a janela com as alterações para o modo de jogo para o torneio, em vez de ter que atualizar seu título.

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>ArbitrationTimeout excede o comprimento da correspondência

Defina o valor para **arbitrationTimeout** seja maior que o comprimento máximo de tempo que uma correspondência, possivelmente, pode demorar. Dito isso, o título pode incluir modos nos quais os comprimentos de correspondência possível são muito longos ou em que há um máximo. Nesses casos, você considerar:

* Limitação de torneio de play para somente modos com comprimentos fixos, máximo de tempo.
* Informando ao usuário do tempo limite e um resultado antes de emissão de relatórios do **arbitrationTimeout** com base em um desempate na correspondência.

Porque **arbitrationTimeout** expiração disparará um resultado de automática para a correspondência, não espere até que todo **arbitrationTimeout** período antes de relatar um resultado do título.  Em vez disso, crie em um buffer de tempo (por exemplo, **arbitrationTimeout** -1000ms), ou usar um valor separado para controlar o comprimento máximo de correspondência.

#### <a name="other-cases"></a>Outros casos

Para quaisquer outros casos de falha, a orientação geral é informar ao usuário sobre o erro e o usuário retornará para a interface do usuário do Xbox Arena.

## <a name="experience-requirements-and-best-practices"></a>Requisitos de experiência e práticas recomendadas

Porque a Arena apresenta apenas o título com correspondências individuais, novos formatos podem ser introduzidos ao longo do tempo sem a necessidade de atualizações para o título. Portanto a experiência do usuário de linha de base para uma Arena integrado título deve ser simples e flexível o suficiente para trabalhar com a concorrência de vários formatos.

Opcionalmente, você pode usar dados do Hub de torneio em seu título para tornar torneios uma parte mais integrada da experiência do jogo.  Essa funcionalidade pode ser encontrada em 'xbox::services::tournaments'.  Usando essas APIs, você tem a capacidade de:
* Detalhes do torneio na superfície do título da interface do usuário (nome do torneio, nome de equipe, etc.)
* Fornecer descoberta de torneios em seu título
* Mantenha os usuários no seu título entre as correspondências da Arena

## <a name="configuring-a-title-for-arena"></a>Configurando um título para Arena

Para habilitar um título para a Arena, algumas etapas adicionais são necessárias quando você configura no Portal de desenvolvedor do Xbox (XDP) ou [Partner Center](https://partner.microsoft.com/dashboard).

### <a name="enabling-arena-for-your-title"></a>Habilitando a Arena do título

Para habilitar a Arena, vá para a página de configuração de serviço para o título no XDP e selecione 'Arena'.

![](../../images/arena/arena-configure-xdp.png)

Aqui, você terá várias opções:

* **Habilitado arena** – Marque esta caixa de seleção para habilitar a Arena para essa área restrita.
* **Recursos da arena** – esta seção inclui caixas de seleção para habilitar torneios gerados pelo usuário na área restrita e habilitar entre plataformas, que permite aos usuários em várias plataformas para participar de torneios mesmos.
* **Plataformas arena** – permite que você selecione as plataformas em que podem ser reproduzidos torneios do título.
* **Ativos de torneio** – (anteriormente conhecido como em 'Com vários participantes e emparelhamento' seção.) Essas são as imagens do torneio para seu título.

Arena também pode ser habilitado no Partner Center na **torneio** menu sob o serviço Xbox Live.

![Menu da arena no Partner Center](../../images/arena/Arena_On_WDC.JPG)

Você deve publicar a configuração do serviço para que as alterações entrem em vigor. Atualmente, a configuração de Arena Self-service não tem suporte por meio de UDC. Se você estiver usando UDC para configuração de serviço, trabalhe com sua conta de gerente de desenvolvimento para integrar com Arena.

### <a name="setting-up-game-modes"></a>Configurando modos de jogo

Modos de jogo são um recurso do Xbox Live que permite que o Editor predefinir as configurações de uma correspondência de torneio. Esses modos de jogo são usados no momento da criação do torneio para definir as regras de imposto pelo Xbox Live e seu título para o torneio. Como o Editor de título, você precisa de pelo menos um modo de jogo para habilitar torneios gerados pelo usuário para seu título. Os elementos do modo de jogo incluem:

#### <a name="supports-ugt"></a>Dá suporte a UGT?

Modos de jogo podem ser habilitados para uso com torneios gerados pelo usuário (UGTs). Se o título está configurado para suportar UGT, você pode marcar os modos de jogo para que possa ser selecionadas pelos usuários do Xbox Live durante a criação de torneios do título.

#### <a name="team-size"></a>Tamanho da equipe

Isso é o número de usuários que podem participar do torneio como uma equipe. Para um título de usuário único ou torneio, o modo de jogo tem um tamanho da equipe de um. Arena atualmente não dá suporte a tamanhos de equipe variável para torneios.

#### <a name="forfeittimeout"></a>forfeitTimeout

Este é o número de milissegundos, após a correspondência de início tempo, antes que a correspondência será cancelada se não mostrar players para ele (ou seja, se nenhum jogador se definir como ativa na sessão). Defina isso como um período de tempo que é pelo menos tempo suficiente para que um player iniciar seu título e chegue a correspondência de torneio desde o momento em que eles veem a notificação do sistema.

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

Isso é o número de milissegundos, após a correspondência de início tempo, após o qual não há mais resultados serão aceitas. Defina-o como o tempo limite de perder mais provavelmente seria uma quantidade de tempo maior do que a correspondência mais longa. Dessa forma, mesmo que os jogadores iniciar a reprodução logo antes da hora de perder, ele ainda tem tempo suficiente para concluir a correspondência. Se nenhum resultado é relatado no momento do **arbitrationTimeout**, os participantes da correspondência todos uma perda de recebimento. Xbox Arena também aguarda até que o **arbitrationTimeout** para todos os membros ativos para relatar os resultados antes de iniciar a arbitragem.

Esse tempo limite atua como uma barreira, caso algo dê errado e não é reportado de um resultado de um player. Em vez de atrasando todo torneio, esse tempo limite coloca um limite superior na quantidade de tempo aguardará o torneio. Por esse motivo, ele não deve ser definido muito alto, pois ela determina o comprimento máximo de cada torneio redondo.

#### <a name="name"></a>Nome

Esse é o nome de usuário voltado para o modo de jogo. Esse valor é localizável.

#### <a name="description"></a>Descrição

Esta é a descrição de voltadas ao usuário para o modo de jogo. Esse valor é localizável.

#### <a name="custom"></a>Personalizado

O **personalizado** seção para o modo de jogo é um conjunto de propriedades em que os desenvolvedores podem inserir quaisquer definições de configuração específicas de título para a correspondência de torneio. Valores definidos como parte da seção personalizada são gravados para a sessão MPSD correspondência em `/properties/custom/`.

Atualmente, a instalação do modo de jogo não tem suporte por meio de XDP ou UDC. Para criar modos de jogo para seu título, entre em contato com seu gerente de conta de desenvolvedor.

### <a name="requirements-for-the-session-template"></a>Requisitos para o modelo de sessão

Como desenvolvedor de título, você deve fornecer o modelo de sessão para o para uso durante a criação de sessões para correspondências. Há algumas expectativas sobre o conteúdo do modelo.

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

Outras propriedades não mostradas aqui podem ser definidas como desejar.

Sessões da arena são sempre a versão 1. Definindo o **inviteProtocol** para "tournamentGame" permite que as notificações de pronto para correspondência a serem enviados aos participantes do torneio. **memberInitialization** é definido como nulo para desabilitar o QoS. A capacidade de jogo deve ser definida porque a sessão representa uma correspondência, e a capacidade de arbitragem é necessária para o relatório de resultados. O **grande**, **difusão**, e **blockBadMsaReputation** recursos devem ser desabilitados porque elas seriam interferir na operação da sessão.

O título pode especificar suas próprias configurações na seção do modelo, para as configurações que têm um valor fixo para todas as torneios que usam o modelo personalizada. Veja um exemplo a seguir.

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

Por fim, o **maxMembersCount** configuração do sistema é necessária. Defina-o como o número total de players podem tocar em uma correspondência de torneio. O número de jogadores pode ser restrito pelas configurações do modo de jogo, portanto certifique-se de que o valor definido na sessão representa o maior número total de players com suporte para o título. Por exemplo, se o número máximo de jogadores seu jogo dá suporte a uma correspondência é 5 vs. 5, defina **maxMembersCount** para 10. Esse modelo MPSD pode ser usado para dar suporte à correspondência de qualquer número de jogadores até a **maxMembersCount**.
