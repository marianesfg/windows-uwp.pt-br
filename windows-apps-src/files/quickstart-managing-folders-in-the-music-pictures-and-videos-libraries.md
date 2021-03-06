---
ms.assetid: 1AE29512-7A7D-4179-ADAC-F02819AC2C39
title: Como gerenciar as bibliotecas de música, imagens e vídeos
description: Adicione pastas existentes de música, fotos ou vídeos às bibliotecas correspondentes. Você também pode remover pastas de bibliotecas, obter a lista de pastas em uma biblioteca e descobrir fotos, músicas e vídeos armazenados.
ms.date: 06/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c75bd62fb5548cc03247772427fb5aabac4fb5a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74735071"
---
# <a name="files-and-folders-in-the-music-pictures-and-videos-libraries"></a>Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos

Adicione pastas existentes de música, fotos ou vídeos às bibliotecas correspondentes. Você também pode remover pastas de bibliotecas, obter a lista de pastas em uma biblioteca e descobrir fotos, músicas e vídeos armazenados.

Uma biblioteca é uma coleção virtual de pastas, que contém uma pasta conhecida por padrão, além de outras pastas que o usuário tiver adicionado à biblioteca usando seu aplicativo ou um dos aplicativos nativos. Por exemplo, a biblioteca Imagens inclui a pasta Imagens conhecida por padrão. O usuário pode adicionar pastas ou removê-las da biblioteca Imagens usando seu aplicativo ou o aplicativo Fotos nativo.

## <a name="prerequisites"></a>Pré-requisitos


-   **Entender a programação assíncrona para aplicativos UWP (Plataforma Universal do Windows)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permissões de acesso ao local**

    No Visual Studio, abra o arquivo de manifesto do aplicativo no Designer de Manifesto. Na página **Recursos**, selecione as bibliotecas que seu aplicativo gerencia.

    -   **Biblioteca de Músicas**
    -   **Biblioteca de Imagens**
    -   **Biblioteca de Vídeos**

    Para saber mais, consulte [Permissões de acesso a arquivo](file-access-permissions.md).

## <a name="get-a-reference-to-a-library"></a>Obtenha uma referência a uma biblioteca

