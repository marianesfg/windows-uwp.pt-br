---
author: normesta
title: Exibir rotas e trajetos em um mapa
description: Solicite rotas e trajeto e os exiba no aplicativo.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.author: normesta
ms.date: 09/20/2017
ms.topic: article
keywords: windows 10, uwp, rota, mapa, localização, direções
ms.localizationpriority: medium
ms.openlocfilehash: 69283f6b53f3a8483376e3b8fe77a4491d4b01b1
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7173769"
---
# <a name="display-routes-and-directions-on-a-map"></a>Exibir rotas e trajetos em um mapa



Solicite rotas e trajeto e os exiba no aplicativo.

>[!Note]
>Para saber mais sobre como usar mapas em seu app, baixe a [amostra de mapa da Plataforma Universal do Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977).
>Caso o mapeamento não seja um dos principais recursos do app, considere iniciar o app Mapas do Windows em vez disso. Você pode usar os esquemas de URI `bingmaps:`, `ms-drive-to:` e `ms-walk-to:` para iniciar o aplicativo Mapas do Windows para mapas específicos e trajetos curva a curva. Para obter mais informações, consulte [Iniciar o aplicativo Mapas do Windows](https://msdn.microsoft.com/library/windows/apps/mt228341).

 
## <a name="an-intro-to-maproutefinder-results"></a>Uma introdução aos resultados de MapRouteFinder


Aqui está como as classes de rotas e sentidos estão relacionadas:

* A classe [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) tem métodos que obtêm rotas e trajetos. Esses métodos retornam um [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939).

* O [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) contém um objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937). Acesse esse objeto por meio da propriedade [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940) do **MapRouteFinderResult**.

* [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) contém uma coleção de objetos [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955). Acesse essa coleção por meio da propriedade [**Legs**](https://msdn.microsoft.com/library/windows/apps/dn636973) do **MapRoute**.

* Cada [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) contém uma coleção de objetos [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961). Acesse essa coleção por meio da propriedade [**Maneuvers**](https://msdn.microsoft.com/library/windows/apps/dn636959) do **MapRouteLeg**.

Obtenha rotas e trajetos ao dirigir ou caminhar chamando os métodos da classe [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938). Por exemplo, [**GetDrivingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636943) ou [**GetWalkingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636953).

Ao solicitar uma rota, você pode especificar o seguinte:

* Você pode fornecer somente um ponto de partida e um ponto de chegada, ou pode fornecer uma série de pontos intermediários para calcular a rota.

    O ponto de intermediário de *Parada* adiciona etapas adicionais à rota, cada uma com seu próprio Itinerário. Para especificar pontos intermediários de *parada*, use qualquer uma das sobrecargas [**GetDrivingRouteFromWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync).

    O ponto intermediário de *via* define localizações intermediárias entre os pontos intermediários de *parada*. Ele não adiciona etapas de rota.  Ele é simplesmente um ponto intermediário pelo qual passa uma rota. Para especificar pontos intermediários de *via*, use qualquer uma das sobrecargas [**GetDrivingRouteFromEnhancedWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync).

* Você pode especificar otimizações (por exemplo, minimizar a distância).

* Você pode especificar restrições (por exemplo, evitar rodovias).

## <a name="display-directions"></a>Exibir trajetos

O objeto [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) contém um objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) que você pode acessar por meio da propriedade [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940).

O [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) calculado possui propriedades que fornecem o tempo de finalização da rota, a distância da rota e a coleção de objetos [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) que contêm as etapas da rota. Cada objeto **MapRouteLeg** contém uma coleção de objetos [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961). O objeto **MapRouteManeuver** contém trajetos que é possível acessar por meio da propriedade [**InstructionText**](https://msdn.microsoft.com/library/windows/apps/dn636964).

>[!IMPORTANT]
>Você deve especificar uma chave de autenticação de mapas para poder usar os serviços de mapa. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

Este exemplo exibe os seguintes resultados para a caixa de texto `tbOutputText`.

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>Exibir rotas


Para exibir um [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) em um [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), construa um [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) com o **MapRoute**. Em seguida, adicione o **MapRouteView** à coleção [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) do **MapControl**.

>[!IMPORTANT]
>Você deve especificar uma chave de autenticação de mapas para usar serviços de mapa ou o controle de mapa. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

Esse exemplo exibe o seguinte em um [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) chamado **MapWithRoute**.

![controle de mapa com rota exibida.](images/routeonmap.png)

Aqui está uma versão deste exemplo que usa um ponto intermediário de *via* entre os dois pontos intermediários de *parada*:

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo do build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo do aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
