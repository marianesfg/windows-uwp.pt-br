---
title: /global/scids/{scid}/data/{pathAndFileName},{type}
assetID: 774ce2dc-15c5-fe12-42b9-4e040bd4d2cf
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapathandfilenametype.html
description: " /global/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8c58ee4888cbbe7d9a752531c489b1da3fdde86
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656231"
---
# <a name="globalscidssciddatapathandfilenametype"></a>/global/scids/{scid}/data/{pathAndFileName},{type}
Baixa um arquivo. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| pathAndFileName| cadeia de caracteres| Nome de arquivo e caminho para o item a ser acessado. Caracteres válidos para a parte do caminho (até e incluindo a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e a barra (/). A parte do caminho pode estar vazia. Caracteres válidos para a parte do nome de arquivo (tudo após a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_), ponto (.) e hífen (-). O nome do arquivo não pode ficar vazio, terminar com um ponto ou conter dois pontos consecutivos.| 
| type| cadeia de caracteres| O formato dos dados. Os valores possíveis são: binário, configuração ou json.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-globalscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Baixa um arquivo.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de armazenamento do título](atoc-reference-storagev2.md)

   