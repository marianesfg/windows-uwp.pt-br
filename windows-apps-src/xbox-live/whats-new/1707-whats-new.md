---
title: O que há de novo em APIs do Xbox Live – julho de 2017
description: O que há de novo em APIs do Xbox Live – julho de 2017
ms.assetid: ''
ms.date: 07/16/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, o que há de novo, julho de 2017
ms.localizationpriority: medium
ms.openlocfilehash: 47db721a98b71fd6f4b5175a88ddccd00048d8d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618821"
---
# <a name="whats-new-for-the-xbox-live-apis---july-2017"></a>O que há de novo para as APIs do Xbox Live – julho de 2017

Consulte a [What's New - junho de 2017](1706-whats-new.md) artigo para o que foi adicionado na versão de junho de 2017.

Você também pode verificar a [histórico de confirmações do GitHub do Xbox Live API](https://github.com/Microsoft/xbox-live-api/commits/master) para ver todas as recentes alterações de código para as APIs do Xbox Live.

## <a name="xbox-live-features"></a>Recursos do Xbox Live

### <a name="multiplayer-updates"></a>Atualizações para múltiplos jogadores

Consultando os identificadores de atividade e identificadores de pesquisa agora inclui as propriedades de sessão personalizado na resposta.

### <a name="tournaments"></a>Torneios

Novas APIs foram adicionadas para dar suporte a torneios. Agora você pode usar a classe xbox::services::tournaments::tournament_service para acessar o serviço de torneios em seu título.
Essas APIs do torneio novo habilitar os seguintes cenários:
* Consulte o serviço para localizar todos os torneios existentes para o título atual.
* Recupere detalhes sobre um torneio de serviço.
* O serviço para recuperar uma lista de equipes para um torneio de consulta.
* Recupere detalhes sobre as equipes de um torneio de serviço.
* Controlar as alterações a torneios e equipes usando as assinaturas de atividade em tempo Real (RTA).
