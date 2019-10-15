---
title: Introdução à conexão de aplicativos node. js a um banco de dados
description: Introdução à conexão de aplicativos node. js a um banco de dados no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, nó no Windows, nó em WSL, nó no Linux no Windows, instalar nó no Windows, NodeJS com vs Code, desenvolver com nó no Windows, desenvolver com NodeJS no Windows, instalar nó em WSL, NodeJS no Windows Subsistema para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bdc3e3c944c4aeb25f5cf880fc4d31df1019da5a
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315110"
---
# <a name="get-started-connecting-nodejs-apps-to-a-database"></a>Introdução à conexão de aplicativos node. js a um banco de dados

Os aplicativos node. js geralmente precisam manter dados, o que pode ocorrer por meio de arquivos, armazenamento local, serviços de nuvem ou bancos de dados. Este guia passo a passo irá ajudá-lo a começar a conectar seu aplicativo node. js a um banco de dados. Optamos por se concentrar em duas opções populares: MongoDB e PostgreSQL.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [configurar seu ambiente de desenvolvimento do node. js com o WSL 2](./setup-on-wsl2.md), incluindo:

- Instale o Windows 10 Insider Preview versão 18932 ou posterior.
- Habilite o recurso WSL 2 no Windows.
- Instale uma distribuição do Linux (Ubuntu 18, 4 para nossos exemplos). Você pode verificar isso com: `wsl lsb_release -a`
- Verifique se a distribuição do Ubuntu 18, 4 está em execução no modo WSL 2. (WSL pode executar distribuições no modo v1 ou v2.) Você pode verificar isso abrindo o PowerShell e inserindo: `wsl -l -v`
- Usando o PowerShell, defina o Ubuntu 18, 4 como sua distribuição padrão, com: `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>Diferenças entre o MongoDB e o PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Instalar o MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Suporte de VS Code para MongoDB

VS Code dá suporte ao trabalho com bancos de dados do MongoDB por meio da [extensão CosmosDB do Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), você pode criar, gerenciar e consultar bancos de dados do MongoDB de dentro vs Code.

Para saber mais, visite o VS Code docs: [Trabalhando com o MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Saiba mais nos documentos do MongoDB:

- [Introdução ao uso do MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Criar usuários](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Conectar-se a uma instância do MongoDB em um host remoto](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: Criar, ler, atualizar, excluir @ no__t-0
- [Documentos de referência](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Instalar o PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Suporte VS Code para PostgreSQL

VS Code dá suporte ao trabalho com bancos de dados PostgreSQL por meio da [extensão PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), você pode criar, conectar-se a, gerenciar e consultar bancos de dados PostgreSQL de dentro vs Code.

## <a name="set-up-profile-aliases"></a>Configurar aliases de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
