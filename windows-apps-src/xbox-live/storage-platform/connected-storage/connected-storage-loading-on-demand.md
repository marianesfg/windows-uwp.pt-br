---
title: Conectado ao carregar o armazenamento sob demanda
description: Saiba como carregar dados de armazenamento conectado sob demanda, em vez de uma só vez.
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 31c1893f09e6d56682b4e718ee8b905ce72c7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631711"
---
# <a name="connected-storage-loading-on-demand"></a>Conectado ao carregar o armazenamento sob demanda

`GetSyncOnDemandForUserAsync` permite que você carregar dados com backup em nuvem de um espaço de armazenamento conectados "sob demanda" em vez de uma só vez. Isso pode melhorar o desempenho em `GetForUserAsync` para casos em que o arquivo salva são muito grandes.

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>Para carregar dados de um espaço de armazenamento conectados por demanda

### <a name="1--call-getsyncondemandforuserasync"></a>1:  Chamada `GetSyncOnDemandForUserAsync`

Isso dispara uma sincronização parcial que baixa uma lista de contêineres e seus metadados de nuvem, mas não seu conteúdo. Esta operação é rápida e, em condições de rede em boas condições, o usuário não verá qualquer interface do usuário de carregamento.

```cpp
auto op = ConnectedStorageSpace::GetSyncOnDemandForUserAsync(user);
op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
{
    switch (status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        auto storageSpace = operation->GetResults();
        break;
    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        // Present user option: ?Would you like to continue without saving progress??
        if (GetUserInputYesOrNo())
            SetGameState(LoadSaveState::NO_SAVE_MODE);
        else
            SetGameState(LoadSaveState::RETRY_LOAD);
        break;
    }
});
```

```csharp
var users = await Windows.System.User.FindAllAsync();

GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetSyncOnDemandForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Paramaters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
```


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2:  Executar uma consulta de contêiner usando o `GetContainerInfo2Async`

Isso retornará uma coleção de `ContainerInfo2`, que contém 3 novos campos de metadados:

    -   `DisplayName`: Qualquer nome de exibição que você tenha escrito usando a sobrecarga de `SubmitUpdatesAsync` que usa uma cadeia de caracteres de nome de exibição como um parâmetro. É recomendável sempre usar esse campo para armazenar um nome amigável.
    -   `LastModifiedTime`: A última vez em que esse contêiner foi modificado. Observe que, no caso de carimbos conflitantes de locais e remotos, aqueles remotos é usado.
    -   `NeedsSync`: Um valor booliano que indica se esse contêiner tem dados que precisam ser baixados da nuvem.

    Usando esses metadados adicionais, você pode apresentar ao usuário informações básicas sobre seu jogo salva (incluindo nome, hora da última usado e se a seleção de um exigirá um download) sem realmente executar um download completo da nuvem.

```cpp
auto containerQuery = storageSpace->CreateContainerInfoQuery(nullptr); //return list of containers in ConnectedStorageSpace
auto queryOperation = containerQuery->GetContainerInfo2Async();

queryOperation->Completed = ref new AsyncOperationCompletedHandler<IVectorView<ContainerInfo2>^ >( 
    [=] (IAsyncOperation<IVectorView<ContainerInfo2>^ >^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            // get the resulting vector of container info
            auto infoVector = operation->GetResults();
            break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
            // handle error cases
            break;
        }
    });
```

```csharp
GameSaveContainerInfoQuery infoQuery = gameSaveProvider.createContainerInfoQuery();
GameSaveContainerInfoGetResult containerInfoResult = await infoQuery.GetContainerInfoAsync();
var containerInfoList;

if(containerInfoResult.Status == GameSaveErrorStatus.Ok)
{
    containerInfoList = containerInfoResult.value;
}

// Use the containerInfoList to inform further actions or display container data to user. 
```

### <a name="3--trigger-a-sync"></a>3:  Disparar uma sincronização

Um synce armazenamento conectado será disparado chamando a API de armazenamento conectados existente seguintes:

**C++**

    -   BlobInfoQueryResult::GetBlobInfoAsync
    -   BlobInfoQueryResult::GetItemCountAsync
    -   ConnectedStorageContainer::GetAsync
    -   ConnectedStorageContainer::ReadAsync
    -   ConnectedStorageSpace::DeleteContainerAsync

**C#**

    -   GameSaveBlobInfoQuery.GetBlobInfoAsync
    -   GameSaveBlobInfoQuery.GetItemCountAsync
    -   GameSaveContainer.GetAsync
    -   GameSaveContainer.ReadAsync
    -   GameSaveProvider.DeleteContainerAsync

Isso fará com que o usuário veja a barra de progresso e da interface do usuário de sincronização normal, como dados de seu contêiner selecionado são baixados. Observe que somente os dados do contêiner selecionado for sincronizados; não são baixados, dados de outros contêineres.

Ao chamar essas APIs no contexto de uma sincronização diante de demanda, essas operações podem todos produzir novos códigos de erro a seguir:

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`(`GameSaveErrorStatus.ContainerSyncFailed` na UWP C# API): Esse erro indica que a operação falhou e o contêiner não pôde ser sincronizado com a nuvem. A causa mais provável é que as condições da rede do usuário causadas a falha na sincronização. Querem tentar novamente depois que eles corrigimos sua rede ou eles podem optar por usar um contêiner que não tiverem a sincronização. A interface do usuário deve permitir que qualquer uma dessas opções. Nenhuma caixa de diálogo de repetição for necessária, pois eles serão já viu a caixa de diálogo de repetição de interface do usuário do sistema.

-   `ConnectedStorageErrorStatus::ContainerNotInSync`(`GameSaveErrorStatus.ContainerNotInSync` na UWP C# API): Esse erro indica que o título da incorretamente tentou gravar em um contêiner não sincronizado. Chamando `ConnectedStorageContainer::SubmitUpdatesAsync`(`GameSaveContainer.SubmitUpdatesAsync` na UWP C# API) em um contêiner que tem o sinalizador NeedsSync definido como true não é permitido. Você deve primeiro ler um contêiner para disparar uma sincronização antes de gravar. Se você gravar em um contêiner sem ler dele, é provável que seu título tem um bug em que você poderá perder o progresso do usuário.

Esse comportamento é diferente de quando um usuário é executada offline. Enquanto off-line, há nenhuma indicação de se os contêineres são sincronizados, então, cabe ao usuário para resolver qualquer conflito em um momento posterior. Nesse caso, no entanto, o sistema sabe que o usuário precisa para sincronizar, portanto, ele não permitirá que eles fazerem em um estado ruim por meio de um contêiner desatualizado (embora caso desejem, eles podem reiniciar o título e reproduzi-lo offline).

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4:  Usar o restante da API de armazenamento conectado como normal

Comportamento de armazenamento conectado permanece inalterado durante a sincronização sob demanda.
