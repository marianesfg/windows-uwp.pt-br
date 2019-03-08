---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04376505b76296e052ea431f2a4e5fcfeac7b9e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613931"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

Dá suporte a uma operação GET para recuperar as estatísticas para um hopper.

> [!IMPORTANT]
> Esse URI é destinado para uso com o contrato 103 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 103 ou posterior em cada solicitação.

<a id="ID4ER"></a>


## <a name="domain"></a>Domínio
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Comentários
Esse URI dá suporte a xuid os valores, gt e me para o identificador do proprietário na configuração do usuário de destino. Somente o criador de um tíquete pode excluir o tíquete ou recuperar o status do URI.  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| O identificador do configuração de serviço (SCID) para a sessão.|
| name| cadeia de caracteres| O nome do hopper.|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>Métodos válidos

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;Obtém as estatísticas para um hopper.

<a id="ID4EQC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ESC"></a>


##### <a name="parent"></a>Parent  

[URIs de cruzamento](atoc-reference-matchtickets.md)
