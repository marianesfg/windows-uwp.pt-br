---
title: Códigos de status da sessão multijogador
description: Descreve os códigos de status retornados do serviço Xbox Live ao solicitar uma sessão com vários participantes.
ms.assetid: 4ab320d6-8050-41a9-9f00-faaad3b128fd
ms.date: 04/04/2017
ms.topic: article
keywords: o Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes de 2015, os códigos de status, a sessão
ms.localizationpriority: medium
ms.openlocfilehash: 8fbddd0070eb24d6fc050c59fa2a0197f98ee08c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646301"
---
# <a name="multiplayer-session-status-codes"></a>Códigos de status da sessão multijogador

Este tópico lista os códigos de status para múltiplos jogadores relacionadas a sessões.

| Observação                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------|
| Os códigos de status 4xx retornando a sessão sempre retornam toda a sessão, mesmo que o URI aponta para um elemento de sessão. |


| Código de Status | String              | Content-Type     | Corpo    | Descrição |
|----|
| 200         | OK                  | aplicativo/json | sessão | Leitura (GET) ou atualização (PUT) com êxito.                                                                                                                                                                                                                                                                                                             |
| 201         | Criado             | aplicativo/json | sessão | Criado com êxito.                                                                                                                                                                                                                                                                                                                                 |
| 202         | Aceito            | texto/simples       | nenhuma    | A solicitação foi aceita, mas ainda não foi concluída.                                                                                                                                                                                                                                                                                             |
| 204         | Sem conteúdo          |                  |         | Em GET para uma sessão, sessão não existe. No GET de um elemento de sessão, a sessão existe, mas não o elemento. Em PUT para uma sessão, a sessão foi excluída como resultado da operação PUT. Em PUT ou DELETE para um elemento de sessão, a sessão existia quando o início da operação, mas a sessão ou o elemento não existe mais. |
| 304         | Não modificado        |                  |         | Em GET com cabeçalho If-None-Match, a sessão não foi alterado.                                                                                                                                                                                                                                                                                        |
| 400         | Solicitação incorreta         | texto/simples       | mensagem | A solicitação é considerada inválida no primeiro exame. Ele é um campo obrigatório ausente ou o arquivo JSON está malformado. O corpo inclui detalhes adicionais.                                                                                                                                                                                        |
| 403         | Proibido           | texto/simples       | mensagem | A solicitação pode ser válida em alguns contextos, mas é inválida para o seu contexto. Falha na autorização.                                                                                                                                                                                                                                                |
|             |                     | aplicativo/json | sessão | A sessão não pode ser atualizada pelo usuário, mas pode ser lido.                                                                                                                                                                                                                                                                                           |
| 404         | Não foi encontrado           | texto/simples       | mensagem | A sessão não pode ser acessada porque o URI é inválido; o identificador, SCID ou modelo de sessão não pode ser encontrado; não é possível encontrar um hopper; um elemento de sessão não pode ser acessado porque a sessão não é encerrado; ou a pesquisa de elemento é inválida para a sessão.                                                                                 |
| 405         | Método não permitido  | texto/simples       | mensagem | O URI da solicitação é plausível, mas o verbo é errado. Por exemplo, a solicitação é para uma operação POST quando uma operação PUT é necessária.                                                                                                                                                                                                                 |
| 409         | Conflito            | texto/simples       | mensagem | Não foi possível atualizar a sessão porque a solicitação é incompatível com a sessão. Por exemplo, constantes na solicitação entram em conflito com constantes na sessão ou modelo de sessão ou membros que não seja o chamador tem foram adicionados ou removidos de uma sessão de grande.                                                                         |
| 412         | Falha na pré-condição |                  |         | O cabeçalho If-Match ou o cabeçalho If-None-Match (para uma operação diferente de GET), não pode ser atendida.                                                                                                                                                                                                                                           |
|             |                     | aplicativo/json | sessão | O cabeçalho If-Match não pôde ser atendido em uma operação PUT ou DELETE para uma sessão existente. O estado atual da sessão é retornado juntamente com o valor de ETag atual.                                                                                                                                                                      |
| 429 | Número excessivo de solicitações | aplicativo/json | mensagem | A chamada de serviço foi limitada por exceder a restrições de limitação de taxa refinada. Para obter mais informações, consulte [bem mais refinado a limitação de taxa](../../using-xbox-live/best-practices/fine-grained-rate-limiting.md). |
| 503         | Serviço indisponível | texto/simples       | nenhuma    | O serviço está sobrecarregado e a solicitação deve ser tentada novamente mais tarde. Esse código inclui um cabeçalho Retry-After que o cliente deve respeitar.                                                                                                                                                                                                              |
