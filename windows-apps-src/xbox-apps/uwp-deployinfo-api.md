---
title: Referência da API de informações de implantação do Device Portal
description: Saiba como acessar a API de informações de implantação de maneira programática.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 7543b41c6ee1d9c07f4540012f84dccc10bb4d76
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8876659"
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