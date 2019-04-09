---
title: Referência da API de Gerenciamento do usuário de teste da Xbox Live
description: Saiba como acessar as APIs de Gerenciamento do usuário de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 71c47767cf026b962f682fb30ca93758dbd5e227
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244072"
---
#<a name="xbox-live-user-management"></a>Gerenciamento de usuário em tempo real do Xbox #

## <a name="request"></a>Solicitação

Você pode obter a lista de usuários no console ou atualizar a lista adicionando, removendo, entrando, saindo ou modificando usuários existentes.

| Método        | URI da solicitação     | 
| ------------- |-----------------|
| OBTER           | /ext/user |
| PUT           | /ext/user |


**Parâmetros do URI**

* Nenhuma

**Cabeçalhos de solicitação**

* Nenhuma

**Corpo da solicitação**

As chamadas para PUT devem incluir uma matriz JSON com a seguinte estrutura:

* Usuários
  * AutoSignIn (opcional): bool desabilitando ou habilitando a entrada automática da conta especificada por EmailAddress ou UserId.
  * Endereço de email (opcional - deve ser fornecido se a ID de usuário não for fornecido, a menos que entrar em um usuário patrocinado): Especificando o usuário para modificar/adicionar/excluir um endereço de email.
  * Senha (opcional - deve ser fornecido se o usuário não estiver atualmente no console): Senha usada para adicionar um novo usuário no console.
  * SignedIn (opcional): bool especificando se deve haver entra ou saída da conta fornecida.
  * ID de usuário (opcional - deve ser fornecido se o endereço de email não for fornecido, a menos que entrar em um usuário patrocinado): UserId especificando o usuário para modificar/adicionar/excluir.
  * SponsoredUser (opcional): bool especificando se é necessário adicionar um usuário patrocinado ou não.
  * Delete (opcional): booliano que especifica para excluir esse usuário no console do

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
