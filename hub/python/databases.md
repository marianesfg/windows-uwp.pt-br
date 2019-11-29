---
title: Introdução ao uso do Python no Windows com um banco de dados
description: Um guia para ajudar você a começar a usar o PostgreSQL ou o MongoDB com o Python no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, PostgreSQL, MongoDB, Postgres, Mongo, Microsoft, Python no Windows, instalar o PostgreSQL no Windows, instalar o MongoDB no Windows, usar o PostgreSQL com o Python, usar o MongoDB com o Python, PostgreSQL no WSL, MongoDB no WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517781"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Introdução ao uso do PostgreSQL ou do MongoDB com o Python no Windows

Este guia passo a passo ajudará você a conectar seu aplicativo do Python a um banco de dados. Optamos por se concentrar em duas opções populares: PostgreSQL e MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Diferenças entre o MongoDB e o PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> O ideal é considerar também o nível de integração da estrutura e das ferramentas que você está usando a um sistema de banco de dados específico. A [Estrutura da Web Django](./web-frameworks.md#hello-world-tutorial-for-django) parece estar mais bem integrada ao PostgreSQL (confira os [documentos do Django](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) e o [psycopg2](https://github.com/psycopg/psycopg2)). A [Estrutura da Web Flask](./web-frameworks.md#hello-world-tutorial-for-flask) parece estar mais bem integrada ao MongoDB (confira o [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) e o [PyMongo](https://github.com/dcrosta/flask-pymongo)).

## <a name="install-postgresql"></a>Instalar o PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Suporte do VS Code para PostgreSQL

O VS Code dá suporte ao trabalho com bancos de dados PostgreSQL por meio da [extensão do PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), você pode criar bancos de dados PostgreSQL no VS Code, conectar-se a eles, gerenciá-los e consultá-los.

## <a name="install-mongodb"></a>Instalar o MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Suporte do VS Code para MongoDB

O VS Code dá suporte ao trabalho com bancos de dados MongoDB por meio da [extensão do Azure Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), você pode criar bancos de dados MongoDB no VS Code, conectar-se a eles, gerenciá-los e consultá-los.

Para saber mais, acesse os documentos do VS Code: [Como trabalhar com o MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Configurar aliases de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
