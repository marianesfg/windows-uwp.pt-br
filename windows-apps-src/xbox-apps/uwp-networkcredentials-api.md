---
title: Referência de API de credenciais de rede do Device Portal
description: Saiba como adicionar, remover ou atualizar as credenciais de rede de maneira programática.
ms.localizationpriority: medium
ms.openlocfilehash: 2da8dae554a0dcbb84d3d3fc3873e2fb035175dc
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8346529"
---
# <a name="network-credentials-api-reference"></a>Referência de API de credenciais de rede
Você pode adicionar, remover ou atualizar as credenciais de rede armazenados no seu devkit usando essa API REST.

## <a name="get-existing-credentials"></a>Obter as credenciais existentes

**Solicitação**

Você pode obter uma lista dos compartilhamentos armazenados juntamente com o nome de usuário do usuário que tenha credenciais de compartilhamento de rede.

Método      | URI da solicitação
:------     | :-----
GET | /ext/networkcredential
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**   

- Nenhum(a)

**Resposta**   

- Matriz JSON no formato a seguir:
* Credenciais
  * NetworkPath - o caminho para o compartilhamento de rede.
  * Nome de usuário - o nome de usuário que armazenou credenciais.

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
| NetworkPath        | O caminho de rede para o compartilhamento de você estiver adicionando credenciais para acessar. |
<br>

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Os seguintes elementos JSON:
* NetworkPath - o caminho para o compartilhamento de rede.
* Nome de usuário - o nome de usuário para armazenar as credenciais em.
* Password - a senha do nova ou atualizada para esse usuário.

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
| NetworkPath        | O caminho de rede para o compartilhamento da qual você está removendo as credenciais armazenadas. |
<br>

**Cabeçalhos de solicitação**

- Nenhum(a)

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


