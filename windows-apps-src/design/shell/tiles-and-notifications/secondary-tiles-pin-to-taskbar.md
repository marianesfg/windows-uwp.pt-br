---
Description: Saiba como fixar blocos secundários na barra de tarefas.
title: Fixar blocos secundários na barra de tarefas
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, UWP, fixar na barra de tarefas, bloco secundário, fixar blocos secundários na barra de tarefas, atalho
ms.localizationpriority: medium
ms.openlocfilehash: 5d4041faf2fbc729291da902e66be1e0979f9d97
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971011"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Fixar blocos secundários na barra de tarefas

Assim como fixação de blocos secundários para iniciar, você pode fixar blocos secundários na barra de tarefas, dando a seus usuários acesso rápido ao conteúdo em seu aplicativo.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API de acesso limitado**: essa API é um recurso de acesso limitado. Para usar essa API, entre em [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)contato com.

> **Requer atualização de outubro de 2018**: você deve ter como destino o SDK 17763 e executar o Build 17763 ou superior para fixar na barra de tarefas.


## <a name="guidance"></a>Orientação

Um bloco secundário fornece uma maneira consistente e eficiente para que os usuários acessem diretamente áreas específicas dentro de um aplicativo. Embora um usuário decida se deseja ou não fixar um bloco secundário na barra de tarefas, as áreas fixas em um aplicativo são determinadas pelo desenvolvedor. Para obter mais diretrizes, consulte [diretrizes de bloco secundário](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. Determine se a API existe e desbloqueie o acesso limitado

Dispositivos mais antigos não têm as APIs de fixação da barra de tarefas (se você estiver visando versões mais antigas do Windows 10). Portanto, você não deve exibir um botão de PIN nesses dispositivos que não são capazes de fixar.

Além disso, esse recurso está bloqueado em acesso limitado. Para obter acesso, entre em contato com a Microsoft. As chamadas à API para **[taskbarprovider. RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**, **[TaskbarManager. IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** e **[TaskbarManager. TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** falharão com uma exceção de acesso negado. Os aplicativos não têm permissão para usar essa API sem permissão e a definição de API pode ser alterada a qualquer momento.

Use o método [ApiInformation. IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) para determinar se as APIs estão presentes. E, em seguida, usar a API **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** para tentar desbloquear a API.

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


## <a name="2-get-the-taskbarmanager-instance"></a>2. obter a instância da barra de tarefas

Os aplicativos do Windows podem ser executados em uma ampla variedade de dispositivos; Nem todos eles dão suporte à barra de tarefas. No momento, somente dispositivos desktop são compatíveis com a barra de tarefas. Além disso, a presença da barra de tarefas pode vir e ir. Para verificar se a barra de tarefas está presente no momento, chame o método **[taskbarprovider. GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** e verifique se a instância retornada não é nula. Não exibir um botão de PIN se a barra de tarefas não estiver presente.

É recomendável manter a instância do pela duração de uma única operação, como fixação e, em seguida, capturar uma nova instância na próxima vez que você precisar realizar outra operação.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Verifique se o bloco está fixado no momento na barra de tarefas

Se o bloco já estiver fixado, você deverá exibir um botão Desafixar. Você pode usar o método **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** para verificar se o bloco está fixado no momento (os usuários podem desafixa-lo a qualquer momento). Nesse método, você passa o **tileid** do bloco que você deseja saber que está fixado.

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


## <a name="4-check-whether-pinning-is-allowed"></a>4. Verifique se a fixação é permitida

A fixação na barra de tarefas pode ser desabilitada por Política de Grupo. A propriedade [TaskbarManager. IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) permite verificar se a fixação é permitida.

Quando o usuário clica no botão PIN, você deve verificar essa propriedade e, se for false, deverá exibir uma caixa de diálogo de mensagem informando que o usuário que está sendo fixado não é permitido neste computador.

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


## <a name="5-construct-and-pin-your-tile"></a>5. construa e fixe seu bloco

O usuário clicou no botão PIN e você determinou que as APIs estão presentes, a barra de tarefas está presente e a fixação é permitida... tempo para fixar!

Primeiro, construa seu bloco secundário assim como faria ao fixar no início. Você pode saber mais sobre as propriedades de bloco secundários lendo [fixar blocos secundários para iniciar](secondary-tiles-pinning.md). No entanto, ao fixar na barra de tarefas, além das propriedades necessárias anteriormente, Square44x44Logo (esse é o logotipo usado pela barra de tarefas) também é necessário. Caso contrário, será gerada uma exceção.

Em seguida, passe o bloco para o método **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** . Como isso está sob acesso limitado, isso não exibirá uma caixa de diálogo de confirmação e não exigirá um thread de interface do usuário. Mas no futuro, quando isso for aberto além do acesso limitado, os chamadores que não utilizam acesso limitado receberão uma caixa de diálogo e precisarão usar o thread da interface do usuário.

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

Esse método retorna um valor booliano que indica se o bloco agora está fixado na barra de tarefas. Se o bloco já foi fixado, o método atualiza o bloco existente e retorna true. Se a fixação não tiver sido permitida ou a barra de tarefas não tiver suporte, o método retornará false.


## <a name="enumerate-tiles"></a>Enumerar blocos

Para ver todos os blocos que você criou e que ainda estão fixados em algum lugar (Iniciar, barra de tarefas ou ambos), use **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Em seguida, você pode verificar se esses blocos estão fixados na barra de tarefas e/ou iniciar. Se a superfície não tiver suporte, esses métodos retornarão false.

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

Para atualizar um bloco já fixado, você pode usar o método [**SecondaryTile. UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) conforme descrito em [atualizando um bloco secundário](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Desafixar um bloco

Seu aplicativo deverá fornecer um botão Desafixar se o bloco estiver fixado no momento. Para desafixar o bloco, basta chamar **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, passando o **tileid** do bloco secundário que você gostaria de desafixado.

Esse método retorna um valor booliano que indica se o bloco não está mais fixado na barra de tarefas. Se o bloco não foi fixado em primeiro lugar, isso também retorna verdadeiro. Se a desanexação não for permitida, isso retornará false.

Se seu bloco foi fixado apenas na barra de tarefas, isso excluirá o bloco, pois ele não está mais fixado em nenhum lugar.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Excluir um bloco

Se você quiser Desafixar um bloco de qualquer lugar (Iniciar, barra de tarefas), use o método **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** .

Isso é apropriado para casos em que o conteúdo fixado pelo usuário não é mais aplicável. Por exemplo, se seu aplicativo permitir que você fixe um bloco de anotações para iniciar e barra de tarefas e, em seguida, o usuário excluir o bloco de anotações, você deverá simplesmente excluir o bloco associado ao bloco de anotações.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Desafixar apenas do início

Se você quiser Desafixar um bloco secundário do início ao deixá-lo na barra de tarefas, poderá chamar o método **[StartScreenManager. TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** . Assim, isso excluirá o bloco se ele não estiver mais fixado em nenhuma outra superfície.

Esse método retorna um valor booliano que indica se o bloco não está mais fixado para iniciar. Se o bloco não foi fixado em primeiro lugar, isso também retorna verdadeiro. Se a desanexação não for permitida ou iniciar não tiver suporte, isso retornará false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Recursos

* [Classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Fixar blocos secundários para iniciar](secondary-tiles-pinning.md)
