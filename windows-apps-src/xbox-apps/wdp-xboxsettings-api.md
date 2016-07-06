---
author: payzer
title: "Referência da API de configurações do desenvolvedor do Xbox do Device Portal"
description: "Aprenda a acessar as configurações do desenvolvedor do Xbox."
ms.sourcegitcommit: c392e6a2356d4180943c17d37512ef8ea35f5719
ms.openlocfilehash: 52fb6e76bd7ad35127eeebeced8f6f8cc076fb7f

---

# Referência da API de configurações do desenvolvedor   
É possível acessar configurações do Xbox One que sejam úteis para o desenvolvimento usando essa API.

## Obter todas as configurações do desenvolvedor de uma só vez

**Solicitação**

É possível usar a solicitação a seguir para obter todas as configurações do desenvolvedor em uma única solicitação.

Método      | URI da solicitação
:------     | :-----
GET | /ext/settings
<br />
**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**   
A resposta é uma matriz JSON de configurações que contém todas as configurações. Todo objeto de configurações contém os seguintes campos:   

Name – (sequência) o nome da configuração.   
Value – (sequência) o valor da configuração.   
RequiresReboot – ("Sim" | "Não") Este campo indica se a configuração requer uma reinicialização para entrar em vigor.
Category – (sequência) a categoria da configuração

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

## Obter configurações uma por vez
As configurações também podem ser recuperadas individualmente.

**Solicitação**

É possível usar a solicitação a seguir para obter informações sobre uma configuração individual.

Método      | URI da solicitação
:------     | :-----
GET | /ext/settings/<setting name>
<br />
**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**   
A resposta é um objeto JSON com os seguintes campos:   

Name – (sequência) o nome da configuração.   
Value – (sequência) o valor da configuração.   
RequiresReboot – ("Sim" | "Não") Este campo indica se a configuração requer uma reinicialização para entrar em vigor.
Category – (sequência) a categoria da configuração

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

## Defina o valor de uma configuração
É possível definir o valor de uma configuração.

**Solicitação**

É possível usar a solicitação a seguir para definir o valor de uma configuração.

Método      | URI da solicitação
:------     | :-----
PUT | /ext/settings/<setting name>
<br />
**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   
O corpo da solicitação é um objeto JSON contendo o seguinte campo:   
Value – (sequência) o novo valor da configuração.

**Resposta**   

- Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox




<!--HONumber=Jun16_HO4-->


