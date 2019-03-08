---
title: Referência de API de credenciais de rede do Portal de Dispositivos
description: Saiba como adicionar, remover ou atualizar credenciais de rede de maneira programática.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: ac30d8db830c51ee40653feb49b443ed44502617
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659211"
---
# <a name="network-credentials-api-reference"></a>Referência de API de credenciais de rede
Você pode adicionar, remover ou atualizar as credenciais de rede armazenadas no seu kit de desenvolvimento usando a API REST.

## <a name="get-existing-credentials"></a>Obter as credenciais existentes

**Solicitação**

Você pode obter uma lista dos compartilhamentos armazenados juntamente com o nome di usuário com as credenciais para o compartilhamento de rede.

Método      | URI da solicitação
:------     | :-----
GET | /ext/networkcredential
<br />
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   

- Nenhuma

**Resposta**   

- Matriz JSON no formato a seguir:
* Credenciais
  * NetworkPath - O caminho para o compartilhamento de rede.
  * Username – O nome do usuário com as credenciais armazenadas.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Êxito
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="add-or-update-stored-credentials-for-a-user"></a>Adicionar ou atualizar as credenciais armazenadas de um usuário

**Solicitação**

Método      | URI da solicitação
:------     | :-----
POST | /ext/networkcredential
<br />
**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| NetworkPath        | O caminho de rede para o compartilhamento ao qual você está adicionando credenciais de acesso. |
<br>

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Os seguintes elementos JSON:
* NetworkPath - O caminho para o compartilhamento de rede.
* Username – O nome do usuário para armazenar as credenciais.
* Password – A senha nova ou atualizada desse usuário.

**Resposta**   

- Nenhuma  

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | Êxito
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="remove-stored-credentials-for-a-share"></a>Remova as credenciais armazenadas de um compartilhamento.

**Solicitação**

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| NetworkPath        | O caminho de rede para o compartilhamento do qual você está removendo as credenciais armazenadas. |
<br>

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   

- Nenhuma

**Resposta**   

- Nenhuma 

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


