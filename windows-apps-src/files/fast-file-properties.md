---
author: laurenhughes
title: Acesso rápido às propriedades de arquivo UWP
description: Coletar com eficiência uma lista de arquivos e respectivas propriedades em uma biblioteca para usar em um aplicativo UWP.
ms.author: lahugh
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, arquivo, propriedades
ms.localizationpriority: medium
ms.openlocfilehash: e2f63e848820361a64a2a96348a8e1cc2419f233
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5560420"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>Acesso rápido às propriedades de arquivo UWP 

Saiba como coletar rapidamente uma lista de arquivos e respectivas propriedades em uma biblioteca e usá-las em um app.  

Pré-requisitos 
- **Programação assíncrona para aplicativos da plataforma Universal do Windows (UWP)**  você pode aprender a escrever aplicativos assíncronos em c# ou Visual Basic, consulte [chamar APIs assíncronas em c# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic).     Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps). 
- **Permissões de acesso a bibliotecas**  o código nestes exemplos exige a funcionalidade **picturesLibrary** , mas o local do arquivo pode exigir uma funcionalidade diferente, ou nenhum recurso de todo o tempo. Para saber mais, consulte [Permissões de acesso a arquivo](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). 
- **Enumeração simples de arquivos**  Este exemplo usa [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) para definir algumas propriedades avançadas de enumeração. Para saber mais sobre como obter uma lista simples de arquivos para um diretório menor, consulte [Enumerar e consultar arquivos e pastas](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders). 

## <a name="usage"></a>Uso  
Muitos apps precisam listar as propriedades de um grupo de arquivos, mas nem sempre precisam interagir diretamente com os arquivos. Por exemplo, um app de reprodução de música reproduz (abre) um arquivo por vez, mas precisa das propriedades de todos os arquivos de uma pasta para que o app possa mostrar a fila de músicas, ou então o usuário pode escolher um arquivo válido para executar. 

Os exemplos desta página não devem ser usados em apps que modificarão os metadados de cada arquivo ou apps que interagem com todos os StorageFiles resultantes além de ler suas propriedades. Consulte [Enumerar e consultar arquivos e pastas](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders) para obter mais informações. 

## <a name="enumerate-all-the-pictures-in-a-location"></a>Enumerar todas as imagens em um local 
Neste exemplo, faremos o seguinte
-  Criaremos um objeto [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) para especificar que o app deseja enumerar os arquivos o mais rápido possível.
-  Busque as propriedades de arquivo percorrendo as páginas de objetos StorageFile no app. A paginação de arquivos reduz a memória usada pelo app e melhora sua capacidade de resposta percebida.

### <a name="creating-the-query"></a>Criando a consulta 
Para criar a consulta, usamos um objeto QueryOptions a fim de especificar que o app está interessado em enumerar apenas determinados tipos de arquivos de imagens e de filtrar arquivos protegidos com a Proteção de Informações do Windows (System.Security.EncryptionOwners). 

É importante definir as propriedades que o app acessará usando [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch). Se o app acessar uma propriedade que não passou por uma pré-busca, ela terá uma perda de desempenho significativa.

Configurar [IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) instrui o sistema a retornar os resultados o mais rápido possível, mas a incluir apenas as propriedades especificadas em [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch). 

### <a name="paging-in-the-results"></a>Percorrendo as páginas de resultados 
Os usuários podem ter milhares ou milhões de arquivos em sua biblioteca de imagens; portanto, chamar [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) sobrecarregaria seu computador, pois criaria um StorageFile para cada imagem. Isso pode ser resolvido através da criação de um número fixo de StorageFiles ao mesmo tempo, processando-os na interface do usuário e, em seguida, liberando a memória. 

