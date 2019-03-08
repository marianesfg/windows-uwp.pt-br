---
title: GET (/inventory/{itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
description: " GET (/inventory/{itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 446197eb20820304088ddac4a6379fa3b2510873
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606691"
---
# <a name="get-inventoryitemid"></a>GET (/inventory/{itemID})
Fornece o conjunto completo de detalhes para um item de estoque específico. O domínio para esses URIs é `inventory.xboxlive.com`.
 
  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EAB)
  * [Corpo da resposta](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Comentários
 
Nenhuma política de verificações, a imposição de, ou filtragem ocorrerá como parte dessa chamada.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| itemID| cadeia de caracteres| a ID exclusiva para cada usuário para um item de estoque no singular| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 
A resposta a uma solicitação GET, supondo que ele passa a autenticação e é atribuído o contexto de autorização apropriados, é um item de estoque único com o conjunto completo de propriedades do item.
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 

[GET (/inventory/{itemID})](uri-inventoryget.md)

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   