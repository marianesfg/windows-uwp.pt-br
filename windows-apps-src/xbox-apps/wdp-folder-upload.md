---
author: WilliamsJason
title: "Referência de API de carregamento da pasta do Device Portal"
description: "Saiba como acessar as APIs de carregamento da pasta de maneira programática."
ms.sourcegitcommit: fdc25fa4bd7bd5bfa598b993f23cd0ae9783dd0e
ms.openlocfilehash: 942ddc13b0deba382ad7758bc30bd9a5b0cceb11

---

# Carregar uma pasta para o diretório de desenvolvimento

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

- Nenhum

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




<!--HONumber=Jun16_HO4-->


