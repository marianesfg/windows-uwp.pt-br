---
author: laurenhughes
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: Criar, gravar e ler um arquivo
description: Leia e grave um arquivo usando o objeto StorageFile.
ms.author: lahugh
ms.date: 06/28/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 9bc19460fe1b9b9c6b637606a737e1157d98feef
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5826204"
---
# <a name="create-write-and-read-a-file"></a>Criar, gravar e ler um arquivo

**APIs importantes**

-   [**Classe StorageFolder**](/uwp/api/windows.storage.storagefolder)
-   [**Classe StorageFile**](/uwp/api/windows.storage.storagefile)
-   [**Classe FileIO**](/uwp/api/windows.storage.fileio)

Leia e grave um arquivo usando um objeto [**StorageFile**](/uwp/api/windows.storage.storagefile).

> [!NOTE]
> Veja também o [Exemplo de acesso a arquivos](http://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para aprender a escrever aplicativos assíncronos em C++ c++ WinRT, consulte [simultaneidade e operações assíncronas com C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency). Para aprender a escrever aplicativos assíncronos em C++ c++ /CX, consulte [programação assíncrona em C++ c++ /CX](/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Saiba como obter o arquivo que você quer ler, gravar ou ambos.**

    Você pode aprender a obter um arquivo usando um seletor de arquivos em [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md).

## <a name="creating-a-file"></a>Criando um arquivo

Consulte aqui como criar um arquivo na pasta local do aplicativo. Se ele já existir, nós o substituiremos.

```csharp
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Create a sample file; replace if exists.
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    co_await storageFolder.CreateFileAsync(L"sample.txt", Windows::Storage::CreationCollisionOption::ReplaceExisting);
}
```

```cpp
// Create a sample file; replace if exists.
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
concurrency::create_task(storageFolder->CreateFileAsync("sample.txt", CreationCollisionOption::ReplaceExisting));
```

```vb
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## <a name="writing-to-a-file"></a>Gravando em um arquivo

Veja aqui como gravar em um arquivo gravável no disco usando a classe [**StorageFile**](/uwp/api/windows.storage.storagefile). A primeira etapa comum para cada uma das maneiras de gravar em um arquivo (a menos que você grave no arquivo imediatamente depois de criá-lo) é obter o arquivo com [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync).

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.CreateFileAsync(L"sample.txt", Windows::Storage::CreationCollisionOption::ReplaceExisting) };
    // Process sampleFile
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    // Process file
});
```

```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Gravando texto em um arquivo**

Grave texto no seu arquivo chamando o método [**FileIO.WriteTextAsync**](/uwp/api/windows.storage.fileio.writetextasync) .

```csharp
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    // Write text to the file.
    co_await Windows::Storage::FileIO::WriteTextAsync(sampleFile, L"Swift as a shadow");
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    //Write text to a file
    create_task(FileIO::WriteTextAsync(sampleFile, "Swift as a shadow"));
});
```

```vb
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**Gravando bytes em um arquivo usando um buffer (2 etapas)**

1.  Primeiro, chame [**CryptographicBuffer.ConvertStringToBinary**](/uwp/api/windows.security.cryptography.cryptographicbuffer.convertstringtobinary) para obter um buffer dos bytes (com base em uma cadeia de caracteres) que você deseja gravar em seu arquivo.

```csharp
var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    // Create the buffer.
    Windows::Storage::Streams::IBuffer buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"What fools these mortals be", Windows::Security::Cryptography::BinaryStringEncoding::Utf8)};
    // The code in step 2 goes here.
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    // Create the buffer
    IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
    ("What fools these mortals be", BinaryStringEncoding::Utf8);
});
```

```vb
Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    "What fools these mortals be",
    Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
```

2.  Em seguida, grave os bytes do seu buffer no seu arquivo chamando o método [**FileIO.WriteBufferAsync**](/uwp/api/windows.storage.fileio.writebufferasync) .

```csharp
await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
```

```cppwinrt
co_await Windows::Storage::FileIO::WriteBufferAsync(sampleFile, buffer);
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    // Create the buffer
    IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
    ("What fools these mortals be", BinaryStringEncoding::Utf8);      
    // Write bytes to a file using a buffer
    create_task(FileIO::WriteBufferAsync(sampleFile, buffer));
});
```

