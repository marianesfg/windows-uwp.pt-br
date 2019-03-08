---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: baf965dcbd23bf00d7d0953726f9f20852324e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662381"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
Obtém uma configuração de serviço com escopo de uma lista delimitada por vírgulas de nomes de estatística de usuário em nome do usuário especificado.
O domínio para esses URIs é `userstats.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EEB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EPB)
  * [Autorização](#ID4EUC)
  * [Cabeçalhos de solicitação necessários](#ID4EPD)
  * [Cabeçalhos de solicitação opcionais](#ID4EYE)
  * [Corpo da solicitação](#ID4E3F)
  * [Códigos de status HTTP](#ID4EHG)
  * [Corpo da resposta](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

os clientes precisam de uma maneira de ler e gravar as estatísticas de título em nome dos jogadores em nosso novo sistema de estatísticas do player.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid| GUID| Xbox usuário ID (XUID) do usuário em cujo nome para acessar a configuração de serviço.|
| scid| GUID| Identificador da configuração do serviço que contém o recurso que está sendo acessado.|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- |
| statNames| cadeia de caracteres| O único parâmetro de cadeia de caracteres de consulta é delimitada por vírgula usuário estatística nome URI substantivo. Por exemplo, o URI a seguir informa ao serviço que quatro estatísticas são solicitadas em nome do id de usuário especificada no URI. `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`Haverá um limite no número de estatísticas que podem ser solicitadas em uma única chamada, e esse limite será considerar cuidadosamente um ponto"ideal" para o desenvolvedor conveniência vs. Viabilidade de comprimento do URI. Por exemplo, o limite pode ser determinado pelo máximo de 10 estatísticas ou qualquer um dos 600 caracteres que vale a pena de texto de nome de estatística (incluindo as vírgulas). Habilitar um GET simples como este habilita o cache de HTTP para estatísticas comumente solicitadas, o que reduz o volume de chamadas de clientes com ruídos. |

<a id="ID4EUC"></a>


## <a name="authorization"></a>Autorização

Não há lógica de autorização implementada para cenários de isolamento de conteúdo e controle de acesso.

   * Estatísticas de placares de líderes e o usuário podem ser lido de clientes em qualquer plataforma, desde que o chamador envia um token válido do XSTS com a solicitação. Gravações são, obviamente, limitadas a clientes com suporte pela plataforma de dados.
   * Os desenvolvedores de título podem marcar as estatísticas como aberta ou restrita com XDP ou Partner Center. Placar de líderes é abertos de estatísticas. Abrir estatísticas podem ser acessadas por Smartglass, bem como iOS, Android, Windows, Windows Phone e aplicativos web, desde que o usuário está autorizado para a área restrita. Autorização de usuário para uma área restrita é gerenciada por meio de XDP ou Partner Center.

O pseudocódigo para a verificação tem esta aparência:


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nome/número do serviço ao qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4EHG"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| A sessão foi recuperada com êxito.|
| 304| Não modificado| Recurso não foi modificado desde a última solicitado.|
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.|
| 401| Não autorizado| A solicitação exige autenticação do usuário.|
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.|
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.|
| 406| Não aceitável| Não há suporte para a versão do recurso.|
| 408| Tempo Limite da Solicitação| Não há suporte para a versão do recurso; deve ser rejeitada pela camada de MVC.|

<a id="ID4E5BAC"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4EECAC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "user": {
    "xuid": "123456789",
        "gamertag": "WarriorSaint",
        "stats": [
            {
                "statname": "Wins",
                "type": "Integer",
                "value": 40
            },
            {
                "statname": "Kills",
                "type": "Integer",
                "value": 700
            },
            {
                "statname": "KDRatio",
                "type": "Double",
                "value": 2.23
            },
            {
                "statname": "Headshots",
                "type": "Integer",
                "value": 173
            }
        ],
    }
}

```


<a id="ID4EOCAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EQCAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
