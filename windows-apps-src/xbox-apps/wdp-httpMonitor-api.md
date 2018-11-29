---
title: Referência de API HTTP do monitor do Portal de Dispositivos
description: Saiba como acessar o tráfego HTTP do aplicativo focado em um Xbox.
ms.localizationpriority: medium
ms.openlocfilehash: 81de2a2a3194384e9c5de1c5c45a827e4d965c91
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7982173"
---
# <a name="http-monitor-api-reference"></a>Referência de API do monitor de HTTP   
Você pode acessar o tráfego HTTP em tempo real para o aplicativo focado usando essa API se o monitor HTTP tiver sido habilitado no console do Xbox, marcando a caixa na Dev Home.

## <a name="get-if-the-http-monitor-is-enabled"></a>Obter se o monitor de HTTP estiver habilitado

**Solicitação**

Você pode obter se o monitor de HTTP foi habilitado na Dev Home.

Método      | URI da solicitação
:------     | :-----
GET | /ext/httpmonitor/sessions
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**   
Um objeto JSON com os seguintes campos:

* Habilitado - (Bool) se o monitor de HTTP tiver sido habilitado no console do Xbox marcando a caixa na Dev Home.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="get-http-traffic-from-the-focused-app"></a>Obter o tráfego HTTP do aplicativo focado
**Solicitação**

Obtenha o tráfego de HTTP do aplicativo focado no Xbox, desde que não seja um aplicativo do sistema, em tempo real, se o monitor de HTTP tiver sido habilitado na Dev Home.

Método      | URI da solicitação
:------     | :-----
Websocket | /ext/httpmonitor/sessions
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**   
Um objeto JSON com os seguintes campos:

* Sessions
    * RequestHeaders - (Objeto JSON) Os cabeçalhos de solicitação da Solicitação de HTTP.
    * RequestContentHeaders - (Objeto JSON) Os cabeçalhos de conteúdo solicitados a partir da Solicitação de HTTP.
    * RequestURL - (String) O URL da solicitação.
    * RequestMethod - (String) O método da solicitação.
    * RequestMessage - (String) A mensagem da solicitação que suporta, atualmente, somente JSON e conteúdo de texto.
    * ResponseHeaders - (Objeto JSON) Os cabeçalhos de resposta da Resposta HTTP.
    * ResponseContentHeaders - (Objeto JSON) Os cabeçalhos de conteúdo de resposta da Resposta HTTP.
    * StatusCode - (Número) O código de status de resposta.
    * ReasponsePhrase - (String) A frase do motivo de resposta.
    * ResponseMessage - (String) A mensagem de resposta que suporta, atualmente, somente JSON e conteúdo de texto.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
403 | Monitor de HTTP desabilitado, deve ser habilitado na Dev Home
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox