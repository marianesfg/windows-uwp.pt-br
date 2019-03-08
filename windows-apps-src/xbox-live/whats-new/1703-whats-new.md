---
title: O que há de novo para o Xbox Live SDK – março de 2017
description: O que há de novo para o Xbox Live SDK – março de 2017
ms.assetid: 03180585-6f87-4929-acfc-750bd78988a0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: be8127e01d8eaae96a1d71f71967a653c00b0280
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595341"
---
# <a name="whats-new-for-the-xbox-live-sdk---march-2017"></a>O que há de novo para o Xbox Live SDK – março de 2017

Consulte a [What's New - dezembro de 2016](1612-whats-new.md) artigo para o que foi adicionado na versão de dezembro de 2016.

## <a name="xbox-services-api"></a>API de serviços do Xbox

### <a name="data-platform-2017"></a>Plataforma de dados de 2017

Introduzimos uma API simplificada de estatísticas.  Tradicionalmente tinha que enviar eventos correspondentes às regras stat definidas em XDP ou Partner Center e elas seriam atualize os valores stat na nuvem.  Nós nos referimos a esse modelo como estatísticas de 2013.

Com estatísticas de 2017, o título agora está no controle dos seus valores de stat.  Você simplesmente chamar uma API com o valor de stat mais recente, e que é enviado para o serviço diretamente sem a necessidade de eventos.  Isso usa o novo `StatsManager` API e você pode ler mais em [estatísticas de Player](../leaderboards-and-stats-2017/player-stats.md)

### <a name="github"></a>GitHub

Xbox Live API (XSAPI) agora está disponível no GitHub em [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Usando os binários que vêm com o XDK ou como NuGet pacotes ainda é recomendável, porém, você está bem-vindo ao usar a fonte e aceitamos contribuições de código de origem.  

## <a name="xbox-live-creators-program"></a>Programa de Criadores do Xbox Live

O programa de criadores do Xbox Live é um programa de desenvolvedor, oferecendo um subconjunto da funcionalidade do Xbox Live para um público mais amplo do desenvolvedor.  Se você já estiver no ID@Xbox programa, isso não terá qualquer impacto sobre você.

Você pode ler mais sobre o programa na [visão geral do programa de desenvolvedor](../developer-program-overview.md).

## <a name="documentation"></a>Documentação

Existem os seguintes novos artigos

| Artigo | Descrição |
|---------|-------------|
|[Configuração de serviço ao vivo do Xbox](../xbox-live-service-configuration.md) | Informações atualizadas sobre a configuração de serviço para o título do Xbox Live
| [Configurar o Xbox Live no Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Novas informações sobre a configuração do Unity para desenvolvedores do programa de criadores do Xbox Live |
| [Áreas restritas do Xbox Live](../xbox-live-sandboxes.md) | Um guia simplificado para áreas restritas do Xbox Live e o isolamento de conteúdo |
| [Contas de teste ao vivo do Xbox](../xbox-live-test-accounts.md) | Informações sobre como testar o trabalho de contas e como criá-los no Partner Center |
