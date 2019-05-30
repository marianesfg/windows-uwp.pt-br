---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Determinando a disponibilidade de arquivos do Microsoft OneDrive
description: Determine se um arquivo do Microsoft OneDrive está disponível usando a propriedade StorageFile.IsAvailable.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: effb28fa453ec884152dbc404245f00f4893ef5a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369421"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Determinando a disponibilidade de arquivos do Microsoft OneDrive


**APIs importantes**

-   [**Classe FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
-   [**Classe de StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)
-   [**Propriedade StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)

Determine se um arquivo do Microsoft OneDrive está disponível usando a propriedade [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable).

## <a name="prerequisites"></a>Pré-requisitos

-   **Compreender a programação assíncrona para aplicativos da plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Declarações de capabilty do aplicativo**

    Consulte [Permissões de acesso a arquivo](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Usando a propriedade StorageFile.IsAvailable

Os usuários podem marcar os arquivos OneDrive como disponível offline (padrão) ou somente online. Com essa funcionalidade, os usuários podem transferir arquivos grandes (como fotos e vídeos) para o OneDrive, marcá-los como somente online e economizar espaço em disco (a única coisa mantida localmente é um arquivo de metadados).

[**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable), é usado para determinar se um arquivo está disponível no momento. A tabela a seguir mostra o valor da propriedade **StorageFile.IsAvailable** em diversos cenários.

| Tipo de arquivo                              | Online | Rede limitada        | Offline |
|-------------------------------------------|--------|------------------------|---------|
| Arquivo local                                | True   | True                   | True    |
| Arquivo OneDrive marcado como disponível offline | True   | True                   | True    |
| Arquivo OneDrive marcado como somente online       | True   | Baseado nas configurações do usuário | False   |
| Arquivo de rede                              | True   | Baseado nas configurações do usuário | False   |

 

As etapas a seguir ilustram como determinar se um arquivo está disponível no momento.

1.  Declare uma funcionalidade apropriada para a biblioteca que você quer acessar.
2.  Inclua o namespace [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage). Esse namespace inclui os tipos para gerenciamento de arquivos, pastas e configurações de aplicativos. Inclui também o tipo [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) necessário.
3.  Obtenha um objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) para o(s) arquivo(s) desejado(s). Se estiver enumerando uma biblioteca, essa etapa será normalmente realizada pela chamada ao método [**StorageFolder.CreateFileQuery**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfilequery) e, depois, chamando o método [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) do objeto resultante [**StorageFileQueryResult**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.StorageFileQueryResult). O método **GetFilesAsync** retorna uma coleção [IReadOnlyList](https://go.microsoft.com/fwlink/p/?LinkId=324970) de objetos **StorageFile**.
4.  Quando você tiver acesso a um objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) representando os arquivos desejados, o valor da propriedade [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) refletirá se o arquivo está ou não disponível.

O método genérico a seguir ilustra como enumerar qualquer pasta e retorna a coleção de objetos [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) dessa pasta. O método chamado faz a iteração sobre a coleção retornada, referenciando a propriedade [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) de cada arquivo.

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```
