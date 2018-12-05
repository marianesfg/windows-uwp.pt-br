---
title: Referência de API da captura de mídia
description: Saiba como acessar a API de captura de mídia de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 7a27d13f7ceedd14a84d5b4b4aa1233445037a1f
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8758566"
---
# <a name="media-capture-api-reference"></a>Referência de API da captura de mídia #

**Solicitação**

Você pode capturar uma representação PNG da tela atual usando o formato de solicitação a seguir.

| Método        | URI da solicitação     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:


| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| transferência (opcional)| Um valor booliano que indica se os cabeçalhos de resposta HTTP devem ser definidos indicando que o navegador do host precisa baixar a captura de tela como anexo em vez renderizá-lo no navegador.  |
<br>

**Cabeçalhos de solicitação**

* Nenhum(a)

**Corpo da solicitação**

* Nenhum(a)

###<a name="response"></a>Resposta # # #

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP   | Descrição     | 
| ------------------ |-----------------|
| 200                | Solicitação de captura de tela bem-sucedida e captura retornada |
| 5XX                | Códigos de erro de falhas inesperadas |
<br>

**Famílias de dispositivos disponíveis**

* Windows Xbox

