---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}
assetID: ae6c6a25-2251-6ffd-ec58-e6c0576a34db
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8698eabc57577746d32f9ee439d6cd7af4820b5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617471"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersindex"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}
Dá suporte a uma operação de exclusão para remover o membro de sessão especificada.
<a id="ID4EO"></a>


## <a name="domain"></a>Domínio
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 do identificador de sessão.|
| sessionTemplateName| cadeia de caracteres| Nome da instância atual do modelo de sessão. Parte 2 do identificador de sessão.|
| sessionName| GUID| ID exclusiva da sessão. Parte 3 do identificador de sessão.|

<a id="ID4EDC"></a>


## <a name="valid-methods"></a>Métodos válidos

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindexdelete.md)

&nbsp;&nbsp;Remove os membros especificados de uma sessão.

<a id="ID4ENC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EPC"></a>


##### <a name="parent"></a>Parent

[URIs do diretório de sessão](atoc-reference-sessiondirectory.md)
