---
author: jwmsft
ms.assetid: 40122343-1FE3-4160-BABE-6A2DD9AF1E8E
title: Otimizar o acesso a arquivos
description: Crie apps da Plataforma Universal do Windows (UWP) que acessem o sistema de arquivos de modo eficiente, evitando problemas de desempenho devido à latência de disco e aos ciclos de memória/CPU.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b0b1a45bc967dd69d38f2e85609a5e13ffd61b8
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6462195"
---
# <a name="optimize-file-access"></a>Otimizar o acesso a arquivos


Crie aplicativos da Plataforma Universal do Windows (UWP) que acessem o sistema de arquivos de modo eficiente, evitando problemas de desempenho devido à latência de disco e aos ciclos de memória/CPU.

Quando quiser acessar uma grande coleção de arquivos e desejar acessar valores de propriedade que não as propriedades típicas Name, FileType e Path, acesse-os criando [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) e chamando [**SetPropertyPrefetch**](https://msdn.microsoft.com/library/windows/apps/hh973319). O método **SetPropertyPrefetch** pode melhorar bastante o desempenho de aplicativos que exibem uma coleção de itens obtidos do sistema de arquivos, como uma coleção de imagens. O próximo grupo de exemplos mostra algumas maneiras de acessar vários arquivos.

O primeiro exemplo usa [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273) para recuperar as informações de nome para um conjunto de arquivos. Essa abordagem proporciona um bom desempenho, porque o exemplo acessa apenas a propriedade de nome.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> 
> for (int i = 0; i < files.Count; i++)
> {
>     // do something with the name of each file
>     string fileName = files[i].Name;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) =
>     Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> 
> For i As Integer = 0 To files.Count - 1
>     ' do something with the name of each file
>     Dim fileName As String = files(i).Name
> Next i
> ```

O segundo exemplo usa [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273) e depois recupera as propriedades de imagem para cada arquivo. Essa abordagem proporciona um desempenho ruim.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> for (int i = 0; i < files.Count; i++)
> {
>     ImageProperties imgProps = await files[i].Properties.GetImagePropertiesAsync();
> 
>     // do something with the date the image was taken
>     DateTimeOffset date = imgProps.DateTaken;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) = Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> For i As Integer = 0 To files.Count - 1
>     Dim imgProps As ImageProperties =
>         Await files(i).Properties.GetImagePropertiesAsync()
> 
>     ' do something with the date the image was taken
>     Dim dateTaken As DateTimeOffset = imgProps.DateTaken
> Next i
> ```

O terceiro exemplo usa [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) para obter informações sobre um conjunto de arquivos. Essa abordagem proporciona um desempenho muito melhor do que o exemplo anterior.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> // Set QueryOptions to prefetch our specific properties
> var queryOptions = new Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, null);
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView, 100,
>         ThumbnailOptions.ReturnOnlyIfCached);
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties, 
>        new string[] {"System.Size"});
> 
> StorageFileQueryResult queryResults = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions);
> IReadOnlyList<StorageFile> files = await queryResults.GetFilesAsync();
> 
> foreach (var file in files)
> {
>     ImageProperties imageProperties = await file.Properties.GetImagePropertiesAsync();
> 
>     // Do something with the date the image was taken.
>     DateTimeOffset dateTaken = imageProperties.DateTaken;
> 
>     // Performance gains increase with the number of properties that are accessed.
>     IDictionary<String, object> propertyResults =
>         await file.Properties.RetrievePropertiesAsync(
>               new string[] {"System.Size" });
> 
>     // Get/Set extra properties here
>     var systemSize = propertyResults["System.Size"];
> }
> ```
> ```vb
> ' Set QueryOptions to prefetch our specific properties
> Dim queryOptions = New Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, Nothing)
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView,
>             100, Windows.Storage.FileProperties.ThumbnailOptions.ReturnOnlyIfCached)
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties,
>                                  New String() {"System.Size"})
> 
> Dim queryResults As StorageFileQueryResult = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions)
> Dim files As IReadOnlyList(Of StorageFile) = Await queryResults.GetFilesAsync()
> 
> 
> For Each file In files
>     Dim imageProperties As ImageProperties = Await file.Properties.GetImagePropertiesAsync()
> 
>     ' Do something with the date the image was taken.
>     Dim dateTaken As DateTimeOffset = imageProperties.DateTaken
> 
>     ' Performance gains increase with the number of properties that are accessed.
>     Dim propertyResults As IDictionary(Of String, Object) =
>         Await file.Properties.RetrievePropertiesAsync(New String() {"System.Size"})
> 
>     ' Get/Set extra properties here
>     Dim systemSize = propertyResults("System.Size")
> 
> Next file
> ```
Se você estiver realizando várias operações em objetos Windows.Storage, como `Windows.Storage.ApplicationData.Current.LocalFolder`, crie uma variável local para fazer referência a essa fonte de armazenamento, de forma que você não precise recriar objetos intermediários toda vez que a acessar.

## <a name="stream-performance-in-c-and-visual-basic"></a>Desempenho de fluxo em C# e Visual Basic

### <a name="buffering-between-uwp-and-net-streams"></a>Buffer entre fluxos UWP e .NET

Há muitos cenários em que você pode converter um fluxo UWP (como um [**Windows.Storage.Streams.IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) ou [**IOutputStream**](https://msdn.microsoft.com/library/windows/apps/BR241728)) em fluxo .NET ([**System.IO.Stream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.aspx)). Isso é útil quando você está escrevendo um aplicativo UWP e quer usar um código .NET existente que funcione em fluxos com o sistema de arquivo UWP, por exemplo. Para habilitar isso, as APIs .NET para aplicativos UWP fornece métodos de extensão que permitem a conversão entre tipos de fluxo .NET e UWP. Para obter mais informações, consulte [**WindowsRuntimeStreamExtensions**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.aspx).

Quando você converte um fluxo UWP em um fluxo .NET, você cria efetivamente um adaptador para o fluxo UWP subjacente. Em algumas circunstâncias, há um custo de tempo de execução associado à invocação de métodos em fluxos UWP. Esse fator pode afetar a velocidade do seu aplicativo, especialmente em cenários em que você executa muitas operações pequenas e frequentes de leitura ou gravação.

Para acelerar os aplicativos, os adaptadores de fluxo UWP contêm um buffer de dados. A amostra de código a seguir demonstra pequenas leituras consecutivas com o uso de um adaptador de fluxo UWP com um tamanho de buffer padrão.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFile file = await Windows.Storage.ApplicationData.Current
>     .LocalFolder.GetFileAsync("example.txt");
> Windows.Storage.Streams.IInputStream windowsRuntimeStream = 
>     await file.OpenReadAsync();
> 
> byte[] destinationArray = new byte[8];
> 
> // Create an adapter with the default buffer size.
> using (var managedStream = windowsRuntimeStream.AsStreamForRead())
> {
> 
>     // Read 8 bytes into destinationArray.
>     // A larger block is actually read from the underlying 
>     // windowsRuntimeStream and buffered within the adapter.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> 
>     // Read 8 more bytes into destinationArray.
>     // This call may complete much faster than the first call
>     // because the data is buffered and no call to the 
>     // underlying windowsRuntimeStream needs to be made.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> }
> ```
> ```vb
> Dim file As StorageFile = Await Windows.Storage.ApplicationData.Current -
> .LocalFolder.GetFileAsync("example.txt")
> Dim windowsRuntimeStream As Windows.Storage.Streams.IInputStream =
>     Await file.OpenReadAsync()
> 
> Dim destinationArray() As Byte = New Byte(8) {}
> 
> ' Create an adapter with the default buffer size.
> Dim managedStream As Stream = windowsRuntimeStream.AsStreamForRead()
> Using (managedStream)
> 
>     ' Read 8 bytes into destinationArray.
>     ' A larger block is actually read from the underlying 
>     ' windowsRuntimeStream and buffered within the adapter.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
>     ' Read 8 more bytes into destinationArray.
>     ' This call may complete much faster than the first call
>     ' because the data is buffered and no call to the 
>     ' underlying windowsRuntimeStream needs to be made.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
> End Using
> ```

