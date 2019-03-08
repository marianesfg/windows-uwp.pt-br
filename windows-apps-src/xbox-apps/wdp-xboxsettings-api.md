---
title: Referência da API de configurações do desenvolvedor do Xbox do Device Portal
description: Aprenda a acessar as configurações do desenvolvedor do Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 402d535bf6ff9ced24bc642c17d13b2d48d79681
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598641"
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
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**   
A resposta é uma matriz JSON de configurações que contém todas as configurações. Todo objeto de configurações contém os seguintes campos:

* Name – (sequência) o nome da configuração.
* Value – (sequência) o valor da configuração.
* RequiresReboot – ("Sim" | "Não") Este campo indica se a configuração requer uma reinicialização para entrar em vigor.
* Desabilitada - ("Sim" | "Não") Este campo indica se a configuração está desabilitada e não pode ser editada.
* Categoria - (cadeia de caracteres) A categoria da configuração.
* Tipo - ("Texto" | "Número" | "Booleano" | "Selecionar") Este campo indica o tipo uma configuração: texto de entrada, um valor booleano ("true" ou "false"), um número com um mín. e máx. ou selecione com uma lista de valores específicos.

Se a configuração é um número:
* Min - (número) esse campo indica o valor numérico mínimo da configuração.
* Max - (número) esse campo indica o valor numérico máximo da configuração.

Se a configuração for selecionada:
* OptionsVariable - ("Sim" | "Não") neste campo indica se as opções de configuração são variáveis, se as opções válidas podem alterar sem uma reinicialização.
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
GET | /ext/Settings/\<nome da configuração\>
<br />
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**   
A resposta é um objeto JSON com os seguintes campos:

* Name – (sequência) o nome da configuração.
* Value – (sequência) o valor da configuração.
* RequiresReboot – ("Sim" | "Não") Este campo indica se a configuração requer uma reinicialização para entrar em vigor.
* Desabilitada - ("Sim" | "Não") Este campo indica se a configuração está desabilitada e não pode ser editada.
* Categoria - (cadeia de caracteres) A categoria da configuração.
* Tipo - ("Texto" | "Número" | "Booleano" | "Selecionar") Este campo indica o tipo uma configuração: texto de entrada, um valor booleano ("true" ou "false"), um número com um mín. e máx. ou selecione com uma lista de valores específicos.

Se a configuração é um número:
* Min - (número) esse campo indica o valor numérico mínimo da configuração.
* Max - (número) esse campo indica o valor numérico máximo da configuração.

Se a configuração for selecionada:
* OptionsVariable - ("Sim" | "Não") neste campo indica se as opções de configuração são variáveis, se as opções válidas podem alterar sem uma reinicialização.
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
PUT | /ext/Settings/\<nome da configuração\>
<br />
**Parâmetros de URI**

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