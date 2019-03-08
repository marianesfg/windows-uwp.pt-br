---
title: POST (/users/me/resetreputation)
assetID: 1a4fed45-f608-dac2-3384-2ee493112f7b
permalink: en-us/docs/xboxlive/rest/uri-usersmeresetreputationpost.html
description: " POST (/users/me/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fc770673ac7e4034004f58032c7fa66cb28413e7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632681"
---
# <a name="post-usersmeresetreputation"></a>POST (/users/me/resetreputation)
Permite que a equipe de imposição definir as pontuações de reputação do usuário atual para alguns valores arbitrários após um sequestro de conta (por exemplo). O domínio para esses URIs é `reputation.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Autorização](#ID4E5)
  * [Cabeçalhos de solicitação necessários](#ID4ETB)
  * [Corpo da solicitação](#ID4END)
  * [Códigos de status HTTP](#ID4EDE)
  * [Corpo da resposta](#ID4EFH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Esse método também pode ser chamado por quaisquer outros parceiros para todas as áreas restritas, exceto de varejo e por usuários em todas as áreas restritas, exceto de varejo, para fins de teste. Observe que essa solicitação define um usuário pontuações de reputação de "base" e seus pesos comentários positivos serão todos serão zerados. A reputação real do usuário depois de fazer essa chamada será essas pontuações base mais seu bônus Embaixador mais seu bônus seguidores.
  
<a id="ID4E5"></a>

 
## <a name="authorization"></a>Autorização
 
Do parceiro: Para a área de restrita do varejo **PartnerClaim** da equipe de imposição; para todas as outras áreas restritas, **PartnerClaim**.
 
Do usuário: Para todas as áreas restritas, exceto de varejo, **XuidClaim** e **TitleClaim**.
  
<a id="ID4ETB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
De todos: **Content-Type: application/json**.
 
Do parceiro: **X-Xbl-contrato-Version** (a versão atual é 101), **Xbl-X-Sandbox**.
 
Do usuário: **X-Xbl-contrato-Version** (versão atual é 101).
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 101.| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 
O corpo da solicitação é uma simples [ResetReputation (JSON)](../../json/json-resetreputation.md) documento.
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK| OK.| 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 500| Erro Interno do Servidor| O servidor encontrou uma condição inesperada que o impediu de atender à solicitação.| 
| 503| Serviço Indisponível| A solicitação foi restringida, tente a solicitação novamente após o valor de repetição do cliente em segundos (por exemplo, 5 segundos mais tarde).| 
  
<a id="ID4EFH"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Em caso de sucesso, o corpo da resposta está vazio. Em caso de falha, uma [ServiceError (JSON)](../../json/json-serviceerror.md) documento é retornado.
 
<a id="ID4ERH"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4E2H"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E4H"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/resetreputation](uri-usersmeresetreputation.md)

   