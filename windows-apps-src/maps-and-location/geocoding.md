---
title: Executar geocódigo e geocódigo reverso
description: Este guia mostra como converter endereços para locais geográficos (geocodificação) e converter locais geográficos em endereços de rua (geocodificação inversa) chamando os métodos da classe MapLocationFinder no namespace Windows. Services. Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, geocodificação, mapa, localização
ms.localizationpriority: medium
ms.openlocfilehash: 5d7e1dda355cf87a2c8e26c11327cfff32e9d0b5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259364"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Executar geocódigo e geocódigo reverso

Este guia mostra como converter endereços para locais geográficos (geocodificação) e converter locais geográficos em endereços de rua (geocodificação inversa) chamando os métodos da classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) no namespace [**Windows. Services. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) .

> [!TIP]
> Para saber mais sobre como usar mapas em seu aplicativo, baixe o exemplo de [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) do [repositório de exemplos do Windows universal](h https://github.com/Microsoft/Windows-universal-samples) no github.

As classes envolvidas em geocodificação e geocodificação inversa são organizadas da seguinte maneira.

-   A classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) contém métodos que manipulam geocodificação ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) e a geocodificação invertida ([**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   Esses métodos retornam uma instância de [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
-   A propriedade [**Locations**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) expõe uma coleção de objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) . 
-   Os objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) têm uma propriedade [**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) , que expõe um objeto [**MapAddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) que representa um endereço de rua e uma propriedade [**Point**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) , que expõe um objeto de [**ponto**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) geográfico que representa uma localização geográfica.

> [!IMPORTANT]
> você deve especificar uma chave de autenticação do Maps antes de poder usar os serviços de mapa. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obter uma localização (geocódigo)

Esta seção mostra como converter um endereço de rua ou um nome de local em um local geográfico (geocodificação).

1.  Chame uma das sobrecargas do método [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) da classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) com um nome de local ou endereço de rua.
2.  O método [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) retorna um objeto [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
3.  Use a propriedade [**Locations**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para expor uma coleção de objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) . Pode haver vários objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) porque o sistema pode encontrar vários locais que correspondem à entrada fornecida.

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

Esta seção mostra como converter uma localização geográfica em um endereço (geocodificação).

1.  Chame o método [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) da classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder).
2.  O método [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) retorna um objeto [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) que contém uma coleção de objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) correspondentes.
3.  Use a propriedade [**Locations**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para expor uma coleção de objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) . Pode haver vários objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) porque o sistema pode encontrar vários locais que correspondem à entrada fornecida.
4.  Acesse objetos [**MapAddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) por meio da propriedade [**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) de cada [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

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

* [Amostra de mapa UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Exemplo de aplicativo de tráfego UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [Diretrizes de design para mapas](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Vídeo: aproveitando mapas e local em telefone, Tablet e PC em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Classe **MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [Método **FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [Método **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
