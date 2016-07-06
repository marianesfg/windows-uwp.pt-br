---
author: WilliamsJason
title: "Referência de API do registro de pasta flexível do Device Portal"
description: "Saiba como acessar as APIs do registro de pasta flexível de maneira programática."
ms.sourcegitcommit: ef0f1339b77a8d1f60a677b2ff19a63b68f0d6cd
ms.openlocfilehash: 41e4cc67120b9e32fac34404ca918edcf58ba267

---

# Registre um aplicativo em uma pasta flexível  

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

- Nenhum

**Corpo da solicitação**

- Nenhum

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




<!--HONumber=Jun16_HO4-->


