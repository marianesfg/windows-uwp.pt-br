---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
description: " POST (/users/me/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a8973390ccbf5dd9980410f60f03a7edd78c134
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608781"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
Faça uma solicitação de carregamento inicial. Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EFB)
  * [Autorização](#ID4EQB)
  * [Cabeçalhos de solicitação necessários](#ID4EKC)
  * [Cabeçalhos de solicitação opcionais](#ID4ENE)
  * [Corpo da solicitação](#ID4ENF)
  * [Exemplo de solicitação](#ID4E1F)
  * [Códigos de status HTTP](#ID4EDG)
  * [Corpo da resposta](#ID4EVAAC)
  * [Resposta de exemplo](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Comentários
 
Isso é a primeira parte do processo de carregamento de clipe de jogo. Após a captura de um vídeo, é recomendável para chamar o serviço de GameClips imediatamente para obter a ID e o URI para o carregamento de bits, mesmo que o carregamento não está agendado para iniciar imediatamente. Essa chamada executará verificações de cota de usuário e outras verificações por meio do isolamento de conteúdo, privacidade, e assim por diante para ver se um vídeo ainda deve ser agendado para carregamento pelo cliente. Uma resposta positiva dessa chamada indica que o serviço está disposto a aceitar o clipe de vídeo para upload. Todos os clipes carregados devem ser associados um título específico (por meio de um SCID) para ser aceito no sistema.
 
Essa chamada não é idempotente; as chamadas subsequentes fará com que as IDs e URIs diferente a ser emitido. Novas tentativas em caso de falha devem seguir o comportamento padrão do lado do cliente retirada.
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| cadeia de caracteres| ID de configuração de serviço do recurso que está sendo acessado. Deve corresponder a SCID do usuário autenticado.| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>Autorização
 
As declarações a seguir são necessárias para este método:
 
   * xuid
   * DeviceType - deve ser o dispositivo para carregar
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| cadeia de caracteres| Codificações de compactação aceitável. Valores de exemplo: gzip, deflate, identidade.| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
O corpo da solicitação deve ser um [InitialUploadRequest](../../json/json-initialuploadrequest.md) objeto no formato JSON.
  
<a id="ID4E1F"></a>

 
## <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
   "clipNameStringId": "1193045",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
      
```

  
<a id="ID4EDG"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A sessão foi recuperada com êxito.| 
| 400| Solicitação Inválida| Ocorreu um erro no corpo da solicitação ou o usuário está acima da sua cota.| 
| 401| Não autorizado| Há um problema com o formato do token de autenticação na solicitação.| 
| 403| Proibido| Algumas necessárias declarações estão ausentes ou DeviceType não é.| 
| 503| Não aceitável| O serviço ou algumas dependências downstream estão inativos. Tente novamente com o comportamento padrão de retirada.| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
A resposta pode ser um [InitialUploadResponse](../../json/json-initialuploadresponse.md) objeto ou uma [ServiceErrorResponse](../../json/json-serviceerrorresponse.md) objeto no formato JSON.
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
   "clipName": "ClipName",
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752",  
   "titleName": "TitleName",
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
         
```

  
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   