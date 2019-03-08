---
title: POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
assetID: fb4cff17-2721-89c5-6646-5ab76952b411
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype-post.html
description: " POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cfe20136ad1320966fc851cf9dd6de765871c957
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623291"
---
# <a name="post-jsonusersbatchscidssciddatapathandfilenamejson"></a>POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
Baixa vários arquivos de vários usuários com o mesmo nome de arquivo. O arquivo a ser baixado é determinado pelo URI da solicitação. O corpo da solicitação contém a lista de XUIDs de usuários cujos arquivos para download. O corpo da resposta será uma mensagem MIME de várias parte, com cada parte que representa um arquivo para um usuário específico com seu próprio conjunto de cabeçalhos. É possível para as partes da resposta para ser uma mistura de êxitos e falhas. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
  * [Autorização](#ID4ECB)
  * [Corpo da solicitação](#ID4EPB)
  * [Códigos de status HTTP](#ID4E3C)
  * [Cabeçalhos de resposta necessária](#ID4EPAAC)
  * [Cabeçalhos de resposta opcional](#ID4ESBAC)
  * [Corpo da resposta](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| pathAndFileName| cadeia de caracteres| Nome de arquivo e caminho para o item a ser acessado. Caracteres válidos para a parte do caminho (até e incluindo a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e a barra (/). A parte do caminho pode estar vazia. Caracteres válidos para a parte do nome de arquivo (tudo após a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_), ponto (.) e hífen (-). O nome do arquivo não pode ficar vazio, terminar com um ponto ou conter dois pontos consecutivos.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorização 
 
A solicitação deve incluir um cabeçalho de autorização válido Xbox LIVE. Se o chamador não tem permissão para acessar este recurso, o serviço retornará uma resposta 403 Proibido. Se o cabeçalho é inválido ou ausente, o serviço retornará uma resposta 401 não autorizado. 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
| Propriedade| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| xuids| matriz de inteiros sem sinal de 64 bits| A lista de XUIDs para o qual fazer o download de arquivos.| 
 
<a id="ID4EQC"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
    "xuids" : 
    [
      12345,
      45678,
      78901
    ]
}
      
```

   
<a id="ID4E3C"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP 
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Disposition| Descreve o conteúdo da parte. As partes "name" e "filename" do cabeçalho são o XUID do que este arquivo pertence ao usuário.| 
| HttpStatusCode| O código de status HTTP relacionado ao recuperar este arquivo específico.| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag é um identificador opaco atribuído por um servidor web para uma versão específica de um recurso encontrado em uma URL. Se o conteúdo de recurso nessa URL for alterado, é atribuído um ETag nova e diferente.| 
| Content-Type| Se o arquivo foi recuperado com êxito, isso é o tipo de conteúdo do arquivo.| 
| Content-Range| Se o arquivo foi recuperado com êxito e é um download parcial, é o intervalo de bytes do arquivo contido na resposta. | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Se a chamada for bem-sucedida, o serviço retornará o conteúdo dos arquivos solicitados em uma resposta com várias parte.
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo 
 

```cpp
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=c0a9fd75-d036-4294-8b7b-85fea15a31bb

228
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="123"; filename="123"
HttpStatusCode: 200
ETag: 0x8CF327717411C31
Content-Type: application/octet-stream

asdf123
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="456"; filename="456"
HttpStatusCode: 200
ETag: 0x8CF32771E954BB8
Content-Type: application/octet-stream

asdf456
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="789"; filename="789"
HttpStatusCode: 404


--c0a9fd75-d036-4294-8b7b-85fea15a31bb--

0

```

   
<a id="ID4EUDAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EWDAC"></a>

 
##### <a name="parent"></a>Parent 

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

   