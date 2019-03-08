---
title: Visão geral do programa de desenvolvedor
description: Saiba mais sobre os programas de desenvolvedor diferentes disponíveis para uso do Xbox Live.
ms.assetid: 1166308a-4079-41b4-8550-ce04b82b4f72
ms.date: 05/30/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, o programa de desenvolvedor, para criadores
ms.localizationpriority: medium
ms.openlocfilehash: 05abd3f28328f4418f5a8a772049b3869b488ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603881"
---
# <a name="developer-program-overview"></a>Visão geral do programa de desenvolvedor

Se você quiser desenvolver títulos do Xbox Live habilitado, há várias opções disponíveis para você. Cada um oferece níveis variados de investimento de tempo de sua parte, os recursos disponíveis para você e as opções de suporte.

## <a name="xbox-live-creators-program"></a>Programa de Criadores do Xbox Live

O programa de criadores do Xbox Live é um bom ponto de partida do Xbox Live, se você estiver procurando para se familiarizar com o desenvolvimento do Xbox Live. Nenhum processo de aprovação da Microsoft é necessária para participar deste programa, e há certificação mínimo e requisitos de publicação.

O programa de criadores do Xbox Live só suporta a criação de títulos para o [plataforma Universal do Windows](https://msdn.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide) (UWP).  Esses títulos criados como jogos UWP executados em computadores com Windows 10 e em consoles Xbox One.  Para obter mais detalhes sobre como executar jogos UWP no Xbox One, consulte [UWP no Xbox One](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/index).  

No Xbox One, que oferece uma experiência de armazenamento estruturado de jogadores, os jogos publicados por meio do programa de criadores do Xbox Live serão vendidos na seção nova coleção criadores a Microsoft Store no Xbox. Isso oferece um equilíbrio entre a garantia de uma plataforma aberta, onde qualquer pessoa pode desenvolver e lançar um jogo e um console de experiência de armazenamento estruturado jogadores passaram a conhecer e esperar. No Windows 10, o título será publicado entre todos os outros jogos Xbox Live em que a Microsoft Store.

### <a name="publishing-and-certification"></a>Certificação e publicação
Você deve ser registrado na [programa de desenvolvedores do Partner Center](https://developer.microsoft.com/store/register) lançar um jogo como parte do programa de criadores do Xbox Live. Há dois conjuntos de requisitos que seu jogo deve seguir:

1. Integrar o Xbox Live entrar e exibir a identidade do usuário (nome de jogador, Gamerpic, etc.). Todos os outros serviços do Xbox Live são opcionais.
2. Siga o padrão [Microsoft Store políticas](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx).

### <a name="supported-xbox-live-services"></a>Serviços com suporte do Xbox Live
Títulos habilitados sob o programa de criadores do Xbox Live podem usar placares de líderes, estatísticas em destaque, armazenamento de título, conectado de armazenamento e um conjunto restrito de recursos sociais. Realizações, multiplayer online e muitos recursos sociais são **não** com suporte para títulos do programa de criadores do Xbox Live.

Para obter uma lista completa de serviços com suporte, consulte o [tabela de recursos](#feature-table).

### <a name="supported-third-party-game-development-engines"></a>Mecanismos de desenvolvimento de jogos de terceiros com suporte
Títulos do programa de criadores do Xbox Live são jogos UWP que podem ser criados com um número de mecanismos de jogos populares. A Microsoft fornece documentação para integrar serviços do Xbox Live em UWP jogos criados com o [mecanismo de jogo do Unity](https://unity.com). Você pode encontrar [documentação](get-started-with-creators/develop-creators-title-with-unity.md) detalhando a integração do Xbox Live com jogos do Unity aqui neste site, bem como fazer o download e saiba mais sobre a Microsoft criou [plug-in do Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin).

Títulos do programa de criadores do Xbox Live também podem ser criados com os mecanismos de jogos [constructo (2 e 3)](https://www.scirra.com/construct2), e [GameMaker Studio 2](https://www.yoyogames.com/gamemaker). Ambos os mecanismos de jogos tem adicionado o suporte do Xbox Live, no entanto, que suporte é manipulado pelo jogo mecanismos para criadores e não da Microsoft. Para obter detalhes e suporte para adicionar o Xbox Live ao seu projeto de construção ou GameMaker Studio 2, você terá consultar a documentação de cada mecanismos de jogos, respectivamente.

[Aprenda a integrar o Xbox Live em seu projeto de construção.](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)

[Aprenda a integrar o Xbox Live em seu projeto GameMaker Studio 2.](https://www.yoyogames.com/gamemaker/xblc)

Para outros mecanismos de desenvolvimento de jogos, como [MonoGame](https://www.monogame.net/) ou [Xenko](https://xenko.com/), que não têm inclusas na funcionalidade do Xbox Live ou um plug-in, você ainda pode usar as APIs do Xbox Live para adicionar o Xbox Live para seu título. Para usar a API do Xbox Live de seu projeto, você pode adicionar referências aos binários com pacotes NuGet ou adicionar a fonte do API. A adição de pacotes NuGet torna a compilação mais rápida enquanto a adição da fonte torna a depuração mais rápida.

### <a name="support-and-feedback"></a>Suporte e comentários
Alguma dúvida, você pode ter que pode ser respondida sobre o [fóruns do MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev).  Também é possível fazer perguntas relacionadas de programação [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) usando a marca "xbox live".  A equipe do Xbox Live estarão engajados com a comunidade e continuarão a melhorar nossas APIs, ferramentas e documentação com base nos comentários recebidos lá.

Para os desenvolvedores em que o programa de criadores do Xbox Live, você pode [enviar uma nova ideia](https://xbox.uservoice.com/forums/363186--new-ideas?category_id=196261) ou [votar em uma ideia existente](https://xbox.uservoice.com/forums/251649?category_id=210838) em nosso [Xbox User Voice](https://xbox.uservoice.com/forums/363186--new-ideas).

## <a name="idxbox"></a>ID@Xbox

O programa de criadores do Xbox Live é ótimo para muitos desenvolvedores e jogos. Mas se você gostaria de acessar a pilha completa de Xbox Live, incluindo multiplayer online, conquistas e jogos, ou você deseja acessar todo o poder da Xbox One família de dispositivos usando os kits de desenvolvimento de hardware, o [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) programa é para você.

Jogos no ID@Xbox programa deve ser aprovado de conceito e percorra certificação completo no Xbox One e Windows 10, que é um compromisso maior com a hora de sua parte.
ID@Xbox títulos de obtém posicionamento na seção principal da Store, em comparação com a coleção de criadores, que podem permitir a maior exposição aos clientes.

Os desenvolvedores no ID@Xbox programa também ganha acesso ao suporte de desenvolvedor e assistência promocional da Microsoft, bem como o conjunto completo de white papers privada e desenvolvedor fóruns técnicos. Você pode continuar a usar [fóruns do MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev) ou faça perguntas relacionadas de programação [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) usando a marca "xbox live" se desejar.

## <a name="microsoft-partners"></a>Parceiros da Microsoft

Os desenvolvedores que trabalham com um publicador de jogo é um Microsoft Partner têm acesso ao conjunto completo de recursos do Xbox Live e dedicada representantes da Microsoft para ajudá-lo em seu desenvolvimento, certificação e processo de lançamento.

## <a name="feature-table"></a>Tabela de recursos

A tabela a seguir ilustra os recursos disponíveis para o programa de criadores do Xbox Live, e [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) programas.  

<table>

<tr>
<th>Área de recurso</th>
<th>Recurso</th>
<th>Descrição</th>
<th> ID@Xbox </th>
<th>Programa de Criadores do Xbox Live</th>
</tr>

<tr>
<td rowspan="2" class="dev-program-feature-name">Identidade</td>
<td>Entrada / inscrição</td>
<td>Permitir que os jogadores entrar no Xbox Live em seu título ou criar uma nova conta do Xbox Live, se necessário</td>
<td class="xbl-features-required">Obrigatório</td>
<td class="xbl-features-required">Obrigatório</td>
</tr>

<tr>
<td>Identidade do usuário</td>
<td>Utilizar a identidade do Xbox Live, exibindo o Gamertag, Gamerpic, etc</td>
<td class="xbl-features-required">Obrigatório</td>
<td class="xbl-features-required">Obrigatório</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="13" class="dev-program-feature-name">Redes sociais</td>

<td>Presença básica</td>
<td>Exibir cadeias de caracteres de presença básica mostrando a atividade do usuário dentro de um título.  Eg: "Steve está reproduzindo Minecraft"</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>Executados recentemente</td>
<td>Aparecem em títulos executados recentemente no Xbox App ou no Xbox One</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>Feed de atividades</td>
<td>São exibidos na atividade de feed no Xbox App ou no Xbox One</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>Hub dos Jogos</td>
<td>Ter um Hub de jogo associado com o título da exibição de estatísticas, vídeos e outros tipos de conteúdo em um feed específico para o título da</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>Clubes</td>
<td>Players podem usar o App Xbox ou no Xbox One para criar o paus que podem ser opcionalmente associada com o título.</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>Procurando por grupo (LFG)</td>
<td>LFG permite que os jogadores para encontrar outras pessoas fora de jogo para agendar um jogo com vários participantes.</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>GameDVR</td>
<td>Players podem capturar vídeo de suas sessões de jogo e compartilhá-los no feed de atividades.</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>Transmissão</td>
<td>Os jogadores podem transmissão ao vivo seu jogo por meio de serviços, como o Mixer e Twitch de streaming</td>
<td class="xbl-features-automatic">Automática</td>
<td class="xbl-features-automatic">Automática</td>
</tr>

<tr>
<td>Presença avançada</td>
<td>Mostra informações mais detalhadas sobre os jogadores em seu título.  Enquanto a presença básica pode mostrar "Usuário está no carro Racing Game", a presença avançada permite que você especifique uma cadeia de caracteres mais detalhada, como "usuário está orientando SuperCar em RainyForest"</td>
<td class="xbl-features-required">Obrigatório</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

<tr>
<td>Amigos</td>
<td>Recupere a lista de amigos do usuário entrar para habilitar cenários de jogo social em seu título.</td>
<td class="xbl-features-required">Obrigatório</td>
<td class="xbl-features-limited">Opcional / limitado (somente os amigos que foram reproduzidos seu título são expostos)</td>
</tr>

<tr>
<td>Privacidade</td>
<td>Permitir que os jogadores como mudo ou bloquear ou outros jogadores</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional</td>
</tr>

<tr>
<td>Reputação</td>
<td>Os jogadores ganharem ou perder reputação por meio de seu comportamento. Comportamento é usado em um relacionamento de pessoas e pode ser usado por seu título formas personalizadas.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

<tr>
<td>Gerenciador de social</td>
<td>Recuperar informações sobre o grafo social de um jogador de maneira eficiente</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-limited">Opcional / limitado (somente os amigos que foram reproduzidos seu título são expostos)</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="4" class="dev-program-feature-name">Plataforma de Dados</td>

<td>Estatísticas dos jogadores</td>
<td>Estatísticas sobre os jogadores que podem ser usados no placar de líderes de carregamento.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional (apenas plataforma de dados de 2017)</td>
</tr>

<tr>
<td>Estatísticas em destaque</td>
<td>Designar certas estatísticas como "Em destaque: estatísticas" que será exibido no Hub de jogo.</td>
<td class="xbl-features-required">Obrigatório</td>
<td class="xbl-features-optional">Opcional (apenas plataforma de dados de 2017)</td>
</tr>

<tr>
<td>Placares de líderes</td>
<td>Recuperar e exibir estatísticas de player de maneira classificada para incentivar a concorrência.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional (apenas plataforma de dados de 2017)</td>
</tr>

<tr>
<td>Realizações com jogos</td>
<td>Designar certas estatísticas como "Em destaque: estatísticas" que será exibido no Hub de jogo.</td>
<td class="xbl-features-required">Obrigatório</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="1" class="dev-program-feature-name">Media</td>

<td>Pesquisa Contextual</td>
<td>Anote GameDVR clipes com palavras-chave para tornar mais fácil de jogadores localizar clipes correspondente ao que eles desejam assistir.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>


<tr class="dev-program-feature-start">
<td rowspan="2" class="dev-program-feature-name">Armazenamento</td>

<td>Armazenamento Conectado</td>
<td>Jogo móvel salva em um Consoles do Xbox e PCs</td>
<td class="xbl-features-required">Obrigatório</td>
<td class="xbl-features-optional">Opcional</td>
</tr>

<tr>
<td>Armazenamento de Títulos</td>
<td>Armazenamento em nuvem para grandes quantidades de por usuário ou dados por título.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="6" class="dev-program-feature-name">On-line com vários participantes</td>

<td>Diretório de sessão multijogador (MPSD)</td>
<td>Armazena informações sobre uma sessão para múltiplos jogadores, como lista de jogadores, estado, etc.</td>
<td class="xbl-features-optional">Obrigatório</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

<tr>
<td>Para que haja correspondência</td>
<td>Xbox Live pode corresponder a players diferentes juntos para uma sessão com vários participantes.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

<tr>
<td>Arena</td>
<td>Players podem competir em relação uns aos outros estilos de torneio.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

<tr>
<td>Jogo bate-papo</td>
<td>Bate-papo de jogadores em um jogo com vários participantes</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

<tr>
<td>Computação de Xbox Live</td>
<td>Implante arquivos executáveis e ativos que seu título pode se comunicar, para descarregar o cálculo do cliente.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">Sem suporte</td>
</tr>

</table>