```vb
Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
```

**Gravando texto em um arquivo usando um fluxo (4 etapas)**

1.  Primeiramente, abra o arquivo chamando o método [**StorageFile.OpenAsync**](/uwp/api/windows.storage.storagefile.openasync). Ele retorna um fluxo de conteúdo do arquivo quando a operação de abertura é concluída.

```csharp
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    Windows::Storage::Streams::IRandomAccessStream stream{ co_await sampleFile.OpenAsync(Windows::Storage::FileAccessMode::ReadWrite) };
    // The code in step 2 goes here.
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    create_task(sampleFile->OpenAsync(FileAccessMode::ReadWrite)).then([sampleFile](IRandomAccessStream^ stream)
    {
        // Process stream
    });
});
```

```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  Em seguida, obtenha um fluxo de saída chamando o método [**IRandomAccessStream.GetOutputStreamAt**](/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) do `stream`. Se você estiver usando c#, em seguida, inclua isso em uma instrução **using** para gerenciar o tempo de vida do fluxo de saída. Se você estiver usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), em seguida, você pode controlar seu tempo de vida envolve em um bloco ou defini-lo como `nullptr` quando terminar com ele.

```csharp
using (var outputStream = stream.GetOutputStreamAt(0))
{
    // We'll add more code here in the next step.
}
stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
```

```cppwinrt
Windows::Storage::Streams::IOutputStream outputStream{ stream.GetOutputStreamAt(0) };
// The code in step 3 goes here.
```

```cpp
// Add to "Process stream" in part 1
IOutputStream^ outputStream = stream->GetOutputStreamAt(0);
```

```vb
Using outputStream = stream.GetOutputStreamAt(0)
' We'll add more code here in the next step.
End Using
```

3.  Agora, adicione esse código (se você estiver usando c#, dentro da declaração **using** existente) para gravar no fluxo de saída, criando um novo objeto [**DataWriter**](/uwp/api/windows.storage.streams.datawriter) e chamando o método [**writeString**](/uwp/api/windows.storage.streams.datawriter.writestring) .

```csharp
using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
{
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
}
```

```cppwinrt
Windows::Storage::Streams::DataWriter dataWriter;
dataWriter.WriteString(L"DataWriter has methods to write to various types, such as DataTimeOffset.");
// The code in step 4 goes here.
```

```cpp
// Added after code from part 2
DataWriter^ dataWriter = ref new DataWriter(outputStream);
dataWriter->WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
```

```vb
Dim dataWriter As New DataWriter(outputStream)
dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
```

4.  Por fim, adicione esse código (se você estiver usando c#, dentro da declaração interna **usando** ) para salvar o texto em seu arquivo com [**datawriter. Storeasync**](/uwp/api/windows.storage.streams.datawriter.storeasync) e feche o fluxo com [**IOutputStream.FlushAsync**](/uwp/api/windows.storage.streams.ioutputstream.flushasync).

```csharp
await dataWriter.StoreAsync();
await outputStream.FlushAsync();
```

```cppwinrt
dataWriter.StoreAsync();
outputStream.FlushAsync();
```

```cpp
// Added after code from part 3
dataWriter->StoreAsync();
outputStream->FlushAsync();
```

```vb
Await dataWriter.StoreAsync()
Await outputStream.FlushAsync()
```

## <a name="reading-from-a-file"></a>Lendo um arquivo

Veja aqui como ler um arquivo no disco usando a classe [**StorageFile**](/uwp/api/Windows.Storage.StorageFile). A primeira etapa comum para cada uma das maneiras de ler um arquivo é obter o arquivo com [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync).

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
// Process file
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    // Process file
});
```

```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Lendo texto de um arquivo**

Leia o texto do seu arquivo chamando o método [**FileIO.ReadTextAsync**](/uwp/api/windows.storage.fileio.readtextasync) .

```csharp
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```

```cppwinrt
Windows::Foundation::IAsyncOperation<winrt::hstring> ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    co_return co_await Windows::Storage::FileIO::ReadTextAsync(sampleFile);
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    return FileIO::ReadTextAsync(sampleFile);
});
```

```vb
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**Leitura do texto de um arquivo usando um buffer (2 etapas)**

