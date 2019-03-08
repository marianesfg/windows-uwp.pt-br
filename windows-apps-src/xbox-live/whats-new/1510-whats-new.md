---
title: O que há de novo para o Xbox Live SDK – outubro de 2015
description: O que há de novo para o Xbox Live SDK – outubro de 2015
ms.assetid: 052be4aa-5f18-4eb7-ba5f-80c5f5cab6f2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0c1c2749ac88582e89f3aa3bbfc215741b34a55a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627851"
---
# <a name="whats-new-for-the-xbox-live-sdk---october-2015"></a>O que há de novo para o Xbox Live SDK – outubro de 2015

Consulte a [What's New - setembro de 2015](1509-whats-new.md) artigo para o que foi adicionado na versão de setembro de 2015.


## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte
O Xbox Live SDK dá suporte ao Windows 10 RTM [versão 10.0.10240] e o Visual Studio 2015 RTM [versão 14.0.23107.0].

## <a name="social-manager"></a>Gerenciador de social
* O Gerenciador de Social destina-se para facilitar o uso das APIs sociais do Xbox Live.  Ele será inteligentemente grafo social de um usuário de cache, fornecer uma API mais simples e muito mais.  Consulte a documentação para obter mais informações.

## <a name="samples"></a>Exemplos
* Temos um novo exemplo de Gerenciador de Social que demonstra a API.

## <a name="tools"></a>Ferramentas
* O analisador de rastreamento do Xbox Live agora está incluído no SDK do Xbox Live.  Coletar rastreamentos, conforme descrito na [analisar chamadas para serviços do Xbox Live](../tools/analyze-service-calls.md), e, em seguida, executar o analisador de rastreamento ao vivo para garantir que seu título é usando o Xbox Live de maneira ideal, exibindo as estatísticas sobre chamadas repetidas, a frequência de chamada e muito mais.

## <a name="bug-fixes"></a>Correções de bugs
* Alterado tempo limite padrão para operações de soquete HTTP para 30 segundos de 5 minutos.  Para operações longas de soquete HTTP, como o armazenamento de título fazer upload e download de chamadas, eles permaneçam usando um tempo limite de 5 minutos.

## <a name="documentation"></a>Documentação
* Introdução ao Gerenciador do Social adicionado
* Introdução ao Gerenciador do Multiplayer atualizado
