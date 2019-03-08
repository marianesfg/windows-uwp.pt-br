---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
description: " POST ({itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 877986ce9d48269295a68dbfd644f14785916b88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640851"
---
# <a name="post-itemid"></a>POST ({itemID})
Indica que todo ou parte de um item de estoque consumível foi usada e diminui a quantidade do que o utilizado pela quantidade solicitada.
O domínio para esses URIs é `inventory.xboxlive.com`.

  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EQB)
  * [Corpo da solicitação](#ID4E2B)
  * [Corpo da resposta](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Comentários

   * Se a quantidade solicitado que o chamador consumir excede o fornecimento restante do item, a chamada será rejeitada.
   * A quantidade solicitado que o chamador consumir deve ser um inteiro positivo acima de 0. Chamadas com um valor de consumo de 0 ou inferior serão rejeitadas.
   * Se o chamador forneça uma ID de transação vazia, a solicitação será rejeitada.
   * Se estiver disponível será registrada a declaração de título para que será possível determinar qual título está relatando o consumo.
   * Postagens adicionais com o mesmo transactionId serão ignoradas por um período de tempo.


> [!NOTE]
> O <b>cabeçalho x-xbl-contrato-version</b> para essa API é "4".


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| itemID| cadeia de caracteres| a ID exclusiva para cada usuário para um item de estoque no singular|

<a id="ID4E2B"></a>


## <a name="request-body"></a>Corpo da solicitação

<a id="ID4EBC"></a>


### <a name="sample-request"></a>Exemplo de solicitação


```cpp
{
  "transactionId": String
  "removeQuantity": Int
}

```


O campo Quantidade remover permite que o chamador indicar a quantidade de consumíveis que desejam remover da quantidade de restante do consumíveis. O campo de ID da transação fornece o chamador com um meio para tentar novamente usando as operações de conteúdo podem ser consumidas ao limitar o risco de duas vezes o mesmo uso de contagem.

<a id="ID4ENC"></a>


## <a name="response-body"></a>Corpo da resposta

A resposta para POST, supondo que ele passa a autenticação e é atribuído o contexto de autorização apropriada é um uma acknolodgement de recebimento com o mesmo transactionId passado para o serviço na solicitação POST, URL do item podem ser consumidos e o item novo valor de quantidade.

<a id="ID4EVC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
  "transactionId": String
  "url": String
  "newQuantity": Int
}

```


<a id="ID4E6C"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EBD"></a>


##### <a name="parent"></a>Parent

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>Informações adicionais

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)
