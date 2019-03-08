---
title: GET (/public/scids/{scid}/clips)
assetID: 15b3e873-1f96-b1da-2f79-6dac1369a4c0
permalink: en-us/docs/xboxlive/rest/uri-publicscidclipsget.html
description: " GET (/public/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5bce1dd261e0ad1172068a0287519cd0480da85f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589801"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
Lista de clipes públicos. O domínio para esse URI é `gameclipsmetadata.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4ECB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Essa API permite várias maneiras de fragmentos de lista que são públicos. A lista de clipes é retornada com base em verificações de privacidade e as verificações de isolamento de conteúdo em relação a XUID solicitante.
 
As consultas são otimizadas por identificador de configuração de serviço (SCID). Especificar ainda mais filtros ou ordens de classificação diferentes dos padrões listados abaixo em algumas circunstâncias demore mais tempo para retornar. Isso é mais evidente para grandes conjuntos de vídeos. Consultas não é possível especificar uma ordem de classificação crescente.
 
O qualificador é necessária para acessar os clipes de ofpublic coleção específica. O usuário solicitante deve ter acesso ao SCID solicitada, caso contrário, HTTP 403 será retornado.
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| cadeia de caracteres| O identificador de configuração de serviço primário dos clipes de público.| 
| TitleID| cadeia de caracteres| TitleId dos clipes de público. Não pode ser especificado no URI mesmo como o SCID. Se for especificado, será usado para pesquisar o SCID primário.| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| <b>?achievementId={achievementId}</b>| Clipes mais recentes de correspondência especificado <b>achievementId</b>.| Não há suporte para a classificação/filtragem adicional.| 
| <b>?greatestMomentId={greatestMomentId}</b>| Clipes mais recentes de correspondência especificado <b>greatestMomentId</b>.| Não há suporte para a classificação/filtragem adicional.| 
| <b>? qualificador = criado </b>| Mais recente| Obrigatório.| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   