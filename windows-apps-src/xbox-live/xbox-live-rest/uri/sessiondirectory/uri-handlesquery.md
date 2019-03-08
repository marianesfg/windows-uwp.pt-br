---
title: /handles/query
assetID: e00d31ad-b9ba-8e52-1333-83192eab0446
permalink: en-us/docs/xboxlive/rest/uri-handlesquery.html
description: " /handles/query"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eaa148972ce1e65056470a6c4082cb4e50de3f09
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598561"
---
# <a name="handlesquery"></a>/handles/query
Dá suporte a operações de POST para criar consultas para identificadores de sessão. 

> [!NOTE] 
> Esse URI é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele destina-se para uso com o modelo de contrato 104/105 ou posterior.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>Domínio
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
Esse URI dá suporte a consultas para identificadores. Ao contrário de consultas de sessão, que são consultas de cadeia de caracteres e o lote, consultas de identificador de usam um estilo de processador de consultas. Até 100 identificadores têm suporte.  
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
Nenhuma   
<a id="ID4EEB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST (/handles/query)](uri-handlesquerypost.md)

&nbsp;&nbsp;Cria consultas para identificadores de sessão.

[POST (/handles/query?include=relatedInfo)](uri-handlesqueryincludepost.md)

&nbsp;&nbsp;Cria consultas para identificadores de sessão que incluem informações de sessão relacionado.
 
<a id="ID4EQB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ESB"></a>

 
##### <a name="parent"></a>Parent 

[URIs do diretório de sessão](atoc-reference-sessiondirectory.md)

   