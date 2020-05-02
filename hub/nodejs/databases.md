---
title: Introdução à conexão de aplicativos Node.js a um banco de dados
description: Introdução à conexão de aplicativos Node.js a um banco de dados no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, aprendizado no nodejs, nó no windows, nó no wsl, nó no linux no windows, instalar nó no windows, nodejs com vs code, desenvolver com nó no windows, desenvolver com nodejs no windows, instalar nó no WSL, NodeJS no Subsistema do Windows para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517822"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Introdução ao uso de MongoDB ou PostgreSQL com Node.js no Windows

Os aplicativos Node.js geralmente precisam persistir dados, o que pode ocorrer por meio de arquivos, do armazenamento local, de serviços de nuvem ou de bancos de dados. Este guia passo a passo ajudará você a conectar seu aplicativo Node.js a um banco de dados. Optamos por se concentrar em duas opções populares: MongoDB e PostgreSQL.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [configurar o ambiente de desenvolvimento do Node.js com o WSL 2](./setup-on-wsl2.md), incluindo:

- Instalar o build 18932 ou posterior do Windows 10 Insider Preview.
- Habilitar o recurso WSL 2 no Windows.
- Instalar uma distribuição do Linux (Ubuntu 18.04 em nossos exemplos). Você pode verificar isso com: `wsl lsb_release -a`
- Garantir que a distribuição do Ubuntu 18.04 esteja em execução no modo WSL 2. (O WSL pode executar distribuições no modo v1 ou v2.) Para verificar isso, abra o PowerShell e digite: `wsl -l -v`
- Usando o PowerShell, definir o Ubuntu 18.04 como a distribuição padrão, com: `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>Diferenças entre o MongoDB e o PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Instalar o MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Suporte do VS Code para MongoDB

O VS Code dá suporte ao trabalho com bancos de dados MongoDB por meio da [extensão do Azure Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), você pode criar bancos de dados MongoDB no VS Code, gerenciá-los e consultá-los.

Para saber mais, acesse os documentos do VS Code: [Como trabalhar com o MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Saiba mais nos documentos do MongoDB:

- [Introdução ao MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Criar usuários](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Conectar-se a uma instância do MongoDB em um host remoto](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: Criar, Ler, Atualizar e Excluir](https://docs.mongodb.com/manual/crud/)
- [Documentos de referência](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Instalar o PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Suporte do VS Code para PostgreSQL

O VS Code dá suporte ao trabalho com bancos de dados PostgreSQL por meio da [extensão do PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), você pode criar bancos de dados PostgreSQL no VS Code, conectar-se a eles, gerenciá-los e consultá-los.

## <a name="set-up-profile-aliases"></a>Configurar aliases de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
