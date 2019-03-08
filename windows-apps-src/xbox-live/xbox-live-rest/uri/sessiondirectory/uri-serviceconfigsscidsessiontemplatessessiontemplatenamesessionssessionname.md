---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
assetID: 55ce6459-1714-49bc-6231-b547ddf04143
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da8d61634d0a849d51e2ab6ff143469ec05cc75c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657471"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
Dá suporte a operações PUT e GET para criar e recuperar as sessões.
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

<a id="ID4EBC"></a>


## <a name="valid-methods"></a>Métodos válidos

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.md)

&nbsp;&nbsp;Obtém um objeto de sessão.

[PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

&nbsp;&nbsp;Cria, atualiza ou ingressa em uma sessão.

<a id="ID4EOC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent

[URIs do diretório de sessão](atoc-reference-sessiondirectory.md)
