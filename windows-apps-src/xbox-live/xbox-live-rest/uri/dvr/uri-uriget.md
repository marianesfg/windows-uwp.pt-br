---
title: GET (/{uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
description: " GET (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 757d84c9ad5a005e042b42d699ada08504dc57ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650611"
---
# <a name="get-uri"></a>GET (/{uri})
Baixe o clipe de jogo. Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EDB)
  * [Cabeçalhos de solicitação necessários](#ID4EEC)
  * [Cabeçalhos de solicitação opcionais](#ID4EQE)
  * [Corpo da solicitação](#ID4EZF)
  * [Cabeçalhos de resposta necessária](#ID4EEG)
  * [Códigos de status HTTP](#ID4EYAAC)
  * [Cabeçalhos de resposta opcional](#ID4EOFAC)
  * [Corpo da resposta](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Comentários
 
O cliente pode baixar qualquer clipe ou miniatura que atingiu o estado publicado e é do tipo que pode ser baixado, conforme especificado em uma **GameClipUri** objeto. O URI de solicitação do arquivo está incluído no corpo da resposta ao recuperar uma lista de clipes públicas ou usuário.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| <b>uri</b>| cadeia de caracteres| O <b>uri</b> campo dentro de <b>GameClipUri</b> objeto.| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| cadeia de caracteres| Codificações de compactação aceitável. Valores de exemplo: gzip, deflate, identidade.| 
| ETag| cadeia de caracteres| Usada para otimização de cache. Valor de exemplo: "686897696a7c876b7e".| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Retry-After| cadeia de caracteres| Instrui o cliente tente novamente mais tarde no caso de um servidor não disponível.| 
| Variar| cadeia de caracteres| Instrui como armazenar em cache as respostas de proxies de downstream.| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A sessão foi recuperada com êxito.| 
| 301| Movido permanentemente| O serviço foi movido para um URI diferente.| 
| 307| Redirecionamento temporário| O serviço foi movido para um URI diferente.| 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável| Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação| A solicitação demorou muito para ser concluída.| 
| 410| Passou| O recurso solicitado não está mais disponível.| 
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| cadeia de caracteres| Usada para otimização de cache. Exemplo: "686897696a7c876b7e".| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EUGAC"></a>

  
 
Se for bem-sucedido, o servidor retornará o clipe de vídeo, possivelmente truncado acordo com o cabeçalho de solicitação de intervalo. Para um clipe truncado, a resposta será conteúdo parcial (206). Se o servidor retornará o arquivo inteiro, ele responderá Okey (200). Em erro, uma **GameClipsServiceErrorResponse** objeto pode ser retornado junto com um código de status HTTP apropriado (por exemplo, 416, intervalo solicitado não satisfatório).
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>Parent 

[/{uri}](uri-uri.md)

   