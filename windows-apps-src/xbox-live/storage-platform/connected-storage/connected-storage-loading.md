---
title: Usar o armazenamento conectado para carregar dados
description: Saiba como usar o armazenamento conectado para carregar dados.
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: a2f7498e8063e290dc506de72b34d2c77d29b26e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637131"
---
# <a name="use-connected-storage-to-load-data"></a>Usar o armazenamento conectado para carregar dados

Dados de forma assíncrona é lido usando o `ReadAsync` ou `GetAsync` conectado o método de armazenamento.

### <a name="to-load-data-from-connected-storage"></a>Para carregar dados de armazenamento conectada

1.  Recuperar um `ConnectedStorageSpace` para o usuário ao chamar `GetForUserAsync`.

    No exemplo XDK retornado `ConnectedStorageSpace` está sendo adicionado a um mapa para habilitar o gerenciamento fácil de `ConnectedStorageSpace` objetos para vários usuários.

2.  Criar uma `ConnectedStorageContainer` chamando `CreateContainer` sobre o `ConnectedStorageSpace`.
3.  Recuperar os dados, chamando `ReadAsync` ou `GetAsync` sobre o `ConnectedStorageContainer`. `ReadAsync` exige que você passe em um buffer ao `GetAsync` aloca buffers de novo para armazenar os dados que é lido.

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

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Save Data is Loading

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        //Successful load logic here.
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        //Fail logic here
        break;

    default:
        //all other possible values of action->status are also failures, alternate fail logic here. 
        break;
    }
}
```

Você pode encontrar que as APIs de armazenamento conectados XDK documentados no arquivo. chm XDK sob o caminho: **Xbox um XDK >> Referência da API >> Referência de API da plataforma >> Referência de API do sistema >> Windows.Xbox.Storage**.
As APIs do XDK também estão documentadas na [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
O link para as APIs do XDK requer que você tenha um Account(MSA) Microsoft que foi habilitada para acesso de Kit(XDK) de desenvolvedor do Xbox.
Windows.Xbox.Storage é o nome do namespace de armazenamento conectados para consoles do Xbox One.

## <a name="c-uwp-sample"></a>C#Exemplo de UWP

Enquanto jogos XDK e aplicativos UWP podem usar APIs diferentes, a API de UWP é modelada após a API do XDK muito próximo. Para carregar os dados você ainda precisará seguir as mesmas etapas básicas ao fazer Observação de algumas alterações de nome de namespace e classe. Em vez de usar o namespace `Windows::Xbox::Storage` você usará `Windows.Gaming.XboxLive.Storage`. A classe `ConnectedStorageSpace`, é equivalente a `GameSaveProvider`. A classe `ConnectedStorageContainer` é equivalente a `GameSaveContainer`. Essas alterações são ainda mais detalhadas na seção de armazenamento conectada do [portabilidade Xbox Live código do XDK para UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 0;
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
    //throw new Exception("Game Save Provider Initialization failed");;
}

//Now you have a GameSaveProvider
//Next you need to call CreateContainer to get a GameSaveContainer

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName);
//Parameter
//string name (name of the GameSaveContainer Created)

//form an array of strings containing the blob names you would like to read.
string[] blobsToRead = new string[] { c_saveBlobName };

// GetAsync allocates a new Dictionary to hold the retrieved data. You can also use ReadAsync
// to provide your own preallocated Dictionary.
GameSaveBlobGetResult result = await gameSaveContainer.GetAsync(blobsToRead);

int loadedData = 0;

//Check status to make sure data was read from the container
if(result.Status == GameSaveErrorStatus.Ok)
{
    //prepare a buffer to receive blob
    IBuffer loadedBuffer;

    //retrieve the named blob from the GetAsync result, place it in loaded buffer.
    result.Value.TryGetValue(c_saveBlobName, out loadedBuffer);

    if(loadedBuffer == null)
    {

        throw new Exception(String.Format("Didn't find expected blob \"{0}\" in the loaded data.", c_saveBlobName));

    }
    DataReader reader = DataReader.FromBuffer(loadedBuffer);
    loadedData = reader.ReadInt32();
}
```

Conectado a APIs de armazenamento para aplicativos UWP são documentados na [referência de API do Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Para ver outro exemplo que usa Confira armazenamento conectado a [jogo de amostras API Xbox Live Salvar projeto](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).