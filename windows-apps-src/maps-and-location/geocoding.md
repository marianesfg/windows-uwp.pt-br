---
title: Executar geocódigo e geocódigo reverso
description: Este guia mostra como converter endereços em localizações geográficas (geocodificação) e converter as localizações geográficas de endereços (geocodificação reversa) chamando os métodos da classe MapLocationFinder no namespace Windows.Services.Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, geocodificação, mapa, localização
ms.localizationpriority: medium
ms.openlocfilehash: a30ca89242b15866019fffc6972bdae7086f3f7e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637621"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Executar geocódigo e geocódigo reverso

Este guia mostra como converter endereços em localizações geográficas (geocodificação) e converter localizações geográficas em endereços (geocodificação reversa) chamando os métodos do [ **MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550) classe de [ **Windows.Services.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn636979) namespace.

> [!TIP]
> Para saber mais sobre o uso de mapas em seu aplicativo, baixe o [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) exemplo do [repositório de exemplos do Windows universal](h https://github.com/Microsoft/Windows-universal-samples) no GitHub.

As classes envolvidas na geocodificação e geocodificação reversa são organizadas da seguinte maneira.

-   O [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) classe contém métodos que manipulam geocodificação ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) e a geocodificação (reversa[**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)).
-   Esses métodos retornam um [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) instância.
-   O [ **locais** ](https://msdn.microsoft.com/library/windows/apps/dn627552) propriedade do [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) expõe uma coleção de [  **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos. 
-   [**MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos têm ambas uma [ **endereço** ](https://msdn.microsoft.com/library/windows/apps/dn636929) propriedade, que expõe um [ **MapAddress** ](https://msdn.microsoft.com/library/windows/apps/dn627533) objeto que representa um endereço de rua e um [ **ponto** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) propriedade, que expõe um [ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) objeto que representa uma localização geográfica.

> [!IMPORTANT]
> Antes de poder usar os serviços de mapa, você deve especificar uma chave de autenticação de mapas. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obter uma localização (geocódigo)

Esta seção mostra como converter um endereço ou nome de um local para um local geográfico (geocódigo).

1.  Chamar uma das sobrecargas do [ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925) método da [ **MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550) classe com um nome de local ou a rua endereço.
2.  O [ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925) método retorna um [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) objeto.
3.  Use o [ **locais** ](https://msdn.microsoft.com/library/windows/apps/dn627552) propriedade do [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) para expor uma coleção [  **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos. Pode haver vários [ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos porque o sistema pode encontrar vários locais que correspondem a determinada entrada.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

Esse código exibe os seguintes resultados na caixa de texto `tbOutputText`.

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>Obter um endereço (geocódigo reverso)

Esta seção mostra como converter uma localização geográfica para um endereço (geocodificação reversa).

1.  Chame o método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  O método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) retorna um objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contém uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) correspondentes.
3.  Use o [ **locais** ](https://msdn.microsoft.com/library/windows/apps/dn627552) propriedade do [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) para expor uma coleção [  **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos. Pode haver vários [ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos porque o sistema pode encontrar vários locais que correspondem a determinada entrada.
4.  Acesso [ **MapAddress** ](https://msdn.microsoft.com/library/windows/apps/dn627533) objetos por meio do [ **endereço** ](https://msdn.microsoft.com/library/windows/apps/dn636929) propriedade de cada [ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549).

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

Esse código exibe os seguintes resultados na caixa de texto `tbOutputText`.

``` syntax
town = Redmond
```

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplo de mapa UWP](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Exemplo de aplicativo do tráfego UWP](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo: Aproveitando o mapas e local entre o telefone, Tablet e PC em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Central de desenvolvedores do Bing Maps](https://www.bingmapsportal.com/)
* [**MapLocationFinder** classe](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync** método](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync** método](https://msdn.microsoft.com/library/windows/apps/dn636928)
