---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
description: " /handles/{handleId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a312d3744e96755a899d73307a47c01e3dc79fd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632631"
---
# <a name="handleshandleid"></a>/handles/{handleId}
Dá suporte a operações DELETE e GET para identificadores de sessão especificadas pelo identificador. 

> [!NOTE] 
> Esse URI é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele destina-se para uso com o modelo de contrato 104/105 ou posterior.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>Domínio
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | 
| handleId| GUID| A ID exclusiva do identificador para a sessão.| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;Exclui os identificadores especificados pela ID do identificador.

[GET (/handles/{handle-id})](uri-handleshandleidget.md)

&nbsp;&nbsp;Recupera os identificadores especificados pela ID do identificador.
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 

[URIs do diretório de sessão](atoc-reference-sessiondirectory.md)

   