---
title: Referência de API do registro de pasta flexível do Device Portal
description: Saiba como acessar as APIs do registro de pasta flexível de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: efdf4214-9738-4df6-bf1f-ed7141696ef6
ms.localizationpriority: medium
ms.openlocfilehash: 6e1a6d2e0c408d37195f5a4764f71c2acc932ab5
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240004"
---
# <a name="register-an-app-in-a-loose-folder"></a>Registre um aplicativo em uma pasta flexível  

**Solicitação**

Você pode registrar um aplicativo em uma pasta flexível usando o formato de solicitação a seguir.

Método      | URI da solicitação
:------     | :------
POSTAR | /api/app/packagemanager/register

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI      | Descrição
:------     | :-----
pasta (obrigatória) | O nome da pasta de destino do pacote a ser registrado. Esta pasta deve existir em d:\developmentfiles\LooseApps no console. Este nome de pasta deverá ser codificado em base64 porque pode conter separadores de caminho se a pasta estiver em uma subpasta em LooseApps.

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Implantar solicitação aceita e em processamento
4XX | Códigos de erro
5XX | Códigos de erro

**Famílias de dispositivos disponíveis**

* Windows Xbox

**Observações**

Existem pelo menos três maneiras diferentes de colocar o aplicativo flexível no console na pasta desejada. É a maneira mais fácil simplesmente copiar os arquivos via SMB para \\\DevelopmentFiles\LooseApps < endereço_IP >. Isso exigirá um nome de usuário e uma senha em kits UWA que podem ser obtidos por meio de [/ext/smb/developerfolder](wdp-smb-api.md). 

A segunda maneira é copiando arquivos individuais para o local correto fazendo um POST em /api/filesystem/apps/file em que knownfolderid é DevelopmentFiles, packagefullname está vazio, além de nome de arquivo e caminho corretamente serem fornecidos (caminho deve começar com LooseApps).

A terceira maneira é copiar uma pasta inteira por vez por meio de [/api/app/packagemanager/upload](wdp-folder-upload.md) em que destinationFolder é o nome da pasta a ser colocada em d:\developmentfiles\looseapps e a carga útil é um corpo http em várias partes do conteúdo do diretório.

