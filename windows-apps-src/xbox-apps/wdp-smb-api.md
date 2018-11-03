---
author: payzer
title: Referência de API SMB do Device Portal
description: Saiba como acessar as APIs SMB de maneira programática.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: 2a337fe722d73a08c1c75a84478fc31e5bdf6b03
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993414"
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
GET | /ext/smb/developerfolder
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

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
<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox
