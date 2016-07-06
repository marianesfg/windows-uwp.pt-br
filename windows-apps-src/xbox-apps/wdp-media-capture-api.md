---
author: WilliamsJason
title: "Referência de API da captura de mídia"
description: "Saiba como acessar a API de captura de mídia de maneira programática."
ms.sourcegitcommit: 4356bd2cfc7697905ed91d60b5829c06d850e109
ms.openlocfilehash: 2e32c4d516a35b5f5bd8d57bce75ff8bdce6b461

---

# Referência de API da captura de mídia #

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

###Resposta###

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP   | Descrição     | 
| ------------------ |-----------------|
| 200                | Solicitação de captura de tela bem-sucedida e captura retornada |
| 5XX                | Códigos de erro de falhas inesperadas |
<br>

**Famílias de dispositivos disponíveis**

* Windows Xbox




<!--HONumber=Jun16_HO4-->


