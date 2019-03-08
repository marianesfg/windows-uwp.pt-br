---
title: O que há de novo para as APIs do Xbox Live – abril de 2017
description: O que há de novo para as APIs do Xbox Live – abril de 2017
ms.assetid: ''
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arena, torneios
ms.localizationpriority: medium
ms.openlocfilehash: a9256bfce05c14a0bfa63c7ec656dd899648d844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651301"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>O que há de novo para as APIs do Xbox Live – abril de 2017

Consulte a [What's New - março de 2017](1703-whats-new.md) artigo para o que foi adicionado na versão de março de 2017.

## <a name="xbox-services-apis"></a>APIs de serviços do Xbox

### <a name="visual-studio-2017"></a>Visual Studio 2017

As APIs do Xbox Live foram atualizadas para Visual Studio 2017, o suporte para títulos de plataforma Universal do Windows (UWP) e Xbox One.

### <a name="tournaments"></a>Torneios

Novas APIs foram adicionadas para dar suporte a torneios. Agora você pode usar o `xbox::services::tournaments::tournament_service` classe para acessar o serviço de torneios em seu título.

Essas APIs do torneio novo habilitar os seguintes cenários:

* Consulte o serviço para localizar todos os torneios existentes para o título atual.
* Recupere detalhes sobre um torneio de serviço.
* O serviço para recuperar uma lista de equipes para um torneio de consulta.
* Recupere detalhes sobre as equipes de um torneio de serviço.
* Controlar as alterações a torneios e equipes usando as assinaturas de atividade em tempo Real (RTA).
