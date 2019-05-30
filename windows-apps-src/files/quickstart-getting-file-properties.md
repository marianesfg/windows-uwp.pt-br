---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obter propriedades do arquivo
description: Obter propriedades do &\#8212; nível superior, básico e estendido &\#8212; para um arquivo representado por um StorageFile do objeto.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 01eda8eefea7e1b3b1102ef154a019e1630e80c2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369313"
---
# <a name="get-file-properties"></a>Obter propriedades do arquivo

**APIs importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Obtenha as propriedades - nível superior, básicas e estendidas - de um arquivo representado pelo objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile).

> [!NOTE]
> Para obter um exemplo completo, consulte o [exemplo de acesso de arquivo](https://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Pré-requisitos

-   **Compreender a programação assíncrona para aplicativos da plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permissões de acesso para o local**

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

Além das propriedades de nível superior e básicas (de um arquivo), há muitas propriedades associadas ao conteúdo do arquivo. Essas propriedades estendidas são acessadas com a chamada ao método [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync). (Um [ **BasicProperties** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) objeto é obtido chamando o [ **StorageFile.Properties** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) propriedade.) Embora as propriedades de arquivo básico e de alto nível são acessíveis como propriedades de uma classe —[**StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) e **BasicProperties**, respectivamente — são as propriedades estendidas obtido, passando um [IEnumerable](https://go.microsoft.com/fwlink/p/?LinkID=313091) coleção de [cadeia de caracteres](https://go.microsoft.com/fwlink/p/?LinkID=325032) objetos que representam os nomes das propriedades que devem ser recuperados para o  **BasicProperties.RetrievePropertiesAsync** método. Esse método então retorna uma coleção [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238). Cada propriedade estendida é recuperada da coleção, por nome ou índice.

Este exemplo enumera todos os arquivos da biblioteca Imagens, especifica os nomes das propriedades desejadas (**DataAccessed** e **FileOwner**) em um objeto [List](https://go.microsoft.com/fwlink/p/?LinkID=325246), transmite esse objeto [List](https://go.microsoft.com/fwlink/p/?LinkID=325246) para [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) para recuperar essas propriedades e, em seguida, recupera as propriedades pelo nome do objeto [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238) retornado.

Consulte as [Propriedades do Windows Core](https://docs.microsoft.com/windows/desktop/properties/core-bumper) para obter uma lista completa das propriedades estendidas de um arquivo.

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

 

 