> [!NOTE]
> Lembre-se de declarar a funcionalidade apropriada. Veja [Declarações de funcionalidade de aplicativo](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para saber mais.
 

Para obter uma referência à biblioteca Música, Imagens ou Vídeo do usuário, chame o método [**StorageLibrary.GetLibraryAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.getlibraryasync). Forneça o valor correspondente da enumeração [**KnownLibraryId**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownLibraryId).

-   [**KnownLibraryId.Music**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.musiclibrary)
-   [**KnownLibraryId.Pictures**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.pictureslibrary)
-   [**KnownLibraryId.Videos**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.videoslibrary)

```cs
var myPictures = await Windows.Storage.StorageLibrary.GetLibraryAsync(Windows.Storage.KnownLibraryId.Pictures);
```

## <a name="get-the-list-of-folders-in-a-library"></a>Obter a lista de pastas em uma biblioteca


Para obter a lista de pastas em uma biblioteca, obtenha o valor da propriedade [**StorageLibrary.Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders).

```cs
using Windows.Foundation.Collections;
IObservableVector<Windows.Storage.StorageFolder> myPictureFolders = myPictures.Folders;
```

## <a name="get-the-folder-in-a-library-where-new-files-are-saved-by-default"></a>Obter a pasta em uma biblioteca onde os novos arquivos são salvos por padrão


Para obter a pasta em uma biblioteca onde os novos arquivos são salvos por padrão, obtenha o valor da propriedade [**StorageLibrary.SaveFolder**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.savefolder).

```cs
Windows.Storage.StorageFolder savePicturesFolder = myPictures.SaveFolder;
```

## <a name="add-an-existing-folder-to-a-library"></a>Adicionar uma pasta existente a uma biblioteca

Para adicionar uma pasta a uma biblioteca, chame [**StorageLibrary.RequestAddFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync). Considerando a biblioteca Imagens como um exemplo, chamar esse método faz com que um seletor de pasta seja mostrado para o usuário com um botão **Adicionar essa pasta a Imagens** . Se o usuário seleciona uma pasta, essa pasta permanecerá em seu local original no disco e se tornará um item na propriedade [**StorageLibrary.Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) (e no aplicativo Fotos nativo), mas a pasta não aparecerá como filho da pasta Imagens no Explorador de Arquivos.


```cs
Windows.Storage.StorageFolder newFolder = await myPictures.RequestAddFolderAsync();
```

## <a name="remove-a-folder-from-a-library"></a>Remover uma pasta de uma biblioteca

Para remover uma pasta de uma biblioteca, chame o método [**StorageLibrary.RequestRemoveFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync) e especifique a pasta a ser removida. Você pode usar [**StorageLibrary.Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) e um controle [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) (ou semelhante) para o usuário selecionar uma pasta para remover.

Quando você chama [**StorageLibrary.RequestRemoveFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync), o usuário vê uma caixa de diálogo de confirmação dizendo que a pasta "não aparecerá mais em Imagens, mas não será excluída". Isso significa que a pasta permanece em seu local original no disco, é removida da propriedade [**StorageLibrary.Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) e não será mais incluída no aplicativo Fotos nativo.

O exemplo a seguir supõe que o usuário selecionou a pasta a ser removida em um [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) controle chamado **lvPictureFolders**.


```cs
bool result = await myPictures.RequestRemoveFolderAsync(folder);
```

## <a name="get-notified-of-changes-to-the-list-of-folders-in-a-library"></a>Receber notificações sobre mudanças na lista de pastas de uma biblioteca


Para ser notificado sobre mudanças na lista de pastas de uma biblioteca, registre um manipulador para o evento [**StorageLibrary.DefinitionChanged**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) da biblioteca.


```cs
myPictures.DefinitionChanged += MyPictures_DefinitionChanged;

void HandleDefinitionChanged(Windows.Storage.StorageLibrary sender, object args)
{
    // ...
}
```

## <a name="media-library-folders"></a>Pastas de bibliotecas de mídia


Um dispositivo fornece cinco locais predefinidos para usuários e aplicativos armazenarem arquivos de mídia. Aplicativos internos armazenam mídia baixada e mídia criada por usuários nesses locais.

Os locais são:

-   Pasta **Imagens**. Contém imagens.

    -   Pasta **Imagens da Câmera** Contém fotos e vídeo da câmera interna.

    -   Pasta **Imagens Salvas**. Contém imagens que o usuário salvou de outros aplicativos.

-   Pasta **Música**. Contém músicas, podcasts e áudio livros.

-   Pasta **Vídeo**. Contém vídeos.

Usuários e aplicativos também armazenam arquivos de mídia fora das pastas de bibliotecas de mídia no cartão SD. Para localizar um arquivo de mídia confiavelmente no cartão SD, examine o conteúdo do cartão SD ou solicite ao usuário que localize o arquivo usando um seletor de arquivos. Para saber mais, consulte [Acessar o cartão SD](access-the-sd-card.md).

## <a name="querying-the-media-libraries"></a>Consultando as bibliotecas de mídia

Para obter uma coleção de arquivos, especifique a biblioteca e o tipo de arquivos que você deseja.

```cs
using Windows.Storage;
using Windows.Storage.Search;

private async void getSongs()
{
    QueryOptions queryOption = new QueryOptions
        (CommonFileQuery.OrderByTitle, new string[] { ".mp3", ".mp4", ".wma" });

    queryOption.FolderDepth = FolderDepth.Deep;

    Queue<IStorageFolder> folders = new Queue<IStorageFolder>();

    var files = await KnownFolders.MusicLibrary.CreateFileQueryWithOptions
      (queryOption).GetFilesAsync();

    foreach (var file in files)
    {
        // do something with the music files
    }
}
```

### <a name="query-results-include-both-internal-and-removable-storage"></a>Os resultados da consulta incluem armazenamento interno e removível

Os usuários podem escolher armazenar arquivos de forma padrão no cartão SD opcional. Os aplicativos, entretanto, podem optar por não permitir que os arquivos sejam armazenados no cartão SD. Com isso, as bibliotecas de mídia podem ficar divididas entre o armazenamento interno do dispositivo e o cartão SD.

Não é necessário gravar código adicional para ter essa possibilidade. Os métodos no namespace [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) que consultam pastas conhecidas, combinam de forma transparente os resultados da consulta dos dois locais. Você também não precisa especificar o recurso **removableStorage** no arquivo de manifesto do aplicativo para obter esses resultados combinados.

Considere o estado do armazenamento do dispositivo mostrado na imagem a seguir:

![imagens no telefone e no cartão SD](images/phone-media-locations.png)

Se você consultar o conteúdo da Biblioteca de imagens chamando o `await KnownFolders.PicturesLibrary.GetFilesAsync()`, os resultados incluem ambos, internalPic.jpg e SDPic.jpg.


## <a name="working-with-photos"></a>Trabalhando com fotos

Nos dispositivos em que a câmera salva imagens de baixa resolução e de alta resolução de cada imagem, as consultas avançadas retornam apenas a imagem de baixa resolução.

As Imagens da câmera e a pasta Imagens salvas não oferecem suporte a consultas avançadas.

**Abrindo uma foto no aplicativo em que ela foi capturada**

Se quiser permitir que o usuário abra novamente uma foto no aplicativo em que foi capturada, você pode salvar a **CreatorAppId** com os metadados da foto usando um código similar ao exemplo seguinte. Neste exemplo, **testPhoto** é um [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile).

```cs
IDictionary<string, object> propertiesToSave = new Dictionary<string, object>();

propertiesToSave.Add("System.CreatorOpenWithUIOptions", 1);
propertiesToSave.Add("System.CreatorAppId", appId);

testPhoto.Properties.SavePropertiesAsync(propertiesToSave).AsyncWait();   
```

## <a name="using-stream-methods-to-add-a-file-to-a-media-library"></a>Usar métodos de fluxo para adicionar um arquivo a uma biblioteca de mídia

Ao acessar uma biblioteca de mídia usando uma pasta conhecida, como a **KnownFolders.PictureLibrary**, e utilizar métodos de fluxo para adicionar um arquivo a uma biblioteca de mídia, você deve certificar-se de fechar todos os fluxos que seu código abre. Caso contrário, esses métodos falham ao adicionar um arquivo à biblioteca de mídia, pois ao menos uma transmissão ainda tem um identificador ao arquivo.

Como exemplo, ao executar o código a seguir, o arquivo não é adicionado à biblioteca de mídia. Na linha do código, `using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))`, os métodos **OpenAsync** e **GetOutputStreamAt** abrem um fluxo. No entanto, somente o fluxo aberto pelo método **GetOutputStreamAt** é descartada como um resultado da instrução **using**. O outro fluxo permanece aberto e impede que o arquivo seja salvo.

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");
using (var sourceStream = (await sourceFile.OpenReadAsync()).GetInputStreamAt(0))
{
    using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))
    {
        await RandomAccessStream.CopyAndCloseAsync(sourceStream, destinationStream);
    }
}
```

Para usar métodos de transmissão com êxito ao adicionar um arquivo à biblioteca de mídia, certifique-se de fechar todos os fluxos abertos pelo seu código, como é mostrado no exemplo a seguir.

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");

using (var sourceStream = await sourceFile.OpenReadAsync())
{
    using (var sourceInputStream = sourceStream.GetInputStreamAt(0))
    {
        using (var destinationStream = await destinationFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            using (var destinationOutputStream = destinationStream.GetOutputStreamAt(0))
            {
                await RandomAccessStream.CopyAndCloseAsync(sourceInputStream, destinationStream);
            }
        }
    }
}
```
