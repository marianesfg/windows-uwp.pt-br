---
title: GET (/media/{marketplaceId}/crossMediaGroupSearch)
assetID: 7c509af1-8dce-f419-c4de-2fad54fd1edb
permalink: en-us/docs/xboxlive/rest/uri-localecrossmediagroupsearchget.html
description: " GET (/media/{marketplaceId}/crossMediaGroupSearch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7258e8870519478ce49b7b2e60493a91a1277bbc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658391"
---
# <a name="get-mediamarketplaceidcrossmediagroupsearch"></a>GET (/media/{marketplaceId}/crossMediaGroupSearch)
Obtém os itens de vários grupos de mídia diferente. O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EEB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EPB)
  * [Corpo da resposta](#ID4ETC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
A API de grupo cruzado permite que os clientes procurar itens de vários grupos de mídia diferente. Essa API requer o uso de um token de continuação de somente avanço para paginação por meio de resultados. Essa API aceita Refiners de consulta.
 
**SandboxId** agora é recuperado da declaração no XToken e impostas. Se o **SandboxId** não estiver presente, em seguida, os serviços de descoberta de entretenimento (EDS) gerará um erro 400 Solicitação incorreta.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| continuationToken| cadeia de caracteres| Opcional. Consulte o parâmetro ContinuationToken.| 
| q| cadeia de caracteres| Obrigatório. Usado na pesquisa de termo da consulta.| 
  
<a id="ID4ETC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EZC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 
O código JSON a seguir é em resposta à chamada `/media/en-us/crossMediaGroupSearch?q=vector&maxItems=25&fields=all`.
 

```cpp
{
    "Items": [{
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "fd16e2fb-eca4-4182-8f69-a98fdd6e57a1",
        "Name": "Vector based massively multiplayer Tanks game.",
        "ReducedName": "Tanks",
        "ReleaseDate": "2013-03-15T00: 00: 00Z",
        "TitleId": "69A60C76",
        "VuiDisplayName": "Tanks",
        "DeveloperName": "Microsoft Studios",
        "Images": [{
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 300,
            "Width": 219,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 120,
            "Width": 85,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Image"],
            "Purpose": "Image",
            "Height": 720,
            "Width": 1280,
            "Order": 1
        }],
        "PublisherName": "Microsoft Studios",
        "RatingId": "10",
        "ParentalRating": "E",
        "ParentalRatingSystem": "ESRB",
        "SortName": "\n            Vector based massively multiplayer Tanks game.\n          ",
        "Capabilities": [{
            "NonLocalizedName": "onlineMultiplayerMin",
            "Value": "3"
        },
        {
            "NonLocalizedName": "onelineMultiplayerMax",
            "Value": "5"
        }],
        "KValue": "30",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "69A60C76"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            }],
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "PackageType": "XVC",
            "LicenseType": "Xbox Live Games Machine and User"
        }],
        "SandboxId": "PART.Dev1"
    },
    {
        "MediaGroup": "MusicArtistType",
        "MediaItemType": "MusicArtist",
        "ID": "61cdf888-7b2b-5111-82f4-87e9c7a86504",
        "Name": "Vector",
        "Genres": [{
            "Name": "Hip Hop"
        }],
        "SortName": "Vector [2]",
        "KValue": "28",
        "KValueNamespace": "bingbox",
        "AmgId": "P   362877",
        "ZuneId": "89C90600-0200-11DB-89CA-0019B92A3933",
        "SubGenres": [{
            "Name": "Contemporary Hip Hop"
        }],
        "AllTimePlayCount": 995.0,
        "SevenDaysPlayCount": 2.0,
        "ThirtyDaysPlayCount": 57.0,
        "LegacyAmgId": "7D890500-0600-11DB-89CA-0019B92A3933",
        "LegacyIds": [{
            "IdType": "AmgId",
            "Value": "P   362877"
        },
        {
            "IdType": "ZuneId",
            "Value": "89C90600-0200-11DB-89CA-0019B92A3933"
        },
        {
            "IdType": "LegacyAmgId",
            "Value": "7D890500-0600-11DB-89CA-0019B92A3933"
        },
        {
            "IdType": "ArtistId",
            "Value": "444809"
        }]
    }],
    "ContinuationToken": "1-----1",
    "ImpressionGuid": "9e41e6e4-1b2e-4710-abe4-b01ffbaa7605"
}
         
```

   
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

  
<a id="ID4EUD"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   