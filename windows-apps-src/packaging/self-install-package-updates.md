---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "Baixar e instalar atualizações de pacote para seu aplicativo"
description: "Saiba como marcar pacotes como obrigatórios no painel do Centro de Desenvolvimento e escrever código no aplicativo para baixar e instalar atualizações do pacote."
translationtype: Human Translation
ms.sourcegitcommit: 7df130e13685b519d5cc1353c8d64878ecc3d213
ms.openlocfilehash: adb9b999c88649fc2c8ade838dfa0dabc407c075

---
# Baixar e instalar atualizações de pacote para seu aplicativo

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Desde o Windows 10, versão 1607, você pode usar uma API no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para verificar programaticamente se há atualizações do pacote do aplicativo atual, baixar e instalar os pacotes atualizados. Você também pode consultar os pacotes que foram [marcados como obrigatórios no painel do Centro de Desenvolvimento do Windows](#mandatory-dashboard) e desabilitar a funcionalidade no aplicativo até que a atualização obrigatória seja instalada.

Esses recursos ajudam a manter a base de usuários atualizada automaticamente com a versão mais recente do aplicativo e os serviços relacionados.

## Baixar e instalar atualizações de pacote no aplicativo

Os aplicativos voltados para o Windows 10, versão 1607, ou posteriores, podem usar os seguintes métodos da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para baixar e instalar atualizações de pacote:

* Use [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) para determinar quais atualizações de pacote estão disponíveis.
* Use [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx) para baixar (mas não instalar) o pacote de atualizações.
* Use [RequestDownloadAndInstallStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706585.aspx) para baixar e instalar o pacote de atualizações. Se você já tiver baixado as atualizações de pacote chamando [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx), esse método ignorará o processo de download e só instalará as atualizações.

A classe [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) representa um pacote de atualizações disponível:
* Use a propriedade [Mandatory](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.mandatory.aspx) para determinar se o pacote é marcado como obrigatório no painel do Centro de Desenvolvimento.
* Use a propriedade [Package](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.package.aspx) para acessar outros dados relacionados ao pacote.

>**Observação** Existe uma latência de até um dia entre o momento quando um pacote passa pelo processo de certificação e quando o método [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) reconhece que a atualização do pacote está disponível para o aplicativo.


### Exemplos de código

Os exemplos de código a seguir demonstram como baixar e instalar atualizações de pacote no aplicativo. Os exemplos pressupõem:
* O código é executado no contexto de uma [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx).
* A **Page** contém uma [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx) chamada ```downloadProgressBar``` para informar o status da operação de download.
* O arquivo de código tenha uma instrução **using** para o namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para um [aplicativo multiusuário](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), use o método [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) para obter um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), e não o método [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx).

#### Baixar e instalar todas as atualizações de pacote

O exemplo de código a seguir demonstra como baixar e instalar todas as atualizações de pacote disponíveis.  

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

#### Manipular atualizações de pacote obrigatórias

O exemplo de código a seguir parte do exemplo anterior e demonstra como determinar se algum pacote de atualizações foi [marcado como obrigatório no painel do Centro de Desenvolvimento do Windows](#mandatory-dashboard). Normalmente, você deverá fazer downgrade da experiência do aplicativo normalmente para o usuário se uma atualização de pacote obrigatória não puder ser baixada ou instalada.

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

<span id="mandatory-dashboard" />
## Tornar o envio de um pacote obrigatório no painel do Centro de Desenvolvimento

Ao criar um envio de pacote para um aplicativo destinado ao Windows 10, versão 1607, ou posterior, você pode marcar o pacote como obrigatório e a data/hora em que ele se torna obrigatório. Quando essa propriedade está definida e o aplicativo descobre que a atualização do pacote está disponível usando-se a API descrita anteriormente neste artigo, o aplicativo pode determinar se o pacote de atualizações é obrigatório e alterar o comportamento até a atualização ser instalada (por exemplo, o aplicativo pode desabilitar recursos).

>**Observação** O status obrigatório de um pacote não é imposto pela Microsoft. Os desenvolvedores devem usar a configuração obrigatória para impor atualizações obrigatórias no próprio código.

Para marcar um envio de pacote como obrigatório:

1. Faça logon no [Painel do Centro de Desenvolvimento](https://dev.windows.com/overview) e navegue até a página de visão geral do aplicativo.
2. Clique no nome do envio que contém a atualização de pacote que você deseja tornar obrigatória.
3. Navegue até a página **Pacotes** do envio. Na parte inferior dessa página, selecione **Make this update mandatory** e, em seguida, escolha o dia e hora em que a atualização de pacote e se tornará obrigatória. Essa opção se aplica a todos os pacotes UWP no envio.

Para obter mais informações sobre como configurar pacotes no painel do Centro de Desenvolvimento, consulte [Carregue os pacotes do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/upload-app-packages).

  >**Observação** Se criar um [pacote de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights), você poderá marcar os pacotes como obrigatórios usando uma interface do usuário semelhante na página **Pacotes** da versão de pré-lançamento. Nesse caso, a atualização de pacote obrigatória só se aplica a clientes que façam parte do grupo da versão de pré-lançamento.



<!--HONumber=Aug16_HO5-->


