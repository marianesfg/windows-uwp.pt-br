---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5d2858b11bf8fb902ea07a016c8f41db375013f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640951"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
Obtenha um clipe de jogo único do sistema se todas as IDs para localizá-lo são conhecidas. Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EVB)
  * [Autorização](#ID4EAC)
  * [Cabeçalhos de solicitação necessários](#ID4EUH)
  * [Cabeçalhos de solicitação opcionais](#ID4EBCAC)
  * [Corpo da solicitação](#ID4ETDAC)
  * [Códigos de status HTTP](#ID4E5DAC)
  * [Cabeçalhos de resposta necessária](#ID4EQIAC)
  * [Cabeçalhos de resposta opcional](#ID4EJLAC)
  * [Corpo da resposta](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Comentários
 
Todos os dados de consulta para o clipe está disponível de **clipe de jogo** objetos conforme retornado de qualquer consulta de metadados. **XUID**, **ServiceConfigId**, **GameClipId** e **SandboxId** nas declarações da solicitação são necessárias para fazer um clipe de jogo único por meio dessa API. Se o jogo clipe foi sinalizado para imposição ou isolamento de conteúdo ou de privacidade verifica determinar que o usuário não tem permissão para obter o clipe de jogo específico, a API retornará um código de status HTTP de 404 (não encontrado).
 
**SandboxId** agora é recuperado da declaração no XToken e impostas. Se o **SandboxId** não estiver presente, em seguida, os serviços de descoberta de entretenimento (EDS) gerará um erro 400 Solicitação incorreta.
 
Essa API também deve ser usado para atualizar os URIs expirada. Quando a consulta for concluída, os URIs expirada para o clipe de jogo será atualizado adequadamente. 

> [!NOTE] 
> Uma atualização URI pode levar até 30 a 40 segundos depois que essa solicitação é feita. Se o URI tiver expirado, a tentativa de usá-lo imediatamente para operações de streaming para obter um código de status HTTP 500 do IIS Smooth Streaming Servers. Estamos trabalhando em maneiras de reduzir isso, e essa observação será atualizada adequadamente como esse trabalho em andamento. 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | 
| ownerId| cadeia de caracteres| Identidade de usuário do usuário cujo recurso está sendo acessado. Formatos com suporte: "me" ou "xuid(123456789)". Comprimento máximo: 16.| 
| scid| cadeia de caracteres| ID de configuração de serviço do recurso que está sendo acessado. Deve corresponder a SCID do usuário autenticado.| 
| gameClipId| cadeia de caracteres| ID do clipe de jogo do recurso que está sendo acessado.| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>Autorização
 
Declarações de autorização usadas | Declaração| Tipo| Necessário?| Valor de exemplo| Comentários| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xuid| inteiro com sinal de 64 bits| sim| 1234567890|  | 
| TitleId| inteiro com sinal de 64 bits| sim| 1234567890| Usado para <b>isolamento de conteúdo</b> verificar.| 
| sandboxId| binário hexadecimal| sim|  | Direciona o sistema para a área correta para pesquisas e usado para <b>isolamento de conteúdo</b> verificar.| 
  
Efeito das configurações de privacidade no recurso | Usuário solicitante| Configuração de privacidade do usuário de destino| Comportamento| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Conforme descrito.| 
| Friend| Todas as pessoas| Proibido.| 
| Friend| somente os amigos| Proibido.| 
| Friend| bloqueado| Proibido.| 
| usuário não friend| Todas as pessoas| Proibido.| 
| usuário não friend| somente os amigos| Proibido.| 
| usuário não friend| bloqueado| Proibido.| 
| site de terceiros| Todas as pessoas| Proibido.| 
| site de terceiros| somente os amigos| Proibido.| 
| site de terceiros| bloqueado| Proibido.| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| cadeia de caracteres| Codificações de compactação aceitável. Valores de exemplo: gzip, deflate, identidade.| 
| ETag| cadeia de caracteres| Usada para otimização de cache. Valor de exemplo: "686897696a7c876b7e".| 
| Intervalo| cadeia de caracteres|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A sessão foi recuperada com êxito.| 
| 301| Movido permanentemente|  | 
| 307| Redirecionamento temporário|  | 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável| Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação| A solicitação demorou muito para ser concluída.| 
| 410| Passou| O recurso solicitado não está mais disponível.| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
| Retry-After| cadeia de caracteres| Instrui o cliente tente novamente mais tarde no caso de um servidor não disponível. Exemplo: <b>application/json</b>.| 
| Variar| cadeia de caracteres| Instrui como armazenar em cache as respostas de proxies de downstream. Exemplo: <b>application/json</b>.| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| cadeia de caracteres| Usada para otimização de cache. Valor de exemplo: "686897696a7c876b7e".| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
 "gameClip": {
   "xuid": "2716903703773872",
   "clipName": "1234567890",
   "titleName": "",
   "gameClipId": "cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
   "state": "Published",
   "dateRecorded": "2013-05-08T21:32:17.4201279Z",
   "lastModified": "2013-05-08T21:34:48.8117829Z",
   "userCaption": "",
   "type": "DeveloperInitiated",
   "source": "Console",
   "visibility": "Public",
   "durationInSeconds": 30,
   "scid": "00000000-0000-0012-0023-000000000070",
   "titleId": 0,
   "rating": 0,
   "ratingCount": 0,
   "views": 0,
   "titleData": "",
   "systemProperties": "",
   "savedByUser": false,
   "thumbnails": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/large",
       "fileSize": 0,
       "thumbnailType": "Large"
     },
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/small",
       "fileSize": 0,
       "thumbnailType": "Small"
     }
   ],
   "gameClipUris": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
       "fileSize": 9332015,
       "uriType": "Download",
       "expiration": "9999-12-31T23:59:59.9999999"
     }
   ]
 }
}
         
```

   
<a id="ID4EZMAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E2MAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[URIs de Marketplace](../marketplace/atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   