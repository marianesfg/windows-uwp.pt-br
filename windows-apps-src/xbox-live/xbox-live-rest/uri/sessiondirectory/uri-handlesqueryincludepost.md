---
title: POST (/handles/query?include=relatedInfo)
assetID: 66ecd1fe-24d4-4cd5-256d-8950ac658529
permalink: en-us/docs/xboxlive/rest/uri-handlesqueryincludepost.html
description: " POST (/handles/query?include=relatedInfo)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eb30561c91a085902dd9d79b6c4a2045dc709bfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632551"
---
# <a name="post-handlesqueryincluderelatedinfo"></a>POST (/handles/query?include=relatedInfo)
Cria consultas para identificadores de sessão que incluem informações de sessão relacionado.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4ECB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EPB)
  * [Códigos de status HTTP](#ID4EAF)
  * [Corpo da solicitação](#ID4EHF)
  * [Corpo da resposta](#ID4EZF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST cria consultas para manipulação de dados com informações de sessão especificadas na cadeia de consulta de "inclusão". O valor de cadeia de caracteres de consulta é projetado para ser extensível para dar suporte a opções de consulta futura, que dão suporte a uma lista delimitada, por exemplo, "? incluem = relatedInfo, sessão". O método POST pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForUsersAsync**.

O campo de tipo no corpo da solicitação para esse método deve ser definido como "atividade". Os itens no corpo da resposta mapeiam diretamente para as propriedades do **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

Uma consulta pode ser modificada usando os parâmetros de cadeia de caracteres de consulta na tabela a seguir.

| <b>Parâmetro</b>| <b>Tipo</b>| <b>Descrição</b>|
| --- | --- | --- | --- |
| Palavra-chave| cadeia de caracteres| A palavra-chave, por exemplo, "foo", que deve ser encontrado em sessões ou modelos se eles devem ser recuperadas. |
| xuid| inteiro sem sinal de 64 bits| A ID de usuário Xbox, por exemplo, "123", para as sessões para incluir na consulta. Por padrão, o usuário deve estar ativo na sessão para que ele seja incluído. |
| reservas| booliano| True para incluir as sessões para o qual o usuário é definido como um player reservado, mas não ingressou para se tornar um player de Active Directory. Esse parâmetro é usado apenas ao consultar suas próprias sessões ou ao consultar as sessões de servidor-para-servidor um usuário específico. |
| inativo| booliano| True para incluir as sessões que o usuário aceitou, mas não estiver em execução ativamente. Contagem de sessões nas quais o usuário está "pronto", mas não "ativo" como inativas. |
| privado| booliano| True para incluir as sessões privadas. Esse parâmetro é usado apenas ao consultar suas próprias sessões ou ao consultar as sessões de servidor-para-servidor um usuário específico. |
| visibilidade| cadeia de caracteres| Estado de visibilidade para as sessões. Os valores possíveis são definidos pela <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>. Se esse parâmetro é definido como "aberta", a consulta deve incluir sessões abertas somente. Se ele for definido como "private", o <i>privada</i> parâmetro deve ser definido como true. |
| version| inteiro com sinal de 32 bits| A versão de máximo de sessão que deve ser incluída. Por exemplo, um valor de 2 Especifica que apenas sessões com uma versão de sessão principal de 2 ou menor deve ser incluída. O número de versão deve ser menor ou igual a mod de versão de contrato da solicitação 100. |
| Take| inteiro com sinal de 32 bits| O número máximo de sessões para recuperar. Esse número deve ser entre 0 e 100. |


Configuração tanto *privados* ou *reservas* como true requer acesso de nível de servidor para a sessão. Como alternativa, essas configurações exigem XUID do chamador de declaração para corresponder ao filtro XUID no URI. Caso contrário, o código de status HTTP/403 é retornado, se realmente existirem tais as sessões ou não.

<a id="ID4EAF"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EHF"></a>


## <a name="request-body"></a>Corpo da solicitação

<a id="ID4ENF"></a>


### <a name="sample-request"></a>Exemplo de solicitação

Para obter todas as atividades para o grafo social de "Favoritos" de um usuário, o corpo do POST tem esta aparência:


```cpp
{
  "type": "activity",
  "scid": "B5B1F71F-A328-4371-89E0-C3AD222D0E92"  // optional filter on scid
  "owners": {
     "people": {
       "moniker": "favorites",
       "monikerXuid": "3210"
     }
  }
}

```


<a id="ID4EZF"></a>


## <a name="response-body"></a>Corpo da resposta

Os resultados são retornados como uma matriz de estruturas de identificador, com uma ID exclusiva inserida em cada identificador. As informações de sessão específica são retornadas na **relatedInfo** objeto. Observe que essas informações não são retornadas para o método POST regular esse URI.

<a id="ID4EDG"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "results": [{
        "id": "11111111-ebe0-42da-885f-033860a818f6",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "party",
            "name": "e3a836aeac6f4cbe9bcab985494d3175"
        },

    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": true,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    },
    {
        "id": "11111111-ebe0-42da-885f-033860a818f7",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "TitleStorageTestDefault",
            "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
        },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": false,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    }]
}

```


<a id="ID4ENG"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EPG"></a>


##### <a name="parent"></a>Parent

[/handles/query](uri-handlesquery.md)
