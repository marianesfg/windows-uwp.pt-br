---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: Rastrear arquivos e pastas usados recentemente
description: Acompanhe os arquivos que o usuário acessa com frequência adicionando-os à lista de itens usados recentemente de seu aplicativo.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 318c58b393a33916df7bab51a4ef2690494d14fb
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259603"
---
# <a name="track-recently-used-files-and-folders"></a>Rastrear arquivos e pastas usados recentemente

**APIs importantes**

- [**MostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist)
- [**FileOpenPicker**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker)

Acompanhe os arquivos que o usuário acessa com frequência adicionando-os à lista de itens usados recentemente de seu aplicativo. A plataforma gerencia a MRU para você classificando os itens com base na data em que foram acessados pela última vez e removendo o item mais antigo quando o limite de 25 itens é atingido. Todos os aplicativos têm seus próprios itens usados recentemente.

Os itens recém-usados do aplicativo são representados pela classe [**StorageItemMostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageItemMostRecentlyUsedList) que você obtém da propriedade estática [**StorageApplicationPermissions.MostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist). Os itens recém-usados são armazenados como objetos [**IStorageItem**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageItem), portanto, os objetos [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) (que representam arquivos) e [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) (que representam pastas) podem ser adicionados aos itens recém-usados.

> [!NOTE]
> Para exemplos completos, consulte o [Exemplo de seletor de arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker) e o [Exemplo de acesso a arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess).

## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos UWP (Plataforma Universal do Windows)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permissões de acesso ao local**

    Consulte [Permissões de acesso a arquivo](file-access-permissions.md).

-   [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md)

    Os arquivos selecionados costumam ser os mesmos arquivos que os usuários acessam com frequência.

 ## <a name="add-a-picked-file-to-the-mru"></a>Adicionar um arquivo selecionado à lista MRU

-   Os arquivos que o usuário seleciona costumam ser os arquivos que eles retornam repetidamente. Portanto, considere adicionar os arquivos selecionados aos itens recém-usados de seu aplicativo assim que eles forem selecionados. Veja aqui como fazer isso.

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) está sobrecarregado. No exemplo, usamos [**Add(IStorageItem, String)** ](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) para podermos associar metadados ao arquivo. A configuração de metadados permite que você registre a finalidade do item, por exemplo, "profile pic". Você também pode adicionar o arquivo à lista de itens recém-usados sem metadados chamando [**Add(IStorageItem)** ](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add). Quando você adiciona um item à lista de itens recém-usados, o método retorna uma cadeia de caracteres de identificação exclusiva, denominada token, que é usada para recuperar o item.

> [!TIP]
> Você precisará do token para recuperar um item da lista de itens recém-usados, portanto, mantenha-o em algum lugar. Para saber mais sobre dados de aplicativos, consulte [Gerenciando dados do aplicativo](https://docs.microsoft.com/previous-versions/windows/apps/hh465109(v=win.10)).

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>Usar um token para recuperar um item da lista MRU

Use o método de recuperação mais apropriado para o item a ser recuperado.

-   Recupere um arquivo como [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) usando [**GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfileasync).
-   Recupere uma pasta como [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) usando [**GetFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfolderasync).
-   Recupere um [**IStorageItem**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageItem) genérico que pode representar um arquivo ou pasta usando [**GetItemAsync**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getitemasync).

Consulte aqui como recuperar o arquivo que acabamos de adicionar.

```cs
StorageFile retrievedFile = await mru.GetFileAsync(mruToken);
```

Consulte aqui como iterar todas as entradas para obter tokens e itens.

```cs
foreach (Windows.Storage.AccessCache.AccessListEntry entry in mru.Entries)
{
    string mruToken = entry.Token;
    string mruMetadata = entry.Metadata;
    Windows.Storage.IStorageItem item = await mru.GetItemAsync(mruToken);
    // The type of item will tell you whether it's a file or a folder.
}
```

O [**AccessListEntryView**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.AccessListEntryView) permite que você promova a iteração de itens recém-usados. Essas entradas são estruturas de [**AccessListEntry**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.AccessListEntry) que contêm o token e os metadados de um item.

## <a name="removing-items-from-the-mru-when-its-full"></a>Removendo itens da lista MRU quando ela está cheia

Quando o limite de 25 itens da MRU for atingido e você tentar adicionar um novo item, o item que foi acessado há mais tempo será removido automaticamente. Portanto, você nunca precisa remover um item antes de adicionar um novo.

## <a name="future-access-list"></a>Lista de acesso futuro

Além da lista MRU, seu aplicativo também tem uma lista de acesso futuro. Selecionando arquivos e pastas, o usuário concede a seu aplicativo permissão para acessar itens que podem não estar acessíveis de outra maneira. Se você adicionar esses itens à sua lista de acesso futuro, manterá essa permissão quando seu aplicativo quiser acessar esses itens novamente mais tarde. A lista de acesso futuro do seu aplicativo é representada pela classe [**StorageItemAccessList**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageItemAccessList) que você obtém da propriedade estática [**StorageApplicationPermissions.FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist).

Quando um usuário seleciona um item, considere adicioná-lo à sua lista de acesso futuro, bem como à seus itens recém-usados.

-   O [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) comporta até 1000 itens. Lembre-se: ele pode conter pastas, bem como arquivos, portanto, muitas pastas.
-   A plataforma nunca remove itens de [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) para você. Quando você atingir o limite de 1000 itens, não poderá adicionar outro item até liberar espaço com o método [**Remove**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.remove).
