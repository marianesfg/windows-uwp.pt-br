---
title: Referência de API da área restrita do Xbox Live do Device Portal
description: Saiba como acessar a área restrita do Xbox Live de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: 8f04514962cf0684daa99ee75d4c4da73c785735
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244082"
---
# <a name="xbox-live-sandbox-api-reference"></a>Referência de API da área restrita do Xbox Live   
Você pode obter e definir a área restrita do Xbox Live usando essa API REST.

## <a name="get-the-xbox-live-sandbox"></a>Obter a área restrita do Xbox Live

**Solicitação**

Você pode ler o valor atual da área restrita do Xbox Live do dispositivo usando a seguinte solicitação:

Método      | URI da solicitação
:------     | :-----
OBTER | /ext/xboxlive/sandbox

**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**   
Sandbox – (sequência) a área restrita atual na qual o dispositivo está.   

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="set-the-xbox-live-sandbox"></a>Definir a área restrita do Xbox Live
Você pode alterar a área restrita do Xbox Live do dispositivo usando a solicitação a seguir. No Xbox One, o dispositivo precisa ser reiniciado antes da configuração entrar em vigor.

**Solicitação**

Você pode definir o valor atual da área restrita do Xbox Live do dispositivo usando a seguinte solicitação:

Método      | URI da solicitação
:------     | :-----
PUT | /ext/xboxlive/sandbox

**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   
O corpo da solicitação é um objeto JSON contendo o seguinte campo:   
Sandbox – (sequência) o novo valor para definir a área restrita do dispositivo.

**Resposta**   
Sandbox – (sequência) a área restrita atual na qual o dispositivo está.   

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

**Famílias de dispositivos disponíveis**

* Windows Xbox

