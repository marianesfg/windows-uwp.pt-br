---
author: M-Stahl
title: Informações de Portal Xbox referência da API do dispositivo
description: Saiba como acessar informações do dispositivo Xbox.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, xbox, portal de dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: db1df2418a2bb60de4a72f51ad01a0bfd547ec20
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "406245"
---
# <a name="xbox-info-api-reference"></a>Referência de API do Xbox Info   
Você pode acessar um Xbox informações do dispositivo usando essa API.

## <a name="get-xbox-one-device-information"></a>Obter um Xbox informações do dispositivo

**Solicitação**

Você pode obter informações do dispositivo sobre sua um Xbox.

Método      | URI da solicitação
:------     | :-----
GET | /ext/xbox/info
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum(a)

**Resposta**   
Um objeto JSON com os seguintes campos:

* OsVersion - (String) a versão do sistema operacional.
* OsEdition - (String) a edição do sistema operacional, como "Março de 2017" ou "março de 2017 QFE 1".
* ConsoleId - ID de. do (String) o console
* DeviceId - dispositivo do (String) o console Xbox Live ID.
* SerialNumber - o número de série do (String) o console.
* DevMode - modo de desenvolvedor atual do (String) o console, como "None" ou "Varejo".
* ConsoleType - tipo do (String) o console, como "Um Xbox" ou "Xbox um S".
* DevkitCertificateExpirationTime - o tempo de UTC (número), em segundos, quando o certificado de kit de desenvolvedor do console irá expirar.

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