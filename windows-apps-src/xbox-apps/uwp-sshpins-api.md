---
author: WilliamsJason
title: Referência de API de marcas SSH do Portal de Dispositivos
description: Saiba como remover todas as marcas SSH confiáveis de maneira programática.
ms.localizationpriority: medium
ms.openlocfilehash: 88ba9d3e35650c8c581b9ddb76911636fc18c72e
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921215"
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

