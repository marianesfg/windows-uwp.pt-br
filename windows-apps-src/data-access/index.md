---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Acesso a dados
description: Esta seção aborda o armazenamento de dados no dispositivo em um banco de dados privado e o uso do mapeamento relacional de objeto em aplicativos da Plataforma Universal do Windows (UWP).
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, dados, banco de dados, relacional, tabelas, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: 8ce2bbb1b72eaa33001e86c9aa81b334e338cb53
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362606"
---
# <a name="data-access"></a>Acesso a dados

Você pode armazenar dados no dispositivo do usuário utilizando um banco de dados SQLite. Você também pode conectar seu aplicativo diretamente para um banco de dados SQL Server sem necessidade de usar nenhum tipo de camada de serviço.

| Tópico | Descrição|
|-------|------------|
| [Usar um banco de dados do SQLite em um aplicativo UWP](sqlite-databases.md) | Mostra como usar o SQLite para armazenar e recuperar dados em um banco de dados leve nos dispositivos dos usuários. SQLite é um mecanismo de banco de dados sem servidor inserido. |
| [Usar um banco de dados do SQL Server em um aplicativo UWP](sql-server-databases.md) | Mostra como conectar-se diretamente a um banco de dados do SQL Server e, em seguida, armazenar e recuperar dados usando classes no namespace [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN). Nenhuma camada de serviço é necessária. |

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplo de banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
