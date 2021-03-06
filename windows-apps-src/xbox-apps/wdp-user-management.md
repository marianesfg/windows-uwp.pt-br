---
title: Referência da API de Gerenciamento do usuário de teste da Xbox Live
description: Saiba como acessar as APIs de Gerenciamento do usuário de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 52f333af73084ed14982b9d09b6770c8294980f7
ms.sourcegitcommit: 6169660ea437915265165c4631d9702587e4793d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74902524"
---
# <a name="xbox-live-user-management"></a>Gerenciamento do usuário do Xbox Live

## <a name="request"></a>Solicitação

Você pode obter a lista de usuários no console ou atualizar a lista adicionando, removendo, entrando, saindo ou modificando usuários existentes.

| Método        | URI da solicitação     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |


**Parâmetros de URI**

* Nenhuma

**Cabeçalhos da solicitação**

* Nenhuma

**Corpo da solicitação**

As chamadas para PUT devem incluir uma matriz JSON com a seguinte estrutura:

* Usuários
  * AutoSignIn (opcional): bool desabilitando ou habilitando a entrada automática da conta especificada por EmailAddress ou UserId.
  * EmailAddress (opcional – deverá ser fornecido se UserId não for fornecido, a menos que haja a entrada de um usuário patrocinado): endereço de e-mail especificando o usuário a ser modificado/adicionado/excluído.
  * Password (opcional – deverá ser fornecido se o usuário não estiver no console no momento): senha usada para adicionar um novo usuário ao console.
  * SignedIn (opcional): bool especificando se deve haver entra ou saída da conta fornecida.
  * UserId (opcional – deverá ser fornecido se EmailAddress não for fornecido, a menos que haja a entrada de um usuário patrocinado): UserId especificando o usuário a ser modificado/adicionado/excluído.
  * SponsoredUser (opcional): bool especificando se é necessário adicionar um usuário patrocinado ou não.
  * Delete (opcional): BOOL especificando para excluir este usuário do console

## <a name="response"></a>Resposta

**Corpo da resposta**

Chamadas para GET retornará uma matriz JSON com as seguintes propriedades:

* Usuários
  * AutoSignIn (opcional)
  * EmailAddress (opcional)
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser (opcional)
  
**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP   | Descrição     | 
| ------------------ |-----------------|
| 200                | A chamada para GET foi bem-sucedida e a matriz JSON de usuários retornou no corpo da resposta |
| 204                | A chamada para PUT foi bem-sucedida e os usuários no console foram atualizados |
| 4XX                | Diversos erros de dados ou formato de solicitação inválidos |
| 5XX                | Códigos de erro de falhas inesperadas |
