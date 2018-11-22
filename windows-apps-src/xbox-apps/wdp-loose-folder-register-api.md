---
author: WilliamsJason
title: Referência de API do registro de pasta flexível do Device Portal
description: Saiba como acessar as APIs do registro de pasta flexível de maneira programática.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: efdf4214-9738-4df6-bf1f-ed7141696ef6
ms.localizationpriority: medium
ms.openlocfilehash: cb80e2dbd7ebdfbb05bd642b9875a9cd7cc356f3
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7564349"
---
# <a name="register-an-app-in-a-loose-folder"></a>Registre um aplicativo em uma pasta flexível  

**Solicitação**

Você pode registrar um aplicativo em uma pasta flexível usando o formato de solicitação a seguir.

Método      | URI da solicitação
:------     | :------
POST | /api/app/packagemanager/register
<br />
**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI      | Descrição
:------     | :-----
pasta (obrigatória) | O nome da pasta de destino do pacote a ser registrado. Esta pasta deve existir em d:\developmentfiles\LooseApps no console. Este nome de pasta deverá ser codificado em base64 porque pode conter separadores de caminho se a pasta estiver em uma subpasta em LooseApps.
<br />

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Implantar solicitação aceita e em processamento
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox

**Observações**

Existem pelo menos três maneiras diferentes de colocar o aplicativo flexível no console na pasta desejada. A maneira mais fácil é simplesmente copiar os arquivos por meio do SMB para \\<IP_Address>\DevelopmentFiles\LooseApps. Isso exigirá um nome de usuário e uma senha em kits UWA que podem ser obtidos por meio de [/ext/smb/developerfolder](wdp-smb-api.md). 

A segunda maneira é copiando arquivos individuais para o local correto fazendo um POST em /api/filesystem/apps/file em que knownfolderid é DevelopmentFiles, packagefullname está vazio, além de nome de arquivo e caminho corretamente serem fornecidos (caminho deve começar com LooseApps).

A terceira maneira é copiar uma pasta inteira por vez por meio de [/api/app/packagemanager/upload](wdp-folder-upload.md) em que destinationFolder é o nome da pasta a ser colocada em d:\developmentfiles\looseapps e a carga útil é um corpo http em várias partes do conteúdo do diretório.

