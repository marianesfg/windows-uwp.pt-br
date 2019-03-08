---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f9610b400e9530f86e264cea30bfdfdd1b09c8d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627991"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
Itens de acessos em uma lista. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| ID de usuário do Xbox (XUID).| 
| listtype| cadeia de caracteres| Tipo de lista (como ele é usado e como ele funciona). Sempre "FIXA" para esses métodos relacionados.| 
| ListName| cadeia de caracteres| Nome da lista (qual lista de um determinado listtype para agir em). Sempre "XBLPins" para itens de pinos.| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[DELETE](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;Remove itens de uma lista.

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;Retorna o conteúdo de uma lista.

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;Insere itens na lista no índice com base no parâmetro de cadeia de caracteres de consulta **insertIndex**.

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;Atualiza os itens em uma lista de acordo com os índices especificados para cada item no corpo da solicitação.
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   