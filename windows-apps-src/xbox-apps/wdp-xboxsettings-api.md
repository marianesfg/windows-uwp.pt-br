---
author: payzer
title: "Referência da API de configurações do desenvolvedor do Xbox do Device Portal"
description: "Aprenda a acessar as configurações do desenvolvedor do Xbox."
translationtype: Human Translation
ms.sourcegitcommit: c51eff41e63d815f6298b4fc46a9b11314bc8bc9
ms.openlocfilehash: 5a983714cda9b5a5f45e555e2cb6f980f082a003

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
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
5XX | Códigos de erro

## Obter configurações uma por vez
As configurações também podem ser recuperadas individualmente.

**Solicitação**

É possível usar a solicitação a seguir para obter informações sobre uma configuração individual.

Método      | URI da Solicitação
:------     | :-----
GET | /ext/settings/\<nome da configuração\>
<br />
**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

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
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
5XX | Códigos de erro

## Defina o valor de uma configuração
É possível definir o valor de uma configuração.

**Solicitação**

É possível usar a solicitação a seguir para definir o valor de uma configuração.

Método      | URI da Solicitação
:------     | :-----
PUT | /ext/settings/\<nome da configuração\>
<br />
**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   
O corpo da solicitação é um objeto JSON contendo o seguinte campo:   
Value – (sequência) o novo valor da configuração.

**Resposta**   

- Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


