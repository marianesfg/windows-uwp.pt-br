---
author: PatrickFarley
title: "Executar geocódigo e geocódigo reverso"
description: "Converta endereços em localizações geográficas (geocódigo) e converta localizações geográficas em endereços (geocódigo reverso) chamando os métodos da classe MapLocationFinder no namespace Windows.Services.Maps."
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, geocodificação, mapa, localização"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 16be7bcafaf286a71e79fb4bca01511ddc7a1ae0
ms.lasthandoff: 02/07/2017

---

# <a name="perform-geocoding-and-reverse-geocoding"></a>Executar geocódigo e geocódigo reverso


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Converta endereços em localizações geográficas (geocódigo) e converta localizações geográficas em endereços (geocódigo reverso) chamando os métodos da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) no namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979).

**Dica** Para saber mais sobre o uso de mapas em seu aplicativo, baixe o exemplo a seguir do [repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub.

-   [Exemplo de mapa da Plataforma Universal do Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

Veja como as classes de geocódigo e geocódigo reverso estão relacionadas:

-   A classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) possui métodos que fazem o geocódigo ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) e o geocódigo reverso ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928))
-   Esses métodos retornam um [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).
-   [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) contém uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549). Acesse essa coleção por meio da propriedade [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) do **MapLocationFinderResult**.
-   Cada objeto [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) contém um objeto [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533). Acesse esse objeto por meio da propriedade [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) de cada **MapLocation**.

**Importante**  Você deve especificar uma chave de autenticação de mapas para poder usar os serviços de mapa. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

 

## <a name="get-a-location-geocode"></a>Obter uma localização (geocódigo)


Converta um endereço ou dê um nome para uma localização geográfica (geocódigo) executando as seguintes etapas.

1.  Chame uma das sobrecargas do método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  O método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) retorna um objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contém uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) correspondentes.
3.  Acesse essa coleção por meio da propriedade [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) do [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).

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


Converta uma localização geográfica em um endereço (geocódigo reverso) realizando as etapas a seguir.

1.  Chame o método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  O método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) retorna um objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contém uma coleção de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) correspondentes.
3.  Acesse essa coleção por meio da propriedade [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) do [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).
4.  Acesse o objeto [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) por meio da propriedade [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) de cada [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo do Build 2015: Aproveitando mapas e localizações no telefone, no Tablet e no computador em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo de aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)

