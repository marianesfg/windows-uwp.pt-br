---
title: O que há de novo para o Xbox Live SDK – agosto de 2015
description: O que há de novo para o Xbox Live SDK – agosto de 2015
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a454b7339035475416934c2f921dae65283c340c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639811"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>O que há de novo para o Xbox Live SDK – agosto de 2015

Consulte a [What's New - junho de 2015](1506-whats-new.md) artigo para o que foi adicionado na versão de junho de 2015.

A versão de agosto do Xbox Live SDK inclui as seguintes atualizações:

## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte
O Xbox Live SDK agora dá suporte ao Windows 10 RTM [versão 10.0.10240] e o Visual Studio 2015 RTM [versão 14.0.23107.0].

## <a name="multiplayer-manager-winrt-apis"></a>APIs do WinRT Manager com vários participantes
Gerenciador de com vários participantes (no namespace experimental) agora dá suporte a APIs do WinRT (além das APIs de C++)

## <a name="submit-batch-feedback-from-a-title"></a>Enviar comentários sobre o lote de um título
Envia um número de itens de comentários ao mesmo tempo a partir de um título.

## <a name="newupdated-documentation"></a>Documentação do novo/atualizado
O pacote do Xbox Live SDK agora inclui uma pasta "Docs", contém referências de API atualizadas e o novo "Xbox Live guia de programação de".

Correções de bug:

* Falha ao remover as assinaturas no serviço de atividade em tempo Real
* Falha ao fazer logon com uma conta de convidado
* Violação de acesso ao desconectar o cabo de rede
* Falhas de túnel agora fornecem um código de erro nas APIs do C++
* Problema de ETag com TitleStorageService::DownloadBlobAsync
* Várias correções de bugs para aplicativos de exemplo.
