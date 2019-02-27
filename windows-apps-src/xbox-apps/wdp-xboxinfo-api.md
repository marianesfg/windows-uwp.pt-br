---
title: Informações do dispositivo Xbox do Portal referência de API
description: Saiba como acessar informações de dispositivo Xbox.
ms.date: 11/072017
ms.topic: article
keywords: Windows 10, uwp, xbox, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 85c2c139aa8064e1f0769064b95eeb531086b8c1
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115976"
---
# <a name="xbox-info-api-reference"></a>Referência de API de informações do Xbox   
Você pode acessar informações de dispositivo Xbox One usando essa API.

## <a name="get-xbox-one-device-information"></a>Obter informações de dispositivo Xbox One

**Solicitação**

Você pode obter informações de dispositivo sobre seu Xbox One.

Método      | URI da solicitação
:------     | :-----
GET | /ext/xbox/info
<br />
**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhum

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