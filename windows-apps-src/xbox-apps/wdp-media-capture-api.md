---
author: WilliamsJason
title: "Referência de API da captura de mídia"
description: "Saiba como acessar a API de captura de mídia de maneira programática."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 9819e632ab6de58eee4358866d3186c0fa31f69f
ms.lasthandoff: 02/08/2017

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
| download (opcional)| Um valor booliano que indica se os cabeçalhos de resposta HTTP devem ser definidos indicando que o navegador do host precisa baixar a captura de tela como anexo em vez renderizá-lo no navegador.  |
<br>

**Cabeçalhos de solicitação**

* Nenhum

**Corpo da solicitação**

* Nenhum

###<a name="response"></a>Resposta###

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP   | Descrição     | 
| ------------------ |-----------------|
| 200                | Solicitação de captura de tela bem-sucedida e captura retornada |
| 5XX                | Códigos de erro de falhas inesperadas |
<br>

**Famílias de dispositivos disponíveis**

* Windows Xbox


