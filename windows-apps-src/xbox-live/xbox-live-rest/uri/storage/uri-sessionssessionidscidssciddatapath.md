---
title: /sessions/{sessionId}/scids/{scid}/data/{path}
assetID: 932459b4-24b4-5b09-8146-ed214de0083a
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapath.html
description: " /sessions/{sessionId}/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1af8befe28c407948dfa03d706f476458bb77c14
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655071"
---
# <a name="sessionssessionidscidssciddatapath"></a>/sessions/{sessionId}/scids/{scid}/data/{path}
Lista informações de arquivo em um caminho especificado. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| sessionId| cadeia de caracteres| a ID da sessão para pesquisar.| 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| path| cadeia de caracteres| O caminho para os itens de dados para retornar. Todos os diretórios e subdiretórios correspondentes são retornados. Caracteres válidos incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e barra (/). Pode estar vazia. Comprimento máximo de 256.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-sessionssessionidscidssciddatapath-get.md)

&nbsp;&nbsp;Lista informações de arquivo em um caminho especificado.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de armazenamento do título](atoc-reference-storagev2.md)

   