---
author: msatranjr
title: Exibir rotas e trajetos em um mapa
description: Solicite rotas e trajeto e os exiba no aplicativo.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, rota, mapa, localização, direções"
ms.openlocfilehash: 62f0f26cc9a78a29e6fab5a8f762e0b28b7df6a3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="display-routes-and-directions-on-a-map"></a>Exibir rotas e trajetos em um mapa


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Solicite rotas e trajeto e os exiba no aplicativo.

**Dica** Para saber mais sobre o uso de mapas em seu aplicativo, baixe o exemplo a seguir do [repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub.

-   [Amostra de mapa da UWP (Plataforma Universal do Windows)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

**Dica**  Caso o mapeamento não seja um dos principais recursos do aplicativo, leve em consideração iniciar o aplicativo Mapas do Windows em vez disso. Você pode usar os esquemas de URI `bingmaps:`, `ms-drive-to:` e `ms-walk-to:` para iniciar o aplicativo Mapas do Windows para mapas específicos e trajetos curva a curva. Para obter mais informações, consulte [Iniciar o aplicativo Mapas do Windows](https://msdn.microsoft.com/library/windows/apps/mt228341).

 

## <a name="an-intro-to-maproutefinder-results"></a>Uma introdução aos resultados de MapRouteFinder


Aqui está como as classes de rotas e sentidos estão relacionadas:

-   A classe [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) tem métodos que obtêm rotas e trajetos.
-   Esses métodos retornam um [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939).
-   O [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) contém um objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937). Acesse esse objeto por meio da propriedade [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940) do **MapRouteFinderResult**.
-   [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) contém uma coleção de objetos [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955). Acesse essa coleção por meio da propriedade [**Legs**](https://msdn.microsoft.com/library/windows/apps/dn636973) do **MapRoute**.
-   Cada [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) contém uma coleção de objetos [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961). Acesse essa coleção por meio da propriedade [**Maneuvers**](https://msdn.microsoft.com/library/windows/apps/dn636959) do **MapRouteLeg**.

## <a name="display-directions"></a>Exibir trajetos


Obtenha rotas e trajetos ao dirigir ou caminhar chamando os métodos da classe [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938), por exemplo, [**GetDrivingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636943) ou [**GetWalkingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636953). O objeto [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) contém um objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) que você pode acessar por meio da propriedade [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940).

Ao solicitar uma rota, você pode especificar o seguinte:

-   Você pode fornecer somente um ponto de partida e um ponto de chegada, ou pode fornecer uma série de pontos intermediários para calcular a rota.
-   Você pode especificar otimizações - por exemplo, minimizar a distância.
-   Você pode especificar restrições - por exemplo, evitar rodovias.

O [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) calculado possui propriedades que fornecem o tempo de finalização da rota, a distância da rota e a coleção de objetos [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) que contêm as etapas da rota. Cada objeto **MapRouteLeg** contém uma coleção de objetos [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961). O objeto **MapRouteManeuver** contém trajetos que é possível acessar por meio da propriedade [**InstructionText**](https://msdn.microsoft.com/library/windows/apps/dn636964).

**Importante**  Você deve especificar uma chave de autenticação de mapas para poder usar os serviços de mapa. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

 

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

**Importante**  Você deve especificar uma chave de autenticação de mapas para usar serviços de mapa ou o controle de mapa. Para obter mais informações, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

 

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

## <a name="related-topics"></a>Tópicos relacionados

* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo do build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo do aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
