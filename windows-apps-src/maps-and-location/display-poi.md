---
author: normesta
title: Exibir pontos de interesse (POI) em um mapa
description: Adicione pontos de interesse (POI) a um mapa usando pinos, imagens, formas e elementos de interface do usuário XAML.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.author: normesta
ms.date: 08/11/2017
ms.topic: article
keywords: windows 10, uwp, mapa, localização, pinos de pressão
ms.localizationpriority: medium
ms.openlocfilehash: 13c0ea463cbab97e03c87c4e558bba0eff92300c
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7148062"
---
# <a name="display-points-of-interest-on-a-map"></a>Exibir pontos de interesse em um mapa

Adicione pontos de interesse (POI) a um mapa usando pinos, imagens, formas e elementos de interface do usuário XAML. Uma ponto de interesse é um ponto específico no mapa que representa algo de seu interesse. Por exemplo, o local de uma empresa, cidade ou amigo.

Para saber mais sobre como exibir POI no seu app, baixe o exemplo a seguir do [repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub: [exemplo de mapa da Plataforma Universal do Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977).

Exiba pinos, imagens e formas no mapa adicionando os objetos [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077), [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard),  [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) e [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) a uma coleção **MapElements** de um objeto [**MapElementsLayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer). Em seguida, adicione esse objeto de camada à coleção **Layers** de um controle de mapa.

>[!NOTE]
> Em versões anteriores, este guia mostrava como adicionar elementos de mapa à coleção [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements). Embora você ainda possa usar essa abordagem, perderá algumas das vantagens do novo modelo de camada do mapa. Para saber mais, consulte a seção [Como trabalhar como camadas](#layers) deste guia.

Você também pode exibir os elementos da interface do usuário XAML como [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), um [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) ou um [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) no mapa adicionando-os ao [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) ou como [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

Caso você tenha um grande número de elementos a serem colocados no mapa, leve em consideração [sobrepor imagens lado a lado no mapa](overlay-tiled-images.md). Para exibir rodovias no mapa, consulte [Exibir rotas e trajetos](routes-and-directions.md)

## <a name="add-a-pushpin"></a>Adicionar um pino

Exiba uma imagem como um pino, com texto opcional, no mapa usando a classe [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077). Você pode aceitar a imagem padrão ou fornecer uma imagem personalizada usando a propriedade [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078). A imagem a seguir exibe a imagem padrão para um **MapIcon** sem um valor especificado para a propriedade [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088), com um título curto, com um título longo e com um título muito longo.

![ícone de mapa de amostra com títulos de comprimentos diferentes.](images/mapctrl-mapicons.png)

O exemplo a seguir mostra um mapa da cidade de Seattle e adiciona um [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) com a imagem padrão e um título opcional para indicar a localização da Space Needle. Também centraliza o mapa sobre o ícone e aumenta. Para obter informações gerais sobre como usar o controle de mapa, consulte [Exibir mapas com modos de exibição 2D, 3D e Streetside](display-maps.md).

```csharp
public void AddSpaceNeedleIcon()
{
    var MyLandmarks = new List<MapElement>();

    BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
    Geopoint snPoint = new Geopoint(snPosition);

    var spaceNeedleIcon = new MapIcon
    {
        Location = snPoint,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        ZIndex = 0,
        Title = "Space Needle"
    };

    MyLandmarks.Add(spaceNeedleIcon);

    var LandmarksLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarks
    };

    myMap.Layers.Add(LandmarksLayer);

    myMap.Center = snPoint;
    myMap.ZoomLevel = 14;

}
```

Este exemplo exibe no mapa o ponto de interesse a seguir (a imagem padrão no centro).

![mapa com mapicon](images/displaypoidefault.png)

A linha de código a seguir exibe o [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) com uma imagem personalizada salva na pasta Ativos do projeto. A propriedade [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) do **MapIcon** espera um valor do tipo [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813). Esse tipo requer uma instrução **using** para o namespace [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791).

>[!NOTE]
>Caso você use a mesma imagem para vários ícones de mapa, declare o [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) no nível da página ou do aplicativo tendo em vista o melhor desempenho.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Tenha estas considerações em mente ao trabalhar com a classe [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077):

-   A propriedade [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) dá suporte à imagem de tamanho máximo 2048×2048 pixels.
-   Por padrão, a exibição da imagem do ícone de mapa não é garantida. Ele pode ficar oculto quando obscurecer outros elementos ou rótulos no mapa. Para manter visível, defina a propriedade [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) do ícone de mapa como [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314).
-   Não há garantia de que o [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) opcional do [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) seja mostrado. Se você não vir o texto, aplique zoom diminuindo o valor da propriedade [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).
-   Quando exibir uma imagem [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) que aponta para um local específico no mapa - por exemplo, um botão de pressão ou uma seta - considere configurar o valor da propriedade [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) para a localização aproximada do ponteiro na imagem. Caso você deixe o valor de **NormalizedAnchorPoint** no valor padrão (0, 0), o que representa o canto superior esquerdo da imagem, as mudanças em [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) do mapa podem deixar a imagem apontando para uma localização diferente.
-   Se você não definir explicitamente uma [Altitude](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.basicgeoposition) e um [AltitudeReferenceSystem](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem), o [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) será colocado na superfície.

## <a name="add-a-3d-pushpin"></a>Adicionar um pino 3D

Você pode adicionar objetos tridimensionais a um mapa. Use a classe [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) para importar um objeto tridimensional de um arquivo [Formato de Manufatura 3D (3MF)](http://3mf.io/specification/).

Esta imagem usa xícaras de café 3D para marcar os locais dos cafeterias em um bairro.

![canecas em mapas](images/mugs.png)

O código a seguir adiciona uma xícara de café ao mapa usando a importação de um arquivo 3MF. Para manter a simplicidade, este código adiciona a imagem ao centro do mapa, mas seu código provavelmente adicionaria a imagem a um local específico.

```csharp
public async void Add3DMapModel()
{
    var mugStreamReference = RandomAccessStreamReference.CreateFromUri
        (new Uri("ms-appx:///Assets/mug.3mf"));

    var myModel = await MapModel3D.CreateFrom3MFAsync(mugStreamReference,
        MapModel3DShadingOption.Smooth);

    myMap.Layers.Add(new MapElementsLayer
    {
       ZIndex = 1,
       MapElements = new List<MapElement>
       {
          new MapElement3D
          {
              Location = myMap.Center,
              Model = myModel,
          },
       },
    });
}
```

## <a name="add-an-image"></a>Adicionar uma imagem

Exiba grandes imagens relacionadas a locais do mapa, como uma imagem de um restaurante ou de um ponto de referência. À medida que os usuários diminuem o zoom, a imagem encolherá proporcionalmente em tamanho para permitir que o usuário exiba mais do mapa. Isso é um pouco diferente de um [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) que marca um local específico, é normalmente pequeno e permanece do mesmo tamanho à medida que os usuários ampliam ou reduzem o zoom em um mapa.

![Imagem do MapBillboard](images/map-billboard.png)

O código a seguir mostra o [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) apresentado na imagem acima.

```csharp
public void AddLandmarkPhoto()
{
    // Create MapBillboard.

    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

    var mapBillboard = new MapBillboard(myMap.ActualCamera)
    {
        Location = myMap.Center,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        Image = mapBillboardStreamReference
    };

    // Add MapBillboard to a layer on the map control.

    var MyLandmarkPhotos = new List<MapElement>();

    MyLandmarkPhotos.Add(mapBillboard);

    var LandmarksPhotoLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarkPhotos
    };

    myMap.Layers.Add(LandmarksPhotoLayer);
}
```

Há três partes deste código que vale a pena examinar um pouco mais de perto: a imagem, a câmera de referência e a propriedade [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint).

### <a name="image"></a>Imagem

Este exemplo mostra uma imagem personalizada salva na pasta **Ativos** do projeto. A propriedade [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) do [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) espera um valor do tipo [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813). Esse tipo requer uma instrução **using** para o namespace [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791).

>[!NOTE]
>Caso você use a mesma imagem para vários ícones de mapa, declare o [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) no nível da página ou do aplicativo tendo em vista o melhor desempenho.

### <a name="reference-camera"></a>Câmera de referência

 Como uma imagem do [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) é ampliada e reduzida à medida que o [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) do mapa muda, é importante definir onde no [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) a imagem é exibida em uma escala de 1x normal. Essa posição é definida na câmera de referência do [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) e, para defini-la, você terá de passar um objeto [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) para o construtor do [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard).

 Você pode definir a posição que você deseja em um [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)e usar esse [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) para criar um objeto [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera).  No entanto, neste exemplo, estamos usando o objeto [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) retornado pela propriedade [**ActualCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera) do controle de mapa. Esta é a câmera interna do mapa. A posição atual dessa câmera se torna a posição da câmera de referência; a posição em que a imagem do [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) é exibida em escala de 1x.

 Se seu aplicativo oferece aos usuários a capacidade de reduzir o zoom no mapa, a imagem diminui em tamanho porque a câmera interna do mapa está aumentando em altitude enquanto a imagem na escala 1x permanece fixa na posição da câmera de referência.

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

O [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) é o ponto da imagem ancorado na propriedade [**Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) do [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard). O ponto 0,5,1 é o centro da parte inferior da imagem. Como definimos a propriedade [**local**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) do [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) para o centro do controle do mapa, o centro da parte inferior da imagem será ancorado no centro do controle dos mapas. Se você quiser que a imagem apareça centralizada diretamente sobre um ponto, defina o [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) como 0,5, 0,5.  

## <a name="add-a-shape"></a>Adicionar uma forma

Exiba uma forma de vários pontos no mapa usando a classe [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103). O exemplo a seguir, da [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), mostra uma caixa vermelha com borda azul no mapa.

```csharp
public void HighlightArea()
{
    // Create MapPolygon.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        ZIndex = 1,
        FillColor = Colors.Red,
        StrokeColor = Colors.Blue,
        StrokeThickness = 3,
        StrokeDashed = false,
    };

    // Add MapPolygon to a layer on the map control.
    var MyHighlights = new List<MapElement>();

    MyHighlights.Add(mapPolygon);

    var HighlightsLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyHighlights
    };

    myMap.Layers.Add(HighlightsLayer);
}
```

## <a name="add-a-line"></a>Adicionar uma linha


Exiba uma linha no mapa usando a classe [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114). O exemplo a seguir, da [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), mostra uma linha tracejada no mapa.

```csharp
public void DrawLineOnMap()
{
    // Create Polyline.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

   // Add Polyline to a layer on the map control.

   var MyLines = new List<MapElement>();

   MyLines.Add(mapPolyline);

   var LinesLayer = new MapElementsLayer
   {
       ZIndex = 1,
       MapElements = MyLines
   };

   myMap.Layers.Add(LinesLayer);

}
```

## <a name="add-xaml"></a>Adicionar XAML

Exiba elementos de interface do usuário personalizados no mapa usando XAML. Posicione o XAML no mapa especificando o local e o ponto de ancoragem normalizado do XAML.

-   Defina a localização no mapa em que a XAML é colocada chamando [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369).
-   Defina a localização relativa no XAML correspondente à localização especificada chamando [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050).

O exemplo a seguir mostra um mapa da cidade de Seattle e adiciona um controle [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) XAML para indicar o local da Space Needle. Também centraliza o mapa sobre a área e aumenta. Para obter informações gerais sobre como usar o controle de mapa, consulte [Exibir mapas com modos de exibição 2D, 3D e Streetside](display-maps.md).

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

Este exemplo exibe uma borda azul no mapa.

![Captura de tela do xaml exibido no ponto de interesse no mapa](images/displaypoixaml.png)

Os exemplos a seguir mostram como adicionar elementos de interface do usuário XAML diretamente na marcação XAML da página usando vinculação de dados. Assim como acontecem com outros elementos XAML que exibem conteúdo, [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) é a propriedade de conteúdo padrão do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) e não precisa ser especificada explicitamente na marcação XAML.

Este exemplo mostra como exibir dois controles XAML como filhos implícitos do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Esses controles aparecem no mapa nas localizações associadas aos dados.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

Defina essas localizações usando propriedades no seu arquivo code-behind.

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

Este exemplo mostra como exibir dois controles XAML contidos dentro de um [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094). Esses controles aparecem no mapa nas localizações associadas aos dados.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

Este exemplo exibe uma coleção de elementos XAML vinculados a um [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094).

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{x:Bind LandmarkOverlays}">
      <maps:MapItemsControl.ItemTemplate>
          <DataTemplate>
              <StackPanel Background="Black" Tapped ="Overlay_Tapped">
                  <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Title}"
                    maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
              </StackPanel>
          </DataTemplate>
      </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

A propriedade ``ItemsSource`` no exemplo acima está associada a uma propriedade do tipo [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist?view=netframework-4.70) no arquivo code-behind.

```csharp
public sealed partial class Scenario1 : Page
{
    public IList LandmarkOverlays { get; set; }

    public MyClassConstructor()
    {
         SetLandMarkLocations();
         this.InitializeComponent();   
    }

    private void SetLandMarkLocations()
    {
        LandmarkOverlays = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        LandmarkOverlays.Add(pikePlaceIcon);

        var SeattleSpaceNeedleIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.6205, Longitude = -122.3493 }),
            Title = "Seattle Space Needle"
        };

        LandmarkOverlays.Add(SeattleSpaceNeedleIcon);
    }
}
```

<a id="layers" />

## <a name="working-with-layers"></a>Como trabalhar com camadas

Os exemplos neste guia adicionam elementos a uma coleção [MapElementLayers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer). Em seguida, eles mostram como adicionar essa coleção à propriedade **Layers** do controle de mapa. Em versões anteriores, este guia mostrava como adicionar elementos de mapa à coleção [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) desta forma:

```csharp
var pikePlaceIcon = new MapIcon
{
    Location = new Geopoint(new BasicGeoposition
    { Latitude = 47.610, Longitude = -122.342 }),
    NormalizedAnchorPoint = new Point(0.5, 1.0),
    ZIndex = 0,
    Title = "Pike Place Market"
};

myMap.MapElements.Add(pikePlaceIcon);
```

Embora você ainda possa usar essa abordagem, perderá algumas das vantagens do novo modelo de camada do mapa. Ao agrupar seus elementos em camadas, você pode manipular cada camada independentemente umas das outras. Por exemplo, como cada camada tem seu próprio conjunto de eventos, você pode responder a um evento em uma camada específica e realizar uma ação específica para tal evento.

Além disso, você pode associar XAML diretamente a uma [MapLayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maplayer). Isso é algo que você não pode fazer usando a coleção [MapElements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements).

Uma maneira para fazer isso é usando uma classe de modelo de exibição, code-behind de uma página XAML e uma página XAML.

### <a name="view-model-class"></a>Classe do modelo de exibição

```csharp
public class LandmarksViewModel
{
    public ObservableCollection<MapLayer> LandmarkLayer
        { get; } = new ObservableCollection<MapLayer>();

    public LandmarksViewModel()
    {
        var MyElements = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        MyElements.Add(pikePlaceIcon);

        var LandmarksLayer = new MapElementsLayer
        {
            ZIndex = 1,
            MapElements = MyElements
        };

        LandmarkLayer.Add(LandmarksLayer);         
    }

```

### <a name="code-behind-a-xaml-page"></a>Código-behind de uma página XAML

Conectar-se à classe de modelo de exibição para sua página de code-behind.

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>Página XAML

Na página XAML, associe à propriedade na sua classe de modelo de exibição que retorna a camada.

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>Tópicos relacionados

* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo do build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo de aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)
