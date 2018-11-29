---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obter propriedades do arquivo
description: Obtenha as propriedades&\#8212;nível superior, básicas e estendidas&\#8212;de um arquivo representado pelo objeto StorageFile.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b0747dd3b8992ab456bdb00a4dc7157211eb8ba
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7990309"
---
# <a name="get-file-properties"></a>Obter propriedades do arquivo



**APIs importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

Obtenha as propriedades - nível superior, básicas e estendidas - de um arquivo representado pelo objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

> [!NOTE]
> Veja também o [Exemplo de acesso a arquivos](http://go.microsoft.com/fwlink/p/?linkid=619995).

 


## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Acessar permissões ao local**

    Por exemplo, o código nesses exemplos exige a funcionalidade **picturesLibrary**, mas o local talvez exija outra funcionalidade ou até mesmo nenhuma funcionalidade. Para obter mais informações, consulte [Permissões de acesso a arquivo](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Obtendo as propriedades de nível superior de um arquivo

Muitas propriedades de nível superior são acessadas como membros da classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). Essas propriedades incluem atributos do arquivo, tipo de conteúdo, data de criação, nome para exibição, tipo de arquivo etc.

**Observação**Lembre-se de declarar a funcionalidade de **picturesLibrary** .

 

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

Muitas propriedades básicas são obtidas chamando primeiro o método [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737). Esse método retorna um objeto [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113), que define propriedades para o tamanho do item (arquivo ou pasta), bem como a data da última modificação do item.

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

Além das propriedades de nível superior e básicas (de um arquivo), há muitas propriedades associadas ao conteúdo do arquivo. Essas propriedades estendidas são acessadas com a chamada ao método [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124). (Um objeto [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) é obtido chamando a propriedade [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225).) Embora propriedades de arquivo de nível superior e básicas sejam acessadas como propriedades de uma classe —[**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) e **BasicProperties**, respectivamente — propriedades estendidas são obtidas transmitindo uma coleção [IEnumerable](http://go.microsoft.com/fwlink/p/?LinkID=313091) de objetos [String](http://go.microsoft.com/fwlink/p/?LinkID=325032) que representam os nomes das propriedades que devem ser recuperadas para o método **BasicProperties.RetrievePropertiesAsync**. Esse método então retorna uma coleção [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238). Cada propriedade estendida é recuperada da coleção, por nome ou índice.

Este exemplo enumera todos os arquivos da biblioteca Imagens, especifica os nomes das propriedades desejadas (**DataAccessed** e **FileOwner**) em um objeto [List](http://go.microsoft.com/fwlink/p/?LinkID=325246), transmite esse objeto [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) para [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) para recuperar essas propriedades e, em seguida, recupera as propriedades pelo nome do objeto [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) retornado.

Consulte as [Propriedades do Windows Core](https://msdn.microsoft.com/library/windows/desktop/mt805470) para obter uma lista completa das propriedades estendidas de um arquivo.

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

 

 
