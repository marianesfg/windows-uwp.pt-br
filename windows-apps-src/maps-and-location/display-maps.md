---
author: msatranjr
title: "Exibir mapas com modos de exibição 2D, 3D e Streetside"
description: "Exiba mapas personalizáveis no aplicativo usando a classe MapControl. Este tópico também apresenta modos de exibição 3D e Streetside."
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
translationtype: Human Translation
ms.sourcegitcommit: d00ba80ac7d0f033a69ad070dc8ee681cbd0ed18
ms.openlocfilehash: 09f87f66802e5143c8eac7fce8fc2118f3d31340

---

# <a name="display-maps-with-2d-3d-and-streetside-views"></a>Exibir mapas com modos de exibição 2D, 3D e Streetside


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Exiba mapas personalizáveis no seu aplicativo usando a classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Este tópico também apresenta modos de exibição 3D e Streetside.

**Dica** Para saber mais sobre o uso de mapas em seu aplicativo, baixe o exemplo a seguir do [repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub.

-   [Exemplo de mapa da Plataforma Universal do Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="add-the-map-control-to-your-app"></a>Adicionar o controle de mapa ao aplicativo


Exiba um mapa em uma página XAML adicionando um [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Para usar o **MapControl**, você deve declarar o namespace [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) na página XAML ou no seu código. Se você arrastar o controle da Caixa de Ferramentas, essa declaração de namespace será adicionada automaticamente. Se você adicionar o **MapControl** à página XAML manualmente, deverá adicionar a declaração de namespace manualmente na parte superior da página.

O exemplo a seguir exibe um controle de mapa básico e configura o mapa para exibir os controles de zoom e inclinação além de aceitar entradas de toque. Para obter mais informações sobre como personalizar a aparência do mapa, consulte [Configurar o mapa](#mapconfig).

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>
  
 </Grid>
</Page>
```

Se você adicionar o controle de mapa ao código, também deverá declarar o namespace manualmente na parte superior do arquivo de código.

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

## <a name="get-and-set-a-maps-authentication-key"></a>Obter e definir uma chave de autenticação de mapas


Para poder usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) e mapear serviços, você deve especificar uma chave de autenticação de mapas como o valor da propriedade [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036). Nos exemplos anteriores, substitua `EnterYourAuthenticationKeyHere` pela chave obtida na [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/). O texto **Aviso: MapServiceToken não especificado** continuará sendo exibido abaixo do controle até que você especifique a chave de autenticação de mapas. Para saber mais sobre como obter e definir uma chave de autenticação de mapas, consulte [Solicitar uma chave de autenticação de mapas](authentication-key.md).

## <a name="set-a-starting-location-for-the-map"></a>Definir uma localização inicial para o mapa


Defina a localização a ser exibida no mapa especificando a propriedade [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) no código ou associando a propriedade na marcação XAML. O exemplo a seguir exibe um mapa com a cidade de Seattle no centro.

**Dica**  Como uma cadeia de caracteres não pode ser convertida em um [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675), você não pode especificar um valor para a propriedade [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) na marcação XAML, a menos que use a vinculação de dados. (Essa limitação também se aplica à propriedade anexada [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264)).

 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![um exemplo de controle de mapa.](images/displaymapsexample1.png)

## <a name="set-the-current-location-of-the-map"></a>Definir a localização atual do mapa


Para acessar a localização do usuário, o aplicativo deve chamar o método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152). Nesse momento, seu aplicativo deve estar em primeiro plano, e **RequestAccessAsync** deve ser chamado do thread da interface do usuário. Até que o usuário conceda permissão para a localização ao seu aplicativo, este não pode acessar dados de localização.

Obtenha a localização atual do dispositivo (caso ela esteja disponível) usando o método [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) da classe [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534). Para obter o [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) correspondente, use a propriedade [**Point**](https://msdn.microsoft.com/library/windows/apps/dn263665) da coordenada geográfica da posição geográfica. Para saber mais, consulte [Obter a localização atual](get-location.md).

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

Ao exibir a localização do seu dispositivo em um mapa, leve em consideração a exibição de elementos gráficos e a definição do nível de zoom com base na precisão dos dados de localização. Para obter mais informações, consulte [Diretrizes para aplicativos com reconhecimento de local](https://msdn.microsoft.com/library/windows/apps/hh465148).

## <a name="change-the-location-of-the-map"></a>Alterar a localização do mapa


Para alterar a localização exibida em um mapa 2D, chame um das sobrecargas do método [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060). Use esse método para especificar novos valores para [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005), [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068), [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) e [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044). Você também pode especificar uma animação opcional para usar quando exibir alterações fornecendo uma constante da enumeração de [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002).

Para alterar a localização de um mapa 3D, use em vez disso o método [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296). Para saber mais, consulte [Mostrar modos de exibição 3D](#display3d).

Chame o método [**TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065) para exibir o conteúdo de uma [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949) no mapa. Use esse método, por exemplo, para exibir uma rota e uma parte de uma rota no mapa. Para saber mais, consulte [Exibir rotas e trajetos em um mapa](routes-and-directions.md).

## <a name="configure-the-map"></a>Configurar o mapa


Configure o mapa e sua aparência definindo os valores das seguintes propriedades do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

**Configurações de mapa**

-   Defina o **centro** do mapa como um ponto geográfico configurando a propriedade [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005).
-   Defina o **nível de zoom** do mapa configurando a propriedade [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) como um valor entre 1 e 20.
-   Defina a **rotação** do mapa configurando a propriedade [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019), em que 0 ou 360 graus = Norte, 90 = Leste, 180 = Sul e 270 = Oeste.
-   Defina a **inclinação** do mapa configurando a propriedade [**DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) como um valor entre 0 e 65 graus.

**Aparência do mapa**

-   Especifique o **tipo** do mapa (por exemplo, um mapa de estrada ou um mapa de vista aérea) configurando a propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) como uma das constantes de [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127).
-   Defina o **esquema de cores** do mapa como claro ou escuro configurando a propriedade [**ColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637010) comi uma das constantes de [**MapColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637003).

Mostre informações no mapa configurando os valores das seguintes propriedades do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

-   Mostre **edifícios e pontos de referência** no mapa habilitando ou desabilitando a propriedade [**LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023).
-   Mostre **recursos para pedestres**, como escadas públicas, no mapa habilitando ou desabilitando a propriedade [**PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042).
-   Mostre o **tráfego** no mapa habilitando ou desabilitando a propriedade [**TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055).
-   Especifique se a **marca d'água** é exibida no mapa configurando a propriedade [**WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) como uma das constantes de [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749).
-   Mostre uma **rota de direção ou caminhada** no mapa adicionando uma [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) à coleção [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) do controle de Mapa. Para saber mais e conhecer um exemplo, consulte [Exibir rotas e trajetos em um mapa](routes-and-directions.md).

Para saber mais sobre como exibir pinos de pressão, formas e controles XAML no [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), consulte [Exibir ponto de interesse (POI) em um mapa](display-poi.md).

## <a name="display-streetside-views"></a>Mostrar modos de exibição Streetside


Um modo de exibição Streetside é uma perspectiva a nível da rua de uma localização que aparece sobre o controle de mapa.

![exemplo de um modo de exibição streetside do controle de mapa.](images/onlystreetside-730width.png)

Considere a experiência "dentro" do modo de exibição Streetside separada do mapa originalmente exibido no controle de mapa. Por exemplo, alterar o local no modo de exibição Streetside não altera o local ou a aparência do mapa "no" modo de exibição Streetside. Depois que você fecha o modo de exibição Streetside (clicando no **X** no canto superior direito do controle), o mapa original permanece inalterado.

Para mostrar um modo de exibição Streetside

1.  Determine se os modos de exibição Streetside são compatíveis com o dispositivo verificando [**IsStreetsideSupported**](https://msdn.microsoft.com/library/windows/apps/dn974271).
2.  Caso não haja suporte para o modo de exibição Streetside, crie um [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) próximo à localização especificada chamando [**FindNearbyAsync**](https://msdn.microsoft.com/library/windows/apps/dn974361).
3.  Determine se um panorama próximo foi encontrado verificando se o [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) não é nulo
4.  Caso um panorama próximo seja encontrado, crie um [**StreetsideExperience**](https://msdn.microsoft.com/library/windows/apps/dn974356) para a propriedade [**CustomExperience**](https://msdn.microsoft.com/library/windows/apps/dn974263) do controle de mapa.

Este exemplo mostra como apresentar um modo de exibição Streetside semelhante à imagem anterior.

**Observação**  O mapa de visão geral não será exibido se o controle de mapa for muito pequeno.

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

## <a name="display-aerial-3d-views"></a>Mostrar modos de exibição aérea 3D


Especifique uma perspectiva 3D do mapa usando a classe [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329). A cena de mapa representa o modo de exibição 3D mostrado no mapa. A classe [**MapCamera**](https://msdn.microsoft.com/library/windows/apps/dn974244) representa a posição da câmera que mostra o modo de exibição.

![Diagrama da localização de MapCamera até o local da cena do mapa](images/mapcontrol-techdiagram.png)

Para fazer com que edifícios e outros recursos na superfície do mapa apareçam em 3D, defina a propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) do controle de mapa como [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127). Este é um exemplo de um modo de exibição 3D com o estilo **Aerial3DWithRoads**.

![exemplo de um modo de exibição de mapa 3D.](images/only3d-730width.png)

Para mostrar um modo de exibição 3D

1.  Determine se os modos de exibição 3D são compatíveis com o dispositivo verificando [**Is3DSupported**](https://msdn.microsoft.com/library/windows/apps/dn974265).
2.  Caso os modos de exibição 3D sejam compatíveis, defina a propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) do controle de mapa como [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127).
3.  Crie um objeto [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) usando um dos vários métodos **CreateFrom**, como [**CreateFromLocationAndRadius**](https://msdn.microsoft.com/library/windows/apps/dn974336) e [**CreateFromCamera**](https://msdn.microsoft.com/library/windows/apps/dn974334).
4.  Chame [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) para mostrar o modo de exibição 3D. Você também pode especificar uma animação opcional para usar quando exibir alterações fornecendo uma constante da enumeração de [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002).

Este exemplo mostra como apresentar um modo de exibição 3D.

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 34.134, Longitude = -118.3216};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## <a name="get-info-about-locations-and-elements"></a>Obter informações sobre localizações e elementos


Obtenha informações sobre localizações no mapa chamando os métodos a seguir do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

-   Método [**GetLocationFromOffset**](https://msdn.microsoft.com/library/windows/apps/dn637016) - Obtenha a localização geográfica correspondente ao ponto especificado no visor do controle de Mapa.
-   Método [**GetOffsetFromLocation**](https://msdn.microsoft.com/library/windows/apps/dn637018) - Obtenha o ponto no visor do controle de Mapa correspondente à localização geográfica especificada.
-   Método [**IsLocationInView**](https://msdn.microsoft.com/library/windows/apps/dn637022) - Determine se a localização geográfica especificada está atualmente visível no visor do controle de Mapa.
-   Método [**FindMapElementsAtOffset**](https://msdn.microsoft.com/library/windows/apps/dn637014) - Obtenha os elementos no mapa localizado no ponto especificado no visor do controle de Mapa.

## <a name="handle-user-interaction-and-changes"></a>Tratar mudanças e interação do usuário


Trate os gestos de entrada do usuário no mapa tratando os eventos a seguir do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Obtenha informações sobre a localização geográfica no mapa e a posição física no visor onde ocorreu o gesto verificando os valores das propriedades [**Location**](https://msdn.microsoft.com/library/windows/apps/dn637091) e [**Position**](https://msdn.microsoft.com/library/windows/apps/dn637093) do [**MapInputEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637090).

-   [**MapTapped**](https://msdn.microsoft.com/library/windows/apps/dn637038)
-   [**MapDoubleTapped**](https://msdn.microsoft.com/library/windows/apps/dn637032)
-   [**MapHolding**](https://msdn.microsoft.com/library/windows/apps/dn637035)

Determine se o mapa está carregando ou se está completamente carregado tratando o evento [**LoadingStatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn637028) do controle.

Trate mudanças que acontecem quando o usuário ou o aplicativo altera as configurações do mapa tratando os eventos a seguir do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). [Diretrizes para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)

-   [**CenterChanged**](https://msdn.microsoft.com/library/windows/apps/dn637006)
-   [**HeadingChanged**](https://msdn.microsoft.com/library/windows/apps/dn637020)
-   [**PitchChanged**](https://msdn.microsoft.com/library/windows/apps/dn637045)
-   [**ZoomLevelChanged**](https://msdn.microsoft.com/library/windows/apps/dn637069)

## <a name="related-topics"></a>Tópicos relacionados

* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Obter a localização atual](get-location.md)
* [Diretrizes de design para aplicativos com detecção de localização](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo do build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo de aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)





<!--HONumber=Dec16_HO1-->


