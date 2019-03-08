---
title: ServiceError (JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
description: " ServiceError (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da3d682a1b66d25a12f21a93e9596d13afae7f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631701"
---
# <a name="serviceerror-json"></a>ServiceError (JSON)
Contém informações sobre o erro retornado quando uma chamada para o serviço falhou. 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
O objeto ServiceError tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| code| inteiro com sinal de 32 bits | O tipo de erro. Consulte a tabela abaixo para os valores possíveis. | 
| Código-fonte| cadeia de caracteres | O nome do serviço que gerou o erro. Por exemplo, um valor de <code>ReputationFD</code> indica que o erro foi no serviço de reputação. | 
| description| cadeia de caracteres| Uma descrição do erro. | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>Códigos de erro
 
| Valor| Descrição| 
| --- | --- | --- | --- | --- | 
| 0| Sucesso sem erro| 
| 4000| Documento de JSON a corpo de solicitação inválido enviado com uma validação de falha na solicitação POST. Consulte o campo de descrição para obter detalhes. | 
| 4100| Usuário faz não existe o XUID contido no URI da solicitação não representa um usuário válido no XBOX Live.| 
| 4500| Erro de autorização do chamador não está autorizado a executar a operação solicitada.| 
| 5000| Serviço de erro ocorreu um erro interno no serviço| 
| 5300| Serviço indisponível o serviço não está disponível.| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
    
```

  
<a id="ID4EZE"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E2E"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   