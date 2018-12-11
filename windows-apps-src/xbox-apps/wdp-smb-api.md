---
title: Referência de API SMB do Device Portal
description: Saiba como acessar as APIs SMB de maneira programática.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: e248a6ff666efe7dca262daa81a21ab44a4dc5aa
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922941"
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
