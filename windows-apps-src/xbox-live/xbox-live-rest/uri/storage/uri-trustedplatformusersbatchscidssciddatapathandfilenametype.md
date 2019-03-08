---
title: /trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
assetID: c92d247a-5ea1-7a06-36db-7c67a1dc3151
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersbatchscidssciddatapathandfilenametype.html
description: " /trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 716632e355fd37512f6777f1b877f11280c689eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661661"
---
# <a name="trustedplatformusersbatchscidssciddatapathandfilenametype"></a>/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
Baixa vários arquivos de vários usuários com o mesmo nome de arquivo. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| pathAndFileName| cadeia de caracteres| Nome de arquivo e caminho para o item a ser acessado. Caracteres válidos para a parte do caminho (até e incluindo a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e a barra (/). A parte do caminho pode estar vazia. Caracteres válidos para a parte do nome de arquivo (tudo após a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_), ponto (.) e hífen (-). O nome do arquivo não pode ficar vazio, terminar com um ponto ou conter dois pontos consecutivos.| 
| type| cadeia de caracteres| O formato dos dados. Os valores possíveis são json ou binário.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST](uri-trustedplatformusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;Baixa vários arquivos de vários usuários com o mesmo nome de arquivo.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de armazenamento do título](atoc-reference-storagev2.md)

   