---
title: /serviceconfigs/{scid}/hoppers/{hoppername}
assetID: ba1e129d-b4c4-6535-46ce-fd184465c85f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppername.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1db069f59eeba565a257531907d6be0b1dcd189d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598421"
---
# <a name="serviceconfigsscidhoppershoppername"></a>/serviceconfigs/{scid}/hoppers/{hoppername}

Dá suporte a uma operação POST para criar tíquetes de correspondência.

> [!IMPORTANT]
> Esse URI é destinado para uso com o contrato 103 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 103 ou posterior em cada solicitação.

<a id="ID4ER"></a>


## <a name="domain"></a>Domínio
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| O identificador do configuração de serviço (SCID) para a sessão.|
| hoppername | cadeia de caracteres | O nome do hopper. |

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>Métodos válidos

[POST (/serviceconfigs/{scid}/hoppers/{hoppername})](uri-serviceconfigsscidhoppershoppernamepost.md)

&nbsp;&nbsp;Cria o tíquete de correspondência especificadas.

<a id="ID4EFC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent  

[URIs de cruzamento](atoc-reference-matchtickets.md)
