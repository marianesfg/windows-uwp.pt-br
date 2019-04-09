---
title: Referência de API SMB do Device Portal
description: Saiba como acessar as APIs SMB de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: a1040ec91af767d9472842b5ba656d347e7782d0
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244062"
---
# <a name="developer-folder-api-reference"></a>Pasta de API da pasta do desenvolvedor

Você pode acessar arquivos relacionados ao desenvolvimento no Xbox One usando um Explorador de Arquivos padrão. Isso permite exibir facilmente e substituir arquivos do computador para o console.

**Solicitação**

Você pode acessar a pasta de desenvolvedor usando a solicitação a seguir. A solicitação retornará:

* O local do compartilhamento de arquivos. Esse local pode ser inserido na barra de endereços em um Explorador de Arquivos.
* O nome de usuário para acessar o compartilhamento de arquivos.
* A senha para acessar o compartilhamento de arquivos.

Método      | URI da solicitação
:------     | :-----
OBTER | /ext/smb/developerfolder

**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**   
Path – o caminho do compartilhamento do desenvolvedor de arquivos.   
Username – o nome do usuário necessário para acessar o compartilhamento de arquivos do desenvolvedor.   
Password – a senha necessária para acessar o compartilhamento de arquivos do desenvolvedor.   

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

**Famílias de dispositivos disponíveis**

* Windows Xbox
