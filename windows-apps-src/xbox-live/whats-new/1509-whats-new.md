---
title: O que há de novo para o Xbox Live SDK – setembro de 2015
description: O que há de novo para o Xbox Live SDK – setembro de 2015
ms.assetid: 84b82fde-f6f3-4dc2-b2df-c7c7313a2cc3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c618a738dc2670d3d3de1fa2f4c4108c24130eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602271"
---
# <a name="whats-new-for-the-xbox-live-sdk---september-2015"></a>O que há de novo para o Xbox Live SDK – setembro de 2015

Consulte a [What's New - agosto de 2015](1508-whats-new.md) artigo para o que foi adicionado na versão de agosto de 2015.

A versão de setembro do Xbox Live SDK inclui as seguintes atualizações:

## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte ##
O Xbox Live SDK dá suporte ao Windows 10 RTM [versão 10.0.10240] e o Visual Studio 2015 RTM [versão 14.0.23107.0].

## <a name="contextual-search-apis"></a>APIs de pesquisa contextual
* Habilite seu título ou o aplicativo a ser pesquisado difusões de seu game(s) com estatísticas em tempo real de sua escolha.
* Consulte as novas APIs no Microsoft::Xbox::Services::ContextualSearch

## <a name="app-insights-for-events"></a>App Insights para eventos

| Observação |
|------|
| App Insights se aplica somente aos títulos UWP.  Se você estiver desenvolvendo um título XDK, esta seção não se aplica a você |

<p/>

* Eventos gravados usando write_in_game_event() podem ser exibidos usando o AppInsights
* Documentação sobre isso estará disponível no futuro, enquanto isso, trabalhe com sua mãe para obter acesso

## <a name="logging"></a>Log
* service_call_logging_config in xbox::services::experimental
* Para iniciar e parar rastreamentos via xbTrace.exe no console do, você precisa chamar register_for_protocol_activation na classe service_call_logging_config.  Fazer essa chamada uma vez durante sua inicialização do jogo.

## <a name="resync-for-rta"></a>Ressincronização RTA
* A ressincronização pode ocorrer quando o serviço RTA acredita que as informações de que os usuários podem estar desatualizadas
* Títulos devem chamar as chamadas HTTP correspondentes para as assinaturas que estão inscritos para
* Títulos não é preciso assinar novamente
* Xbox::services::real_time_activity_service::add_resync_handler adicionado
* Xbox::services::real_time_activity_service::remove_resync_handler removido
* Http_status_429_too_many_requests adicionado
* Essa condição de erro será vista quando um título está sendo limitado para enviar um número excessivo de solicitações de http

## <a name="documentation"></a>Documentação
* Migrando para o Xbox Live Services API 2.0
* Tratamento de erro
* Autenticação de Xbox Live no Windows 10
* Pesquisa Contextual
