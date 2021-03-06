---
Description: Saiba como criar interações de várias etapas em suas notificações.
title: Notificação do sistema com ativação de atualização pendente
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp, notificação do sistema, atualização pendente, atualizaçãopendente, interatividade multietapas, interações de várias etapas
ms.localizationpriority: medium
ms.openlocfilehash: b1574ee2913bd2889af204aae1089dc170df95b8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648551"
---
# <a name="toast-with-pending-update-activation"></a>Notificação do sistema com ativação de atualização pendente

Você pode usar **PendingUpdate** para criar interações multietapas nas notificações do sistema. Por exemplo, conforme visto abaixo, você pode criar uma série de notificações do sistema em que as notificações subsequentes dependem de respostas das anteriores.

![Notificação do sistema com atualizações pendentes](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Requer a área de trabalho Fall Creators Update e 2.0.0 da biblioteca de notificações**: Você deve estar executando Desktop build 16299 ou superior para ver o trabalho de atualização pendente. Você deve usar a versão 2.0.0 ou posterior da [Biblioteca NuGet de notificações do kit de ferramentas da comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para atribuir **PendingUpdate** nos botões. **PendingUpdate** é suportada somente na Área de trabalho e será ignorada em outros dispositivos.


## <a name="prerequisites"></a>Pré-requisitos

Este artigo presume um conhecimento prático de...

- [Construindo o conteúdo de notificação do sistema](adaptive-interactive-toasts.md)
- [Enviar uma notificação do sistema e manipulação de ativação do plano de fundo](send-local-toast.md)


## <a name="overview"></a>Visão geral

Para implementar uma notificação do sistema que usa uma atualização pendente como comportamento após a ativação...

1. Nos botões de ativação em segundo plano, especifique um **AfterActivationBehavior** de **PendingUpdate**

2. Atribuir uma **Marca** (e, como alternativa, **Grupo**) ao enviar a notificação do sistema

3. Quando o usuário clica no botão, sua tarefa em segundo plano será ativada e a notificação do sistema será mantida na tela em um estado de atualização pendente

4. Na tarefa em segundo plano, envie uma nova notificação do sistema com o novo conteúdo, usando a mesma **Marca** e **Grupo**


## <a name="assign-pendingupdate"></a>Atribuir PendingUpdate

Nos botões de ativação em segundo plano, defina um **AfterActivationBehavior** para **PendingUpdate** Observe que isso funciona somente para botões que com um **ActivationType** de **Background**.

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>Usar uma Marca na notificação

Para substituir a notificação posteriormente, é preciso atribuir a **Marca** (e, opcionalmente, o **Grupo**) na notificação.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>Substituir a notificação do sistema pelo novo conteúdo

Em resposta ao clique do usuário no botão, a tarefa em segundo plano é disparada e você deve substituir a notificação do sistema pelo novo conteúdo. Você substitui a notificação do sistema ao enviar uma nova notificação do sistema com a mesma **Marca** e **Grupo**.

É altamente recomendável **ajustar o áudio para o modo silencioso** nas substituições em resposta a um clique de botão, pois o usuário já está interagindo com a notificação do sistema.

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>Tópicos relacionados

- [Exemplo de código completo no GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Enviar uma ativação de notificação do sistema e o identificador de local](send-local-toast.md)
- [Documentação de conteúdo de notificação do sistema](adaptive-interactive-toasts.md)
- [Barra de progresso de notificação do sistema](toast-progress-bar.md)