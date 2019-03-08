---
title: Referência de API HTTP do monitor do Portal de Dispositivos
description: Saiba como acessar o tráfego HTTP do aplicativo focado em um Xbox.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 8b8828b060e0401e7938517e497bae20e1234baf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645671"
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
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

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
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

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