1.  Primeiro, chame o método [**FileIO.ReadBufferAsync**](/uwp/api/windows.storage.fileio.readbufferasync) .

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
Windows::Storage::Streams::IBuffer buffer{ co_await Windows::Storage::FileIO::ReadBufferAsync(sampleFile) };
// The code in step 2 goes here.
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    return FileIO::ReadBufferAsync(sampleFile);

}).then([](Streams::IBuffer^ buffer)
{
    // Process buffer
});
```

```vb
Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
```

2.  Em seguida, use o objeto [**DataReader**](/uwp/api/windows.storage.streams.datareader) para ler primeiro o tamanho do buffer e depois seu conteúdo.

```csharp
using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
{
    string text = dataReader.ReadString(buffer.Length);
}
```

```cppwinrt
auto dataReader{ Windows::Storage::Streams::DataReader::FromBuffer(buffer) };
winrt::hstring bufferText{ dataReader.ReadString(buffer.Length()) };
```

```cpp
// Add to "Process buffer" section from part 1
auto dataReader = DataReader::FromBuffer(buffer);
String^ bufferText = dataReader->ReadString(buffer->Length);
```

```vb
Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
Dim text As String = dataReader.ReadString(buffer.Length)
```

**Lendo texto de um arquivo usando um fluxo (4 etapas)**

1.  Abra um fluxo para o seu arquivo chamando o método [**StorageFile.OpenAsync**](/uwp/api/windows.storage.storagefile.openasync). Ele retorna um fluxo de conteúdo do arquivo quando a operação é concluída.

```csharp
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
Windows::Storage::Streams::IRandomAccessStream stream{ co_await sampleFile.OpenAsync(Windows::Storage::FileAccessMode::Read) };
// The code in step 2 goes here.
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    create_task(sampleFile->OpenAsync(FileAccessMode::Read)).then([sampleFile](IRandomAccessStream^ stream)
    {
        // Process stream
    });
});
```

```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read)
```

2.  Obtenha o tamanho do fluxo para usar mais tarde.

```csharp
ulong size = stream.Size;
```

```cppwinrt
uint64_t size{ stream.Size() };
// The code in step 3 goes here.
```

```cpp
// Add to "Process stream" from part 1
UINT64 size = stream->Size;
```

```vb
Dim size = stream.Size
```

3.  Obtenha um fluxo de entrada chamando o método [**IRandomAccessStream.GetInputStreamAt**](/uwp/api/windows.storage.streams.irandomaccessstream.getinputstreamat) . Coloque isso em uma declaração **using** para gerenciar o ciclo de vida do fluxo de entrada. Especifique 0 ao chamar **GetInputStreamAt** para definir a posição para o início do fluxo.

```csharp
using (var inputStream = stream.GetInputStreamAt(0))
{
    // We'll add more code here in the next step.
}
```

```cppwinrt
Windows::Storage::Streams::IInputStream inputStream{ stream.GetInputStreamAt(0) };
Windows::Storage::Streams::DataReader dataReader{ inputStream };
// The code in step 4 goes here.
```

```cpp
// Add after code from part 2
IInputStream^ inputStream = stream->GetInputStreamAt(0);
auto dataReader = ref new DataReader(inputStream);
```

```vb
Using inputStream = stream.GetInputStreamAt(0)
' We'll add more code here in the next step.
End Using
```

4.  Por fim, adicione este código à instrução **using** existente para obter um objeto [**DataReader**](/uwp/api/windows.storage.streams.datareader) no fluxo e, em seguida, leia o texto chamando [**DataReader.LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync) e [**DataReader.ReadString**](/uwp/api/windows.storage.streams.datareader.readstring).

```csharp
using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
{
    uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
    string text = dataReader.ReadString(numBytesLoaded);
}
```

```cppwinrt
unsigned int cBytesLoaded{ co_await dataReader.LoadAsync(size) };
winrt::hstring streamText{ dataReader.ReadString(cBytesLoaded) };
```

```cpp
// Add after code from part 3
create_task(dataReader->LoadAsync(size)).then([sampleFile, dataReader](unsigned int numBytesLoaded)
{
    String^ streamText = dataReader->ReadString(numBytesLoaded);
});
```

```vb
Dim dataReader As New DataReader(inputStream)
Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
Dim text As String = dataReader.ReadString(numBytesLoaded)
```
