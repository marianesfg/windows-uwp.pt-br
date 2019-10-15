---
title: Introdução ao uso do Python no Windows com um banco de dados
description: Um guia para ajudá-lo a começar a usar o PostgreSQL ou o MongoDB com Python no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, PostgreSQL, MongoDB, Postgres, Mongo, Microsoft, Python no Windows, instalar o PostgreSQL no Windows, instalar o MongoDB no Windows, usar PostgreSQL com Python, usar o MongoDB com Python, PostgreSQL no WSL, MongoDB no WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 42a257361cffec974d060a6518dfdf5254d62082
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314850"
---
# <a name="get-started-using-databases-with-python-on-windows"></a>Introdução ao uso de bancos de dados com Python no Windows

Os aplicativos Python geralmente precisam manter os dados, o que pode acontecer por meio de arquivos, armazenamento local, serviços de nuvem ou bancos de dado. Este guia passo a passo ajudará você a começar a conectar seu aplicativo Python a um banco de dados. Optamos por se concentrar em duas opções populares: PostgreSQL e MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Diferenças entre o MongoDB e o PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> Talvez você também queira considerar como a estrutura e as ferramentas que você está usando são integradas a um sistema de banco de dados específico. A [estrutura da Web do Django](./web-frameworks.md#hello-world-tutorial-for-django) parece ser melhor integrada ao PostgreSQL (consulte o [Django docs](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) e [psycopg2](https://github.com/psycopg/psycopg2)). A [estrutura da Web do Flask](./web-frameworks.md#hello-world-tutorial-for-flask) parece ser melhor integrada ao MongoDB (consulte [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) e [PyMongo](https://github.com/dcrosta/flask-pymongo)).

## <a name="install-postgresql"></a>Instalar o PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Suporte VS Code para PostgreSQL

VS Code dá suporte ao trabalho com bancos de dados PostgreSQL por meio da [extensão PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), você pode criar, conectar-se a, gerenciar e consultar bancos de dados PostgreSQL de dentro vs Code.

## <a name="install-mongodb"></a>Instalar o MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Suporte de VS Code para MongoDB

VS Code dá suporte ao trabalho com bancos de dados do MongoDB por meio da [extensão CosmosDB do Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), você pode criar, conectar-se a, gerenciar e consultar bancos de dados do MongoDB de dentro vs Code.

Para saber mais, visite o VS Code docs: [Trabalhando com o MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Configurar aliases de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
