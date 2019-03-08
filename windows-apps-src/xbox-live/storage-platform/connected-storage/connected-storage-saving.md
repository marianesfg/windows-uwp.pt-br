---
title: Usar o armazenamento conectado para salvar dados
description: Saiba como usar o armazenamento conectado para salvar os dados.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 4140e3bbe0f0ab229e3637008e01892f4179292e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617611"
---
# <a name="use-connected-storage-to-save-data"></a>Usar o armazenamento conectado para salvar dados


Dados de forma assíncrona são salvos com a criação de um `ConnectedStorageContainer` em um `ConnectedStorageSpace` de um usuário e chamar o `SubmitUpdatesAsync` método no contêiner.

> [!IMPORTANT]
> As dependências de dados em vários contêineres de armazenamento conectados são não seguras. Por exemplo, o carregamento de um contêiner para a nuvem pode concluir, enquanto outro pode permanecer na fila para carregar. Se o usuário é movido para outro console, a operação de sincronização permitiria que o primeiro contêiner a ser sincronizada e acessados no console do segundo, sem o primeiro contêiner estarem presentes.

## <a name="to-save-data-to-connected-storage"></a>Para salvar dados no armazenamento conectado

1.  Recuperar um `ConnectedStorageSpace` objeto do usuário chamando `GetForUserAsync`.

    No exemplo XDK retornado `ConnectedStorageSpace` objeto está sendo adicionado a um mapa para habilitar o gerenciamento fácil de `ConnectedStorageSpace` objetos para vários usuários.

2.  Criar uma `ConnectedStorageContainer` objeto chamando `CreateContainer` sobre o `ConnectedStorageSpace` objeto.
3.  Chame `SubmitUpdatesAsync` sobre o `ConnectedStorageContainer` com jogos salvar dados de blob como o `blobsToWrite` parâmetro.

## <a name="c-xdk-sample"></a>Exemplo de C++ XDK

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

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

enum Color { RED, BLUE };
enum EngineSize { BIG, SMALL };

struct CarData
{
    Color color;
    bool hasWheels;
    bool hasFancyRims;
    EngineSize engineSize;
};


const int MAX_CARS = 10;

struct SaveData
{
    CarData cars[MAX_CARS];
    int numCars;
    int currentCar;
    int cash;
};

SaveData gMySaveData;

void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

bool ItIsTimeToSaveACheckpoint() {return true;};

void Update()
{
    if (ItIsTimeToSaveACheckpoint())
        SaveCheckpoint(WrapRawBuffer(&gMySaveData,sizeof(SaveData)),gCurrentUser);
}


void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user)
{
     auto storageSpace = gConnectedStorageSpaceForUsers->Lookup( user->Id );

     auto container = storageSpace->CreateContainer("Saves/Checkpoint");

     auto updates = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
     updates->Insert("data", buffer);

     auto op = container->SubmitUpdatesAsync(updates->GetView(), nullptr);

     //Save is happening here asynchronously

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   //Save function has completed
                   //This area can be filled with further post save logic.
     });
}
```

Você pode encontrar que as APIs de armazenamento conectados XDK documentados no arquivo. chm XDK sob o caminho: **Xbox um XDK >> Referência da API >> Referência de API da plataforma >> Referência de API do sistema >> Windows.Xbox.Storage**.
As APIs do XDK também estão documentadas na [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
O link para as APIs do XDK requer que você tenha um Account(MSA) Microsoft que foi habilitada para acesso de Kit(XDK) de desenvolvedor do Xbox.
Windows.Xbox.Storage é o nome do namespace de armazenamento conectados para consoles do Xbox One.

## <a name="c-uwp-sample"></a>C#Exemplo de UWP

Enquanto jogos XDK e aplicativos UWP podem usar APIs diferentes, a API de UWP é modelada após a API do XDK muito próximo. Para salvar os dados você ainda precisará seguir as mesmas etapas básicas ao fazer Observação de algumas alterações de nome de namespace e classe. Em vez de usar o namespace `Windows::Xbox::Storage` você usará `Windows.Gaming.XboxLive.Storage`. A classe `ConnectedStorageSpace`, é equivalente a `GameSaveProvider`. A classe `ConnectedStorageContainer` é equivalente a `GameSaveContainer`. Essas alterações são ainda mais detalhadas na seção de armazenamento conectada do [portabilidade Xbox Live código do XDK para UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

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
//string name

// To store a value in the container, it needs to be written into a buffer, then stored with
// a blob name in a Dictionary.

DataWriter writer = new DataWriter();

writer.WriteInt32(intData); //some number you want to save, in this case 23.

IBuffer dataBuffer = writer.DetachBuffer();

var blobsToWrite = new Dictionary<string, IBuffer>();

blobsToWrite.Add(c_saveBlobName, dataBuffer);

GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(blobsToWrite, null, c_saveContainerDisplayName);
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName
```

Conectado a APIs de armazenamento para aplicativos UWP são documentados na [referência de API do Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Para ver outro exemplo que usa Confira armazenamento conectado a [jogo de amostras API Xbox Live Salvar projeto](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).