Esse comportamento de buffer padrão é desejável na maioria dos cenários em que você converte um fluxo UWP em fluxo .NET. No entanto, em alguns cenários, pode você querer ajustar o comportamento do buffer para melhorar o desempenho.

### <a name="working-with-large-data-sets"></a>Trabalhando com grandes conjuntos de dados

Quando estiver lendo ou gravando conjuntos de dados maiores, você talvez consiga aumentar o rendimento de sua leitura ou gravação fornecendo um tamanho de buffer maior para os métodos de extensão [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) e [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx). Isso dá ao adaptador de fluxo um tamanho de buffer interno maior. Por exemplo, na transferência do fluxo de um arquivo grande para um analisador XML, o analisador pode executar várias leituras pequenas e sequenciais do fluxo. Um buffer grande pode reduzir a quantidade de chamadas para o fluxo UWP subjacente e impulsionar o desempenho.

> **Observação**  você deve ter cuidado ao definir um tamanho de buffer que é maior do que aproximadamente 80 KB, pois isso pode causar fragmentação na pilha de coletor de lixo (consulte [melhorar desempenho da coleta de lixo](improve-garbage-collection-performance.md)). O exemplo de código a seguir cria um adaptador de fluxo gerenciado com um buffer de 81.920 bytes.

> [!div class="tabbedCodeSnippets"]
```csharp
// Create a stream adapter with an 80 KB buffer.
Stream managedStream = nativeStream.AsStreamForRead(bufferSize: 81920);
```
```vb
' Create a stream adapter with an 80 KB buffer.
Dim managedStream As Stream = nativeStream.AsStreamForRead(bufferSize:=81920)
```

