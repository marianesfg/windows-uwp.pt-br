---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
assetID: 4f8e1ece-2ba8-9ea4-e551-2a69c499d7b9
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93ecaa95488493fa8779a44d76bf7d635d1751ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612591"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
Dá suporte a uma operação POST para criar uma consulta em lotes no nível de modelo da sessão.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

<a id="ID4ER"></a>


## <a name="domain"></a>Domínio
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 do identificador de sessão.|
| sessionTemplateName| cadeia de caracteres| Nome da instância atual do modelo de sessão. Parte 2 do identificador de sessão.|

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>Métodos válidos

[POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.md)

&nbsp;&nbsp;Cria uma consulta em lotes em vários IDs de usuário do Xbox.

<a id="ID4EFC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent

[URIs do diretório de sessão](atoc-reference-sessiondirectory.md)
