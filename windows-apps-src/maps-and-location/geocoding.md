---
title: Executar geocódigo e geocódigo reverso
description: Este guia mostra como converter endereços em localizações geográficas (geocódigo) e converta localizações geográficas em endereços (geocódigo reverso) chamando os métodos da classe MapLocationFinder no namespace Windows.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, geocodificação, mapa, localização
ms.localizationpriority: medium
ms.openlocfilehash: e8b0efe39578974090844a4224055821c29f8ced
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8471534"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Executar geocódigo e geocódigo reverso

Este guia mostra como converter endereços em localizações geográficas (geocódigo) e converta localizações geográficas em endereços (geocódigo reverso) chamando os métodos da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) no Maps [** **](https://msdn.microsoft.com/library/windows/apps/dn636979)namespace.

> [!TIP]
> Para saber mais sobre como usar mapas em seu aplicativo, baixe o exemplo [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) do [repositório de amostras universais do Windows](hhttps://github.com/Microsoft/Windows-universal-samples) no GitHub.

As classes envolvidas na geocodificação e geocodificação reversa são organizadas da seguinte maneira.

-   A classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) contém métodos que manipulam geocodificação ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) e reverter geocodificação ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)).
-   Esses métodos retornam uma instância de [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) .
-   A propriedade de [**locais**](https://msdn.microsoft.com/library/windows/apps/dn627552) do [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) expõe uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) . 
-   Objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) têm uma propriedade de [**endereço**](https://msdn.microsoft.com/library/windows/apps/dn636929) , que expõe um objeto de [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) que representa um endereço e uma propriedade de [**ponto**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) , que expõe um objeto [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) que representa uma localização geográfica.

> [!IMPORTANT]
> Você deve especificar uma chave de autenticação de mapas para poder usar os serviços de mapa. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obter uma localização (geocódigo)

Esta seção mostra como converter um endereço ou dê um nome para uma localização geográfica (geocódigo).

1.  Chame um das sobrecargas do método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) com um nome de lugar ou endereço residencial.
2.  O método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) retorna um objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) .
3.  Use a propriedade de [**locais**](https://msdn.microsoft.com/library/windows/apps/dn627552) do [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) para expor uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) . Pode haver vários objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) porque o sistema pode encontrar vários locais que correspondem à entrada determinada.

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

Esta seção mostra como converter uma localização geográfica em um endereço (geocódigo reverso).

1.  Chame o método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  O método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) retorna um objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contém uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) correspondentes.
3.  Use a propriedade de [**locais**](https://msdn.microsoft.com/library/windows/apps/dn627552) do [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) para expor uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) . Pode haver vários objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) porque o sistema pode encontrar vários locais que correspondem à entrada determinada.
4.  Acesse [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) objetos por meio da propriedade de [**endereço**](https://msdn.microsoft.com/library/windows/apps/dn636929) de cada [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Exemplo do aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo: Aproveitando mapas e localização no telefone, Tablet e computador em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Classe **MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [Método **FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [Método **FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)
