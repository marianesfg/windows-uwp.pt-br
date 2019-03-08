---
Description: Saiba como fixar blocos secundários na barra de tarefas.
title: Fixar blocos secundários na barra de tarefas
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: o Windows 10, uwp, Fixar na barra de tarefas, o bloco secundário, fixar blocos secundários na barra de tarefas, atalho
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591971"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Fixar blocos secundários na barra de tarefas

Assim como fixar blocos secundários para iniciar, você pode fixar blocos secundários na barra de tarefas, dando a seus usuários acesso rápido ao conteúdo dentro de seu aplicativo.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API de acesso limitado**: Essa API é um recurso de acesso limitado. Para usar essa API, entre em contato com [ taskbarsecondarytile@microsoft.com ](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Requer a atualização de outubro de 2018**: Você deve ter como destino SDK 17763 e executar o build 17763 ou superior para fixar na barra de tarefas.


## <a name="guidance"></a>Orientação

Um bloco secundário fornece uma maneira consistente e eficiente para que os usuários acessem diretamente a áreas específicas dentro de um aplicativo. Embora um usuário escolhe se deseja ou não "fixar" em um bloco secundário na barra de tarefas, as áreas de conversação em um aplicativo são determinadas pelo desenvolvedor. Para obter instruções, consulte [diretrizes de bloco secundário](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. Determine se existe uma API e desbloquear o acesso limitado

Dispositivos mais antigos não têm a fixação de APIs (se você estiver direcionando versões mais antigas do Windows 10) na barra de tarefas. Portanto, você não deve exibir um botão de pin nesses dispositivos que não são capazes de fixar.

Além disso, esse recurso está bloqueado em acesso limitado. Para obter acesso, entre em contato com a Microsoft. Chamadas à API para  **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**,  **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** e **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** falhará com uma exceção de acesso negado. Aplicativos não têm permissão para usar essa API sem permissão e a definição de API pode mudar a qualquer momento.

Use o [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) método para determinar se as APIs estão presentes. E, em seguida, use o **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API para tentar desbloquear a API.

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. Obter a instância de TaskbarManager

Os aplicativos UWP podem ser executados em uma ampla variedade de dispositivos; nem todas eles são compatíveis com a barra de tarefas. No momento, somente dispositivos desktop são compatíveis com a barra de tarefas. Além disso, a presença da barra de tarefas pode vêm e vão. Para verificar se a barra de tarefas está presente no momento, chame o **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** método e verifique se a instância retornada não for nulo. Não exiba um botão de pin se a barra de tarefas não estiver presente.

Recomendamos que espera para a instância para a duração de uma única operação, como fixação e, em seguida, pegando uma nova instância na próxima vez que você precisa fazer outra operação.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Verifique se o seu bloco atualmente está fixado na barra de tarefas

Se seu bloco já está fixado, você deve exibir um botão Desafixar em vez disso. Você pode usar o **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** método para verificar se o seu bloco está fixado no momento (os usuários podem desafixá-lo a qualquer momento). Esse método, você pode passar o **TileId** do bloco que você deseja saber é fixado.

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. Verifique se a anexação é permitida

Fixar na barra de tarefas pode ser desabilitado pela diretiva de grupo. O [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) propriedade permite que você verifique se a anexação é permitida.

Quando o usuário clica o botão de pin, você deve verificar essa propriedade e, se for false, você deve exibir uma caixa de diálogo de mensagem informando ao usuário que fixação não é permitida neste computador.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. Construir e fixar o bloco

O usuário clicou o botão de pin e, em seguida, você determinou que as APIs estão presentes, na barra de tarefas está presente e fixando é permitida.. hora de pin!

Primeiro, construa seu bloco secundário exatamente como você faria ao fixar para iniciar. Você pode aprender mais sobre as propriedades do bloco secundário lendo [fixar blocos secundários para iniciar](secondary-tiles-pinning.md). No entanto, ao Fixar na barra de tarefas, além das propriedades necessárias anteriormente, Square44x44Logo (esse é o logotipo usado pela barra de tarefas) também é necessária. Caso contrário, uma exceção será lançada.

Em seguida, passe o bloco para o **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** método. Uma vez que isso está sob acesso limitado, isso não será exibida uma caixa de diálogo de confirmação e não requer um thread de interface do usuário. Mas no futuro quando isso for aberto para cima além do acesso limitado, os chamadores não utilizar o acesso limitado receberá uma caixa de diálogo e ser necessário usar o thread de interface do usuário.

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

Esse método retorna um valor booliano que indica se o seu bloco agora está fixado na barra de tarefas. Se seu bloco já foi fixado, o método atualiza o bloco existente e retorna true. Se a anexação não foi permitida ou não há suporte para uma barra de tarefas, o método retornará false.


## <a name="enumerate-tiles"></a>Enumerar os blocos

Para ver todos os blocos que você criou e ainda são fixados em algum lugar (início, na barra de tarefas ou ambos), use  **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Posteriormente, você pode verificar se esses blocos são fixados para o início e/ou na barra de tarefas. Se não há suporte para a superfície, esses métodos retornam falsos.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>Atualizar um bloco

Para atualizar um bloco já fixado, você pode usar o [ **SecondaryTile.UpdateAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) método conforme descrito na [atualização de um bloco secundário](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Desafixar blocos

Seu aplicativo deve fornecer um botão Desafixar se o bloco é fixado no momento. Para desafixar o mosaico, simplesmente chame  **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, passando o **TileId** do bloco secundário você gostaria de desafixada.

Esse método retorna um valor booliano que indica se o bloco não está fixado na barra de tarefas. Se o bloco não foi fixado em primeiro lugar, isso também retornará true. Se não fosse permitido Desafixar, isso retorna false.

Se apenas o seu bloco foi fixado na barra de tarefas, isso excluirá o bloco, pois ele não está fixado em qualquer lugar.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Excluir um bloco

Se você quiser Desafixar um bloco de qualquer lugar na barra de tarefas (início), use o **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** método.

Isso é adequado para casos em que o usuário fixado de conteúdo não é mais aplicável. Por exemplo, se seu aplicativo permite que você fixar um bloco de anotações para iniciar e barra de tarefas e, em seguida, o usuário exclui o bloco de anotações, você deve simplesmente excluir o bloco associado com o bloco de anotações.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Somente Desafixar da tela inicial

Se você quiser apenas Desafixar um bloco secundário do início, deixando-o na barra de tarefas, você pode chamar o **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** método. Isso excluirá o bloco da mesma forma se ele não estiver mais fixado para quaisquer outras superfícies.

Esse método retorna um valor booliano que indica se o bloco não está fixado para iniciar. Se o bloco não foi fixado em primeiro lugar, isso também retornará true. Se Desafixar não foi permitido ou não há suporte para iniciar, retorna false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Recursos

* [Classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Fixar blocos secundários para iniciar](secondary-tiles-pinning.md)