---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
description: " POST (/handles/query)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7540c117931c01c24c79cef6c8ab6540cb65bbcb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651501"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
Cria consultas para identificadores de sessão.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EDB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EQB)
  * [Códigos de status HTTP](#ID4EBF)
  * [Corpo da solicitação](#ID4EIF)
  * [Corpo da resposta](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST cria consultas para manipulação de dados somente, sem qualquer informação de sessão. Ele pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**.

O campo de tipo no corpo da solicitação para esse método deve ser definido como "atividade". Os itens no corpo da resposta mapeiam diretamente para as propriedades do **MultiplayerActivityDetails**.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

Uma consulta pode ser modificada usando os parâmetros de cadeia de caracteres de consulta na tabela a seguir.

| <b>Parâmetro</b>| <b>Tipo</b>| <b>Descrição</b>|
| --- | --- | --- | --- |
| Palavra-chave| cadeia de caracteres| A palavra-chave, por exemplo, "foo", que deve ser encontrado em sessões ou modelos se eles devem ser recuperadas. |
| xuid| inteiro sem sinal de 64 bits| A ID de usuário Xbox, por exemplo, "123", para as sessões para incluir na consulta. Por padrão, o usuário deve estar ativo na sessão para que ele seja incluído. |
| reservas| booliano| True para incluir as sessões para o qual o usuário é definido como um player reservado, mas não ingressou para se tornar um player de Active Directory. Esse parâmetro é usado apenas ao consultar suas próprias sessões ou ao consultar as sessões de servidor-para-servidor um usuário específico. |
| inativo| booliano| True para incluir as sessões que o usuário aceitou, mas não estiver em execução ativamente. Contagem de sessões nas quais o usuário está "pronto", mas não "ativo" como inativas. |
| privado| booliano| True para incluir as sessões privadas. Esse parâmetro é usado apenas ao consultar suas próprias sessões ou ao consultar as sessões de servidor-para-servidor um usuário específico. |
| visibilidade| cadeia de caracteres| Estado de visibilidade para as sessões. Os valores possíveis são definidos pela <b>MultiplayerSessionVisibility</b>. Se esse parâmetro é definido como "aberta", a consulta deve incluir sessões abertas somente. Se ele for definido como "private", o <i>privada</i> parâmetro deve ser definido como true. |
| version| inteiro com sinal de 32 bits| A versão de máximo de sessão que deve ser incluída. Por exemplo, um valor de 2 Especifica que apenas sessões com uma versão de sessão principal de 2 ou menor deve ser incluída. O número de versão deve ser menor ou igual a mod de versão de contrato da solicitação 100. |
| Take| inteiro com sinal de 32 bits| O número máximo de sessões para recuperar. Esse número deve ser entre 0 e 100. |


Configuração tanto *privados* ou *reservas* como true requer acesso de nível de servidor para a sessão. Como alternativa, essas configurações exigem XUID do chamador de declaração para corresponder ao filtro XUID no URI. Caso contrário, o código de status HTTP/403 é retornado, se realmente existirem tais as sessões ou não.

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EIF"></a>


## <a name="request-body"></a>Corpo da solicitação

Para todas as atividades para o grafo social de "Favoritos" de um usuário, o corpo da solicitação tem esta aparência:


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


<a id="ID4ETF"></a>


## <a name="response-body"></a>Corpo da resposta

Os identificadores são recuperados como uma matriz de estruturas de identificador, com uma ID exclusiva incorporada em cada estrutura.


```cpp
{
  "results": [
    {
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
    }
  }]
}

```


<a id="ID4E4F"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E6F"></a>


##### <a name="parent"></a>Parent

[/handles/query](uri-handlesquery.md)
