---
author: laurenhughes
ms.assetid: CAC6A7C7-3348-4EC4-8327-D47EB6E0C238
title: Acessar o cartão SD
description: Você pode armazenar e acessar dados não essenciais em um cartão microSD opcional, especialmente em dispositivos móveis de baixo custo que têm armazenamento interno limitado.
ms.author: lahugh
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, uwp, cartão sd, armazenamento
ms.localizationpriority: medium
ms.openlocfilehash: 498b43dc82100102c90fc7a920bed1538a164afc
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5838015"
---
# <a name="access-the-sd-card"></a>Acessar o cartão SD



Você pode armazenar e acessar dados não essenciais em um cartão microSD opcional, principalmente em dispositivos de baixo custo que possuem espaço de armazenamento interno limitado e slot de cartão SD.

Na maioria dos casos, você precisa especificar a funcionalidade do **removableStorage** no manifesto do aplicativo antes de ele poder armazenar e acessar arquivos no cartão SD. Geralmente, você também precisa registrar-se para controlar o tipo de arquivos que seu aplicativo armazena e acessa.

Você pode armazenar e acessar arquivos no cartão SD opcional usando os métodos a seguir:
- Seletores de arquivo
- APIs do [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

## <a name="what-you-can-and-cant-access-on-the-sd-card"></a>O que você e não pode acessar no cartão SD

### <a name="what-you-can-access"></a>O que você pode acessar

- Seu aplicativo só pode ler e gravar arquivos de tipos de arquivos que o aplicativo registrou para manipular no arquivo de manifesto do aplicativo.
- Seu aplicativo também pode criar e gerenciar pastas.

### <a name="what-you-cant-access"></a>O que você não pode acessar

- Seu aplicativo não pode ver ou acessar pastas do sistema e os arquivos que elas contêm.
- Seu aplicativo não pode ver arquivos que estão marcados com o atributo Oculto. O atributo Hidden costuma ser usado para reduzir o risco de deletar dados acidentalmente.
- Seu aplicativo não pode ver ou acessar a Biblioteca de documentos usando [**KnownFolders.DocumentsLibrary**](https://msdn.microsoft.com/library/windows/apps/br227152). Entretanto, você pode acessar a Biblioteca de documentos no cartão SD percorrendo o sistema de arquivos.

## <a name="security-and-privacy-considerations"></a>Considerações de segurança e privacidade

Quando um aplicativo salva arquivos em uma localização global no cartão SD, esses arquivos não são criptografados para que sejam normalmente acessíveis a outros aplicativos.

- Enquanto o cartão SD está no dispositivo, seus arquivos estão acessíveis a outros aplicativos que fizeram o registro para manipular o mesmo tipo de arquivos.
- Quando o cartão SD for removido do dispositivo e aberto em um computador, os seus arquivos ficam visíveis no Explorador de Arquivos e acessíveis a outros aplicativos.

Porém, quando um aplicativo instalado no cartão SD salva arquivos no [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621), tais arquivos ficam criptografados e inacessíveis a outros aplicativos.

## <a name="requirements-for-accessing-files-on-the-sd-card"></a>Requisitos para acessar arquivos no cartão SD

Para acessar arquivos no cartão SD, costuma ser necessário especificar o seguinte.

1.  Você tem que especificar a funcionalidade do **removableStorage** no arquivo de manifesto do aplicativo.
2.  Você também precisa registrar-se para lidar com extensões de arquivos associadas ao tipo de mídia que quer acessar.

Também use o método anterior para acessar arquivos de mídia no cartão SD sem fazer referência a uma pasta conhecida como **KnownFolders.MusicLibrary**, ou para acessar arquivos de mídia fora dos arquivos da Biblioteca de mídia.

Para acessar arquivos de mídia depositados nas Bibliotecas de mídia—Música, Fotos ou Vídeos—usando pastas conhecidas, você só precisa especificar a funcionalidade associada no arquivo de manifesto do aplicativo—**musicLibrary**, **picturesLibrary**, or **videoLibrary**. Você não precisa especificar a funcionalidade de **removableStorage**. Para obter mais informações, consulte [Files and folders in the Music, Pictures, and Videos libraries](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

## <a name="accessing-files-on-the-sd-card"></a>Acessando arquivos no cartão SD

### <a name="getting-a-reference-to-the-sd-card"></a>Obtendo uma referência ao cartão SD

A pasta [**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158) é a raiz lógica [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) do conjunto de dispositivos removíveis conectados atualmente ao dispositivo. Se um cartão SD estiver presente, a primeira (e única) **StorageFolder** na pasta **KnownFolders.RemovableDevices** representa o cartão SD.

Use o código como a seguir para determinar se um cartão SD está ou não presente e para obter uma referência para ele como [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230).

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    // An SD card is present and the sdCard variable now contains a reference to it.
}
else
{
    // No SD card is present.
}
```

> [!NOTE]
> Se o leitor de cartão SD é um leitor inserido (por exemplo, um slot no notebook ou computador em si), talvez não seja acessível por meio de KnownFolders.RemovableDevices.

### <a name="querying-the-contents-of-the-sd-card"></a>Consultando os conteúdos do cartão SD

O cartão SD pode conter muitas pastas e arquivos que não são reconhecidos como pastas reconhecidas e não podem ser consultados usando uma localização de [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151). Para encontrar arquivos, o seu aplicativo tem que enumerar os conteúdos do cartão percorrendo o sistema de arquivos recursivamente. Use [**GetFilesAsync (CommonFileQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227274) e [**GetFoldersAsync (CommonFolderQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227281) para obter os conteúdos do cartão SD de maneira eficaz.

Recomendamos que você use u m thread de segundo plano para percorrer o cartão SD. Um cartão SD pode conter vários gigabytes de dados.

Seu aplicativo também pode solicitar ao usuário que escolha pastas específicas usando o seletor de pastas.

Quando você acessa o sistema de arquivos no cartão SD com um caminho você derivou de [**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158), os métodos a seguir comportam-se da seguinte maneira.

-   O método [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227273) devolve a união das extensões de arquivo que você registrou para manipular e as extensões de arquivo associadas a funcionalidades de qualquer biblioteca de mídia que você especificou.
-   O método [**GetFileFromPathAsync**](https://msdn.microsoft.com/library/windows/apps/br227206) falha se você não registrou para manipular a extensão do arquivo que está tentando acessar.

## <a name="identifying-the-individual-sd-card"></a>Identificando o cartão SD individual

Quando o cartão SD é montado pela primeira vez, o sistema operacional geral um identificador exclusivo para o cartão. Ele armazena esse ID em um arquivo na pasta WPSystem na raiz do cartão. Um aplicativo pode usar esse ID para determinar se ele reconhece o cartão. Se um aplicativo reconhece o cartão, ele é capaz de adiar certas operações que foram concluídas anteriormente. Entretanto, o conteúdo do cartão pode ter sido alterado desde a última vez que o cartão foi acessado pelo aplicativo.

Por exemplo, considere um aplicativo que cria índices de ebooks. Se o aplicativo tiver examinado todo o cartão SD anteriormente com os arquivos de ebook e tiver criado um índice dos ebooks, ele poderá exibir a lista imediatamente quando o cartão for inserido e reconhecer o cartão. Separadamente, ele pode iniciar um thread de segundo plano para pesquisar novos livros eletrônicos. Ele também pode lidar com uma falha para encontrar um livro eletrônico que existia antes, quando o usuário tento acessá-lo deletado.

O nome da propriedade que contém esse ID é **WindowsPhone.ExternalStorageId**.

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    var allProperties = sdCard.Properties;
    IEnumerable<string> propertiesToRetrieve = new List<string> { "WindowsPhone.ExternalStorageId" };

    var storageIdProperties = await allProperties.RetrievePropertiesAsync(propertiesToRetrieve);

    string cardId = (string)storageIdProperties["WindowsPhone.ExternalStorageId"];

    if (...) // If cardID matches the cached ID of a recognized card.
    {
        // Card is recognized. Index contents opportunistically.
    }
    else
    {
        // Card is not recognized. Index contents immediately.
    }
}
```

 

 
