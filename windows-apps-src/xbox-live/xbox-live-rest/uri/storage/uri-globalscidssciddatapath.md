---
title: /global/scids/{scid}/data/{path}
assetID: d6353cd3-9127-98d4-bb99-5df690e07022
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapath.html
description: " /global/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b8788ae6773c53c2f86b3f51ee9023876416feeb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618861"
---
# <a name="globalscidssciddatapath"></a>/global/scids/{scid}/data/{path}
Lista informações de arquivo em um caminho especificado. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| path| cadeia de caracteres| O caminho para os itens de dados para retornar. Todos os diretórios e subdiretórios correspondentes são retornados. Caracteres válidos incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e barra (/). Pode estar vazia. Comprimento máximo de 256.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-globalscidssciddatapath-get.md)

&nbsp;&nbsp;Lista informações de arquivo em um caminho especificado.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de armazenamento do título](atoc-reference-storagev2.md)

   