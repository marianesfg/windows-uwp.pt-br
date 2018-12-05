---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Determinando a disponibilidade de arquivos do Microsoft OneDrive
description: Determine se um arquivo do Microsoft OneDrive está disponível usando a propriedade StorageFile.IsAvailable.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e431694f3f0effb6fd5e7688b146109dfc1f5dc7
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8734618"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Determinando a disponibilidade de arquivos do Microsoft OneDrive


**APIs importantes**

-   [**Classe FileIO**](https://msdn.microsoft.com/library/windows/apps/Hh701440)
-   [**Classe StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)
-   [**Propriedade StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)

Determine se um arquivo do Microsoft OneDrive está disponível usando a propriedade [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx).

## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/Mt187334).

-   **Declarações de recursos do aplicativo**

    Consulte [Permissões de acesso a arquivo](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Usando a propriedade StorageFile.IsAvailable

Os usuários podem marcar os arquivos OneDrive como disponível offline (padrão) ou somente online. Com essa funcionalidade, os usuários podem transferir arquivos grandes (como fotos e vídeos) para o OneDrive, marcá-los como somente online e economizar espaço em disco (a única coisa mantida localmente é um arquivo de metadados).

[**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) é usada para determinar se um arquivo está disponível no momento. A tabela a seguir mostra o valor da propriedade **StorageFile.IsAvailable** em diversos cenários.

| Tipo de arquivo                              | Online | Rede limitada        | Offline |
|-------------------------------------------|--------|------------------------|---------|
| Arquivo local                                | True   | True                   | True    |
| Arquivo OneDrive marcado como disponível offline | True   | True                   | True    |
| Arquivo OneDrive marcado como somente online       | True   | Baseado nas configurações do usuário | False   |
| Arquivo de rede                              | True   | Baseado nas configurações do usuário | False   |

 

As etapas a seguir ilustram como determinar se um arquivo está disponível no momento.

1.  Declare uma funcionalidade apropriada para a biblioteca que você quer acessar.
2.  Inclua o namespace [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346). Esse namespace inclui os tipos para gerenciamento de arquivos, pastas e configurações de aplicativos. Inclui também o tipo [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) necessário.
3.  Obtenha um objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) para o(s) arquivo(s) desejado(s). Se estiver enumerando uma biblioteca, essa etapa será normalmente realizada pela chamada ao método [**StorageFolder.CreateFileQuery**](https://msdn.microsoft.com/library/windows/apps/BR227252) e, depois, chamando o método [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276.aspx) do objeto resultante [**StorageFileQueryResult**](https://msdn.microsoft.com/library/windows/apps/BR208046). O método **GetFilesAsync** retorna uma coleção [IReadOnlyList](http://go.microsoft.com/fwlink/p/?LinkId=324970) de objetos **StorageFile**.
4.  Quando você tiver acesso a um objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) representando os arquivos desejados, o valor da propriedade [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) refletirá se o arquivo está ou não disponível.

O método genérico a seguir ilustra como enumerar qualquer pasta e retorna a coleção de objetos [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) dessa pasta. O método chamado faz a iteração sobre a coleção retornada, referenciando a propriedade [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) de cada arquivo.

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
