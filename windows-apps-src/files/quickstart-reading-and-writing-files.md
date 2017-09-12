---
author: laurenhughes
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: Criar, gravar e ler um arquivo
description: Leia e grave um arquivo usando o objeto StorageFile.
ms.author: lahugh
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d566bac72b9e3a7e00adb9225342017168ca5f43
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/06/2017
---
# <a name="create-write-and-read-a-file"></a>Criar, gravar e ler um arquivo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**Classe StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**Classe StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**Classe FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440)

Leia e grave um arquivo usando um objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

> [!NOTE] 
> Veja também o [Exemplo de acesso a arquivos](http://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Saiba como obter o arquivo que você quer ler, gravar ou ambos.**

    Você pode aprender a obter um arquivo usando um seletor de arquivos em [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md).

## <a name="creating-a-file"></a>Criando um arquivo

Consulte aqui como criar um arquivo na pasta local do aplicativo. Se ele já existir, nós o substituiremos.

> [!div class="tabbedCodeSnippets"]
```cs  
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
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

Veja aqui como gravar em um arquivo gravável no disco usando a classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). A primeira etapa comum para cada uma das maneiras de gravar em um arquivo (a menos que você grave no arquivo imediatamente depois de criá-lo) é obter o arquivo com [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272).

> [!div class="tabbedCodeSnippets"]
```cs  
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
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

Grave texto no seu arquivo chamando o método [**WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505) da classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).

> [!div class="tabbedCodeSnippets"]
```cs  
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
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

1.  Primeiro, chame [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385) para obter um buffer dos bytes (com base em uma cadeia de caracteres arbitrária) que você deseja gravar no arquivo.

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
            "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
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

2.  Em seguida, grave os bytes do seu buffer no seu arquivo chamando o método [**WriteBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701490) da classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
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

1.  Primeiramente, abra o arquivo chamando o método [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851). Ele retorna um fluxo de conteúdo do arquivo quando a operação de abertura é concluída.

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
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

2.  Em seguida, obtenha um fluxo de saída chamando o método [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738) de `stream`. Coloque isso em uma declaração **using** para gerenciar o ciclo de vida do fluxo de saída.

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
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

3.  Agora, adicione esse código à instrução **using** existente para gravar no fluxo de saída, criando um novo objeto [**DataWriter**](https://msdn.microsoft.com/library/windows/apps/br208154) e chamando o método [**DataWriter.WriteString**](https://msdn.microsoft.com/library/windows/apps/br241642).

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
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

4.  Por fim, adicione esse código (na instrução **using** interna) para salvar o texto em seu arquivo com [**StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) e feche o fluxo com [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br241729).

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
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

Veja aqui como ler um arquivo no disco usando a classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). A primeira etapa comum para cada uma das maneiras de ler um arquivo é obter o arquivo com [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272).

> [!div class="tabbedCodeSnippets"]
```cs  
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
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

Leia texto do seu arquivo chamando o método [**ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) da classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).

> [!div class="tabbedCodeSnippets"]
```cs  
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
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

1.  Chame inicialmente o método [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701468) da classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
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

2.  Em seguida, use o objeto [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) para ler primeiro o tamanho do buffer e depois seu conteúdo.

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
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

1.  Abra um fluxo para o seu arquivo chamando o método [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851). Ele retorna um fluxo de conteúdo do arquivo quando a operação é concluída.

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
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

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    ulong size = stream.Size;
    ```
    ```cpp  
    // Add to "Process stream" from part 1
    UINT64 size = stream->Size;
    ```
    ```vb  
    Dim size = stream.Size
    ```

3.  Obtenha um fluxo de entrada chamando o método [**GetInputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241737). Coloque isso em uma declaração **using** para gerenciar o ciclo de vida do fluxo de entrada. Especifique 0 ao chamar **GetInputStreamAt** para definir a posição para o início do fluxo.

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
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

4.  Por fim, adicione este código à instrução **using** existente para obter um objeto [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) no fluxo e, em seguida, leia o texto chamando [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) e [**DataReader.ReadString**](https://msdn.microsoft.com/library/windows/apps/br208147).

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
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

 

 
