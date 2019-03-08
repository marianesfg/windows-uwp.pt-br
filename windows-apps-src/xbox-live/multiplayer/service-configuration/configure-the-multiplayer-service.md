---
title: Configuração de serviço para vários jogadores
description: Saiba como configurar o serviço Xbox Live com vários participantes.
ms.assetid: d042d4d5-1c75-4257-8a6f-07eddd39ca7e
ms.date: 07/12/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes, configuração de serviço, o modelo de sessão, cadeia de caracteres de convite personalizado, smartmatch hopper
ms.localizationpriority: medium
ms.openlocfilehash: bf829069824443cdc1c8c0658fcfdfcbe72d0b93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655871"
---
# <a name="multiplayer-service-configuration"></a>Configuração de serviço para vários jogadores
Na ordem do título tirar proveito dos serviços do Xbox Live fornece, você deve primeiro definir sua configuração de serviço. Essa configuração de serviço existe no serviço de nuvem do Xbox Live e define como o serviço Xbox Live interage com todos os dispositivos que estão executando o título e de jogos.

Para serviços para múltiplos jogadores, há três aspectos com vários participantes que você pode configurar:
* Modelos de sessão
* SmartMatch Hoppers
* Cadeias de caracteres de convite personalizado

## <a name="session-templates"></a>Modelos de sessão
O serviço com vários participantes do Xbox permite que os jogadores criar e ingressar em sessões, para trocar mensagens de sessão com outros jogadores na mesma sessão e postar os resultados do seu jogo para a sessão. (Os resultados de lançamento limpa a sessão e também atualiza os placares de líderes de todos os jogadores na sessão.)

Por exemplo, uma sessão para múltiplos jogadores poderia ser um jogo simples de xadrez entre dois jogadores. Como alternativa, ele poderia ser uma sessão de continuidade de uma ação e adventure desempenhado por um número muito maior de jogadores de título.

Quando um jogo cria uma nova sessão, ele cria a sessão com base em um modelo predefinido de sessão. Este modelo é basicamente um objeto JSON que contém atributos que descrevem a sessão.

Quando você cria um modelo de nova sessão, você deve definir o seguinte:

| Campo | Descrição |
| --- | --- |
| Nome da sessão | Insira um nome que caracteriza o modelo de sessão com vários participantes e que você facilmente Lembre-se e reconhecer. O nome deve ser uma cadeia de caracteres de texto, com um máximo de 100 caracteres. |
| Versão do contrato | Esse valor é preenchido automaticamente pelo sistema e denota a versão atual do sistema do contrato de JSON. Não editá-lo. |
| Modelo de sessão (texto JSON) | Especifique os dados JSON que descreve os diferentes atributos associados a uma sessão para múltiplos jogadores. |

Para obter mais informações sobre modelos de sessão para múltiplos jogadores, incluindo vários modelos predefinidos que você pode usar como base para o texto JSON, consulte [modelos de sessão para múltiplos jogadores](session-templates.md).

> **Importante:** Depois que um título aprovado na certificação Final, sessões para múltiplos jogadores existentes nesse título podem não ser alteradas ou excluídas.

## <a name="smartmatch-hoppers"></a>SmartMatch hoppers

Uma adição opcional para o serviço com vários participantes do Xbox é o serviço de cruzamento de baseada em servidor Xbox, que fornece um método de jogadores de agrupamento juntos com base nas informações fornecidas pelo título armazenado nas estatísticas de usuário ou com base nas preferências do usuário, ou com base na qualidade de serviço.

Como o Xbox One para que haja correspondência é baseada em servidor, os usuários podem fornecer uma solicitação para o serviço e, em seguida, ser notificados posteriormente, sempre que uma correspondência for encontrada. Ou seja: o usuário não será forçado a aguardar em seu título enquanto ocorre o processo de emparelhamento — são livres para executar a parte de jogador do seu título, ou até mesmo para reproduzir outros títulos e ainda ser candidatos para o emparelhamento. Isso elimina a necessidade de alcançar uma "massa crítica" de jogadores antes de que podem ser encontradas correspondências.

Hopper um emparelhamento deve ser baseado em um modelo de sessão definido anteriormente.

Quando você cria um novo hopper de emparelhamento, você deve definir o seguinte:

| Campo | Descrição |
|---|---|
|Nome| Insira um nome que caracteriza o hopper de relacionamento de pessoas e que você facilmente Lembre-se e reconhecer. O nome deve ser uma cadeia de caracteres de texto, com um máximo de 140 caracteres. |
| Tamanho do grupo de min | Especifique o número mínimo aceitável de jogadores. Valor mínimo é 1. |
| Tamanho máximo de grupo | Especifique o número máximo aceitável de jogadores. Valor máximo é 256. |
| Deve ciclos de expansão de regra | O valor padrão é 3. O valor padrão não deve precisar ser alterado para populações player normal. |
| Hopper com classificação | Se um hopper estiver marcado como um Hopper classificados Isso permite que os jogadores no que hopper a serem correspondidos juntos, mesmo se eles estão bloqueados uns aos outros. Isso ajuda a impedir que pessoas tentando evitar players com maior habilidade, bloqueando-os. |
| Atualização automática da sessão | Quando esse campo estiver habilitado, as alterações feitas à lista de membros da sessão ou propriedades personalizadas de membros serão propagadas automaticamente para um tíquete enviado anteriormente. |

> **Importante:** Depois que um título aprovado na certificação Final, hoppers de emparelhamento existente nesse título podem não ser alteradas ou excluídas.

## <a name="custom-invite-strings"></a>Cadeias de caracteres de convite personalizado
Quando seu título envia um convite para um player para ingressar em um jogo com vários participantes, você pode optar por exibir uma cadeia de caracteres de texto de convite personalizado em vez da cadeia de convite padrão.

Quando você cria uma nova cadeia de caracteres de convite personalizado, você deve definir o seguinte:

| Campo | Descrição |
|---|---|
| ID | A ID do personalizado convidar cadeia de caracteres que será usada para identificar a cadeia de caracteres. "custominvitestrings_" serão acrescentados automaticamente para o início da sua ID. Máximo de 100 caracteres |
| Valor | O texto da cadeia de caracteres de convite personalizado que serão exibido em sua notificação do sistema de convite personalizado. Máximo de 100 caracteres |

## <a name="additional-information"></a>Informações adicionais

Para obter mais informações sobre como configurar o serviço com vários participantes, consulte os artigos a seguir:

**Artigo** | **Descrição**
--- | ---
[Configurar seu AppXManifest para vários jogadores](configure-your-appxmanifest-for-multiplayer.md) | Descreve como configurar um arquivo AppXManifest UWP para trabalhar com o serviço com vários participantes do Xbox Live.
[Modelos de sessão para múltiplos jogadores](session-templates.md) | Fornece uma visão geral dos modelos de sessão para múltiplos jogadores e fornece vários exemplos de modelos que você pode copiar e modificar de suas sessões com vários participantes.
[Constantes do modelo de sessão](session-template-constants.md) | Descreve os elementos predefinidos de um modelo de sessão para múltiplos jogadores.
[Sessões grandes](large-sessions.md) | Descreve quando e como usar sessões grandes.
