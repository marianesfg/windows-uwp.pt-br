---
author: M-Stahl
title: Informações do dispositivo Xbox do Portal referência de API
description: Saiba como acessar informações de dispositivo Xbox.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
keywords: Windows 10, uwp, xbox, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 4b0e2bab0ce7d5525e8032809954ff656a74a61c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6992131"
---
# <a name="xbox-info-api-reference"></a>Referência de API de informações do Xbox   
Você pode acessar informações de dispositivo Xbox One usando essa API.

## <a name="get-xbox-one-device-information"></a>Obter informações do dispositivo Xbox One

**Solicitação**

Você pode obter informações de dispositivo sobre seu Xbox One.

Método      | URI da solicitação
:------     | :-----
GET | /ext/xbox/info
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**   
Um objeto JSON com os seguintes campos:

* OsVersion - (String) a versão do sistema operacional.
* OsEdition - (String) a edição do sistema operacional, como "Março de 2017" ou "março de 2017 QFE 1".
* ConsoleId - ID. do (String) o console
* DeviceId - dispositivo do (String) o console Xbox Live ID.
* SerialNumber - o número de série do (String) o console.
* DevMode - modo atual de desenvolvedor do (String) o console, como "None" ou "Retail".
* ConsoleType - tipo do (String) o console, como "Xbox One" ou "Xbox One S".
* DevkitCertificateExpirationTime - (número) o horário UTC em segundos quando o certificado de kit do desenvolvedor do console irá expirar.

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