---
title: Referência de API de informações do Xbox do Portal do dispositivo
description: Saiba como acessar as informações de dispositivo do Xbox.
ms.date: 11/072017
ms.topic: article
keywords: Windows 10, uwp, xbox, portal do dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: 85c2c139aa8064e1f0769064b95eeb531086b8c1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617491"
---
# <a name="xbox-info-api-reference"></a>Referência de API de informações do Xbox   
Você pode acessar as informações de dispositivo do Xbox One usando essa API.

## <a name="get-xbox-one-device-information"></a>Obter informações de dispositivo do Xbox One

**Solicitação**

Você pode obter informações de dispositivo sobre o Xbox One.

Método      | URI da solicitação
:------     | :-----
GET | /ext/xbox/info
<br />
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**   
Um objeto JSON com os seguintes campos:

* OsVersion – (cadeia de caracteres) A versão do sistema operacional.
* OsEdition – (cadeia de caracteres) A edição do sistema operacional, como "março de 2017" ou "março de 2017 QFE 1".
* ConsoleId - (cadeia de caracteres) A ID do console.
* DeviceId - (cadeia de caracteres) A Id do dispositivo Xbox Live do console.
* SerialNumber - (cadeia de caracteres) o número de série do console.
* DevMode - (cadeia de caracteres) O modo de desenvolvedor atual do do console, como "Nenhum" ou "Vendas".
* ConsoleType - (cadeia de caracteres) O tipo do console, como "Xbox One" ou "Xbox One S".
* DevkitCertificateExpirationTime - (número) A hora UTC em segundos quando o certificado do kit de desenvolvedor do console expira.

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