---
title: Executar geocódigo e geocódigo reverso
description: Este guia mostra como converter endereços em localizações geográficas (geocodificação) e converter as localizações geográficas de endereços (geocodificação reversa) chamando os métodos da classe MapLocationFinder no namespace Windows.Services.Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, geocodificação, mapa, localização
ms.localizationpriority: medium
ms.openlocfilehash: 88687f6e7074ff7c914927a81a08720336b3c60c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371870"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Executar geocódigo e geocódigo reverso

Este guia mostra como converter endereços em localizações geográficas (geocodificação) e converter localizações geográficas em endereços (geocodificação reversa) chamando os métodos do [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) classe de [ **Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace.

> [!TIP]
> Para saber mais sobre o uso de mapas em seu aplicativo, baixe o [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) exemplo do [repositório de exemplos do Windows universal](h https://github.com/Microsoft/Windows-universal-samples) no GitHub.

As classes envolvidas na geocodificação e geocodificação reversa são organizadas da seguinte maneira.

-   O [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) classe contém métodos que manipulam geocodificação ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) e a geocodificação (reversa[**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   Esses métodos retornam um [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) instância.
-   O [ **locais** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propriedade do [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) expõe uma coleção de [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos. 
-   [**MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos têm ambas uma [ **endereço** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) propriedade, que expõe um [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) objeto que representa um endereço de rua e um [ **ponto** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) propriedade, que expõe um [ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) objeto que representa uma localização geográfica.

> [!IMPORTANT]
> Antes de poder usar os serviços de mapa, você deve especificar uma chave de autenticação de mapas. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obter uma localização (geocódigo)

Esta seção mostra como converter um endereço ou nome de um local para um local geográfico (geocódigo).

1.  Chamar uma das sobrecargas do [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) método da [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) classe com um nome de local ou a rua endereço.
2.  O [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) método retorna um [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) objeto.
3.  Use o [ **locais** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propriedade do [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para expor uma coleção [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos. Pode haver vários [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos porque o sistema pode encontrar vários locais que correspondem a determinada entrada.

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

Esse código exibe os seguintes resultados na caixa de texto `tbOutputText` .

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>Obter um endereço (geocódigo reverso)

Esta seção mostra como converter uma localização geográfica para um endereço (geocodificação reversa).

1.  Chame o método [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) da classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder).
2.  O método [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) retorna um objeto [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) que contém uma coleção de objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) correspondentes.
3.  Use o [ **locais** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propriedade do [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para expor uma coleção [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos. Pode haver vários [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos porque o sistema pode encontrar vários locais que correspondem a determinada entrada.
4.  Acesso [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) objetos por meio do [ **endereço** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) propriedade de cada [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

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

Esse código exibe os seguintes resultados na caixa de texto `tbOutputText` .

``` syntax
town = Redmond
```

## <a name="related-topics"></a>Tópicos relacionados

* [Amostra de mapa UWP](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Exemplo de aplicativo de tráfego UWP](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Diretrizes de design para mapas](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Vídeo: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [**MapLocationFinder** classe](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** método](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync** método](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
