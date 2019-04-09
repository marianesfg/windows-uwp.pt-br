---
title: Referência de API de carregamento da pasta do Device Portal
description: Saiba como acessar as APIs de carregamento da pasta de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 870d203271cb75ecf5531106bb2c10b3736db9b9
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244042"
---
# <a name="upload-a-folder-to-the-development-directory"></a>Carregar uma pasta para o diretório de desenvolvimento

**Solicitação**

É possível carregar uma pasta inteira de uma só vez na ID de pasta conhecida para DevelopmentFiles (ou em uma subpasta dentro dessa pasta).

Método      | URI da solicitação
:------     | :------
POSTAR | /api/app/packagemanager/upload 

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI      | Descrição
:------     | :-----
destinationFolder (obrigatória) | O nome da pasta de destino da pasta a ser carregada. Essa pasta será colocada em d:\developmentfiles\LooseApps no console. Este nome de pasta deverá ser codificado em base64 porque pode conter separadores de caminho se a pasta estiver em uma subpasta em LooseApps.


**Cabeçalhos de solicitação**

- Nenhuma

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

**Famílias de dispositivos disponíveis**

* Windows Xbox

