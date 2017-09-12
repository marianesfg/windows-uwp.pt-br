---
author: mijacobs
Description: "Saiba como enviar uma notificação do sistema local e manipular o usuário ao clicar na notificação do sistema."
title: "Enviar uma notificação do sistema local"
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7f21c6a72c00ae2677adb0c1196030997ed48491
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="send-a-local-toast-notification"></a>Enviar uma notificação do sistema local

Uma notificação do sistema é uma mensagem que um aplicativo pode construir e fornecer ao usuário enquanto ele/ela não está atualmente no aplicativo. Este Guia de início rápido conduz você pelas etapas para criar, disponibilizar e exibir uma notificação do sistema Windows 10 com os novos modelos adaptáveis e ações interativas. Estas ações são demonstradas por meio de uma notificação local, que é a notificação mais simples para implementação. Explicaremos os seguintes procedimentos:

### <a name="sending-a-toast"></a>Enviar uma notificação do sistema

* Construir a parte visual (texto e imagem) da notificação
* Adicionar ações à notificação
* Definir um tempo de expiração na notificação do sistema
* Definir a marcação/grupo para que você possa substituir ou remover a notificação do sistema posteriormente
* Enviar sua notificação do sistema usando as APIs locais

### <a name="handling-activation"></a>Manipular a ativação

* Manipulação de ativação quando o corpo ou os botões são clicados
* Manipulação de ativação em primeiro plano
* Manipulação de ativação em segundo plano


## <a name="prerequisites"></a>Pré-requisitos

Para entender completamente este tópico, os itens a seguir serão úteis...

* Um conhecimento prático dos termos e conceitos de notificações do sistema. Para obter mais informações, consulte [Visão geral de notificações do sistema e da central de ações](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Uma familiaridade com o conteúdo da notificação do sistema Windows 10. Para obter mais informações, consulte [documentação de conteúdo da notificação do sistema](tiles-and-notifications-adaptive-interactive-toasts.md).
* Um projeto de aplicativo UWP do Windows 10

**Observação**: diferente do Windows 8/8.1, você não precisa mais declarar no manifesto do aplicativo que ele é capaz de mostrar notificações do sistema. Todos os aplicativos são capazes de enviar e exibir notificações do sistema.

**Aplicativos do Windows 8/8.1**:, em vez disso, use [documentação arquivada](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh868254.aspx).


## <a name="install-nuget-packages"></a>Instalar pacotes NuGet

Recomendamos a instalação destes dois pacotes NuGet para o projeto. Nosso exemplo de código usará esses pacotes. No final do artigo forneceremos os trechos de código "Baunilha" que não usam pacotes NuGet.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): gerar cargas de notificações do sistema por meio de objetos em vez de XML bruto.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): gerar e analisar cadeias de caracteres de consulta com C#


## <a name="add-namespace-declarations"></a>Adicionar declarações de namespace

Windows.UI.Notifications inclui as APIs de gravação.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>Enviar uma notificação do sistema

No Windows 10, o conteúdo da notificação do sistema é descrito usando uma linguagem adaptável que possibilita uma grande flexibilidade com a aparência de sua notificação. Consulte a [documentação de conteúdo da notificação do sistema](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/02/adaptive-and-interactive-toast-notifications-for-windows-10.aspx) para obter mais informações.

### <a name="constructing-the-visual-part-of-the-content"></a>Construir a parte visual do conteúdo

Vamos começar construindo a parte visual do conteúdo, que inclui o conteúdo de texto e imagem que você deseja exibir para o usuário.

A geração de o conteúdo XML é simplificada devido à biblioteca de Notificações. Se você não instalar a biblioteca de Notificações do NuGet, é necessário construir o XML manualmente, o que deixa espaço para erros.

