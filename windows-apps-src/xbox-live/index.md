---
title: Guia do desenvolvedor do Xbox Live
description: Saiba como usar serviços do Xbox Live para conectar seu jogo à rede de jogos do Xbox Live.
ms.date: 08/22/2017
ms.topic: article
keywords: 'Windows 10, uwp, jogos, xbox e xbox live'
ms.localizationpriority: medium
---
# <a name="what-is-xbox-live"></a>O que é o Xbox Live?

O Xbox Live é uma rede de jogos de primeira linha que conecta milhões de jogadores em todo o mundo. Você pode adicionar o Xbox Live a seu jogo do Windows 10 ou Xbox One para aproveitar as vantagens de recursos e serviços do Xbox Live.

Com o Programa de Criadores do Xbox Live, qualquer pessoa que tenham uma conta do [Partner Center](https://partner.microsoft.com/dashboard) pode criar um jogo da UWP (Plataforma Universal do Windows) habilitado para Xbox Live que pode ser executado em consoles do Xbox One e computadores com Windows 10.

Para desenvolvedores de jogos que querem aproveitar a experiência completa do Xbox Live, incluindo vários jogadores, conquistas e desenvolvimento nativo do console Xbox, existem programas de desenvolvedor adicionais detalhados em [Visão geral de Programa de Desenvolvedores](developer-program-overview.md).

Aqui estão alguns motivos para adicionar o Xbox Live a seu jogo:

- O Xbox Live une jogadores no Xbox One e no Windows 10 para que os jogadores possam jogar com seus amigos e conectar-se com uma grande comunidade de jogadores.
- O Xbox Live permite aos jogadores criar um legado de jogos desbloqueando conquistas, compartilhando clipes de jogos épicos, acumular pontuações de jogos e aperfeiçoando seu avatar.
- O Xbox Live permite aos jogadores reproduzir e escolher o ponto em que pararam em outro Xbox One ou computador, trazendo todo o material salvo de outro dispositivo.
- Com mais de 1 bilhão de partidas de vários jogadores jogadas a cada mês, o Xbox Live é criado para desempenho, velocidade e confiabilidade.
- Com vários jogadores entre dispositivos, os jogadores podem jogar com seus amigos, independentemente de se eles jogam no Xbox One ou em um computador com Windows 10.

> [!note]
> Estes tópicos destinam-se a desenvolvedores de jogos que desejam adicionar suporte para o Xbox Live a seus jogos. Se você estiver procurando informações sobre o Xbox Live do consumidor, veja [Xbox Live](https://www.xbox.com/live/).

## <a name="how-xbox-live-works"></a>Como funciona a Xbox Live

Em um nível técnico, o Xbox Live é uma coleção de microsserviços que expõem os recursos do Xbox Live, como o perfil, amigos e presença, estatísticas, placares de líderes, conquistas, vários jogadores e compatibilidade. Os dados do Xbox Live são armazenados na nuvem e podem ser acessados por meio de pontos de extremidade REST e websockets seguros acessíveis por meio de um conjunto de APIs do lado do cliente projetadas para desenvolvedores de jogos.

Além das APIs REST, há APIs que encapsulam a funcionalidade REST do lado do cliente. Para obter mais informações, veja [Introdução às APIs do Xbox Live](introduction-to-xbox-live-apis.md).

### <a name="get-started-with-xbox-live"></a>Introdução ao Xbox Live

Os guias a seguir podem ajudá-lo a começar a usar o desenvolvimento do Xbox Live, não importa se você é um desenvolvedor para console Xbox Live ou UWP.  Também há guias para obter a configuração de mecanismos de jogos.

| Tópico                                                                                                                                             | Descrição                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Visão Geral do Programa de Desenvolvedor](developer-program-overview.md) | Discute vários programas de desenvolvedor que permitem o desenvolvimento para Xbox Live. |
| [Introdução ao Programa de Criadores do Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) | Como começar com o Xbox Live no Programa de Criadores do Xbox Live. |
| [Introdução ao Xbox Live como um ID@Xbox ou desenvolvedor gerenciado](get-started-with-partner/get-started-with-xbox-live-partner.md) | Como começar com o Xbox Live como um desenvolvedor no Programa ID@Xbox. |

### <a name="using-xbox-live"></a>Usando o Xbox Live

Depois de um título ter sido criado e os fundamentos estarem funcionando, esta seção apresenta o contexto necessário antes de você começar a codificar.

| Tópico                                                                                                                                             | Descrição                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Usando o Xbox Live](using-xbox-live/using-xbox-live.md) | Depois de ter configurado seu título e integrado o SDK do Xbox Live, você está pronto para implementar a entrada e saber mais sobre a programação do Xbox Live.
| [Melhores práticas para chamar o Xbox Live](using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) | Familiarize-se com as noções básicas e as melhores práticas de padrões de chamada do Xbox Live para garantir que seu título funcione bem e não sofra limitação de taxa.
| [Solucionar problemas da API de Serviços do Xbox Live](using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md) | Problemas comuns que você pode encontrar e sugestões de como corrigi-los.

