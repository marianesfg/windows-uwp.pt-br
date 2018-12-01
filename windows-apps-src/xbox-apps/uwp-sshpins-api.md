---
title: Referência de API de marcas SSH do Portal de Dispositivos
description: Saiba como remover todas as marcas SSH confiáveis de maneira programática.
ms.localizationpriority: medium
ms.openlocfilehash: 1ddf15d3cdb4089a8ef010a4ae46d247a06a10d7
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335517"
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

