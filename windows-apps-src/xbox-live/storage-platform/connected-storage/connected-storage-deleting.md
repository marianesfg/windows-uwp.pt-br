---
title: Usar o armazenamento conectado para excluir dados
description: Saiba como usar o armazenamento conectado para excluir dados de blob e contêiner.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 756de46d05cdbf64d85491b4e8c6f783122f2356
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601311"
---
# <a name="use-connected-storage-to-delete-data"></a>Usar o armazenamento conectado para excluir dados

Blobs de dados de forma assíncrona são excluídos com a criação de um `ConnectedStorageContainer` em um `ConnectedStorageSpace` de um usuário e chamar o `SubmitUpdatesAsync` método no contêiner, fornecendo uma lista de cadeias de caracteres que representa os blobs nomeados a ser excluído para o parâmetro blobsToDelete.

Contêineres de dados são excluídos assincronamente com a criação de um `ConnectedStorageContainer` e chamar seu `DeleteContainerAsync` método.

## <a name="to-delete-blob-data-from-connected-storage"></a>Para excluir dados de blob de armazenamento conectado

1.  Recuperar um `ConnectedStorageSpace` objeto do usuário chamando `GetForUserAsync`.

    No exemplo XDK retornado `ConnectedStorageSpace` objeto está sendo adicionado a um mapa para habilitar o gerenciamento fácil de `ConnectedStorageSpace` objetos para vários usuários.

2.  Criar uma `ConnectedStorageContainer` objeto chamando `CreateContainer` sobre o `ConnectedStorageSpace` objeto.
3.  Chame `SubmitUpdatesAsync` sobre o `ConnectedStorageContainer` objeto.

## <a name="to-delete-a-container-from-connected-storage"></a>Para excluir um contêiner de armazenamento conectado

1.  Recuperar um `ConnectedStorageSpace` objeto do usuário chamando `GetForUserAsync`.

    No exemplo XDK retornado `ConnectedStorageSpace` objeto está sendo adicionado a um mapa para habilitar o gerenciamento fácil de `ConnectedStorageSpace` objetos para vários usuários.

2.  Chamar o `DeleteContainerAsync` método dos métodos ConnectedStorageSpace.

## <a name="c-xdk-sample"></a>Exemplo de C++ XDK
```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

// Acquire a Connected Storage space for a user. A Connected Storage space is required to manipulate Connected Storage Data.
void PrepareConnectedStorage(User^ user)
{
  auto op = ConnectedStorageSpace::GetForUserAsync(user);
  op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
      switch(status)
      {
        case Windows::Foundation::AsyncStatus::Completed:
          gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
          break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
          // Present user option: ?Would you like to continue without saving progress??
          if( GetUserInputYesOrNo() )
            //If the users opts yes, continue in offline mode
          else
            //If the users opts no, retry.
          break;
      }
    });
}

// Delete data from a fixed container/blob name into an IBuffer
void DeleteBlob(User^ user)
{

    //Create a list of blob names to delete
    std::vector<Platform::String^> blobsToDelete;

    //Add the blob name to your Generic Iterable
    blobsToDelete.push_back(L"someBlobName");

    auto blobNames = ref new Platform::Collections::VectorView<Platform::String^>(blobsToDelete);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Delete is occurring asynchronously at this point.

    auto op = container->SubmitUpdatesAsync(nullptr, blobNames);

    op->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });
}

void DeleteContainer(User^ user)
{
    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);

    auto deleteOperation = storageSpace->DeleteContainerAsync("Saves/Checkpoint");

    deleteOperation->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });

}
```

Você pode encontrar que as APIs de armazenamento conectados XDK documentados no arquivo. chm XDK sob o caminho: **Xbox um XDK >> Referência da API >> Referência de API da plataforma >> Referência de API do sistema >> Windows.Xbox.Storage**.
As APIs do XDK também estão documentadas na [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
O link para as APIs do XDK requer que você tenha um Account(MSA) Microsoft que foi habilitada para acesso de Kit(XDK) de desenvolvedor do Xbox.
Windows.Xbox.Storage é o nome do namespace de armazenamento conectados para consoles do Xbox One.


## <a name="c-uwp-sample"></a>C#Exemplo de UWP

Enquanto jogos XDK e aplicativos UWP podem usar APIs diferentes, a API de UWP é modelada após a API do XDK muito próximo. Para excluir os dados ainda será necessário seguir as mesmas etapas básicas ao fazer Observação de algumas alterações de nome de namespace e classe. Em vez de usar o namespace `Windows::Xbox::Storage` você usará `Windows.Gaming.XboxLive.Storage`. A classe `ConnectedStorageSpace`, é equivalente a `GameSaveProvider`. A classe `ConnectedStorageContainer` é equivalente a `GameSaveContainer`. Essas alterações são ainda mais detalhadas na seção de armazenamento conectada do [portabilidade Xbox Live código do XDK para UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 23;
const string c_saveBlobName = "Jersey";
const string c_saveContainerDisplayName = "GameSave";
const string c_saveContainerName = "GameSaveContainer";
GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetForUserAsync(users[0], context.AppConfig.ServiceConfigurationId);
//Parameters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
else
{
    return;
    //throw new Exception("Game Save Provider Initialization failed");
}

//Now you have a GameSaveProvider (formerly ConnectedStorageSpace)
//Next you need to call CreateContainer to get a GameSaveContainer (formerly ConnectedStorageContainer)

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName); // this will create a new named game save container with the name = to the input name
//Parameter
//string name (name of container to be created) 

//DELETE A BLOB
//form an array of strings containing the blob names you would like to delete.
string[] blobsToDelete = new string[] { c_saveBlobName };

//Call SubmitUpdatesAsync with the dictionary of the names of blobs to delete as the second parameter.
GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(null, blobsToDelete, c_saveContainerDisplayName);
//Parameters
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName

//Check status of the operation to ensure successful delete.
if(gameSaveOperationResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}

//DELETE A SAVE CONTAINER
GameSaveOperationResult deleteContainerResult = await gameSaveProvider.DeleteContainerAsync(c_saveContainerName);
//Parameter
//string name (name of container to be deleted)

//Check status of the operation to ensure successful delete.
if(deleteContainerResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}
```

Conectado a APIs de armazenamento para aplicativos UWP são documentados na [referência de API do Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Para ver outro exemplo que usa Confira armazenamento conectado a [jogo de amostras API Xbox Live Salvar projeto](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).