No nosso exemplo, fazemos isso usando [StorageFileQueryResult.GetFilesAsync(UInt32 StartIndex, UInt32 maxNumberOfItems)](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) para buscar apenas 100 arquivos por vez. O app processará os arquivos e permitirá que o sistema operacional libere essa memória posteriormente. Esta técnica encapsula a memória máxima do app e garante que o sistema permaneça receptivo. Obviamente, você precisará ajustar o número de arquivos retornados para seu cenário, mas, para garantir uma experiência receptiva para todos os usuários, é recomendável não obter mais de 500 arquivos ao mesmo tempo.


**Exemplo**  
```csharp
StorageFolder folderToEnumerate = KnownFolders.PicturesLibrary; 
// Check if the folder is indexed before doing anything. 
IndexedState folderIndexedState = await folderToEnumerate.GetIndexedStateAsync(); 
if (folderIndexedState == IndexedState.NotIndexed || folderIndexedState == IndexedState.Unknown) 
{ 
    // Only possible in indexed directories.  
    return; 
} 
 
QueryOptions picturesQuery = new QueryOptions() 
{ 
    FolderDepth = FolderDepth.Deep, 
    // Filter out all files that have WIP enabled
    ApplicationSearchFilter = "System.Security.EncryptionOwners:[]", 
    IndexerOption = IndexerOption.OnlyUseIndexerAndOptimizeForIndexedProperties 
}; 

picturesQuery.FileTypeFilter.Add(".jpg"); 
string[] otherProperties = new string[] 
{ 
    SystemProperties.GPS.LatitudeDecimal, 
    SystemProperties.GPS.LongitudeDecimal 
}; 
 
picturesQuery.SetPropertyPrefetch(PropertyPrefetchOptions.BasicProperties | PropertyPrefetchOptions.ImageProperties, 
                                    otherProperties); 
SortEntry sortOrder = new SortEntry() 
{ 
    AscendingOrder = true, 
    PropertyName = "System.FileName" // FileName property is used as an example. Any property can be used here.  
}; 
picturesQuery.SortOrder.Add(sortOrder); 
 
// Create the query and get the results 
uint index = 0; 
const uint stepSize = 100; 
if (!folderToEnumerate.AreQueryOptionsSupported(picturesQuery)) 
{ 
    log("Querying for a sort order is not supported in this location"); 
    picturesQuery.SortOrder.Clear(); 
} 
StorageFileQueryResult queryResult = folderToEnumerate.CreateFileQueryWithOptions(picturesQuery); 
IReadOnlyList<StorageFile> images = await queryResult.GetFilesAsync(index, stepSize); 
while (images.Count != 0 || index < 10000) 
{ 
    foreach (StorageFile file in images) 
    { 
        // With the OnlyUseIndexerAndOptimizeForIndexedProperties set, this won't  
        // be async. It will run synchronously. 
        var imageProps = await file.Properties.GetImagePropertiesAsync(); 
 
        // Build the UI 
        log(String.Format("{0} at {1}, {2}", 
                    file.Path, 
                    imageProps.Latitude, 
                    imageProps.Longitude)); 
    } 
    index += stepSize; 
    images = await queryResult.GetFilesAsync(index, stepSize); 
} 
```

### <a name="results"></a>Resultados 
Os arquivos de StorageFile resultantes contêm apenas as propriedades solicitadas, mas são retornados 10 vezes mais rápido que os outros IndexerOptions.O app ainda pode solicitar o acesso a propriedades que ainda não estão incluídas na consulta, mas há uma perda de desempenho para abrir o arquivo e recuperar essas propriedades.  

## <a name="adding-folders-to-libraries"></a>Adicionando pastas a bibliotecas 
Os apps podem solicitar ao usuário que adicione o local ao índice usando [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync). Depois que o local for incluído, ele será indexado automaticamente e os apps poderão usar essa técnica para enumerar os arquivos.
 
## <a name="see-also"></a>Consulte também
[Referência de API de QueryOptions](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions)  
[Enumerar e consultar arquivos e pastas](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[Permissões de acesso a arquivo](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)  
[Passo a passo de acesso rápido à propriedade](https://blogs.msdn.microsoft.com/adamdwilson/2017/12/20/fast-file-enumeration-with-partially-initialized-storagefiles/)
 
 
