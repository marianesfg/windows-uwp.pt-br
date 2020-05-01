---
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: Baixar e instalar atualizações de pacote da Store
description: Saiba como marcar pacotes como obrigatórios no Partner Center e escrever código no aplicativo para baixar e instalar atualizações de pacote.
ms.date: 04/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cb1ac05bdc5dcaaf31074f1b89e5bbb35e4f850d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68682722"
---
# <a name="download-and-install-package-updates-from-the-store"></a>Baixar e instalar atualizações de pacote da Store

Desde o Windows 10, versão 1607, você pode usar métodos da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para verificar programaticamente se há atualizações de pacote do aplicativo atual na Microsoft Store, baixar e instalar os pacotes atualizados. Também é possível consultar os pacotes que você marcou como obrigatórios no Partner Center e desabilitar a funcionalidade no aplicativo até a atualização obrigatória ser instalada.

Os métodos [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) adicionais introduzidos no Windows 10, versão 1803, permitem que você baixe e instale atualizações de pacote silenciosamente (sem exibir uma interface do usuário de notificação para o usuário), desinstale um [pacote opcional](/windows/msix/package/optional-packages) e obtenha informações sobre pacotes na fila de download e de instalação para seu aplicativo.

Esses recursos ajudam a manter a base de usuários atualizada automaticamente com a versão mais recente do aplicativo, pacotes opcionais e os serviços relacionados da Store.

## <a name="download-and-install-package-updates-with-the-users-permission"></a>Baixar e instalar atualizações do pacote com a permissão do usuário

Este exemplo de código demonstra como usar o método [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) para descobrir todas as atualizações de pacotes disponíveis na Store e chamar o método [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) para baixar e instalar as atualizações. Ao usar esse método para baixar e instalar atualizações, o sistema operacional exibe uma caixa de diálogo que solicita a permissão do usuário antes de baixar as atualizações.

> [!NOTE]
> Esses métodos dão suporte a [pacotes opcionais](/windows/msix/package/optional-packages) e obrigatórios para seu aplicativo. Os pacotes opcionais são úteis para complementos de DLC (conteúdo para download), dividindo seu aplicativo grande por restrições de tamanho ou para enviar conteúdo adicional separado do aplicativo principal. Para obter a permissão para enviar um aplicativo que usa pacotes adicionais (incluindo complementos de DLC) para a Store, confira [Suporte para desenvolvedores do Windows](https://developer.microsoft.com/windows/support).

Este exemplo de código pressupõe que:

* O código é executado no contexto de uma [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page).
* A **Page** contém uma [ProgressBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar) chamada ```downloadProgressBar``` para informar o status da operação de download.
* O arquivo de código tem uma declaração **using** para os namespaces **Windows.Services.Store**, **Windows.Threading.Tasks** e **Windows.UI.Popups**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para um [aplicativo multiusuário](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), use o método [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) para obter um objeto **StoreContext**, e não o método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault).

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

> [!NOTE]
> Para apenas baixar (mas não instalar) as atualizações de pacote disponíveis, use o método [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync).

### <a name="display-download-and-install-progress-info"></a>Exibir informações de progresso do download e da instalação

Ao chamar o método [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) ou [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) é possível atribuir um manipulador de [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress) que é chamado uma vez para cada etapa no processo de download (ou download e instalação) para cada pacote na solicitação. O manipulador recebe um objeto do [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) que fornece informações sobre o pacote de atualização que gerou a notificação de progresso. O exemplo anterior usa o campo **PackageDownloadProgress** do objeto **StorePackageUpdateStatus** para exibir o progresso do processo de download e instalação.

Saiba que, quando você chamar **RequestDownloadAndInstallStorePackageUpdatesAsync** para baixar e instalar pacotes de atualizações em uma única operação, o campo **PackageDownloadProgress** aumentará de 0,0 para 0,8 durante o processo de download de um pacote, depois aumentará de 0,8 para 1,0 durante a instalação. Portanto, se você mapear a porcentagem mostrada na interface de usuário personalizada do progresso diretamente para o valor do campo **PackageDownloadProgress**, sua interface de usuário será exibida como 80% quando o pacote terminar de baixar e o sistema operacional exibir a caixa de diálogo de instalação. Se você quiser que sua interface de usuário personalizada do progresso exiba 100% quando o pacote for baixado e estiver pronto para ser instalado, você poderá modificar o código para atribuir 100% à interface de usuário do progresso quando o campo **PackageDownloadProgress** atingir 0,8.

