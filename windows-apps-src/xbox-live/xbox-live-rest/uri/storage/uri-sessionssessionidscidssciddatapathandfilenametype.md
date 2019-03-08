---
title: /sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}
assetID: f5e91763-0704-8526-77d4-c55dfec3b5a5
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapathandfilenametype.html
description: " /sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 402b4589a2812e34792622c253fb4b919d3f9c74
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599661"
---
# <a name="sessionssessionidscidssciddatapathandfilenametype"></a>/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}
Baixa um arquivo. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| sessionId| cadeia de caracteres| a ID da sessão para pesquisar.| 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| pathAndFileName| cadeia de caracteres| Nome de arquivo e caminho para o item a ser acessado. Caracteres válidos para a parte do caminho (até e incluindo a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e a barra (/). A parte do caminho pode estar vazia. Caracteres válidos para a parte do nome de arquivo (tudo após a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_), ponto (.) e hífen (-). O nome do arquivo não pode ficar vazio, terminar com um ponto ou conter dois pontos consecutivos.| 
| type| cadeia de caracteres| O formato dos dados. Os valores possíveis são json ou binário.| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})](uri-sessionssessionidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;Exclui um arquivo. 

[GET (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})](uri-sessionssessionidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Baixa um arquivo.

[PUT (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})](uri-sessionssessionidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;Carrega um arquivo. Os dados podem ser carregados em um carregamento completo no qual os dados e metadados são enviados em uma única mensagem, ou como um upload de vários bloco no qual os dados e metadados são enviados em uma série de blocos menores. Somente os arquivos que são menores do que quatro megabytes podem ser enviados como uma única mensagem. Não há suporte para upload de vários bloco de dados do tipo json. 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 

[URIs de armazenamento do título](atoc-reference-storagev2.md)

   