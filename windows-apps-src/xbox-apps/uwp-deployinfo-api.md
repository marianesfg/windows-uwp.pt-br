---
title: Referência da API de informações de implantação do Device Portal
description: Saiba como acessar a API de informações de implantação de maneira programática.
ms.localizationpriority: medium
ms.openlocfilehash: c44089313b100880b419e9b55a26101e877496f3
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7709848"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Solicita informações de implantação para um ou mais pacotes instalados.

**Solicitação**

Método      | URI da solicitação
:------     | :------
POST | /ext/app/deployinfo
<br />
**Parâmetros do URI**

 - Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

Uma matriz JSON no formato a seguir:

* DeployInfo
  * PackageFullName - nome do pacote sobre o qual estamos solicitando informações.
  * OverlayFolder: caminho opcional para um caminho de pasta de sobreposição se estiver usando esse recurso.

###<a name="response"></a>Resposta

**Corpo da resposta**

Uma matriz JSON no formato a seguir (alguns campos são opcionais):

* DeployInfo
  * PackageFullName - nome do pacote sobre o qual estamos recebendo informações.
  * DeployType - o tipo de implantação.
  * DeployPathOrSpecifiers - um caminho de implantação para implantações flexíveis ou especificadores instalados para implantações empacotadas.
  * DeployDrive: a unidade em que o pacote é implantado para tipos de implantação aplicáveis.
  * DeploySizeInBytes: o tamanho em bytes do pacote para tipos de implantação aplicáveis.
  * OverlayFolder: a pasta de sobreposição para implantações que oferecem suporte a esse recurso.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Êxito
4XX | Códigos de erro
5XX | Códigos de erro
<br />

**Famílias de dispositivos disponíveis**

* Windows Xbox