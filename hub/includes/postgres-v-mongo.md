---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314870"
---
Duas [opções populares](https://insights.stackoverflow.com/survey/2019#technology-_-databases) para um sistema de banco de dados são o [MongoDB](https://www.mongodb.com/what-is-mongodb) e o [PostgreSQL](https://www.postgresql.org/about/). 

O MongoDB é um banco de dados de documentos NoSQL projetado para funcionar com JSON e armazenar dados sem esquema. É ótimo para flexibilidade e dados não estruturados, armazenamento em cache de análise em tempo real e dimensionamento horizontal. 

O PostgreSQL (às vezes chamado de Postgres) é um banco de dados relacional SQL com ênfase em extensibilidade e conformidade com os padrões. Agora ele também pode tratar JSON, mas geralmente é melhor para dados estruturados, dimensionamento vertical e necessidades em conformidade com ACID, como comércio eletrônico e transações financeiras.

Esquemas:

**PostgreSQL:** Tabela | Coluna | Valor | Gravações.

**MongoDB (NoSQL):** Coleção | Chave | Valor | Documento.

O tipo de banco de dados escolhido deve depender do tipo de aplicativo com o qual você usará o banco de dados. Recomendamos que você pesquise as vantagens e as desvantagens de bancos de dados estruturados e não estruturados e faça uma escolha com base em seu caso de uso. Há também vários outros sistemas de bancos de dados a serem considerados, além do PostgreSQL e do MongoDB.