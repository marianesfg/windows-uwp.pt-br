---
author: andrewleader
Description: Learn how to use Notification Listener to access all of the user's notifications.
title: Ouvinte de notificação
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ouvinte de notificação, usernotificationlistener, documentação, notificações de acesso
ms.localizationpriority: medium
ms.openlocfilehash: f4d8cb9ef7589bd8f0c56586ab8fcfec7c1f01e3
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4749779"
---
# <a name="notification-listener-access-all-notifications"></a>Ouvinte de notificação: acessar todas as notificações

O ouvinte de notificação fornece acessar às notificações de um usuário. Os smartwatches e outros dispositivos acessórios podem usar o ouvinte de notificação para enviar as notificações do telefone ao dispositivo acessório. Os apps de automação doméstica poderão usar o ouvinte de notificação para executar ações específicas quando as notificações forem recebidas, como fazer as luzes piscarem quando você receber uma chamada. 

> [!IMPORTANT]
> **Requer Atualização de Aniversário**: você precisa usar o SDK 14393 e estar executando o build 14393 ou posterior para usar o ouvinte de notificação.


> **APIs importantes**: [UserNotificationListener class](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [UserNotificationChangedTrigger class](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>Habilitar o ouvinte adicionando a funcionalidade de notificação do usuário 

Para usar o ouvinte de notificação, você deve adicionar a funcionalidade de ouvinte de notificação ao manifesto do app.

1. No Visual Studio, no Gerenciador de Soluções, clique duas vezes no arquivo `Package.appxmanifest` para abrir o designer de manifesto.
2. Abra a guia Recursos.
3. Marque o recurso **Ouvinte de Notificação de Usuário**.


## <a name="check-whether-the-listener-is-supported"></a>Verificar se o ouvinte é compatível

Se o app oferecer suporte às versões mais antigas do Windows 10, será necessário usar a [classe ApiInformation](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) para verificar se há suporte para o listener.  Se não houver suporte para o listener, evite a execução de chamadas às APIs do ouvinte.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Notifications.Management.UserNotificationListener"))
{
    // Listener supported!
}
 
else
{
    // Older version of Windows, no Listener
}
```


## <a name="requesting-access-to-the-listener"></a>Solicitando o acesso ao ouvinte

Assim que o ouvinte permitir o acesso às notificações do usuário, os usuários deverão dar ao app permissão para acessar suas notificações. Durante a experiência de primeira execução do app, você deverá solicitar acesso para usar o ouvinte de notificação. Se você quiser, poderá mostrar uma interface do usuário preliminar que explica por que o app precisa de acesso às notificações do usuário antes de chamar [RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync), para que o usuário entenda por que deve permitir o acesso.

```csharp
// Get the listener
UserNotificationListener listener = UserNotificationListener.Current;
 
// And request access to the user's notifications (must be called from UI thread)
UserNotificationListenerAccessStatus accessStatus = await listener.RequestAccessAsync();
 
switch (accessStatus)
{
    // This means the user has granted access.
    case UserNotificationListenerAccessStatus.Allowed:
 
        // Yay! Proceed as normal
        break;
 
    // This means the user has denied access.
    // Any further calls to RequestAccessAsync will instantly
    // return Denied. The user must go to the Windows settings
    // and manually allow access.
    case UserNotificationListenerAccessStatus.Denied:
 
        // Show UI explaining that listener features will not
        // work until user allows access.
        break;
 
    // This means the user closed the prompt without
    // selecting either allow or deny. Further calls to
    // RequestAccessAsync will show the dialog again.
    case UserNotificationListenerAccessStatus.Unspecified:
 
        // Show UI that allows the user to bring up the prompt again
        break;
}
```

O usuário pode revogar o acesso a qualquer momento por meio das Configurações do Windows. Portanto, o app sempre deve verificar o status do acesso por meio do método [GetAccessStatus](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) antes de executar o código que usa o ouvinte de notificação. Se o usuário revogar o acesso, as APIs apresentarão uma falha silenciosa, em vez de gerar uma exceção (por exemplo, a API que retornará todas as notificações simplesmente retornará uma lista vazia).


## <a name="access-the-users-notifications"></a>Acessar as notificações do usuário

Com o ouvinte de notificação, você pode obter uma lista das notificações atuais do usuário. Basta chamar o método [GetNotificationsAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) e especificar o tipo de notificação que você deseja receber (atualmente, o único tipo de notificação com suporte são as notificações do sistema).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>Exibindo as notificações

Cada notificação é representada como uma [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification), que fornece informações sobre o app que gerou a notificação, a hora em que a notificação foi criada, a ID da notificação e a notificação propriamente.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

A propriedade [AppInfo](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.AppInfo) fornece as informações necessárias para exibir a notificação.

> [!NOTE]
> É recomendável envolver todo o código para processar uma única notificação em um try/catch, caso ocorra uma exceção inesperada quando você estiver capturando uma única notificação. Você não deve deixar de exibir outras notificações apenas devido a um problema com uma notificação.

```csharp
// Select the first notification
UserNotification notif = notifs[0];
 
// Get the app's display name
string appDisplayName = notif.AppInfo.DisplayInfo.DisplayName;
 
// Get the app's logo
BitmapImage appLogo = new BitmapImage();
RandomAccessStreamReference appLogoStream = notif.AppInfo.DisplayInfo.GetLogo(new Size(16, 16));
await appLogo.SetSourceAsync(await appLogoStream.OpenReadAsync());
```

O conteúdo da notificação propriamente, como o texto da notificação, está contido na propriedade [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.Notification). Essa propriedade contém a parte visual da notificação. (Se você estiver familiarizado com o envio de notificações no Windows, observará que as propriedades [Visual](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification.Visual) e [Visual.Bindings](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationvisual.Bindings) no objeto [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification) correspondem ao que os desenvolvedores enviam quando exibem uma notificação).

Queremos procurar a associação da notificação do sistema (para código de prova de erro, você deve verificar se a associação não é nula). Na associação, você pode obter os elementos de texto. Você pode escolher quantos elementos de texto deseja exibir. (O ideal é exibir todos eles.) Você pode optar por tratar os elementos de texto de forma diferente; por exemplo, trate o primeiro como texto de título e os elementos subsequentes como corpo do texto.

```csharp
// Get the toast binding, if present
NotificationBinding toastBinding = notif.Notification.Visual.GetBinding(KnownNotificationBindings.ToastGeneric);
 
if (toastBinding != null)
{
    // And then get the text elements from the toast binding
    IReadOnlyList<AdaptiveNotificationText> textElements = toastBinding.GetTextElements();
 
    // Treat the first text element as the title text
    string titleText = textElements.FirstOrDefault()?.Text;
 
    // We'll treat all subsequent text elements as body text,
    // joining them together via newlines.
    string bodyText = string.Join("\n", textElements.Skip(1).Select(t => t.Text));
}
```


## <a name="remove-a-specific-notification"></a>Remover uma notificação específica

Se o dispositivo acessório ou o serviço permitir que o usuário ignore as notificações, você poderá remover a notificação real para que o usuário não a veja mais tarde no telefone ou no computador. Basta fornecer a ID (obtida no objeto [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification)) da notificação que você deseja remover: 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>Limpar todas as notificações

O método [UserNotificationListener.ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) limpa todas as notificações do usuário. Use esse método com cautela. Você só deve limpar todas as notificações se o dispositivo acessório ou o serviço exibir TODAS as notificações. Se o dispositivo acessório ou o serviço só exibir determinadas notificações, quando o usuário clicar no botão "Clear notifications", o usuário esperará que somente essas notificações sejam removidas; no entanto, se o método [ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) for chamado, todas as notificações serão removidas, incluindo as que o dispositivo acessório ou o serviço não estava exibindo.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>Tarefa em segundo plano para notificação adicionada/ignorada

Uma maneira comum de permitir que um aplicativo detecte as notificações é configurando uma tarefa em segundo plano, para que você possa saber quando uma notificação foi adicionada ou ignorada, independentemente do app que está sendo executado.

Graças ao [modelo de processo único](../../../launch-resume/create-and-register-an-inproc-background-task.md) adicionado à Atualização de Aniversário, adicionar tarefas em segundo plano é muito simples. No código do app principal, depois que você tiver recebido o acesso do usuário ao ouvinte de notificação e acesso para executar tarefas em segundo plano, registre uma nova tarefa em segundo plano e defina o [UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) usando o [Tipo de notificação do sistema](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationkinds).

```csharp
// TODO: Request/check Listener access via UserNotificationListener.Current.RequestAccessAsync
 
// TODO: Request/check background task access via BackgroundExecutionManager.RequestAccessAsync
 
// If background task isn't registered yet
if (!BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals("UserNotificationChanged")))
{
    // Specify the background task
    var builder = new BackgroundTaskBuilder()
    {
        Name = "UserNotificationChanged"
    };
 
    // Set the trigger for Listener, listening to Toast Notifications
    builder.SetTrigger(new UserNotificationChangedTrigger(NotificationKinds.Toast));
 
    // Register the task
    builder.Register();
}
```

Em seguida, no App.xaml.cs, substitua o método [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.OnBackgroundActivated), caso ainda não tenha feito isso, e use uma instrução switch no nome da tarefa para determinar quais dos seus gatilhos de tarefa em segundo plano foram invocados.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

A tarefa em segundo plano é apenas uma chamada de atenção: ele não fornece nenhuma informação sobre quais notificações foram adicionadas ou removidas. Quando sua tarefa em segundo plano for acionada, você deverá sincronizar as notificações do dispositivo acessório para que elas reflitam as notificações da plataforma. Isso garantirá que, se a tarefa em segundo plano apresentar falha, as notificações no dispositivo acessório ainda poderão ser recuperadas na próxima vez que a tarefa em segundo plano for executada.

`SyncNotifications` é um método que você implementa; a próxima seção mostra como fazer. 


## <a name="determining-which-notifications-were-added-and-removed"></a>Determinando quais notificações foram adicionadas e removidas

No método `SyncNotifications`, para determinar quais notificações foram adicionadas ou removidas (sincronizando notificações com o dispositivo acessório), você precisa calcular o delta entre sua coleção de notificações atual e as notificações da plataforma.

```csharp
// Get all the current notifications from the platform
IReadOnlyList<UserNotification> userNotifications = await listener.GetNotificationsAsync(NotificationKinds.Toast);
 
// Obtain the notifications that our wearable currently has displayed
IList<uint> wearableNotificationIds = GetNotificationsOnWearable();
 
// Copy the currently displayed into a list of notification ID's to be removed
var toBeRemoved = new List<uint>(wearableNotificationIds);
 
// For each notification in the platform
foreach (UserNotification userNotification in userNotifications)
{
    // If we've already displayed this notification
    if (wearableNotificationIds.Contains(userNotification.Id))
    {
        // We want to KEEP it displayed, so take it out of the list
        // of notifications to remove.
        toBeRemoved.Remove(userNotification.Id);
    }
 
    // Othwerise it's a new notification
    else
    {
        // Display it on the Wearable
        SendNotificationToWearable(userNotification);
    }
}
 
// Now our toBeRemoved list only contains notification ID's that no longer exist in the platform.
// So we will remove all those notifications from the wearable.
foreach (uint id in toBeRemoved)
{
    RemoveNotificationFromWearable(id);
}
```


## <a name="foreground-event-for-notification-addeddismissed"></a>Evento em primeiro plano para notificação adicionada/ignorada

> [!IMPORTANT] 
> Problema conhecido: O evento em primeiro plano fará um loop de CPU em versões recentes do Windows e anteriormente não funcionou antes disso. Não use o evento em primeiro plano. Em uma atualização futura para Windows, podemos irá corrigir isso.

Em vez de usar o evento em primeiro plano, use o código mostrado anteriormente para uma tarefa de plano de fundo do [modelo de processo único](../../../launch-resume/create-and-register-an-inproc-background-task.md) . A tarefa em segundo plano também permitirá que você receba eventos notificações de alteração de ambos os enquanto o aplicativo está em execução ou fechado.

```csharp
// Subscribe to foreground event (DON'T USE THIS)
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // NOTE: This event WILL CAUSE CPU LOOPS, DO NOT USE. Use the background task instead.
}
```


## <a name="how-to-fix-delays-in-the-background-task"></a>Como corrigir atrasos na tarefa em segundo plano

Ao testar o app, você pode perceber que a tarefa em segundo plano atrasa algumas vezes e não dispara por vários minutos. Para corrigir isso, você solicitará que o usuário vá para Configurações do sistema -> Sistema -> Bateria -> Uso de bateria por aplicativo, localize o app na lista, selecione-o e altere-o para "Sempre permitido em segundo plano". Depois disso, a tarefa em segundo plano sempre será disparada em até, aproximadamente, um segundo após a notificação ter sido recebida.