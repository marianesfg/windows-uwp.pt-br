---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
description: " POST (/system/strings/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70e86567f449674c7a046e072437d9ee715dc6d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595501"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
Aceita uma matriz de cadeias de caracteres para validação e retorna uma matriz de resultados de tamanho igual. O domínio para esses URIs é `client-strings.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Cabeçalhos de solicitação necessários](#ID4EIB)
  * [Corpo da solicitação](#ID4ELC)
  * [Códigos de status HTTP](#ID4E4C)
  * [Corpo da resposta](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Cada resultado indica se a cadeia de caracteres correspondente é aceitável no Xbox LIVE e contém a cadeia de caracteres incorreta, se aplicável.
 
Cadeias de caracteres idênticas sempre dará resultados idênticos. Se você receber um resultado sem êxito, analise o resultado e modificar a cadeia de caracteres de acordo.
 
 

> [!NOTE] 
> Resultante <b>VerifyStringResult</b> reportará apenas a primeira palavra incorreta na cadeia de caracteres. Pode haver mais problemático palavras na cadeia de caracteres. Se você planeja substituir as palavras incorretas para tornar a cadeia de caracteres utilizável, você deve substituir a palavra incorreta ou a subcadeia de caracteres e, em seguida, verificar novamente a cadeia de caracteres para procurar subcadeias de caracteres incorretas adicionais.  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | 
| Autorização| Token de autenticação. Exemplo: XBL3.0 x = [hash]; token.| 
| x-xbl-contract-version| Versão do contrato de API de inteiro. Deve ser 1 ou 2 para esta API.| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
O corpo da solicitação é uma matriz de cadeias de caracteres com caracteres de 512 por cadeia de caracteres e não há limite para o tamanho da matriz.
 
<a id="ID4ETC"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
    "stringstoVerify":
    [
        "Poppycock",
        "The quick brown fox jumped over the lazy dog.",
        "Hey, want to hang out?",
        "oh noes"
    ]
}
      
```

   
<a id="ID4E4C"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK| Todas as cadeias de caracteres foram processadas com êxito. Isso não significa necessariamente todas as cadeias de caracteres tinham HResults positivo.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 406| Não aceitável| Faltando <b>Content-type: application/json</b> cabeçalho.| 
| 408| Tempo Limite da Solicitação| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Retorna uma matriz de [VerifyStringResult (JSON)](../../json/json-verifystringresult.md), do mesmo tamanho que a matriz de solicitação.
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>Parent 

[/system/strings/validate](uri-systemstringsvalidate.md)

   