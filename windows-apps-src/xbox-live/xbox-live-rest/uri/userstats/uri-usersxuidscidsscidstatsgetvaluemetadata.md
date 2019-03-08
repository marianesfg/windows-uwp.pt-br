---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 55ad44d4c29a2d7a43c76c4df2a78e08462fa65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612711"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
Obtém uma lista de estatística especificado, incluindo os metadados associados com os valores de estatística, para um usuário em uma configuração de serviço especificado.
O domínio para esses URIs é `userstats.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EAB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ELB)
  * [Autorização](#ID4EWC)
  * [Cabeçalhos de solicitação necessários](#ID4ERD)
  * [Cabeçalhos de solicitação opcionais](#ID4EDF)
  * [Corpo da solicitação](#ID4EHG)
  * [Códigos de status HTTP](#ID4ESG)
  * [Corpo da resposta](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

O? incluem = valuemetadata parâmetro de consulta permite que a resposta incluir todos os metadados associados com os valores de stat de usuário, como o modelo e a cor de um carro usado para alcançar uma vez em uma faixa da corrida.

Para incluir o valor de metadados na resposta, a chamada de solicitação também deve definir o valor do cabeçalho X-Xbl-versão de contrato para 3.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid| GUID| Xbox usuário ID (XUID) do usuário em cujo nome para acessar a configuração de serviço.|
| scid| GUID| Identificador da configuração do serviço que contém o recurso que está sendo acessado.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- |
| statNames| cadeia de caracteres| Lista de nomes de estatística de usuário delimitada por uma vírgula. Por exemplo, o URI a seguir informa ao serviço que quatro estatísticas são solicitadas em nome do id de usuário especificada no URI. {:: nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| include=valuemetadata| cadeia de caracteres| Indica que a resposta inclui quaisquer metadados de valor associado com os valores de stat use.|

<a id="ID4EWC"></a>


## <a name="authorization"></a>Autorização

Não há lógica de autorização implementada para cenários de isolamento de conteúdo e controle de acesso.

   * Estatísticas de placares de líderes e o usuário podem ser lido de clientes em qualquer plataforma, desde que o chamador envia um token válido do XSTS com a solicitação. Gravações são limitadas aos clientes com suporte na plataforma de dados.
   * Os desenvolvedores de título podem marcar as estatísticas como aberta ou restrita com XDP ou Partner Center. Placar de líderes é abertos de estatísticas. Abrir estatísticas podem ser acessadas por Smartglass, bem como iOS, Android, Windows, Windows Phone e aplicativos web, desde que o usuário está autorizado para a área restrita. Autorização de usuário para uma área restrita é gerenciada por meio de XDP ou Partner Center.

O pseudocódigo para a verificação tem esta aparência:


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| X-Xbl-Contract-Version| cadeia de caracteres| Indica qual versão de API a ser usada. Esse valor deve ser definido como "3" para incluir o valor de metadados na resposta.|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nome/número do serviço ao qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.|

<a id="ID4EHG"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4ESG"></a>


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

<a id="ID4EJCAC"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4EPCAC"></a>


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
        "value": 40,
        "valuemetadata" : "{\"region\" : \"EU\", \"isRanked\" : true}"
      },
      {
        "statname": "Kills",
        "type": "Integer",
        "value": 700,
        "valuemetadata" : "{\"longestKillStreak" : 15, \"favoriteTarget\" : \"CrazyPigeon\"}"
      },
      {
        "statname": "KDRatio",
        "type": "Double",
        "value": 2.23,
        "valuemetadata" : "{\"totalKills\" : 700, \"totalDeaths\" : 314}"
      },
      {
        "statname": "Headshots",
        "type": "Integer",
        "value": 173,
        "valuemetadata" : ""
      }
    ],
  }
}

```


<a id="ID4EZCAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E2CAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
