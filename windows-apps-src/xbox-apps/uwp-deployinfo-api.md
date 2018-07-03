---
author: WilliamsJason
title: Referência da API de informações de implantação do Device Portal
description: Saiba como acessar a API de informações de implantação de maneira programática.
ms.localizationpriority: medium
ms.openlocfilehash: c0e8c6ea8fb42c6e11de8002da4b6c78d35e675b
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921164"
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