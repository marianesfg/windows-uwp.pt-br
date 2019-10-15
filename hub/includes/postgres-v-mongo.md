---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314870"
---
Duas [Opções populares](https://insights.stackoverflow.com/survey/2019#technology-_-databases) para um sistema de banco de dados são [MongoDB](https://www.mongodb.com/what-is-mongodb) e [PostgreSQL](https://www.postgresql.org/about/). 

O MongoDB é um banco de dados de documentos NoSQL projetado para funcionar com JSON e armazenar os dados sem esquema. É bom para flexibilidade e dados não estruturados, armazenamento em cache de análise em tempo real e dimensionamento horizontal. 

O PostgreSQL (às vezes chamado de Postgres) é um banco de dados relacional SQL com ênfase em extensibilidade e conformidade com os padrões. Ele também pode lidar com o JSON, mas é geralmente melhor para dados estruturados, dimensionamento vertical e necessidades em conformidade com ACID, como comércio eletrônico e transações financeiras.

Esquemas

**PostgreSQL:** Tabela | Coluna | Valor | Grava.

**MongoDB (NoSQL):** Coleção | Chave | Valor | Document.

O tipo de banco de dados que você escolher deve depender do Type do aplicativo com o qual você usará o banco de dados. Recomendamos que você procure as vantagens e desvantagens de bancos de dados estruturados e não estruturados e escolha com base no caso de uso. Também há vários outros sistemas de banco de dados para considerar além do PostgreSQL e do MongoDB.