## <a name="download-and-install-package-updates-silently"></a>Baixar e instalar silenciosamente as atualizações de pacote

A partir do Windows 10, versão 1803, é possível usar os métodos [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) e [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) para baixar e instalar atualizações de pacote silenciosamente, sem exibir uma interface do usuário de notificação para o usuário. Essa operação funcionará somente se o usuário tiver habilitado a configuração **Atualizar aplicativos automaticamente** na Store e não estiver em uma rede limitada. Antes de chamar esses métodos, você pode verificar primeiro a propriedade [CanSilentlyDownloadStorePackageUpdates](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates) para determinar se essas condições são atendidas no momento.

Este exemplo de código demonstra como usar o método [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) para descobrir todas as atualizações de pacotes disponíveis e chamar os métodos [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) e [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) para baixar e instalar as atualizações silenciosamente.

Este exemplo de código pressupõe que:
* O arquivo de código tem uma declaração em **using** para os namespaces **Windows.Services.Store** e **System.Threading.Tasks**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para um [aplicativo multiusuário](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), use o método [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) para obter um objeto **StoreContext**, e não o método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault).

> [!NOTE]
> Os métodos **IsNowAGoodTimeToRestartApp**, **RetryDownloadAndInstallLater** e **RetryInstallLater** chamados pelo código no exemplo são métodos de espaço reservado que devem ser implementados conforme necessário, de acordo com o design do seu próprio aplicativo.

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesInBackgroundAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> storePackageUpdates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (storePackageUpdates.Count > 0)
    {

        if (!context.CanSilentlyDownloadStorePackageUpdates)
        {
            return;
        }

        // Start the silent downloads and wait for the downloads to complete.
        StorePackageUpdateResult downloadResult =
            await context.TrySilentDownloadStorePackageUpdatesAsync(storePackageUpdates);

        switch (downloadResult.OverallState)
        {
            case StorePackageUpdateState.Completed:
                // The download has completed successfully. At this point, confirm whether your app
                // can restart now and then install the updates (for example, you might only install
                // packages silently if your app has been idle for a certain period of time). The
                // IsNowAGoodTimeToRestartApp method is not implemented in this example, you should
                // implement it as needed for your own app.
                if (IsNowAGoodTimeToRestartApp())
                {
                    await InstallUpdate(storePackageUpdates);
                }
                else
                {
                    // Retry/reschedule the installation later. The RetryInstallLater method is not  
                    // implemented in this example, you should implement it as needed for your own app.
                    RetryInstallLater();
                    return;
                }
                break;
            // If the user cancelled the download or you can't perform the download for some other
            // reason (for example, Wi-Fi might have been turned off and the device is now on
            // a metered network) try again later. The RetryDownloadAndInstallLater method is not  
            // implemented in this example, you should implement it as needed for your own app.
            case StorePackageUpdateState.Canceled:
            case StorePackageUpdateState.ErrorLowBattery:
            case StorePackageUpdateState.ErrorWiFiRecommended:
            case StorePackageUpdateState.ErrorWiFiRequired:
            case StorePackageUpdateState.OtherError:
                RetryDownloadAndInstallLater();
                return;
            default:
                break;
        }
    }
}

private async Task InstallUpdate(IReadOnlyList<StorePackageUpdate> storePackageUpdates)
{
    // Start the silent installation of the packages. Because the packages have already
    // been downloaded in the previous method, the following line of code just installs
    // the downloaded packages.
    StorePackageUpdateResult downloadResult =
        await context.TrySilentDownloadAndInstallStorePackageUpdatesAsync(storePackageUpdates);

    switch (downloadResult.OverallState)
    {
        // If the user cancelled the installation or you can't perform the installation  
        // for some other reason, try again later. The RetryInstallLater method is not  
        // implemented in this example, you should implement it as needed for your own app.
        case StorePackageUpdateState.Canceled:
        case StorePackageUpdateState.ErrorLowBattery:
        case StorePackageUpdateState.OtherError:
            RetryInstallLater();
            return;
        default:
            break;
    }
}
```

## <a name="mandatory-package-updates"></a>Atualizações de pacote obrigatórias

Ao criar um envio de pacote no Partner Center para um aplicativo destinado ao Windows 10, versão 1607 ou posterior, você pode [marcar o pacote como obrigatório](../publish/upload-app-packages.md#mandatory-update) e a data e hora em que ele se torna obrigatório. Quando essa propriedade está definida e o aplicativo descobre que a atualização do pacote está disponível, ele pode determinar se o pacote de atualizações é obrigatório e alterar o comportamento até a atualização ser instalada (por exemplo, o aplicativo pode desabilitar recursos).

> [!NOTE]
> O status obrigatório de uma atualização de pacote não é imposto pela Microsoft, e o sistema operacional não fornece uma interface do usuário para indicar aos usuários que uma atualização de aplicativo obrigatória deve ser instalada. Os desenvolvedores devem usar a configuração obrigatória para impor atualizações obrigatórias de aplicativos no próprio código.  

Para marcar um envio de pacote como obrigatório:

1. Faça logon no [Partner Center](https://partner.microsoft.com/dashboard) e navegue até a página de visão geral do aplicativo.
2. Clique no nome do envio que contém a atualização de pacote que você deseja tornar obrigatória.
3. Navegue até a página **Pacotes** do envio. Na parte inferior dessa página, selecione **Make this update mandatory** e, em seguida, escolha o dia e hora em que a atualização de pacote e se tornará obrigatória. Essa opção se aplica a todos os pacotes UWP no envio.

Para obter mais informações, confira [Carregar pacotes de aplicativos](../publish/upload-app-packages.md).

> [!NOTE]
> Se criar um [pacote de pré-lançamento](../publish/package-flights.md), você poderá marcar os pacotes como obrigatórios usando uma interface do usuário semelhante na página **Pacotes** da versão de pré-lançamento. Nesse caso, a atualização de pacote obrigatória só se aplica a clientes que façam parte do grupo da versão de pré-lançamento.

### <a name="code-example-for-mandatory-packages"></a>Exemplo de código para pacotes obrigatórios

O exemplo de código a seguir demonstra como determinar se algum pacote de atualização é obrigatório. Normalmente, você deverá fazer downgrade da experiência do aplicativo normalmente para o usuário se uma atualização de pacote obrigatória não puder ser baixada ou instalada.

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

## <a name="uninstall-optional-packages"></a>Desinstalar pacotes opcionais

A partir do Windows 10, versão 1803, você pode usar os métodos [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync) ou [RequestUninstallStorePackageByStoreIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync) para desinstalar um [pacote opcional](/windows/msix/package/optional-packages) (incluindo um pacote de DLC) do aplicativo atual. Por exemplo, caso você tenha um aplicativo com conteúdo que é instalado por meio de pacotes opcionais, talvez seja necessário fornecer uma interface do usuário que permita aos usuários desinstalar os pacotes opcionais para liberar espaço em disco.

O exemplo de código a seguir demonstra como chamar [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync). Este exemplo pressupõe que:
* O arquivo de código tem uma declaração em **using** para os namespaces **Windows.Services.Store** e **System.Threading.Tasks**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para um [aplicativo multiusuário](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), use o método [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) para obter um objeto **StoreContext**, e não o método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault).

```csharp
public async Task UninstallPackage(Windows.ApplicationModel.Package package)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    var storeContext = StoreContext.GetDefault();
    IAsyncOperation<StoreUninstallStorePackageResult> uninstallOperation =
        storeContext.RequestUninstallStorePackageAsync(package);

    // At this point, you can update your app UI to show that the package
    // is installing.

    uninstallOperation.Completed += (asyncInfo, status) =>
    {
        StoreUninstallStorePackageResult result = uninstallOperation.GetResults();
        switch (result.Status)
        {
            case StoreUninstallStorePackageStatus.Succeeded:
                {
                    // Update your app UI to show the package as uninstalled.
                    break;
                }
            default:
                {
                    // Update your app UI to show that the package uninstall failed.
                    break;
                }
        }
    };
}
```

## <a name="get-download-queue-info"></a>Obter informações da fila de download

A partir do Windows 10, versão 1803, você pode usar os métodos [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) e [GetStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync) para obter informações sobre os pacotes que estão na fila atual de download e instalação na Store. Esses métodos são úteis quando seu aplicativo ou jogo dá suporte a grandes pacotes opcionais (incluindo DLCs) que podem levar horas ou dias para serem baixados e instalados, e você deseja lidar normalmente com a situação em que um cliente fecha seu aplicativo ou jogo antes da conclusão do processo de download e instalação. Quando o cliente iniciar seu aplicativo ou jogo novamente, seu código poderá usar esses métodos a fim de obter informações sobre o estado dos pacotes que ainda estão na fila de download e instalação para que você possa exibir o status de cada um ao cliente.

O exemplo de código a seguir demonstra como chamar [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) para obter a lista de atualizações de pacote em andamento para o aplicativo atual e recuperar informações sobre o status de cada pacote. Este exemplo pressupõe que:
* O arquivo de código tem uma declaração em **using** para os namespaces **Windows.Services.Store** e **System.Threading.Tasks**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para um [aplicativo multiusuário](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), use o método [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) para obter um objeto **StoreContext**, e não o método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault).

> [!NOTE]
> Os métodos **MarkUpdateInProgressInUI**, **RemoveItemFromUI**, **MarkInstallCompleteInUI**, **MarkInstallErrorInUI** e **MarkInstallPausedInUI** chamados pelo código no exemplo são métodos de espaço reservado que devem ser implementados conforme necessário, de acordo com o design de seu próprio aplicativo.

```csharp
private StoreContext context = null;

private async Task GetQueuedInstallItemsAndBuildInitialStoreUI()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the Store packages in the install queue.
    IReadOnlyList<StoreQueueItem> storeUpdateItems = await context.GetAssociatedStoreQueueItemsAsync();

    foreach (StoreQueueItem storeItem in storeUpdateItems)
    {
        // In this example we only care about package updates.
        if (storeItem.InstallKind != StoreQueueItemKind.Update)
            continue;

        StoreQueueItemStatus currentStatus = storeItem.GetCurrentStatus();
        StoreQueueItemState installState = currentStatus.PackageInstallState;
        StoreQueueItemExtendedState extendedInstallState =
            currentStatus.PackageInstallExtendedState;

        // Handle the StatusChanged event to display current status to the customer.
        storeItem.StatusChanged += StoreItem_StatusChanged;

        switch (installState)
        {
            // Download and install are still in progress, so update the status for this  
            // item and provide the extended state info. The following methods are not
            // implemented in this example; you should implement them as needed for your
            // app's UI.
            case StoreQueueItemState.Active:
                MarkUpdateInProgressInUI(storeItem, extendedInstallState);
                break;
            case StoreQueueItemState.Canceled:
                RemoveItemFromUI(storeItem);
                break;
            case StoreQueueItemState.Completed:
                MarkInstallCompleteInUI(storeItem);
                break;
            case StoreQueueItemState.Error:
                MarkInstallErrorInUI(storeItem);
                break;
            case StoreQueueItemState.Paused:
                MarkInstallPausedInUI(storeItem, installState, extendedInstallState);
                break;
        }
    }
}

private void StoreItem_StatusChanged(StoreQueueItem sender, object args)
{
    StoreQueueItemStatus currentStatus = sender.GetCurrentStatus();
    StoreQueueItemState installState = currentStatus.PackageInstallState;
    StoreQueueItemExtendedState extendedInstallState = currentStatus.PackageInstallExtendedState;

    switch (installState)
    {
        // Download and install are still in progress, so update the status for this  
        // item and provide the extended state info. The following methods are not
        // implemented in this example; you should implement them as needed for your
        // app's UI.
        case StoreQueueItemState.Active:
            MarkUpdateInProgressInUI(sender, extendedInstallState);
            break;
        case StoreQueueItemState.Canceled:
            RemoveItemFromUI(sender);
            break;
        case StoreQueueItemState.Completed:
            MarkInstallCompleteInUI(sender);
            break;
        case StoreQueueItemState.Error:
            MarkInstallErrorInUI(sender);
            break;
        case StoreQueueItemState.Paused:
            MarkInstallPausedInUI(sender, installState, extendedInstallState);
            break;
    }
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Criação de pacotes opcionais e conjunto relacionado](/windows/msix/package/optional-packages)
