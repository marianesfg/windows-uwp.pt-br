---
author: WilliamsJason
title: Referência de API de carregamento da pasta do Device Portal
description: Saiba como acessar as APIs de carregamento da pasta de maneira programática.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 481ec666c327b15088d8e60577c51fa1697918fe
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6661346"
---
# <a name="upload-a-folder-to-the-development-directory"></a>Carregar uma pasta para o diretório de desenvolvimento

**Solicitação**

É possível carregar uma pasta inteira de uma só vez na ID de pasta conhecida para DevelopmentFiles (ou em uma subpasta dentro dessa pasta).

Método      | URI da solicitação
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI      | Descrição
:------     | :-----
destinationFolder (obrigatória) | O nome da pasta de destino da pasta a ser carregada. Essa pasta será colocada em d:\developmentfiles\LooseApps no console. Este nome de pasta deverá ser codificado em base64 porque pode conter separadores de caminho se a pasta estiver em uma subpasta em LooseApps.
<br />

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- várias partes de acordo com o corpo http do conteúdo do diretório.

**Resposta**

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

