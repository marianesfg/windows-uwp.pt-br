---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "Baixar e instalar atualizações de pacote para seu aplicativo"
description: "Saiba como marcar pacotes como obrigatórios no painel do Centro de Desenvolvimento e escrever código no aplicativo para baixar e instalar atualizações do pacote."
ms.author: mcleans
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 07b8b769cbcaf86bfa70a562de568cab65c91a77
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="download-and-install-package-updates-for-your-app"></a>Baixar e instalar atualizações de pacote para seu aplicativo

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Desde o Windows 10, versão 1607, você pode usar uma API no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para verificar programaticamente se há atualizações do pacote do aplicativo atual, baixar e instalar os pacotes atualizados. Você também pode consultar os pacotes que foram [marcados como obrigatórios no painel do Centro de Desenvolvimento do Windows](#mandatory-dashboard) e desabilitar a funcionalidade no aplicativo até que a atualização obrigatória seja instalada.

Esses recursos ajudam a manter a base de usuários atualizada automaticamente com a versão mais recente do aplicativo e os serviços relacionados.

## <a name="api-overview"></a>Visão geral de API

Os aplicativos voltados para o Windows 10, versão 1607, ou posteriores, podem usar os seguintes métodos da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) para baixar e instalar atualizações de pacote.

|  Método  |  Descrição  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppAndOptionalStorePackageUpdatesAsync) | Chame esse método para obter a lista de atualizações de pacote que estão disponíveis. Observe que pode haver um atraso de até um dia entre o momento quando um pacote passa pelo processo de certificação e quando o método **GetAppAndOptionalStorePackageUpdatesAsync** reconhece que a atualização do pacote está disponível para o aplicativo. |
| [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | Chame esse método para baixar (mas não instalar) as atualizações de pacote disponíveis. Esse sistema operacional exibe uma caixa de diálogo que solicita a permissão do usuário para baixar as atualizações. |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadAndInstallStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | Chame esse método para baixar e instalar as atualizações de pacote disponíveis. O sistema operacional exibe as caixas de diálogo que pedem a permissão do usuário para baixar e instalar as atualizações. Se você já tiver baixado as atualizações de pacote chamando **RequestDownloadStorePackageUpdatesAsync**, esse método ignorará o processo de download e só instalará as atualizações.  |

<span/>

Esses métodos utilizam objetos [StorePackageUpdate](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate) para representar pacotes de atualização disponíveis. Use as seguintes propriedades **StorePackageUpdate** para obter informações sobre um pacote de atualização.

|  Propriedade  |  Descrição  |
|----------|---------------|
| [Obrigatório](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Mandatory) | Use essa propriedade para determinar se o pacote é marcado como obrigatório no painel do Centro de Desenvolvimento. |
| [Pacote](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Package) | Use essa propriedade para acessar os dados subjacentes relacionados ao pacote. |

<span/>

## <a name="code-examples"></a>Exemplos de código

Os exemplos de código a seguir demonstram como baixar e instalar atualizações de pacote no aplicativo. Os exemplos pressupõem:
* O código é executado no contexto de uma [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx).
* A **Page** contém uma [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx) chamada ```downloadProgressBar``` para informar o status da operação de transferência.
* O arquivo de código tem uma declaração **em uso** para o **Windows.Services.Store**, **Windows.Threading.Tasks** e namespaces **Windows.UI.Popups**.
* O aplicativo é um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para um [aplicativo multiusuário](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), use o método [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetForUser_Windows_System_User_) para obter um objeto **StoreContext**, e não o método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetDefault).

<span/>

### <a name="download-and-install-all-package-updates"></a>Baixar e instalar todas as atualizações de pacote

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

### <a name="handle-mandatory-package-updates"></a>Manipular atualizações de pacote obrigatórias

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

### <a name="display-progress-info-for-the-download-and-install"></a>Exibir informações de progresso para transferência e instalação

Quando você chama **RequestDownloadStorePackageUpdatesAsync** ou **RequestDownloadAndInstallStorePackageUpdatesAsync**, você pode atribuir um manipulador de [Progresso](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) que é chamado uma vez para cada etapa no processo de download (ou download e instalação) para cada pacote nesta solicitação. O manipulador recebe um objeto do [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) que fornece informações sobre o pacote de atualização que gerou a notificação de progresso. Os exemplos anteriores usam o campo **PackageDownloadProgress** do objeto **StorePackageUpdateStatus** para exibir o progresso da transferência e instalar o processo.

Saiba que quando você chamar **RequestDownloadAndInstallStorePackageUpdatesAsync** para baixar e instalar pacotes de atualizações em uma única operação, o campo **PackageDownloadProgress** aumenta de 0,0 para 0,8 durante o processo de transferência de um pacote, e depois ele aumenta de 0,8 para 1,0 durante a instalação. Portanto, se você mapear a porcentagem mostrada em seu progresso de interface de usuário personalizado diretamente ao valor do campo **PackageDownloadProgress**, sua interface de usuário será exibida como 80% quando o pacote terminar de baixar e o OS exibir o diálogo de instalação. Se você quiser que seu progresso personalizado da interface do usuário exiba 100% quando o pacote é baixado e está pronto para ser instalado, você pode modificar o código para atribuir 100% ao seu progresso da interface do usuário quando o campo **PackageDownloadProgress** atingir 0,8.

<span id="mandatory-dashboard" />
## <a name="make-a-package-submission-mandatory-in-the-dev-center-dashboard"></a>Tornar o envio de um pacote obrigatório no painel do Centro de Desenvolvimento

Ao criar um envio de pacote para um aplicativo destinado ao Windows 10, versão 1607, ou posterior, você pode marcar o pacote como obrigatório e a data/hora em que ele se torna obrigatório. Quando essa propriedade está definida e o aplicativo descobre que a atualização do pacote está disponível usando-se a API descrita anteriormente neste artigo, o aplicativo pode determinar se o pacote de atualizações é obrigatório e alterar o comportamento até a atualização ser instalada (por exemplo, o aplicativo pode desabilitar recursos).

> [!NOTE]
> O status obrigatório de uma atualização de pacote não é imposto pela Microsoft, e o sistema operacional não fornece uma interface do usuário para indicar aos usuários que uma atualização de aplicativo obrigatória deve ser instalada. Os desenvolvedores devem usar a configuração obrigatória para impor atualizações obrigatórias de aplicativos no próprio código.  

Para marcar um envio de pacote como obrigatório:

1. Faça logon no [Painel do Centro de Desenvolvimento](https://dev.windows.com/overview) e navegue até a página de visão geral do aplicativo.
2. Clique no nome do envio que contém a atualização de pacote que você deseja tornar obrigatória.
3. Navegue até a página **Pacotes** do envio. Na parte inferior dessa página, selecione **Make this update mandatory** e, em seguida, escolha o dia e hora em que a atualização de pacote e se tornará obrigatória. Essa opção se aplica a todos os pacotes UWP no envio.

Para obter mais informações sobre como configurar pacotes no painel do Centro de Desenvolvimento, consulte [Carregue os pacotes do aplicativo](../publish/upload-app-packages.md).

  > [!NOTE]
  > Se criar um [pacote de pré-lançamento](../publish/package-flights.md), você poderá marcar os pacotes como obrigatórios usando uma interface do usuário semelhante na página **Pacotes** da versão de pré-lançamento. Nesse caso, a atualização de pacote obrigatória só se aplica a clientes que façam parte do grupo da versão de pré-lançamento.
