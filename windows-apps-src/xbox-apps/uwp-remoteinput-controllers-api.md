---
title: Referência da API de controles do Device Portal
description: Saiba como obter o número de controles físicos conectados e desativá-los de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8b5061f9193d78d4ff23f5fa707b0bea67a10f98
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8478391"
---
# <a name="controller-api-reference"></a>Referência de API do controle   
Você pode obter o número de controles físicos conectados e desativá-los de usando a API REST.

## <a name="determine-the-number-of-attached-physical-controllers"></a>Determinar o número de controles físicos conectados

**Solicitação**

Você pode verificar o número de controles físicos conectados no dispositivo usando a solicitação a seguir.

Método      | URI da solicitação
:------     | :-----
GET | /ext/remoteinput/controllers
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   

- Nenhum(a)

**Resposta**   

- A propriedade de número JSON ConnectedControllerCount que especifica o número de controles físicos conectados.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Êxito
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>Desconectar todos os controles físicos do devkit

**Solicitação**

Você pode desconectar todos os controles físicos no dispositivo usando a solicitação a seguir.

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/remoteinput/controllers
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   

- Nenhum(a)

**Resposta**   

- Nenhum(a) 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | A solicitação para desconectar os controles foi bem-sucedida.
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox
