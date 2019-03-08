---
title: Cabeçalhos de solicitação e resposta HTTP padrão
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " Cabeçalhos de solicitação e resposta HTTP padrão"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca97e82365eab40266b3ffdd84924f71289eede6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636511"
---
# <a name="standard-http-request-and-response-headers"></a>Cabeçalhos de solicitação e resposta HTTP padrão
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>Cabeçalhos de solicitação
 
A tabela a seguir lista os cabeçalhos HTTP padrão usados ao fazer solicitações de serviços do Xbox Live.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | 
| x-xbl-contract-version| 1| Versão do contrato de API. Necessário em todas as solicitações de serviços do Xbox Live.| 
| Autorização| STSTokenString| Token de autenticação do STS. O valor para esse cabeçalho é recuperado do <b>GetTokenAndSignatureResult.Token</b> propriedade. | 
| Content-Type| aplicativo/xml, application/json, com diversas partes/dados de formulário ou application/x-www-form-urlencoded| Especifica o tipo de conteúdo que está sendo enviado com uma solicitação.| 
| Content-Length| Valor inteiro| Especifica o comprimento dos dados que estão sendo enviados em uma solicitação POST.| 
| Accept-Language | String| Especifica como localizar qualquer cadeia de caracteres retornada. Ver <a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">Advanced programação de 360 Xbox</a> para obter uma lista de combinações de idioma/localidade válido.| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>Cabeçalhos de Resposta
 
A tabela a seguir lista o cabeçalho HTTP padrão usado em respostas de serviços do Xbox Live.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/xml, application/json| Especifica o tipo de conteúdo que está sendo retornado.| 
| Content-Length| Valor inteiro| Especifica o comprimento dos dados que está sendo retornados.| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)

   