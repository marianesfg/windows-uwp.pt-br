---
Description: Saiba como agendar uma notificação do sistema local para ser exibida posteriormente.
title: Agendar uma notificação do sistema
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: Windows 10, UWP, notificação do sistema agendado, scheduledtoastnotification, instruções, início rápido, introdução, exemplo de código, passo a passos
ms.localizationpriority: medium
ms.openlocfilehash: 07339cf793bdada51f79d70d9e9e6b6d4a41851b
ms.sourcegitcommit: 017f2f1492f3220da0fae8b4c99de7206a185dff
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81386425"
---
# <a name="schedule-a-toast-notification"></a>Agendar uma notificação do sistema

As notificações agendadas do sistema permitem agendar uma notificação para aparecer mais tarde, independentemente de seu aplicativo estar em execução naquele momento. Isso é útil para cenários como exibir lembretes ou outras tarefas de acompanhamento para o usuário, em que o tempo e o conteúdo da notificação são conhecidos com antecedência.

Observe que as notificações agendadas do sistema têm uma janela de entrega de 5 minutos. Se o computador for desligado durante o horário de entrega agendado e permanecer desativado por mais de 5 minutos, a notificação será "ignorada", pois não é mais relevante para o usuário. Se você precisar de entrega garantida de notificações, independentemente de quanto tempo o computador estava desligado, é recomendável usar uma tarefa em segundo plano com um gatilho de tempo, conforme ilustrado neste [exemplo de código](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off).

> [!IMPORTANT]
> Os aplicativos de área de trabalho (pacotes MSIX/esparsos e Win32 clássico) têm etapas um pouco diferentes para enviar notificações e manipular a ativação. Acompanhe as instruções abaixo, no entanto, substitua `ToastNotificationManager` pela classe `DesktopNotificationManagerCompat` da documentação dos [aplicativos da área de trabalho](toast-desktop-apps.md) .

> **APIs importantes**: [classe ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

Para entender completamente este tópico, os itens a seguir serão úteis...

* Um conhecimento prático dos termos e conceitos de notificações do sistema. Para obter mais informações, consulte [visão geral do sistema de notificação e de ação](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Uma familiaridade com o conteúdo da notificação do sistema Windows 10. Para obter mais informações, consulte [documentação de conteúdo da notificação do sistema](adaptive-interactive-toasts.md).
* Um projeto de aplicativo UWP do Windows 10


## <a name="install-nuget-packages"></a>Instalar os pacotes NuGet

Recomendamos a instalação destes dois pacotes NuGet para o projeto. Nosso exemplo de código usará esses pacotes.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): gerar cargas de notificações do sistema por meio de objetos em vez de XML bruto.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): gerar e analisar cadeias de caracteres de consulta com C#


## <a name="add-namespace-declarations"></a>Adicionar declarações de namespace

`Windows.UI.Notifications` inclui as APIs do sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>Construir o conteúdo do sistema

No Windows 10, o conteúdo da notificação do sistema é descrito usando uma linguagem adaptável que possibilita uma grande flexibilidade com a aparência de sua notificação. Consulte a [documentação de conteúdo da notificação do sistema](adaptive-interactive-toasts.md) para obter mais informações.

Graças à biblioteca de notificações, a geração do conteúdo XML é simples. Se você não instalar a biblioteca de Notificações do NuGet, é necessário construir o XML manualmente, o que deixa espaço para erros.

Você sempre deve definir a propriedade **Launch** para que, quando o usuário tocar o corpo da notificação do sistema e o aplicativo for iniciado, ele saiba qual conteúdo deverá ser exibido.

```csharp
// In a real app, these would be initialized with actual data
string title = "ASTR 170B1";
string content = "You have 3 items due today!";

// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
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
                }
            }
        }
    },
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewClass" },
        { "classId", "3910938180" }
 
    }.ToString()
};
```

## <a name="create-the-scheduled-toast"></a>Criar o sistema de notificação agendado

Depois de inicializar o conteúdo do sistema, crie um novo [ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) e passe o XML do conteúdo e a hora em que você deseja que a notificação seja entregue.

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>Fornecer uma chave primária para a notificação do sistema

Se desejar cancelar, remover ou substituir programaticamente a notificação agendada, você precisará usar a propriedade Tag (e, opcionalmente, a Propriedade Group) para fornecer uma chave primária para sua notificação. Em seguida, você pode usar essa chave primária no futuro para cancelar, remover ou substituir a notificação.

Para ver mais detalhes sobre como substituir/remover notificações do sistema disponibilizadas, consulte [Guia de início rápido: gerenciamento de notificações do sistema na central de ações (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

Tag e Group combinadas atuam como uma chave primária composta. Group é o identificador mais genérico, no qual você pode atribuir a grupos como "wallPosts", "messages", "friendRequests" etc. Tag deve identificar exclusivamente a notificação dentro do grupo. Ao usar um grupo genérico, você pode remover todas as notificações dele usando a [API RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>Agendar a notificação

Por fim, crie um [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) e chame AddToSchedule (), passando a notificação do sistema agendado.

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>Cancelar notificações agendadas

Para cancelar uma notificação agendada, primeiro você precisa recuperar a lista de todas as notificações agendadas.

Em seguida, localize seu sistema de notificação de estado correspondente à marca (e opcionalmente, grupo) especificado anteriormente e chame RemoveFromSchedule ().

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>Tratamento de ativação

Consulte [enviar um docs local do sistema de notificação](send-local-toast.md) para saber mais sobre como lidar com a ativação. A ativação de uma notificação do sistema agendada é tratada da mesma forma que a ativação de uma notificação do sistema local.


## <a name="adding-actions-inputs-and-more"></a>Adicionando ações, entradas e muito mais

Consulte [enviar um docs local do sistema de notificação](send-local-toast.md) para saber mais sobre tópicos avançados, como ações e entradas. Ações e entradas funcionam da mesma forma em notificações locais, como nas notificações agendadas.


## <a name="resources"></a>Recursos

* [Documentação do conteúdo do sistema](adaptive-interactive-toasts.md)
* [Classe ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
