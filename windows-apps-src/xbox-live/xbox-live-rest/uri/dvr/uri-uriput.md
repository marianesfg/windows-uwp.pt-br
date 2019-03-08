---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
description: " PUT (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 862b956e29222bb9d28f98510f13d42fd1a51b6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633111"
---
# <a name="put-uri"></a>PUT (/{uri})
Carrega dados de clipe de jogo.
Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.

  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EQB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ERC)
  * [Cabeçalhos de solicitação necessários](#ID4EBE)
  * [Cabeçalhos de solicitação opcionais](#ID4ENG)
  * [Corpo da solicitação](#ID4EWH)
  * [Códigos de status HTTP](#ID4ECAAC)
  * [Cabeçalhos de resposta necessária](#ID4EYEAC)
  * [Cabeçalhos de resposta opcional](#ID4ELHAC)
  * [Corpo da resposta](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Comentários

Após o **InitialUploadResponse** for retornado, o carregamento é realizado por meio de **uploadUri** retornado no objeto. O cliente deve dividir o arquivo em **expectedBlocks** blocos sequenciais, não pode exceder 2 MB. Eles podem ser carregados em paralelo.

Se você estiver carregando o arquivo em blocos, o servidor retornará um código de status HTTP de aceito (202) para cada solicitação, até que ele tenha recebido esperados todos os blocos, nesse caso, ele confirma a todos os blocos como um arquivo, retornando criado (201). Nesses casos, a resposta não contém um objeto e o servidor pode agendar o processamento adicional. Em erro, uma **ServiceErrorResponse** objeto pode ser retornado junto com um código de status HTTP apropriado.

Em um código de erro recuperável, o cliente deve repetir usando um mecanismo de repetição de retirada padrão.

> [!NOTE] 
> Mesmo se um carregamento for concluído com êxito, continuar o processamento ocorrerá que pode rejeitar o clipe por motivos não relacionado para o upload ou metadados complementação de processo.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| <b>uri</b>| cadeia de caracteres| O <b>uploadUri</b> campo dentro de <b>InitialUploadResponse</b> objeto.|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| inteiro sem sinal de 32 bits| Necessário se <b>expectedBlocks</b> está definido. Número de bloco indexado por zero determinando a ordenação de bloco no arquivo. Por exemplo, se <b>expectedBlocks</b> é 7, em seguida, <b>blockNum</b> pode ser de 0 a 6. |
| <b>uploadId</b>| cadeia de caracteres| Obrigatório. ID opaco na <b>GameClipsServiceUploadResponse</b> objeto.|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <b>Xauth=&lt;authtoken></b>|
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.|
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.|
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.|
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| cadeia de caracteres| Codificações de compactação aceitável. Valores de exemplo: gzip, deflate, identidade.|
| ETag| cadeia de caracteres| Usada para otimização de cache. Valor de exemplo: "686897696a7c876b7e".|

<a id="ID4EWH"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
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

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.|
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.|
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.|
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.|
| Retry-After| cadeia de caracteres| Instrui o cliente tente novamente mais tarde no caso de um servidor não disponível.|
| Variar| cadeia de caracteres| Instrui como armazenar em cache as respostas de proxies de downstream.|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| cadeia de caracteres| Usada para otimização de cache. Exemplo: "686897696a7c876b7e".|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>Corpo da resposta

Não há objetos são enviados no corpo da resposta.

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>Parent

[/{uri}](uri-uri.md)
