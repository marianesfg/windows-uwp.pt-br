---
title: /users/me/consumables/{itemID}
assetID: 45724827-5e35-326f-3f17-f49e606d9e08
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurl.html
description: O ponto de extremidade rESTful para itens de consumo do Xbox para um usuário.
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4822180f11879417be67351f901474a38f89d82e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614081"
---
# <a name="usersmeconsumablesitemid"></a>/users/me/consumables/{itemID}
Acessa o conjunto completo de detalhes de um item de estoque consumível específico.
O domínio para esses URIs é `inventory.xboxlive.com`.

  * [Parâmetros de URI](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| itemID| cadeia de caracteres| a ID exclusiva para cada usuário para um item de estoque no singular|

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>Métodos válidos

[POST ({itemID})](uri-inventoryconsumablesitemurlpost.md)

&nbsp;&nbsp;Indica que todo ou parte de um item de estoque consumível foi usada e diminui a quantidade do que o utilizado pela quantidade solicitada.

<a id="ID4E4B"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent

[URIs de Marketplace](atoc-reference-marketplace.md)


<a id="ID4EJC"></a>


##### <a name="further-information"></a>Informações adicionais

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)