Os métodos [**Stream.CopyTo**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copyto.aspx) e [**CopyToAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copytoasync.aspx) também alocam um buffer local para copiar entre os fluxos. Como com o método de extensão [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx), você pode ser capaz de obter melhor desempenho para copias de fluxo grandes anulando o tamanho de buffer padrão. O exemplo de código a seguir demonstra a alteração no tamanho de buffer padrão de uma chamada **CopyToAsync**.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> MemoryStream destination = new MemoryStream();
> // copies the buffer into memory using the default copy buffer
> await managedStream.CopyToAsync(destination);
> 
> // Copy the buffer into memory using a 1 MB copy buffer.
> await managedStream.CopyToAsync(destination, bufferSize: 1024 * 1024);
> ```
> ```vb
> Dim destination As MemoryStream = New MemoryStream()
> ' copies the buffer into memory using the default copy buffer
> Await managedStream.CopyToAsync(destination)
> 
> ' Copy the buffer into memory using a 1 MB copy buffer.
> Await managedStream.CopyToAsync(destination, bufferSize:=1024 * 1024)
> ```

Esse exemplo usa um tamanho de buffer de 1 MB, que é maior do que o de 80 KB recomendado anteriormente. O uso de um buffer tão grande pode melhorar a produtividade da operação de cópia para conjuntos de dados muito grandes (com várias centenas de megabytes). No entanto, esse buffer é alocado no heap de objetos grandes e pode reduzir potencialmente o desempenho da coleta de lixo. Recomendamos que você use buffers grandes apenas se isso resultar em uma melhora substancial no desempenho do seu aplicativo.

Quando você está trabalhando com uma grande quantidade de fluxos simultaneamente, é bom reduzir ou eliminar a sobrecarga de memória do buffer. Você pode especificar um buffer menor ou definir o parâmetro *bufferSize* para 0, para desligar completamente o buffering para esse adaptador de fluxo. Você também pode obter um bom desempenho de produtividade sem buffer, se executar leituras e gravações grandes no fluxo gerenciado.

### <a name="performing-latency-sensitive-operations"></a>Executando operações dependentes de latência

Você também pode querer evitar o buffer se preferir leituras e gravações de baixa latência e não quiser ler em blocos grandes do fluxo UWP adjacente. Por exemplo, leituras e gravações de baixa latência podem ser desejáveis, se você está usando o fluxo para comunicações em rede.

Em um aplicativo de chat, é possível usar um fluxo por uma interface de rede para enviar e receber mensagens. Nesse caso, você quer enviar mensagens assim que estiverem prontas, e não esperar até o buffer ficar cheio. Se você definir o tamanho do buffer para 0 quando chamar os métodos de extensão [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) e [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx), o adaptador resultante não alocará um buffer e todas as chamadas manipularão o fluxo UWP subjacente diretamente.


