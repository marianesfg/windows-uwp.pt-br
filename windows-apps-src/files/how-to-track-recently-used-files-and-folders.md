---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: Rastrear arquivos e pastas usados recentemente
description: Acompanhe os arquivos que o usuário acessa com frequência adicionando-os à lista de itens usados recentemente de seu aplicativo.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a810c097b4f162395106e74b68d5e9cdb2f8538
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8217012"
---
# <a name="track-recently-used-files-and-folders"></a>Rastrear arquivos e pastas usados recentemente

**APIs importantes**

- [**MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458)
- [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369)

Acompanhe os arquivos que o usuário acessa com frequência adicionando-os à lista de itens usados recentemente de seu aplicativo. A plataforma gerencia os itens usados recentemente para você classificando-os com base na data em que foram acessados pela última vez e removendo o item mais antigo quando o limite de 25 itens é atingido. Todos os aplicativos têm seus próprios itens usados recentemente.

Os itens recém-usados do aplicativo são representados pela classe [**StorageItemMostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207475) que você obtém da propriedade estática [**StorageApplicationPermissions.MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458). Os itens recém-usados são armazenados como objetos [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129), portanto, os objetos [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) (que representam arquivos) e [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) (que representam pastas) podem ser adicionados aos itens recém-usados.

> [!NOTE]
> Veja também o [Exemplo de seletor de arquivos](http://go.microsoft.com/fwlink/p/?linkid=619994) e o [Exemplo de acesso a arquivos](http://go.microsoft.com/fwlink/p/?linkid=619995).

 

## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Acessar permissões ao local**

    Consulte [Permissões de acesso a arquivo](file-access-permissions.md).

-   [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md)

    Os arquivos selecionados costumam ser os mesmos arquivos que os usuários acessam com frequência.

 ## <a name="add-a-picked-file-to-the-mru"></a>Adicionar um arquivo selecionado à lista MRU

-   Os arquivos que o usuário seleciona costumam ser os arquivos que eles retornam repetidamente. Portanto, considere adicionar os arquivos selecionados aos itens recém-usados de seu aplicativo assim que eles forem selecionados. Consulte aqui como fazer isso.

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](https://msdn.microsoft.com/library/windows/apps/br207476) está sobrecarregado. No exemplo, usamos [**Add(IStorageItem, String)**](https://msdn.microsoft.com/library/windows/apps/br207481) para podermos associar metadados ao arquivo. A configuração de metadados permite que você registre a finalidade do item, por exemplo, "profile pic". Você também pode adicionar o arquivo à lista de itens recém-usados sem metadados chamando [**Add(IStorageItem)**](https://msdn.microsoft.com/library/windows/apps/br207480). Quando você adiciona um item à lista de itens recém-usados, o método retorna uma cadeia de caracteres de identificação exclusiva, denominada token, que é usada para recuperar o item.

> [!TIP]
> Você precisará do token para recuperar um item da lista MRU, portanto, mantenha-o em algum lugar. Para saber mais sobre dados de aplicativos, consulte [Gerenciando dados do aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465109).

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>Usar um token para recuperar um item da lista MRU

Use o método de recuperação mais apropriado para o item a ser recuperado.

-   Recupere um arquivo como [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) usando [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207486).
-   Recupere uma pasta como [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) usando [**GetFolderAsync**](https://msdn.microsoft.com/library/windows/apps/br207489).
-   Recupere um [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129) genérico que pode representar um arquivo ou pasta usando [**GetItemAsync**](https://msdn.microsoft.com/library/windows/apps/br207492).

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

O [**AccessListEntryView**](https://msdn.microsoft.com/library/windows/apps/br227349) permite que você promova a iteração de itens recém-usados. Essas entradas são estruturas de [**AccessListEntry**](https://msdn.microsoft.com/library/windows/apps/br227348) que contêm o token e os metadados de um item.

## <a name="removing-items-from-the-mru-when-its-full"></a>Removendo itens da lista MRU quando ela está cheia

Quando o limite de 25 itens da MRU for atingido e você tentar adicionar um novo item, o item que foi acessado há mais tempo será removido automaticamente. Portanto, você nunca precisa remover um item antes de adicionar um novo.

## <a name="future-access-list"></a>Lista de acesso futuro

Além da lista MRU, seu aplicativo também tem uma lista de acesso futuro. Selecionando arquivos e pastas, o usuário concede a seu aplicativo permissão para acessar itens que podem não estar acessíveis de outra maneira. Se você adicionar esses itens à sua lista de acesso futuro, manterá essa permissão quando seu aplicativo quiser acessar esses itens novamente mais tarde. A lista de acesso futuro do seu aplicativo é representada pela classe [**StorageItemAccessList**](https://msdn.microsoft.com/library/windows/apps/br207459) que você obtém da propriedade estática [**StorageApplicationPermissions.FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457).

Quando um usuário seleciona um item, considere adicioná-lo à sua lista de acesso futuro, bem como à seus itens recém-usados.

-   O [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) comporta até 1000 itens. Lembre-se: ele pode conter pastas, bem como arquivos, portanto, muitas pastas.
-   A plataforma nunca remove itens de [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) para você. Quando você atingir o limite de 1000 itens, não poderá adicionar outro item até liberar espaço com o método [**Remove**](https://msdn.microsoft.com/library/windows/apps/br207497).