### <a name="xbox-live-social-platform"></a>Plataforma Social do Xbox Live

Os recursos de redes sociais do Xbox Live podem aumentar seu público, distribuindo reconhecimento para os mais de 55 milhões de usuários ativos.  Esta seção descreve como começar a usar os recursos sociais do Xbox Live.

| Tópico                                                                                                                                             | Descrição                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma Social do Xbox Live](social-platform/social-platform.md) | Se você pode conectar um usuário, comece a usar os recursos sociais do Xbox Live, como uso de grafo social de um usuário, presença avançada e outros. |

### <a name="xbox-live-data-platform"></a>Plataforma de Dados do Xbox Live

A Plataforma de Dados do Xbox Live impulsiona o uso de estatísticas do jogador, conquistas e placares de líderes.  Leia esta série de tópicos para saber mais sobre como usar esses itens em seu título.

| Tópico                                                                                                                                             | Descrição                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma de Dados do Xbox Live](data-platform/data-platform.md) | Uma breve visão geral da Plataforma de Dados, bem como diretrizes sobre como melhor incorporar estatísticas, placares de líderes e conquistas a seu título.
| [Estatísticas do jogador](leaderboards-and-stats-2017/player-stats.md) | Estatísticas são a base do placar de líderes.  Saiba aqui como definir e usar estatísticas.
| [Placares](leaderboards-and-stats-2017/leaderboards.md) | Exponha os lados competitivos de seus usuários incorporando placares de líderes de maneira inteligente.
| [Conquistas](achievements-2017/achievements.md) | Realizações são um dos recursos mais conhecidos no Xbox Live e uma parte importante da interação do jogador. Saiba como usá-las em seu título.

### <a name="xbox-live-multiplayer-platform"></a>Plataforma para Vários Jogadores do Xbox Live

Ter vários jogadores é uma ótima maneira de estender o tempo de vida de seu título e manter experiências de jogo atualizadas.  O Xbox Live oferece amplos recursos para vários jogadores e organização de partida.  Você também tem várias opções de API que fornecem níveis variados de simplicidade vs. flexibilidade.

| Tópico                                                                                                                                             | Descrição                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma para Vários Jogadores do Xbox Live](multiplayer/multiplayer-intro.md) | Se você não tem experiência no desenvolvimento para vários jogadores do Xbox Live ou não está familiarizado com as novas APIs, como o Gerenciador de Vários Jogadores e o XIM (Xbox Integrated Multiplayer), comece aqui. |
| [Cenários com vários jogadores](multiplayer/multiplayer-scenarios.md) | Sugestões e orientação sobre como você pode incorporar vários jogadores a seu título. |
| [Xbox Integrated Multiplayer](multiplayer/xbox-integrated-multiplayer.md) | O XIM (Xbox Integrated Multiplayer) é uma interface independente fácil para adicionar vários jogadores, rede em tempo real e bate-papo a seu título. |
| [Gerenciador de Vários Jogadores](multiplayer/multiplayer-manager.md) | O Multiplayer Manager fornece uma API concentrada em cenários comuns de vários jogadores. |

### <a name="xbox-live-storage-platform"></a>Plataforma de Armazenamento do Xbox Live

A Plataforma de Armazenamento do Xbox Live fornece Armazenamento de Títulos e Armazenamento Conectado.  Esses são dois serviços diferentes, mas complementares.  O Armazenamento Conectado permite que você implemente jogo salvos na nuvem, que serão alternados entre dispositivos, independentemente do local em que um usuário está conectado.  Armazenamento de títulos permite armazenar blobs de dados que podem ser por usuário ou por título e compartilhados entre usuários diferentes.

| Tópico                                                                                                                                             | Descrição                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma de Armazenamento do Xbox Live](storage-platform/storage-platform.md) | Use os serviços de armazenamento do Xbox Live para armazenar os salvamentos de jogos, repetições instantâneas, preferências do usuário e outros dados na nuvem. |
| [Armazenamento Conectado](storage-platform/connected-storage/connected-storage-technical-overview.md) | Uma visão geral e guia de programação sobre Armazenamento Conectado. |
| [Armazenamento de Títulos](storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) | Uma visão geral e guia de programação sobre Armazenamento de Títulos. |
