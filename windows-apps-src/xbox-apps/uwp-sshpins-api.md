---
title: Referência de API de marcas SSH do Portal de Dispositivos
description: Saiba como remover todas as marcas SSH confiáveis de maneira programática.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2c7dc6fab021c11c98276ee53af161bea25601a9
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874918"
---
# <a name="ssh-pins-api-reference"></a>Referência de API de marcas SSH
Você pode remover todas as marcas SSH confiáveis em seu kit de desenvolvimento usando essa API REST.

## <a name="remove-trusted-ssh-pins"></a>Remover marcas SSH confiáveis

**Solicitação**

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/app/sshpins
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
204 | A solicitação para apagar as marcas foi bem-sucedida.
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox

