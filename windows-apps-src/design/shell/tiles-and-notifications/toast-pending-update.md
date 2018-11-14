---
author: andrewleader
Description: Learn how to create multi-step interactions in your notifications.
title: Notificação do sistema com ativação de atualização pendente
label: Toast with pending update activation
template: detail.hbs
ms.author: mijacobs
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp, notificação do sistema, atualização pendente, atualizaçãopendente, interatividade multietapas, interações de várias etapas
ms.localizationpriority: medium
ms.openlocfilehash: 4d21c96676eec1b8b1a1f4397880af937dd8f4b6
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6446957"
---
# <a name="toast-with-pending-update-activation"></a>Notificação do sistema com ativação de atualização pendente

Você pode usar **PendingUpdate** para criar interações multietapas nas notificações do sistema. Por exemplo, conforme visto abaixo, você pode criar uma série de notificações do sistema em que as notificações subsequentes dependem de respostas das anteriores.

![Notificação do sistema com atualizações pendentes](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Requer a Fall Creators Update da Área de trabalho e a biblioteca de Notificações 2.0.0**: você deve executar o build 16299 ou posterior para ver o trabalho de atualização pendente. Você deve usar a versão 2.0.0 ou posterior da [Biblioteca NuGet de notificações do kit de ferramentas da comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para atribuir **PendingUpdate** nos botões. **PendingUpdate** é suportada somente na Área de trabalho e será ignorada em outros dispositivos.


## <a name="prerequisites"></a>Pré-requisitos

Este artigo presume um conhecimento prático de...

- [Construção de conteúdo de notificações do sistema](adaptive-interactive-toasts.md)
- [Enviar uma notificação e processar a ativação em segundo plano](send-local-toast.md)


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

- [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Enviar uma notificação do sistema local e manipular a ativação](send-local-toast.md)
- [Conteúdo e documentação sobre notificações do sistema](adaptive-interactive-toasts.md)
- [Barra de progresso da notificação do sistema](toast-progress-bar.md)