---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b56b563c72c206340aa2c1ce9f73aa8dfe50809d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641221"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Move um item dentro de uma lista. O domínio para esses URIs é `eplists.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| XUID| cadeia de caracteres| XUID do usuário.| 
| ListName| cadeia de caracteres| Nome da lista para manipular.| 
| Índice| cadeia de caracteres| Especifica o índice atual do item a ser movido. Se o valor de índice é zero ou um número inteiro positivo, isso se refere ao índice do item atual e o corpo da solicitação da chamada deve estar vazio. No entanto, se o valor de índice for "-1", o item a ser movido deve ser especificado por ItemId ou provedor/ProviderID no corpo da solicitação da chamada. | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;Move um item em uma lista para uma posição diferente na lista.
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   