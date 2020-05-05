---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obter propriedades do arquivo
description: Obtenha as propriedades&\#8212;nível superior, básicas e estendidas&\#8212;de um arquivo representado pelo objeto StorageFile.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 81fcb4b62f9a10e8ff1fcb233c95317746cdb3b0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259585"
---
# <a name="get-file-properties"></a>Obter propriedades do arquivo

**APIs importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Obtenha as propriedades - nível superior, básicas e estendidas - de um arquivo representado pelo objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile).

> [!NOTE]
> Para obter um exemplo completo, confira o [Exemplo de acesso a arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess).

## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos UWP (Plataforma Universal do Windows)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permissões de acesso ao local**

    Por exemplo, o código nesses exemplos exige a funcionalidade **picturesLibrary**, mas o local talvez exija outra funcionalidade ou até mesmo nenhuma funcionalidade. Para saber mais, consulte [Permissões de acesso a arquivo](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Obtendo as propriedades de nível superior de um arquivo

Muitas propriedades de nível superior são acessadas como membros da classe [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile). Essas propriedades incluem atributos do arquivo, tipo de conteúdo, data de criação, nome para exibição, tipo de arquivo etc.

> [!NOTE]
> Lembre-se de declarar a funcionalidade **picturesLibrary**.

Este exemplo enumera todos os arquivos da biblioteca Imagens, acessando algumas das propriedades de nível superior de cada arquivo.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>Obtendo as propriedades básicas de um arquivo

Muitas propriedades básicas são obtidas chamando primeiro o método [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync). Esse método retorna um objeto [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties), que define propriedades para o tamanho do item (arquivo ou pasta), bem como a data da última modificação do item.

Este exemplo enumera todos os arquivos da biblioteca Imagens, acessando algumas das propriedades básicas de cada arquivo.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>Obtendo as propriedades estendidas de um arquivo

Além das propriedades de nível superior e básicas (de um arquivo), há muitas propriedades associadas ao conteúdo do arquivo. Essas propriedades estendidas são acessadas com a chamada ao método [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync). (Um objeto [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) é obtido chamando a propriedade [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties).) Embora propriedades de arquivo de nível superior e básicas sejam acessadas como propriedades de uma classe —[**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) e **BasicProperties**, respectivamente — propriedades estendidas são obtidas transmitindo uma coleção [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) de objetos [String](https://msdn.microsoft.com/library/system.string.aspx) que representam os nomes das propriedades que devem ser recuperadas para o método **BasicProperties.RetrievePropertiesAsync**. Esse método então retorna uma coleção [IDictionary](https://msdn.microsoft.com/library/system.collections.idictionary.aspx). Cada propriedade estendida é recuperada da coleção, por nome ou índice.

Este exemplo enumera todos os arquivos da biblioteca Imagens, especifica os nomes das propriedades desejadas (**DataAccessed** e **FileOwner**) em um objeto [List](https://msdn.microsoft.com/library/6sh2ey19.aspx), transmite esse objeto [List](https://msdn.microsoft.com/library/6sh2ey19.aspx) para [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) para recuperar essas propriedades e, em seguida, recupera as propriedades pelo nome do objeto [IDictionary](https://msdn.microsoft.com/library/system.collections.idictionary.aspx) retornado.

Consulte as [Propriedades Básicas do Windows](https://docs.microsoft.com/windows/desktop/properties/core-bumper) para obter uma lista completa das propriedades estendidas de um arquivo.

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 
