---
author: payzer
title: "Referência de API SMB do Device Portal"
description: "Saiba como acessar as APIs SMB de maneira programática."
translationtype: Human Translation
ms.sourcegitcommit: 3d76bf181baa9dfd973467d43241230fddf2daf7
ms.openlocfilehash: d6a097c84e6b967c41507d9e60c266f0bacb93fe

---

# Pasta de API da pasta do desenvolvedor   
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

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

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



<!--HONumber=Aug16_HO3-->


