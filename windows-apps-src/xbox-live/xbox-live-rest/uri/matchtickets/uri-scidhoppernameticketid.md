---
title: /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9722d06af64c5fd24f53485a1bcfe765f89b08cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627311"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}

Dá suporte a uma operação de exclusão para um tíquete de correspondência.

> [!IMPORTANT]
> Esse URI é destinado para uso com o contrato 103 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 103 ou posterior em cada solicitação.

<a id="ID4ER"></a>


## <a name="domain"></a>Domínio
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Comentários
Esse URI dá suporte a xuid os valores, gt e me para o identificador do proprietário na configuração do usuário de destino.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| O identificador do configuração de serviço (SCID) para a sessão.|
| name| cadeia de caracteres| O nome do hopper.|
| ticketId| GUID| A ID do tíquete.|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>Métodos válidos

[DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;Remove um tíquete de correspondência.

<a id="ID4ETC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent  

[URIs de cruzamento](atoc-reference-matchtickets.md)
