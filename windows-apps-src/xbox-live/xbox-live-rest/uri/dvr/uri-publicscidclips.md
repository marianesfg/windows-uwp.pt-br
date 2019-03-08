---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
description: " /public/scids/{scid}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db279d546780ed40158894f73ecb84687ef35ba6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627971"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
Clipes de acesso públicos. Esse URI, na verdade, pode ser especificado em duas formas, `/public/scids/{scid}/clips` e `/public/titles/{titleId}/clips`. Veja abaixo para obter mais detalhes. O domínio para esse URI é `gameclipsmetadata.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| cadeia de caracteres| O identificador de configuração de serviço primário dos clipes de público.| 
| TitleID| cadeia de caracteres| TitleId dos clipes de público. Não pode ser especificado no URI mesmo como o SCID. Se for especificado, será usado para pesquisar o SCID primário.| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/public/scids/{scid}/clips)](uri-publicscidclipsget.md)

&nbsp;&nbsp;Lista de clipes públicos.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de Marketplace](../marketplace/atoc-reference-marketplace.md)

   