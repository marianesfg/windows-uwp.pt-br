---
author: payzer
title: "Referência da API de configurações do desenvolvedor do Xbox do Device Portal"
description: "Aprenda a acessar as configurações do desenvolvedor do Xbox."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.openlocfilehash: 43e4bb289d12439bbc0f6de347d187b067288d51
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developer-settings-api-reference"></a>Referência da API de configurações do desenvolvedor   
É possível acessar configurações do Xbox One que sejam úteis para o desenvolvimento usando essa API.

## <a name="get-all-developer-settings-at-once"></a>Obter todas as configurações do desenvolvedor de uma só vez

**Solicitação**

É possível usar a solicitação a seguir para obter todas as configurações do desenvolvedor em uma única solicitação.

Método      | URI da solicitação
:------     | :-----
GET | /ext/settings
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

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

## <a name="get-settings-one-at-a-time"></a>Obter configurações uma por vez
As configurações também podem ser recuperadas individualmente.

**Solicitação**

É possível usar a solicitação a seguir para obter informações sobre uma configuração individual.

Método      | URI da solicitação
:------     | :-----
GET | /ext/settings/\<nome da configuração\>
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

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

## <a name="set-the-value-of-a-setting"></a>Defina o valor de uma configuração
É possível definir o valor de uma configuração.

**Solicitação**

É possível usar a solicitação a seguir para definir o valor de uma configuração.

Método      | URI da solicitação
:------     | :-----
PUT | /ext/settings/\<nome da configuração\>
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**   
O corpo da solicitação é um objeto JSON contendo o seguinte campo:   
Value – (sequência) o novo valor da configuração.

**Resposta**   

- Nenhum(a)

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

