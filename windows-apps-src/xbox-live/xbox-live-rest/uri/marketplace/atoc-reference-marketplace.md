---
title: URIs de marketplace
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
description: " URIs de marketplace"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9fd8112c6e16b3e9d9fb70c34381e88ba5aa6273
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593871"
---
# <a name="marketplace-uris"></a>URIs de marketplace

Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) e os métodos de protocolo de transporte de hipertexto (HTTP) associados do Xbox Live Services pela *marketplace* de serviços, também conhecido como entretenimento Serviços de descoberta (EDS).

Somente os jogos em execução em um Xbox 360, em um dispositivo Windows Phone ou em Xbox.com podem usar esse serviço.

Os domínios para esses URIs são eds.xboxlive.com e inventory.xboxlive.com.

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/Users/me/Inventory](uri-inventory.md)

&nbsp;&nbsp;Acessa o conjunto de inventário associado no momento o usuário fornecido.

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;Acessa o conjunto completo de detalhes de um item de estoque consumível específico.

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;Acessa o conjunto completo de detalhes de um item de estoque específico.

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;Itens de acessos de vários grupos de mídia diferente.

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;Permite a navegação para itens dentro de um único grupo de mídia.

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;A classificação de conteúdo de token de acesso.

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;Retorna oferece detalhes e os metadados sobre um ou mais itens.

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;Acessa o token de campos.

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;Lista todos os mediaGroups com suporte para a versão especificada da EDS.

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;Acessa o mediaItemTypes disponíveis por grupo de mídia para a versão especificada da EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;Campos de acessos do qual um pode esperar dados para um determinado mediaitemtype e uma determinada versão de EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;Acessa os refiners de consulta para a mídia de determinado tipo de item.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;Acessa os valores aceitáveis para o nome de refinamento de determinada consulta e recebem o tipo de item de mídia.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;Acesse a lista de valores inferiores para um valor de refinamento de determinada consulta (por exemplo, "subgenres em um determinado gênero").

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;Acessa todos os mediaItemTypes com suporte para a versão especificada da EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;Acessos disponíveis classificar pedidos para um tipo de dado mediaitem e uma determinada versão de EDS.

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;Permite pesquisar itens dentro de um único grupo de mídia.

<a id="ID4EFD"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EHD"></a>


##### <a name="parent"></a>Parent

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>Informações adicionais

[Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)
