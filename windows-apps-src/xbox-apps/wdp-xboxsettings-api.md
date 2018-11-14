---
author: m-stahl
title: Referência da API de configurações do desenvolvedor do Xbox do Device Portal
description: Aprenda a acessar as configurações do desenvolvedor do Xbox.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 8f3d0c09b242f8d60b06ee0dc510ad9a756466c5
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6648136"
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

* Name – (sequência) o nome da configuração.
* Value – (sequência) o valor da configuração.
* RequiresReboot – ("Sim" | "Não") Este campo indica se a configuração requer uma reinicialização para entrar em vigor.
* Desabilitada - ("Sim" | "Não") Este campo indica se a configuração está desabilitada e não pode ser editada.
* Categoria - (cadeia de caracteres) A categoria da configuração.
* Tipo - ("Texto" | "Número" | "Booleano" | "Selecionar") Este campo indica o tipo uma configuração: texto de entrada, um valor booleano ("true" ou "false"), um número com um mín. e máx. ou selecione com uma lista de valores específicos.

Se a configuração for um número:
* Mín - (número) Este campo indica o valor numérico mínimo da configuração.
* Máx - (número) Este campo indica o valor numérico máximo da configuração.

Selecione se a configuração é:
* OptionsVariable - ("Sim" | "Não") Este campo indica se as opções de configuração são variáveis, se as opções válidas podem mudar sem uma reinicialização.
* Opções - Array JSON com as opções de seleção válidas como cadeias de caracteres.

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

* Name – (sequência) o nome da configuração.
* Value – (sequência) o valor da configuração.
* RequiresReboot – ("Sim" | "Não") Este campo indica se a configuração requer uma reinicialização para entrar em vigor.
* Desabilitada - ("Sim" | "Não") Este campo indica se a configuração está desabilitada e não pode ser editada.
* Categoria - (cadeia de caracteres) A categoria da configuração.
* Tipo - ("Texto" | "Número" | "Booleano" | "Selecionar") Este campo indica o tipo uma configuração: texto de entrada, um valor booleano ("true" ou "false"), um número com um mín. e máx. ou selecione com uma lista de valores específicos.

Se a configuração for um número:
* Mín - (número) Este campo indica o valor numérico mínimo da configuração.
* Máx - (número) Este campo indica o valor numérico máximo da configuração.

Selecione se a configuração é:
* OptionsVariable - ("Sim" | "Não") Este campo indica se as opções de configuração são variáveis, se as opções válidas podem mudar sem uma reinicialização.
* Opções - Array JSON com as opções de seleção válidas como cadeias de caracteres.

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