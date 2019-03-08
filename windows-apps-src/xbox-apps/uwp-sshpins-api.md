---
title: Referência de API de marcas SSH do Portal de Dispositivos
description: Saiba como remover todas as marcas SSH confiáveis de maneira programática.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2c7dc6fab021c11c98276ee53af161bea25601a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663351"
---
# <a name="ssh-pins-api-reference"></a>Referência de API de marcas SSH
Você pode remover todas as marcas SSH confiáveis em seu kit de desenvolvimento usando essa API REST.

## <a name="remove-trusted-ssh-pins"></a>Remover marcas SSH confiáveis

**Solicitação**

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   

- Nenhuma

**Resposta**   

- Nenhuma 

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

