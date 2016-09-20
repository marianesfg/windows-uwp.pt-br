---
author: normesta
description: Mostra como integrar feeds sociais ao aplicativo Pessoas
MSHAttr: PreferredLib:/library/windows/apps
title: Fornecer feeds sociais ao aplicativo Pessoas
translationtype: Human Translation
ms.sourcegitcommit: 767acdc847e1897cc17918ce7f49f9807681f4a3
ms.openlocfilehash: c5b9666d8654a4065bc0e4e400d3e47de4773b8b

---

# Fornecer feeds sociais ao aplicativo Pessoas

Integre dados de feeds sociais de seu banco de dados ao aplicativo Pessoas.

Seus dados de feed aparecerão nas páginas **Novidades** do aplicativo Pessoas ou na página **Perfil** de um contato.

Os usuários podem tocar em um item de feed para abrir seu aplicativo.

![Feeds sociais no aplicativo Pessoas](images/social-feeds.png)

Para começar, crie um aplicativo em primeiro plano que marque contatos para feeds sociais e um agente de segundo plano que envie dados de feeds ao aplicativo Pessoas.

Para obter um exemplo mais completo, consulte o [Exemplo de informações sociais](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp).

## Criar um aplicativo em primeiro plano

Primeiro, crie um projeto da Plataforma Universal do Windows (UWP) e adicione o **Windows Mobile Extensions for UWP** a ele.

![Mobile Extensions](images/mobile-extensions.png)

### Localizar ou criar contatos

Você pode localizar contatos por nome, endereço de email ou número de telefone.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```
Você também pode criar contatos e depois adicioná-los a uma lista de contatos.

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);
```

### Marcar cada contato com uma anotação

Essa *anotação* faz com que o aplicativo Pessoas solicite dados de feed para o contato de seu agente de segundo plano.

Como parte da anotação, associe a ID do contato a uma ID que seu aplicativo usa internamente para identificar esse contato.

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.SocialFeeds;

await annotationList.TrySaveAnnotationAsync(annotation);

```
### Provisionar o agente de segundo plano

Certifique-se de que o contrato de API [SocialInfoContract](https://msdn.microsoft.com/library/windows/apps/dn706146.aspx) esteja disponível no dispositivo que executará seu aplicativo.

Se estiver disponível, provisione o agente de segundo plano.

```cs
if (Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
"Windows.ApplicationModel.SocialInfo.SocialInfoContract",
1))
{
    bool isProvisionSuccessful = await Windows.ApplicationModel.SocialInfo.Provider.SocialInfoProviderManager.ProvisionAsync();

    // Throw an exception if the app could not be provisioned
    if (!isProvisionSuccessful)
    {
        throw new Exception("Could not provision the app with the SocialInfoProviderManager");
    }
}
```
## Criar o agente de segundo plano

O agente de segundo plano é um componente do Tempo de Execução do Windows que responde a solicitações de feed do aplicativo Pessoas.

Em seu agente, você vai responder a essas solicitações fornecendo ao aplicativo Pessoas dados de feed de seu banco de dados.

### Criar um componente do Tempo de Execução do Windows

Adicione um projeto **Componente do Tempo de Execução do Windows (Universal do Windows)** à sua solução.

![Componente do Tempo de Execução do Windows](images/windows-runtime-component.png)

### Registre o agente de segundo plano como um serviço de aplicativo

Registre adicionando manipuladores de protocolo ao elemento ``Extensions`` do manifesto.

```xml
<Extensions>
  <uap:Extension Category="windows.appService" EntryPoint="SocialFeeds.BackgroundAgent">
    <uap:AppService Name="com.microsoft.windows.social-feeds" />
  </uap:Extension>
