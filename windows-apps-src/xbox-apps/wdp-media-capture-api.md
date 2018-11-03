---
author: WilliamsJason
title: Referência de API da captura de mídia
description: Saiba como acessar a API de captura de mídia de maneira programática.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: f58fa4c3a9a1abd407f635f27de3a545c3aafc6c
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5983687"
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

