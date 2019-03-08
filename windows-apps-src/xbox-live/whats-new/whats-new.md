---
title: O que há de novo para o Xbox Live
description: O que há de novo para o Xbox Live SDK
ms.date: 10/23/2018
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33e5b6afbf0d60679bfce1789be2d965fd881f1c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614111"
---
# <a name="whats-new-for-xbox-live"></a>Novidades do Xbox Live
Você também pode verificar a [histórico de confirmações do GitHub do Xbox Live API](https://github.com/Microsoft/xbox-live-api/commits/master) para ver todas as recentes alterações de código para as APIs do Xbox Live.

## <a name="in-this-article"></a>Neste artigo

* [Junho 2018](#june-2018)
* [Agosto 2017](#august-2017)
* [Julho 2017](#july-2017)
* [Junho 2017](#june-2017)
* [Maio 2017](#may-2017)
* [Abril 2017](#april-2017)
* [Março 2017](#march-2017)
* [Arquivado](#archived)

## <a name="june-2018"></a>Junho de 2018

### <a name="xbox-live-features"></a>Recursos do Xbox Live

#### <a name="c-api-layer-for-xsapi"></a>Camada de API de C para XSAPI

APIs de C agora estão disponíveis para alguns recursos do Xbox Live. A nova camada de API fornece uma série de benefícios para os recursos com suporte, incluindo gerenciamento de memória personalizado, gerenciamento de threads manual para tarefas assíncronas e uma nova biblioteca HTTP.

Para obter mais informações, consulte [APIs do Xbox Live C](../xsapi-flat-c.md).

## <a name="august-2017"></a>Agosto de 2017

### <a name="xbox-live-features"></a>Recursos do Xbox Live

#### <a name="in-game-clubs"></a>No jogo paus

Os desenvolvedores agora podem criar "paus do jogo". No jogo paus diferem de paus padrão do Xbox em que eles são totalmente personalizáveis por um desenvolvedor e podem ser usados dentro e fora do jogo. Como desenvolvedor de jogos, você pode usá-los para compilar rapidamente qualquer tipo de cenários de grupo persistente dentro de seus jogos, como as equipes, clans, squads, guilds, etc. que correspondam aos seus requisitos exclusivos.

Xbox membros dinâmicos podem acessar no jogo paus fora do seu jogo entre qualquer experiência do Xbox para permanecer conectados uns aos outros e ao jogo usando recursos do clube como bate-papo, feed, LFG e Mixer livremente em dispositivos iOS/Android, PC ou console Xbox.

As APIs estão disponíveis para criar e gerenciar clubes de jogos diretamente de dentro do seu jogo. Essas APIs existem no namespace xbox::services::clubs.

## <a name="july-2017"></a>Julho de 2017

### <a name="xbox-live-features"></a>Recursos do Xbox Live

#### <a name="tournaments"></a>Torneios

Novas APIs foram adicionadas para dar suporte a torneios. Agora você pode usar a classe xbox::services::tournaments::tournament_service para acessar o serviço de torneios em seu título.
Essas APIs do torneio novo habilitar os seguintes cenários:

* Consulte o serviço para localizar todos os torneios existentes para o título atual.
* Recupere detalhes sobre um torneio de serviço.
* O serviço para recuperar uma lista de equipes para um torneio de consulta.
* Recupere detalhes sobre as equipes de um torneio de serviço.
* Controlar as alterações a torneios e equipes usando as assinaturas de atividade em tempo Real (RTA).

## <a name="june-2017"></a>Junho de 2017

### <a name="xbox-live-features"></a>Recursos do Xbox Live

#### <a name="game-chat-2"></a>Chat de Jogo 2

Uma versão atualizada e aprimorada de jogo de bate-papo agora está disponível. Para obter mais informações, consulte o [visão geral de 2 de bate-papo do jogo](../multiplayer/chat/game-chat-2-overview.md).

### <a name="xbox-live-tools"></a>Ferramentas do Xbox Live

#### <a name="xbox-live-powershell-module"></a>Módulo do PowerShell do Xbox Live

* Módulos do PowerShell foram adicionados para tornar mais fácil alternar áreas restritas em seu computador de desenvolvimento. Para obter mais informações, consulte [ferramentas](../tools/tools.md)

#### <a name="bug-fixes"></a>Correções de bugs

* Várias correções de bugs. Verifique as [histórico de confirmações do GitHub](https://github.com/Microsoft/xbox-live-api/commits/master) para obter uma lista completa.

## <a name="may-2017"></a>Maio de 2017

### <a name="xbox-services-apis"></a>APIs de serviços do Xbox

#### <a name="multiplayer"></a>Multijogadores

* Consultar os identificadores de pesquisa agora inclui as propriedades de sessão personalizado na resposta.

#### <a name="bug-fixes"></a>Correções de bugs

* Corrigido "ruim" json"que está sendo retornado em vez de um código de erro HTTP válido.

## <a name="april-2017"></a>Abril de 2017

### <a name="xbox-services-apis"></a>APIs de serviços do Xbox

#### <a name="visual-studio-2017"></a>Visual Studio 2017

As APIs do Xbox Live foram atualizadas para Visual Studio 2017, o suporte para títulos de plataforma Universal do Windows (UWP) e Xbox One.

#### <a name="tournaments"></a>Torneios

Novas APIs foram adicionadas para dar suporte a torneios. Agora você pode usar o `xbox::services::tournaments::tournament_service` classe para acessar o serviço de torneios em seu título.

Essas APIs do torneio novo habilitar os seguintes cenários:

* Consulte o serviço para localizar todos os torneios existentes para o título atual.
* Recupere detalhes sobre um torneio de serviço.
* O serviço para recuperar uma lista de equipes para um torneio de consulta.
* Recupere detalhes sobre as equipes de um torneio de serviço.
* Controlar as alterações a torneios e equipes usando as assinaturas de atividade em tempo Real (RTA).

## <a name="march-2017"></a>Março de 2017

### <a name="xbox-services-api"></a>API de serviços do Xbox

#### <a name="data-platform-2017"></a>Plataforma de dados de 2017

Introduzimos uma API simplificada de estatísticas.  Tradicionalmente tinha que enviar eventos correspondentes às regras stat definidas em XDP ou Partner Center e elas seriam atualize os valores stat na nuvem.  Nós nos referimos a esse modelo como estatísticas de 2013.

Com estatísticas de 2017, o título agora está no controle dos seus valores de stat.  Você simplesmente chamar uma API com o valor de stat mais recente, e que é enviado para o serviço diretamente sem a necessidade de eventos.  Isso usa o novo `StatsManager` API e você pode ler mais em [estatísticas de Player](../leaderboards-and-stats-2017/player-stats.md)

#### <a name="github"></a>GitHub

Xbox Live API (XSAPI) agora está disponível no GitHub em [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Usando os binários que vêm com o XDK ou como NuGet pacotes ainda é recomendável, porém, você está bem-vindo ao usar a fonte e aceitamos contribuições de código de origem.  

### <a name="xbox-live-creators-program"></a>Programa de Criadores do Xbox Live

O programa de criadores do Xbox Live é um programa de desenvolvedor, oferecendo um subconjunto da funcionalidade do Xbox Live para um público mais amplo do desenvolvedor.  Se você já estiver no ID@Xbox programa, isso não terá qualquer impacto sobre você.

Você pode ler mais sobre o programa na [visão geral do programa de desenvolvedor](../developer-program-overview.md).

### <a name="documentation"></a>Documentação

Há novos artigos a seguir:

| Artigo | Descrição |
|---------|-------------|
|[Configuração de serviço ao vivo do Xbox](../xbox-live-service-configuration.md) | Informações atualizadas sobre a configuração de serviço para o título do Xbox Live
| [Configurar o Xbox Live no Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Novas informações sobre a configuração do Unity para desenvolvedores do programa de criadores do Xbox Live |
| [Áreas restritas do Xbox Live](../xbox-live-sandboxes.md) | Um guia simplificado para áreas restritas do Xbox Live e o isolamento de conteúdo |
| [Contas de teste ao vivo do Xbox](../xbox-live-test-accounts.md) | Informações sobre como testar o trabalho de contas e como criá-los no Partner Center |

## <a name="archived"></a>Arquivado

* [Dezembro 2016](1612-whats-new.md)
* [Novembro de 2016](1611-whats-new.md)
* [Agosto de 2016](1608-whats-new.md)
* [Junho de 2016](1606-whats-new.md)
* [Abril de 2016](1603-whats-new.md)
* [Março de 2016](1603-whats-new.md)
* [Fevereiro de 2016](1602-whats-new.md)
* [Outubro de 2015](1510-whats-new.md)
* [Setembro de 2015](1509-whats-new.md)
* [Agosto de 2015](1508-whats-new.md)
* [Junho de 2015](1506-whats-new.md)
