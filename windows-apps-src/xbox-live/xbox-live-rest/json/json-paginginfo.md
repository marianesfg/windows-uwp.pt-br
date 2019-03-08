---
title: PagingInfo (JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
description: " PagingInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0e773d73499e79fe23f736a536027932ca1a07b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651211"
---
# <a name="paginginfo-json"></a>PagingInfo (JSON)
Contém informações de paginação de resultados que são retornados em páginas de dados. 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| continuationToken| cadeia de caracteres| Um token de continuação opaco usado para acessar a próxima página de resultados. Máximo de 32 caracteres. Os chamadores podem fornecer esse valor na <b>continuationToken</b> parâmetro de consulta para recuperar o próximo conjunto de itens na coleção. Se essa propriedade estiver <b>nulo</b>, em seguida, não há nenhum item adicional na coleção. Essa propriedade é necessária e é fornecida, mesmo se a coleção está sendo paginada com <b>skipItems</b>.| 
| totalItems| inteiro com sinal de 32 bits| O número total de itens na coleção. Isso não for fornecido, se o serviço não puder fornecer uma exibição em tempo real para o tamanho da coleção.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   