---
title: GET (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: 2f52b487-4221-713b-a5a0-7ec85417698e
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-get.html
description: " GET (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a58a85a83da70edb4a0aaba26432ddee7e523c69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659941"
---
# <a name="get-untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>GET (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
Baixa um arquivo. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
  * [Autorização](#ID4ECB)
  * [Parâmetros de cadeia de caracteres de consulta opcionais](#ID4EPB)
  * [Cabeçalhos de solicitação necessários](#ID4EQC)
  * [Cabeçalhos de solicitação opcionais](#ID4EZD)
  * [Corpo da solicitação](#ID4EDF)
  * [Códigos de status HTTP](#ID4EQF)
  * [Cabeçalhos de resposta](#ID4EDDAC)
  * [Corpo da resposta](#ID4EGEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| O usuário Xbox ID (XUID) do player de quem está fazendo a solicitação.| 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| pathAndFileName| cadeia de caracteres| Nome de arquivo e caminho para o item a ser acessado. Caracteres válidos para a parte do caminho (até e incluindo a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e a barra (/). A parte do caminho pode estar vazia. Caracteres válidos para a parte do nome de arquivo (tudo após a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_), ponto (.) e hífen (-). O nome do arquivo não pode ficar vazio, terminar com um ponto ou conter dois pontos consecutivos.| 
| type| cadeia de caracteres| O formato dos dados. Os valores possíveis são json ou binário.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorização 
 
A solicitação deve incluir um cabeçalho de autorização válido Xbox LIVE. Se o chamador não tem permissão para acessar este recurso, o serviço retornará uma resposta 403 Proibido. Se o cabeçalho é inválido ou ausente, o serviço retornará uma resposta 401 não autorizado. 
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta opcionais 
 
Varia por tipo de blob. Blobs binários não dão suporte a parâmetros de consulta.
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| selecionar| cadeia de caracteres| Só pode ser usado quando o tipo é json. Especifica que a resposta deve conter apenas um determinado propriedade/valor do JSON, conforme determinado por esse parâmetro. Use um "ponto" (.) para especificar subpropriedades e colchetes ('[' e ']') para especificar os índices da matriz. Por exemplo, "array1 .prop2 [4]" Especifica a propriedade "prop2" de 4 do índice da matriz "array1".| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Versão do contrato de API.| 
| Autorização| XBL3.0 x = [hash]; [token]| Token de autenticação do STS. STSTokenString é substituído pelo token retornado pela solicitação de autenticação. Para obter mais informações sobre como recuperar um token STS e criar um cabeçalho de autorização, consulte Autenticando e autorizando Xbox LIVE as solicitações de serviços.| 
  
<a id="ID4EZD"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Especifica um ETag que deve corresponder a um item existente para concluir a operação.| 
| If-None-Match| Especifica um ETag que não deve corresponder a todos os itens existentes para concluir a operação.| 
| Intervalo| Especifica o intervalo de bytes para baixar. Segue o formato do cabeçalho HTTP intervalo padrão.| 
  
<a id="ID4EDF"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EQF"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP 
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK | A solicitação foi bem-sucedida.| 
| 201| Criado | A entidade foi criada.| 
| 400| Solicitação Inválida | Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado | A solicitação exige autenticação do usuário.| 
| 403| Proibido | A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado | O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável | Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação | A solicitação demorou muito para ser concluída.| 
| 500| Erro Interno do Servidor | O servidor encontrou uma condição inesperada que o impediu de atender à solicitação.| 
| 503| Serviço Indisponível | A solicitação foi restringida, tente a solicitação novamente após o valor de repetição do cliente em segundos (por exemplo, 5 segundos mais tarde).| 
  
<a id="ID4EDDAC"></a>

 
## <a name="response-headers"></a>Cabeçalhos de Resposta
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag é um identificador opaco atribuído por um servidor web para uma versão específica de um recurso encontrado em uma URL. Se o conteúdo de recurso nessa URL for alterado, é atribuído um ETag nova e diferente.| 
| Content-Range| Se esse foi um download parcial, esse cabeçalho Especifica o intervalo de bytes baixados.| 
  
<a id="ID4EGEAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Se a chamada for bem-sucedida, o serviço retornará o conteúdo do arquivo.
  
<a id="ID4EREAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ETEAC"></a>

 
##### <a name="parent"></a>Parent  

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

  
<a id="ID4E6EAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Referência [TitleBlob (JSON)](../../json/json-titleblob.md)

   