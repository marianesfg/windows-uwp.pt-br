---
Description: Learn how to pin secondary tiles to taskbar.
title: Fixar blocos secundários na barra de tarefas
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, uwp, Fixar na barra de tarefas, o bloco secundário, fixar blocos secundários na barra de tarefas, atalho
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8199304"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Fixar blocos secundários na barra de tarefas

Assim como fixar blocos secundários em Iniciar, você pode fixar blocos secundários na barra de tarefas, dando aos usuários acesso rápido à conteúdo dentro de seu aplicativo.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API de acesso limitado**: essa API é um recurso de acesso limitado. Para usar essa API, entre em contato com [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Requer outubro de 2018 atualização**: você deve usar o SDK 17763 e executar o build 17763 ou superior para fixar na barra de tarefas.


## <a name="guidance"></a>Diretrizes

Um bloco secundário fornece uma maneira consistente e eficiente para os usuários acessarem diretamente áreas específicas dentro de um aplicativo. Embora um usuário escolha se deseja ou não "fixar" um bloco secundário na barra de tarefas, as áreas fixáveis em um aplicativo são determinadas pelo desenvolvedor. Para saber mais, consulte [diretrizes de bloco secundário](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. determine se existe uma API e desbloquear acesso limitado

Dispositivos mais antigos não têm a barra de tarefas anexação APIs (se você estiver visando versões mais antigas do Windows 10). Portanto, você não deve exibir um botão de pin nesses dispositivos que não são capazes de fixação.

Além disso, esse recurso está bloqueado em acesso limitado. Para obter acesso, contate a Microsoft. Chamadas de API para **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**, **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** e **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** falhará com uma exceção de acesso negado. Aplicativos não têm permissão para usar essa API sem permissão e a definição de API pode mudar a qualquer momento.

Use o método [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) para determinar se as APIs estão presentes. E, em seguida, use **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API para tentar desbloquear a API.

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


## <a name="2-get-the-taskbarmanager-instance"></a>2. obter a instância de TaskbarManager

Os aplicativos UWP podem ser executados em uma ampla variedade de dispositivos; nem todas eles são compatíveis com a barra de tarefas. No momento, somente dispositivos desktop são compatíveis com a barra de tarefas. Além disso, a presença da barra de tarefas pode vêm e vão. Para verificar se a barra de tarefas está presente no momento, chame o método **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** e verifique que a instância retornada não é nulo. Não exiba um botão de pin se a barra de tarefas não estiver presente.

É recomendável segurando na instância para a duração de uma única operação, como fixação e, em seguida, segurando uma nova instância na próxima vez em que você precisa fazer outra operação.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Verifique se o bloco está fixado na barra de tarefas

Se o bloco já está fixado, você deve exibir um botão Desafixar em vez disso. Você pode usar o método **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** para verificar se o bloco está fixado (os usuários podem desafixá-lo a qualquer momento). Nesse método, você passa o **TileId** do bloco que você deseja saber está fixado.

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


## <a name="4-check-whether-pinning-is-allowed"></a>4. Verifique se a fixação for permitida

Fixar na barra de tarefas pode ser desabilitado pela política de grupo. A propriedade [Ispinningallowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) permite que você verifique se a fixação for permitida.

Quando o usuário clica no botão de pin, você deve verificar essa propriedade, e se for falso, você deve exibir uma caixa de diálogo de mensagem informando ao usuário que não é permitida anexação neste computador.

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


## <a name="5-construct-and-pin-your-tile"></a>5. construir e fixar o bloco

O usuário clicar no botão de pin, e você determinou que as APIs estão presentes, barra de tarefas está presente e fixação for permitida... tempo para fixar!

Primeiro, construa o bloco secundário exatamente como você faria ao fixar em Iniciar. Você pode saber mais sobre as propriedades de bloco secundário lendo [Fixar blocos secundários em Iniciar](secondary-tiles-pinning.md). No entanto, ao Fixar na barra de tarefas, além de propriedades necessárias anteriormente, Square44x44Logo (Este é o logotipo usado pela barra de tarefas) também é necessária. Caso contrário, uma exceção será gerada.

Em seguida, passe o bloco para o método **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** . Como isso é em acesso limitado, isso não exibirá uma caixa de diálogo de confirmação e não exige um thread de interface do usuário. Mas, no futuro quando isso for aberto para cima além do acesso limitado, chamadores não utilizando o acesso limitado receberá uma caixa de diálogo e ser necessária para usar o thread de interface do usuário.

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

Esse método retorna um valor booliano que indica se o bloco agora está fixado na barra de tarefas. Se o bloco já foi fixado, o método atualiza o bloco existente e retorna true. Se não tiver sido permitido fixar ou não é compatível com a barra de tarefas, o método retornará false.


## <a name="enumerate-tiles"></a>Enumerar os blocos

Para ver todos os blocos que você criou e ainda é fixados em algum lugar (inicial, barra de tarefas ou ambos), usam **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Posteriormente, você pode verificar se esses blocos são fixados na barra de tarefas e/ou à tela inicial. Se não houver suporte a superfície, esses métodos retornam falsos.

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

Para atualizar um bloco já fixado, você pode usar o método [**SecondaryTile.UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) conforme descrito em [Atualizando um bloco secundário](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Desafixar um bloco

Seu aplicativo deve fornecer um botão Desafixar se o bloco está fixado. Para desafixar o bloco, basta chame **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, passando o **TileId** do bloco secundário que você gostaria de desafixou.

Esse método retorna um valor booliano que indica se o bloco não está fixado na barra de tarefas. Se o bloco não foi fixado em primeiro lugar, isso também retorna true. Se desafixando não foi permitida, isso retornará false.

Se o bloco somente foi fixado na barra de tarefas, isso excluirá o bloco já que ele não está fixado em qualquer lugar.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Excluir um bloco

Se você quiser Desafixar um bloco de todos os lugares (inicial, barra de tarefas), use o método **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** .

Isso é adequado para casos em que o usuário fixado o conteúdo não é aplicável. Por exemplo, se seu aplicativo permite que você fixar um bloco de anotações para iniciar e barra de tarefas e, em seguida, o usuário exclui o bloco de anotações, você deve excluir o bloco associado com o bloco de anotações simplesmente.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Desafixar apenas da tela inicial

Se você só deseja Desafixar um bloco secundário da tela inicial, deixando-lo na barra de tarefas, você pode chamar o método **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** . Isso excluirá o bloco da mesma forma se ele não for fixado em qualquer outras superfícies.

Esse método retorna um valor booliano que indica se o bloco não está fixado na tela inicial. Se o bloco não foi fixado em primeiro lugar, isso também retorna true. Se desafixando não foi permitida ou inicial não é compatível, isso retornará false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Recursos

* [Classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Fixar blocos secundários em Iniciar](secondary-tiles-pinning.md)