</Extensions>
```
Você também pode adicioná-los à guia **Declarações** do designer de manifesto no Visual Studio.

![Serviço de aplicativo no designer de manifesto](images/manifest-designer-app-service.png)

### Solicitar operações do aplicativo Pessoas

Pergunte ao aplicativo Pessoas qual tipo de dados ele quer em seguida. O aplicativo Pessoas responderá à sua solicitação com um código que indica de qual feed ele quer dados.

Esta tabela descreve cada feed:

| Feed | Descrição |
|-------|-------------|
| Página inicial | Feed que aparece na página Novidades do aplicativo Pessoas. |
| Contato | Feed que aparece na página Novidades de um contato. |
| Painel | Feed que aparece no cartão de contato ao lado da foto de perfil. |
<br>
Você questionará o aplicativo Pessoas solicitando uma *operação*. Implemente a interface [IBackgroundTask](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx) e substitua o método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx).

No método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), envie ao aplicativo Pessoas dois pares de chave-valor. Um deles contém a versão do protocolo e o outro contém o tipo da operação.

Em seguida, ouça a resposta do aplicativo Pessoas. Essa resposta conterá um código.

```cs
public sealed class BackgroundAgent : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
        BackgroundTaskDeferral backgroundTaskDeferral = taskInstance.GetDeferral();

        AppServiceTriggerDetails triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;

        AppServiceConnection appServiceConnection = triggerDetails.AppServiceConnection;

        bool continueProcessing = true;

        while (continueProcessing)
        {
            // Get next operation.
            ValueSet fields =
                new ValueSet();

            fields.Add("Version",
                (1 << 16) | 0);

            fields.Add(
                "Type", 0x10);

            AppServiceResponse response =
                await appServiceConnection.SendMessageAsync(fields);

            if (response == null || response.Status != AppServiceResponseStatus.Success)
            { /* throw exception */ }

            UInt32 type = (UInt32)response.Message["Type"];

            switch (type)
            {
                //Get Next Operation.
                case 0x10:
                    break;
                //Download Home Feed Operation.
                case 0x11:
                    break;
                //Download Contact Feed Operation.
                case 0x13:
                    break;
                //Download Dashboard Feed Operation.
                case 0x15:
                    break;
                //Operation Result.
                case 0x80:
                    break;
                //Good Bye.
                case 0xF1:
                    continueProcessing = false;
                    break;
            }
        }
        // Complete the task deferral
        backgroundTaskDeferral.Complete();

        backgroundTaskDeferral = null;
        appServiceConnection = null;
    }

}
```

Consulte o elemento ``Type`` da propriedade [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) para obter esse código. Aqui está uma lista completa dos códigos.

| Tipo| Descrição |
|-----|-------------|
| 0x10 | Uma solicitação ao aplicativo Pessoas para a próxima operação. |
| 0x11 | Uma solicitação do aplicativo Pessoas para fornecer o feed da página inicial do usuário principal. |
| 0x13 | Uma solicitação do aplicativo Pessoas para obter o feed do contato selecionado. |
| 0x15 | Uma solicitação do aplicativo Pessoas para obter o item do painel do contato selecionado. |
| 0x80 | Indica que a operação foi concluída. Isso notifica o aplicativo Pessoas que os dados já estão disponíveis. |
| 0xF1 | Uma mensagem do aplicativo Pessoas indicando que ele não requer outras operações. O agente de segundo plano pode ser encerrado agora. |
<br>
A propriedade [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) também retorna uma coleção de outros pares de chave-valor que descrevem a resposta. Aqui está uma lista desses valores.

| Chave | Tipo | Descrição |
|-----|------|-------------|
| Version | UINT32 | (Obrigatória) Identifica a versão do protocolo de mensagem. Os 16 bits superiores são a versão principal, e os 16 bits inferiores são a versão secundária. |
| Tipo | UINT32 | (Obrigatória) O tipo de operação a executar. O exemplo anterior usa a chave Type para determinar qual operação o aplicativo Pessoas está solicitando.
| OperationId | UINT32 | A ID da operação. |
| OwnerRemoteId | String | ID que seu aplicativo usa internamente para identificar esse contato. |
| LastFeedItemTimeStamp | String | A ID do último item de feed que foi recuperado. |
| LastFeedItemTimeStamp | DateTime | O carimbo de data e hora do último item de feed que foi recuperado. |
| ItemCount | UINT32 | O número de itens solicitado pelo aplicativo Pessoas. |
| IsFetchMore | BOOLEAN | Determina quando o cache interno é atualizado. |
| ErrorCode | UINT32 | O código de erro associado à operação do agente de segundo plano. |
<br>
### Fornecer um feed de dados ao aplicativo Pessoas

Um valor **Type** de ``0x11``, ``0x13`` ou ``0x15`` é uma solicitação de dados de feed do aplicativo Pessoas.  

Os próximos trechos de código mostram uma abordagem para fornecer esses dados ao aplicativo Pessoas.

> [!NOTE]
> Estes trechos de código são provenientes do [Exemplo de informações sociais](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp). Eles contêm referências a interfaces, classes e membros que estão definidos em outra parte do exemplo. Use estes trechos de código juntamente com os outros exemplos deste tópico para compreender o fluxo de tarefas e consulte o exemplo se você estiver interessado em se aprofundar mais na pilha de interfaces, classes e tipos.

**Obter itens de feed de contato**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the contact feeds
    IEnumerable<FeedItem> contactFeedItems =
        InMemorySocialCache.Instance.GetContactFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the SocialInfo APIs
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.ContactFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Translate the sample feed into Social info feed items.
    foreach (FeedItem fi in contactFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

**Obter itens do painel**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the dashboard feed item from your database.
    FeedItem dashboardFeedItem = InMemorySocialCache.Instance.GetDashboardFeed(OwnerRemoteId);

    if (dashboardFeedItem != null)
    {
        // Check if the platform supports the social info APIs.
        if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
             "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
        {

            return;
        }

        SocialDashboardItemUpdater dashboard =
            await SocialInfoProviderManager.CreateDashboardItemUpdaterAsync(OwnerRemoteId);

        dashboard.Content.Message = dashboardFeedItem.Message;
        dashboard.Content.Title = dashboardFeedItem.Title;
        dashboard.Timestamp = dashboardFeedItem.Timestamp;

        // The TargetUri of the dashboard always has to be set.
        dashboard.TargetUri = dashboardFeedItem.TargetUri;

        // For a thumbnail, there must always be a target Uri.
        if ((dashboardFeedItem.ImageUri != null) && (dashboardFeedItem.TargetUri != null))
        {
            dashboard.Thumbnail = new SocialItemThumbnail()
            {
                ImageUri = dashboardFeedItem.ImageUri,
                TargetUri = dashboardFeedItem.TargetUri
            };
        }

        await dashboard.CommitAsync();
    }
}
```

**Obter itens de feed da página inicial**

```cs
public override async Task DownloadFeedAsync()
{
    // Query the "database" for the home feeds.
    IEnumerable<FeedItem> homeFeedItems =
        InMemorySocialCache.Instance.GetHomeFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the social info APIs.
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.HomeFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Generate each of the feed items.
    foreach (FeedItem fi in homeFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

### Enviar a notificação de êxito ou falha ao aplicativo Pessoas

Encapsule suas chamadas em um bloco try catch e passe uma mensagem de êxito ou falha para o aplicativo Pessoas depois de fornecer dados do feed.

```cs
try
{
    // Calls to methods that fetch data and populate feed.
}
catch (Exception exception)
{
    errorCode = (UInt32)exception.HResult;
}

// Send status back to the people app.
ValueSet fields =
    new ValueSet();

fields.Add("ErrorCode", errorCode);

UInt32 operationID = (UInt32)response.Message["OperationId"];

fields.Add("OperationId", operationID);

await this.mAppServiceConnection.SendMessageAsync(fields);

```



<!--HONumber=Aug16_HO4-->


