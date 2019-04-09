---
title: Referência de API da captura de mídia
description: Saiba como acessar a API de captura de mídia de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 7dcd4c6c39a983ab11bfacd391bfa78942601258
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244052"
---
# <a name="media-capture-api-reference"></a>Referência de API da captura de mídia #

## <a name="request"></a>Solicitação

Você pode capturar uma representação PNG da tela atual usando o formato de solicitação a seguir.

| Método        | URI da solicitação     | 
| ------------- |-----------------|
| OBTER           | /ext/screenshot |


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:


| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| download (opcional)| Um valor booliano que indica se os cabeçalhos de resposta HTTP devem ser definidos indicando que o navegador do host precisa baixar a captura de tela como anexo em vez renderizá-lo no navegador.  |

**Cabeçalhos de solicitação**

* Nenhuma

**Corpo da solicitação**

* Nenhuma

## <a name="response"></a>Resposta

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP   | Descrição     | 
| ------------------ |-----------------|
| 200                | Solicitação de captura de tela bem-sucedida e captura retornada |
| 5XX                | Códigos de erro de falhas inesperadas |
<br>

**Famílias de dispositivos disponíveis**

* Windows Xbox

