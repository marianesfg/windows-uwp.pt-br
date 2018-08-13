---
author: WilliamsJason
title: Referência de API de credenciais de rede do dispositivo Portal
description: Saiba como adicionar, remover ou atualizar as credenciais de rede de maneira programática.
ms.localizationpriority: medium
ms.openlocfilehash: 7e00169f92ee6f0aa48df64ec4a1186f9682b358
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "410104"
---
# <a name="network-credentials-api-reference"></a>Referência de API de credenciais de rede
Você pode adicionar, remover ou atualizar as credenciais de rede armazenados na sua devkit usando essa API REST.

## <a name="get-existing-credentials"></a>Obter credenciais existentes

**Solicitação**

Você pode obter uma lista dos compartilhamentos armazenados juntamente com o nome de usuário do usuário que tenha credenciais para o compartilhamento de rede.

Método      | URI da solicitação
:------     | :-----
GET | /ext/networkcredential
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   

- Nenhum(a)

**Resposta**   

- Matriz JSON no seguinte formato:
* Credenciais
  * NetworkPath - o caminho para o compartilhamento de rede.
  * Username - o nome de usuário que tenha sido credenciais armazenadas.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Êxito
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="add-or-update-stored-credentials-for-a-user"></a>Adicionar ou atualizar as credenciais armazenadas para um usuário

**Solicitação**

Método      | URI da solicitação
:------     | :-----
POST | /ext/networkcredential
<br />
**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| NetworkPath        | O caminho para o compartilhamento de rede você está adicionando credenciais de acesso. |
<br>

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Os seguintes elementos JSON:
* NetworkPath - o caminho para o compartilhamento de rede.
* Username - o nome de usuário para armazenar as credenciais em.
* Password - a senha nova ou atualizada para este usuário.

**Resposta**   

- Nenhum(a)  

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | Êxito
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="remove-stored-credentials-for-a-share"></a>Remova as credenciais armazenadas para um compartilhamento.

**Solicitação**

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| NetworkPath        | O caminho de rede para o compartilhamento da qual você está removendo credenciais armazenadas. |
<br>

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   

- Nenhum(a)

**Resposta**   

- Nenhum(a) 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | A solicitação para as credenciais foi bem-sucedida.
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox


