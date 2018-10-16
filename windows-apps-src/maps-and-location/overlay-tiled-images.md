---
author: normesta
title: Sobrepor imagens lado a lado em um mapa
description: Sobreponha imagens em blocos de terceiros ou personalizados em um mapa usando fontes de blocos. Use fontes de blocos para sobrepor informações especializadas, como dados de previsão do tempo, dados de população ou dados sísmicos; ou use fontes de blocos para substituir por completo o mapa padrão.
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.author: normesta
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, mapa, local, imagens, sobreposição
ms.localizationpriority: medium
ms.openlocfilehash: ba1f7d52a1b16fbb421202229ce724dab384ffa0
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4617848"
---
# <a name="overlay-tiled-images-on-a-map"></a>Sobrepor imagens lado a lado em um mapa

Sobreponha imagens em blocos de terceiros ou personalizados em um mapa usando fontes de blocos. Use fontes de blocos para sobrepor informações especializadas, como dados de previsão do tempo, dados de população ou dados sísmicos; ou use fontes de blocos para substituir por completo o mapa padrão.

**Dica** Para saber mais sobre como usar mapas em seu app, baixe o [exemplo de mapa da Plataforma Universal do Windows (UWP) (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977) do Github.

<a id="tileintro" />

## <a name="tiled-image-overview"></a>Visão geral da imagem lado a lado

Serviços de mapa como Nokia Maps e Bing Mapas cortam mapas em blocos quadrados para recuperação e exibição rápidas. Esses blocos têm 256 pixels por 256 pixels de tamanho e são previamente renderizados em vários níveis de detalhes. Vários serviços de terceiros também oferecem dados baseados em mapa cortados em blocos. Use fontes de blocos para recuperar blocos de terceiros ou criar seus próprios blocos personalizados e os sobreponha no mapa exibido no [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

**Importante**  
Quando você usa fontes de blocos, não precisa escrever código para solicitar ou posicionar blocos individuais. O [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) solicita blocos conforme a necessidade. Cada solicitação especifica as coordenadas X e Y e o nível de zoom para o bloco individual. Você simplesmente especifica o formado do Uri ou do nome de arquivo para usar para recuperar os blocos na propriedade **UriFormatString**. Ou seja, você deve inserir parâmetros substituíveis no Uri ou nome de arquivo de base para indicar para onde passar as coordenadas X e Y e o nível de zoom para cada bloco.

Aqui está um exemplo da propriedade [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) para um [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) que mostra os parâmetros substituíveis para as coordenadas X e Y e o nível de zoom.

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

(As coordenadas X e Y representam a localização do bloco individual dentro do mapa do mundo no nível de detalhe especificado. O sistema de numeração de bloco começa em {0, 0} no canto superior esquerdo do mapa. Por exemplo, o bloco em {1, 2} está na segunda coluna da terceira linha da grade de blocos).

Para obter mais informações sobre o sistema de blocos usado pelos serviços de mapeamento, consulte [Sistema lado a lado do Bing Mapas](http://go.microsoft.com/fwlink/p/?LinkId=626692).

### <a name="overlay-tiles-from-a-tile-source"></a>Sobreponha blocos de uma fonte de blocos

Sobreponha imagens lado a lado de uma fonte de blocos em um mapa usando a [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141).

1.  Instancie uma das três classes de fontes de dados de blocos que herdam da [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141).

    -   [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)
    -   [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)
    -   [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983)

    Configure o **UriFormatString** para usar para solicitar os blocos inserindo parâmetros substituíveis no Uri ou no nome do arquivo de base.

    O exemplo a seguir instancia um [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986). Este exemplo especifica o valor do [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) no construtor do **HttpMapTileDataSource**.

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  Instancie e configure um [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144). Especifique o [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141) que você configurou no passo anterior como o [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149) do **MapTileSource**.

    O exemplo a seguir especifica o [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149) no construtor do [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    Você pode restringir as condições em que os blocos são exibidos usando propriedades do [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).

    -   Exiba blocos somente em uma área geográfica específica fornecendo um valor para a propriedade [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147).
    -   Exiba blocos somente em determinados níveis de detalhe fornecendo um valor para a propriedade [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171).

    Opcionalmente, configure outras propriedades do [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) que afetam o carregamento ou a exibição dos blocos, como [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157), [**AllowOverstretch**](https://msdn.microsoft.com/library/windows/apps/dn637145), [**IsRetryEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637153) e [**IsTransparencyEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637155).

3.  Adicione o [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) para a coleção [**TileSources**](https://msdn.microsoft.com/library/windows/apps/dn637053) do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>Sobreponha blocos de um serviço Web


Sobreponha imagens lado a lado recuperadas de um serviço Web usando o [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986).

1.  Instancie um [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986).
2.  Especifique o formado do Uri que o serviço Web espera como o valor da propriedade [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992). Para criar esse valor, insira parâmetros substituíveis no Uri base. Por exemplo, no exemplo de código a seguir, o valor do **UriFormatString** é:

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    O serviço Web precisar oferecer suporte para um Uri que contenha os parâmetros substituíveis {x}, {y} e {zoomlevel}. A maioria dos serviços Web (por exemplo, Nokia, Bing e Google) oferece suporte a URIs nesse formato. Se o serviço Web exigir argumentos adicionais que não estiverem disponíveis com a propriedade [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992), você deve criar um Uri personalizado. Crie e retorne um Uri personalizado manipulando o evento [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn636993). Para obter mais informações, consulte a seção [Forneça um URI personalizado](#customuri) posteriormente neste tópico.

3.  Em seguida, siga as etapas restantes descritas anteriormente na seção [Visão geral da imagem lado a lado](#tileintro).

O exemplo a seguir sobrepõe blocos de um serviço Web fictício em um mapa da América do Norte. O valor da [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) é especificado no construtor do [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986). Neste exemplo, os blocos são exibidos apenas nos limites geográficos especificados pela propriedade [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147) opcional.

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>Sobreponha blocos de um armazenamento local


Sobreponha imagens lado a lado armazenadas como arquivos em armazenamento local usando o [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994). Tipicamente, você empacota e distribui esses arquivos com seu aplicativo.

1.  Instancie um [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994).
2.  Especifique o formado dos nomes dos arquivos como o valor da propriedade [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998). Para criar esse valor, insira parâmetros substituíveis no nome de arquivo base. Por exemplo, no exemplo de código a seguir, o valor do [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) é:

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    Se o formato dos nomes dos arquivos exigir argumentos adicionais que não estiverem disponíveis com a propriedade [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998), você deve criar um Uri personalizado. Crie e retorne um Uri personalizado manipulando o evento [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn637001). Para obter mais informações, consulte a seção [Forneça um URI personalizado](#customuri) posteriormente neste tópico.

3.  Em seguida, siga as etapas restantes descritas anteriormente na seção [Visão geral da imagem lado a lado](#tileintro).

Você pode usar os seguintes protocolos e locais para carregar blocos do armazenamento local:

| Uri | Mais informações |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | Aponta para a raiz da pasta de instalação do aplicativo. |
|  | Esse é o local referenciado pela propriedade [Package.InstalledLocation](https://msdn.microsoft.com/library/windows/apps/br224681). |
| ms-appdata:///local | Aponta para a raiz do armazenamento local do aplicativo. |
|  | Este é o local referenciado pela propriedade [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621). |
| ms-appdata:///temp | Aponta para a pasta temporária do aplicativo. |
|  | Este é o local referenciado pela propriedade [ApplicationData.TemporaryFolder](https://msdn.microsoft.com/library/windows/apps/br241629). |

 

O exemplo a seguir carrega blocos que são armazenados como arquivos na pasta de instalação do aplicativo usando o protocolo `ms-appx:///`. O valor da [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) é especificado no construtor do [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994). Neste exemplo, os blocos são exibidos apenas quando o nível de zoom do mapa está dentro da margem especificada pela propriedade [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171) opcional.

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>Forneça um URI personalizado

Se os parâmetros substituíveis disponíveis com a propriedade [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) do [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) ou a propriedade [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) do [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994) não são suficientes para recuperar seus blocos, você deve criar um Uri personalizado. Crie e retorne um Uri personalizado fornecendo um manipulador personalizado para o evento **UriRequested**. O evento **UriRequested** é gerado para cada bloco individual.

1.  Em seu manipulador personalizado para o evento **UriRequested**, junte os argumentos personalizados exigidos com as propriedades [**X**](https://msdn.microsoft.com/library/windows/apps/dn610743), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn610744) e [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn610745) do [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177) para criar o Uri personalizado.
2.  Retorne o Uri personalizado na propriedade [**Uri**](https://msdn.microsoft.com/library/windows/apps/dn610748) do [**MapTileUriRequest**](https://msdn.microsoft.com/library/windows/apps/dn637173), que está contido na propriedade [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637179) do [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177).

O exemplo a seguir mostra como fornecer um Uri personalizado criando um manipulador personalizado para o evento **UriRequested**. Ele também mostra como implementar o padrão de adiamento se você tiver que fazer algo assincronamente para criar o Uri personalizado.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>Sobreponha blocos de uma fonte personalizada

Sobreponha blocos personalizados usando o [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983). Crie blocos programaticamente na memória em tempo real ou escreva seu próprio código para carregar blocos existentes de outra fonte.

Para criar ou carregar blocos personalizados. forneça um manipulador personalizado para o evento [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984). O evento **BitmapRequested** é gerado para cada bloco individual.

1.  Em seu manipulador personalizado para o evento [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984), junte os argumentos personalizados exigidos com as propriedades [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) e [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) do [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132) para criar ou recuperar um bloco personalizado.
2.  Retorne o bloco personalizado na propriedade [**PixelData**](https://msdn.microsoft.com/library/windows/apps/dn637140) do [**MapTileBitmapRequest**](https://msdn.microsoft.com/library/windows/apps/dn637128), que está contido na propriedade [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637134) do [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132). A propriedade **PixelData** é do tipo [**IRandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701664).

O exemplo a seguir mostra como fornecer blocos personalizados criando um manipulador personalizado para o evento **BitmapRequested**. Este exemplo cria blocos vermelhos idênticos que são parcialmente opacos. O exemplo ignora as propriedades [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) e [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) do [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132). Embora esse não seja um exemplo do mundo real, ele demonstra como você pode criar blocos personalizados na memória em tempo real. O exemplo também mostra como implementar o padrão de adiamento se você tiver de fazer algo assincronamente para criar os blocos personalizados.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>Substituir o mapa padrão

Para substituir o mapa padrão inteiramente com blocos de terceiros ou personalizados:

-   Especifique [**MapTileLayer**](https://msdn.microsoft.com/library/windows/apps/dn637143).**BackgroundReplacement** como o valor da propriedade [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157) do [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).
-   Especifique [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127).**None** como o valor da propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) do [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

## <a name="related-topics"></a>Tópicos relacionados

* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo do build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo do aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
