---
title: /handles/{handleId}/session
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
description: " /handles/{handleId}/session"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7b6990917437c22dd4d9282492e2a0eab37893b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628141"
---
# <a name="handleshandleidsession"></a>/handles/{handleId}/session
Dá suporte a operações PUT e GET para uma sessão, usando o identificador de referência. 

> [!NOTE] 
> Esse URI é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele destina-se para uso com o modelo de contrato 104/105 ou posterior.  

 

> [!NOTE] 
> Esse URI atualmente só é acessível externamente, consoles Xbox One e servidores usando um identificador de serviço.  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>Domínio
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| A ID exclusiva do identificador para a sessão.| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/handles/{handleId}/session)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;Obtém um objeto de sessão para o identificador do identificador especificado. 

[PUT (/handles/{handle-id}/session)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;Cria ou atualiza uma sessão ao desreferenciar um identificador.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[URIs do diretório de sessão](atoc-reference-sessiondirectory.md)

   