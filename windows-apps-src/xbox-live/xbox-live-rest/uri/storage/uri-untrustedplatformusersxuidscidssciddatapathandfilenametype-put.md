---
title: PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: b40a1a88-02c2-624f-de00-49664825bde3
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-put.html
description: " PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ad6f60c53b1654ec85c4f78d39c1c2605c55bc88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655391"
---
# <a name="put-untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
Carrega um arquivo. Os dados podem ser carregados em um carregamento completo no qual os dados e metadados são enviados em uma única mensagem, ou como um upload de vários bloco no qual os dados e metadados são enviados em uma série de blocos menores. Somente os arquivos que são menores do que quatro megabytes podem ser enviados como uma única mensagem. Não há suporte para upload de vários bloco de dados do tipo json. O domínio para esses URIs é `titlestorage.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
  * [Autorização](#ID4EEB)
  * [Parâmetros de cadeia de caracteres de consulta opcionais](#ID4ERB)
  * [Cabeçalhos de solicitação necessários](#ID4EOE)
  * [Cabeçalhos de solicitação opcionais](#ID4EXF)
  * [Corpo da solicitação](#ID4E1G)
  * [Códigos de status HTTP](#ID4EFH)
  * [Corpo da resposta](#ID4EYEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| O usuário Xbox ID (XUID) do player de quem está fazendo a solicitação.| 
| scid| guid| a ID da configuração de serviço para pesquisar.| 
| pathAndFileName| cadeia de caracteres| Nome de arquivo e caminho para o item a ser acessado. Caracteres válidos para a parte do caminho (até e incluindo a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_) e a barra (/). A parte do caminho pode estar vazia. Caracteres válidos para a parte do nome de arquivo (tudo após a barra final) incluem as letras maiusculas (A-Z), letras minúsculas (a-z), números (0-9), sublinhado (_), ponto (.) e hífen (-). O nome do arquivo não pode ficar vazio, terminar com um ponto ou conter dois pontos consecutivos.| 
| type| cadeia de caracteres| O formato dos dados. Os valores possíveis são json ou binário.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Autorização 
 
A solicitação deve incluir um cabeçalho de autorização válido Xbox LIVE. Se o chamador não tem permissão para acessar este recurso, o serviço retornará uma resposta 403 Proibido. Se o cabeçalho é inválido ou ausente, o serviço retornará uma resposta 401 não autorizado. 
  
<a id="ID4ERB"></a>

 
## <a name="optional-query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta opcionais 
Para carregamentos de única mensagem, os parâmetros de cadeia de caracteres de consulta são: 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| Data/hora do arquivo no cliente de qualquer arquivo carregado pela última vez.| 
| displayName| cadeia de caracteres| Nome do arquivo que deve ser mostrado ao usuário.| 
 
Para carregamentos de múltiplos blocos, os parâmetros de cadeia de caracteres de consulta são:
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| Data/hora do arquivo no cliente de qualquer arquivo carregado pela última vez.| 
| displayName| cadeia de caracteres| Nome do arquivo que deve ser mostrado ao usuário.| 
| continuationToken| cadeia de caracteres| O token de continuação da resposta da solicitação de carregamento anteriores. Se esse for o primeiro bloco, isso não deve ser especificado. | 
| finalBlock| bool| Definido como true para o último bloco do arquivo. Definido como false para todos os outros blocos.| 
  
<a id="ID4EOE"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Versão do contrato de API.| 
| Autorização| XBL3.0 x = [hash]; [token]| Token de autenticação do STS. STSTokenString é substituído pelo token retornado pela solicitação de autenticação. Para obter mais informações sobre como recuperar um token STS e criar um cabeçalho de autorização, consulte Autenticando e autorizando Xbox LIVE as solicitações de serviços.| 
  
<a id="ID4EXF"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Especifica um ETag que deve corresponder a um item existente para concluir a operação.| 
| If-None-Match| Especifica um ETag que não deve corresponder a todos os itens existentes para concluir a operação.| 
  
<a id="ID4E1G"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
O corpo da solicitação contém o conteúdo do arquivo que está sendo carregado. Para carregamentos de única mensagem, o corpo é todo o conteúdo do arquivo. Para carregamentos de múltiplos blocos, o corpo é a parte do arquivo especificado nos parâmetros de cadeia de caracteres de consulta. 
  
<a id="ID4EFH"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP 
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EYEAC"></a>

 
## <a name="response-body"></a>Corpo da resposta 
 
Se a chamada é uma solicitação de múltiplos bloco e ele for bem-sucedido, o serviço retornará um token de continution para passar o próximo bloco.
 
<a id="ID4EEFAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
    "continuationToken":"abcd1234-1111-2222-3333-abcd12345678-1"
}
         
```

   
<a id="ID4EQFAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ESFAC"></a>

 
##### <a name="parent"></a>Parent  

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

   