Observação: as imagens podem ser usadas do pacote do aplicativo, do armazenamento local do aplicativo ou da Web. As imagens da Web devem ter menos de 200 KB.

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://unsplash.it/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
{
    BindingGeneric = new ToastBindingGeneric()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = title
            },
 
            new AdaptiveText()
            {
                Text = content
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>Construção de parte de ações do conteúdo

Agora, vamos adicionar ações ao conteúdo.

Na exemplo abaixo, incluímos um elemento de entrada que permite ao usuário inserir de texto, que, em seguida, pode ser recuperado pelo aplicativo usando sua id, depois que ele for ativado em primeiro ou segundo plano, dependendo de como as interações estão definidas para cada uma das ações.

Em seguida, criamos dois botões, cada um especificando seu próprio tipo de ativação, conteúdo e argumentos.
* ActivationType é usada para especificar como deseja que o aplicativo seja ativado quando a ação é executada pelo usuário. Você pode optar por iniciar o aplicativo em primeiro plano, iniciar uma tarefa em segundo plano ou iniciar o protocolo de outro aplicativo. Se o aplicativo seleciona o tipo de ativação como em primeiro ou segundo plano, sempre há uma maneira de recuperar a entrada do usuário, bem como os argumentos previamente definidos no atributo arguments, para que você tenha o contexto completo do que o usuário fez à notificação.

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>Combinação das informações acima para construir o conteúdo completo

A construção do conteúdo agora está completa e podemos usá-lo para instanciar o objeto ToastNotification.

**Observação**: você também pode fornecer um tipo de ativação no elemento raiz para especificar qual tipo de ativação deve acontecer quando o usuário toca no corpo da notificação do sistema. Normalmente, ao tocar no corpo da notificação do sistema, o aplicativo é iniciado em primeiro plano para criar uma experiência de usuário consistente, mas você pode usar outros tipos de ativação para se ajustar ao cenário específico em que faz mais sentido para o usuário.

Assim como antes, você pode e deve sempre adicionar um parâmetro de inicialização ao elemento raiz, então quando o usuário toca o corpo da notificação do sistema, o aplicativo ainda pode ser iniciado com uma exibição relacionada ao conteúdo da notificação.

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>Definir um tempo de expiração

No Windows 10, todas as notificações do sistema vão para a Central de Ações (anteriormente disponível somente no telefone, mas agora está disponível em todos os dispositivos Windows) depois que são descartadas ou ignorados pelo usuário para que possam visualizar a notificação depois que o pop-up some.

No entanto, se a mensagem na notificação for relevante somente para um período de tempo, você deve definir um tempo de expiração na notificação do sistema para que os usuários não vejam informações obsoletas do aplicativo. No código a seguir, definimos o tempo de expiração como dois dias.

**Observação**: o tempo de expiração máximo e padrão para notificações do sistema local é de três dias.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Fornecer uma chave primária para a notificação do sistema

Se você quiser remover ou substituir a notificação enviada por meio de programação, é necessário usar a propriedade Tag (e, opcionalmente, a propriedade Group) a fim de fornecer uma chave primária para a notificação. Em seguida, você pode usar essa chave primária no futuro para remover ou substituir a notificação.

Para ver mais detalhes sobre como substituir/remover notificações do sistema disponibilizadas, consulte [Guia de início rápido: gerenciamento de notificações do sistema na central de ações (XAML)](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dn631260.aspx).

Tag e Group combinadas atuam como uma chave primária composta. Group é o identificador mais genérico, no qual você pode atribuir a grupos como "wallPosts", "messages", "friendRequests" etc. Tag deve identificar exclusivamente a notificação dentro do grupo. Ao usar um grupo genérico, você pode remover todas as notificações dele usando a [API RemoveGroup](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>Enviar a notificação

Depois de construir sua notificação do sistema, crie um ToastNotifier e chame Show(), passando a notificação do sistema.

```
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>Limpar as notificações

Os aplicativos UWP serão responsáveis pela remoção e limpeza de suas próprias notificações. Quando o aplicativo é iniciado, as notificações NÃO são apagadas automaticamente.

O Windows removerá uma notificação automaticamente apenas se o usuário clicar explicitamente nela.

Este é um exemplo do que um aplicativo de mensagens deve fazer...

1. O usuário recebe várias notificações do sistema sobre novas mensagens em uma conversa
2. O usuário toca em um dessas notificações do sistema para abrir a conversa
3. O aplicativo abre a conversa e, em seguida, apaga todas as notificações do sistema relativas a ela (usando [RemoveGroup](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) no grupo fornecido pelo aplicativo para a conversa)
4. Agora, a Central de Ações do usuário reflete adequadamente o estado da notificação, pois não há notificações obsoletas dessa conversa na Central de Ações.

Para saber sobre como limpar todas as notificações ou remover notificações específicas, consulte [Guia de início rápido: gerenciamento de notificações do sistema na central de ações (XAML)](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dn631260.aspx).


## <a name="handling-activation"></a>Manipular a ativação

No Windows 10, quando o usuário clica na notificação do sistema, você pode fazer a notificação ativar o aplicativo de duas maneiras diferentes...

* Ativação em primeiro plano
* Ativação em segundo plano

**Observação**: se você estiver usando os modelos de notificação do sistema herdados do Windows 8.1, OnLaunched será chamado em vez disso. A documentação a seguir se aplica somente às notificações modernas do Windows 10 usando a biblioteca de Notificações (ou o modelo ToastGeneric se estiver usando XML bruto).


### <a name="handling-foreground-activation"></a>Manipulação de ativação em primeiro plano

No Windows 10, quando um usuário clica em uma notificação do sistema moderna (ou um botão na notificação), OnActivated é invocado em vez de OnLaunched, com um novo tipo de ativação – ToastNotification. Assim, o desenvolvedor é capaz de distinguir facilmente uma ativação de notificação do sistema e realizar tarefas adequadas.

No exemplo abaixo, você pode recuperar a cadeia de caracteres de argumentos fornecida inicialmente no conteúdo da notificação do sistema. Você também pode recuperar a entrada do usuário fornecida nas caixas de texto e de seleção.

**Observação**: você deve inicializar seu quadro e ativar a janela como no código OnLaunched. **OnLaunched não será chamada se o usuário clicar na notificação do sistema**, mesmo se o aplicativo estava fechado e está sendo iniciado pela primeira vez. Em geral, é recomendável combinar OnLaunched e OnActivated em seu próprio método OnLaunchedOrActivated, pois a mesma inicialização precisa ocorrer em ambos.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="handling-background-activation"></a>Manipulação de ativação em segundo plano

Quando você especifica a ativação em segundo plano na notificação do sistema (ou em um botão de notificação do sistema), a tarefa em segundo plano será executada em vez de ativar o aplicativo em primeiro plano.

Para saber mais sobre tarefas em segundo plano, consulte [Suporte ao aplicativo com tarefas em segundo plano](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Se você estiver usando a compilação 14393 ou posterior, você pode usar tarefas em segundo plano em processamento, o que simplifica as ações. Observe que tarefas em segundo plano em processamento não são executadas em computadores mais antigos. Vamos usar uma tarefa em segundo plano em processamento nesse exemplo de código.

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = "MyToastNotificationActionTrigger",
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


Em seguida, em App.xaml.cs, substitua o método OnBackgroundActivated para recuperar os argumentos predefinidos e a entrada do usuário, semelhante à ativação em primeiro plano.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```



## <a name="plain-vanilla-code-snippets"></a>Trechos de código "Simples"

Se você não estiver usando a biblioteca de Notificações do NuGet, você pode construir manualmente o XML, como demonstrado abaixo para criar um ToastNotification.

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>Recursos

* [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast)
* [Conteúdo e documentação sobre notificações do sistema](tiles-and-notifications-adaptive-interactive-toasts.md)