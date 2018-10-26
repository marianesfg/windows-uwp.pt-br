---
author: normesta
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Acesso a dados
description: Esta seção aborda o armazenamento de dados no dispositivo em um banco de dados privado e o uso do mapeamento relacional de objeto em apps da Plataforma Universal do Windows (UWP).
ms.author: normesta
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, dados, banco de dados, relacional, tabelas, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: beca20d358430ecd82cd1bc57459a6f6af36be77
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5569858"
---
# <a name="data-access"></a>Acesso a dados

Você pode armazenar dados no dispositivo do usuário usando um banco de dados SQLite. Você também pode conectar seu aplicativo diretamente a um banco de dados do SQL Server sem usar qualquer tipo de camada de serviço.

| Tópico | Descrição|
|-------|------------|
| [Usar um banco de dados do SQLite em um aplicativo UWP](sqlite-databases.md) | Mostra como usar o SQLite para armazenar e recuperar dados em um banco de dados leve no dispositivo do usuário. SQLite é um mecanismo de banco de dados sem servidor inserido. |
| [Usar um banco de dados do SQL server em um aplicativo UWP](sql-server-databases.md) | Mostra como se conectar diretamente a um banco de dados do SQL Server e, em seguida, armazenar e recuperar dados usando classes no namespace [SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) . Nenhuma camada de serviço é necessária. |

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplo de banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
