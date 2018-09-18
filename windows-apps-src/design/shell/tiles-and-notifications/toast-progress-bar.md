---
author: andrewleader
Description: Learn how to use a progress bar within your toast notification.
title: Barra de progresso de notificação do sistema e associação de dados
label: Toast progress bar and data binding
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, notificação do sistema, barra de progresso, barra de progresso de notificação do sistema, notificação, associação de dados do sistema
ms.localizationpriority: medium
ms.openlocfilehash: b99c2479bef3c10ecc82707e475f49fd2b9014ec
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4021348"
---
# <a name="toast-progress-bar-and-data-binding"></a>Barra de progresso de notificação do sistema e associação de dados

Uma barra de progresso na notificação do sistema permite que você transmita o status das operações de longa duração para o usuário, como downloads, renderização de vídeo, metas de exercício e muito mais.

> [!IMPORTANT]
> **Requer a Atualização para Criadores e a biblioteca de Notificações 1.4.0**: você deve ter como meta o SDK 15063 e executar o build 15063 ou superior para usar barras de progresso em notificações do sistema. Você deve usar a versão 1.4.0 ou posterior da [biblioteca NuGet de notificações do kit de ferramentas da comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para inserir a barra de progresso no conteúdo da notificação do sistema.

Uma barra de progresso dentro de uma notificação do sistema pode ser "indeterminada" (sem valor específico, os pontos animados indicam que uma operação está ocorrendo) ou "determinada" (uma porcentagem específica da barra é preenchida, como 60%).

> **APIs importantes**: [classe NotificationData](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationdata), [método ToastNotifier.Update](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [classe ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> Somente a Área de trabalho oferece suporte às barras de progresso em notificações do sistema. Em outros dispositivos, a barra de progresso será descartada da notificação.

A imagem abaixo mostra uma barra de progresso determinada com todas as propriedades correspondente identificadas.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| Propriedade | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| **Título** | sequência ou [BindableString](toast-schema.md#bindablestring) | false | Obtém ou define uma sequência de título opcional. Suporte à associação de dados. |
| **Valor** | dobro ou [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) ou [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | Obtém ou define o valor da barra de progresso. Suporte à associação de dados. Assume 0 como valor padrão. Pode ser um duplo entre 0,0 e 1,0; `AdaptiveProgressBarValue.Indeterminate` ou `new BindableProgressBarValue("myProgressValue")`. |
| **ValueStringOverride** | sequência ou [BindableString](toast-schema.md#bindablestring) | false | Obtém ou define uma sequência para exibição em vez da sequência de percentual padrão. Caso não seja fornecida, algo como "70%" será exibido. |
| **Status** | sequência ou [BindableString](toast-schema.md#bindablestring) | true | Obtém ou define uma sequência de status (obrigatória), que é exibida abaixo da barra de progresso à esquerda. Essa sequência deve refletir o status da operação, como "Baixando..." ou "Instalando..." |


Veja como você pode gerar a notificação acima...

```csharp
ToastContent content = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Downloading your weekly playlist..."
                },
 
                new AdaptiveProgressBar()
                {
                    Title = "Weekly playlist",
                    Value = 0.6,
                    ValueStringOverride = "15/26 songs",
                    Status = "Downloading..."
                }
            }
        }
    }
};
```

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

No entanto, você deve atualizar dinamicamente os valores da barra de progresso para que seja realmente "dinâmica". Isso pode ser feito usando associação de dados para atualizar a notificação do sistema.


## <a name="using-data-binding-to-update-a-toast"></a>Uso da associação de dados para atualizar uma notificação do sistema

O uso da associação de dados envolve as seguintes etapas...

1. Construir o conteúdo da notificação do sistema que utiliza os campos de dados associados
2. Atribuir uma **Marca** (e, opcionalmente, um **Grupo**) à **ToastNotification**
3. Definir os valores de **Dados** iniciais na **ToastNotification**
4. Enviar a notificação do sistema.
5. Utilizar a **Marca** e **Grupo** para atualizar os valores de **Dados** com os novos valores

O trecho de código a seguir mostra as etapas 1 a 4. O trecho a seguir mostrará como atualizar a os valores de **Dados** da notificação do sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContent()
    {
        Visual = new ToastVisual()
        {
            BindingGeneric = new ToastBindingGeneric()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Downloading your weekly playlist..."
                    },
    
                    new AdaptiveProgressBar()
                    {
                        Title = "Weekly playlist",
                        Value = new BindableProgressBarValue("progressValue"),
                        ValueStringOverride = new BindableString("progressValueString"),
                        Status = new BindableString("progressStatus")
                    }
                }
            }
        }
    };
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

Em seguida, quando você quiser alterar os valores de **Dados**, use o método [**Update**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) para fornecer os novos dados sem reconstruir o conteúdo inteiro da notificação do sistema.

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

O uso do método **Update** em vez da substituição da notificação inteira garante que a notificação do sistema permaneça na mesma posição na Central de ações e não se mova para cima ou para baixo. Seria muito confuso para o usuário ver a notificação do sistema mudar para a parte superior da Central de ações a cada segundo enquanto a barra de progresso é preenchida.

O método **Update** retorna uma enumeração, [**NotificationUpdateResult**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationupdateresult), que permite a você saber se a atualização foi bem-sucedida ou se não foi possível encontrar a notificação (ou seja, o usuário provavelmente ignorou a notificação e você deve parar de enviar atualização dela). Não recomendamos enviar outra notificação até que a operação em andamento seja concluída (por exemplo, quando o download é concluído).


## <a name="elements-that-support-data-binding"></a>Elementos que oferecem suporte à associação de dados
Os seguintes elementos em notificações do sistema oferecem suporte à associação de dados

- Todas as propriedades em **AdaptiveProgress**
- A propriedade **Text** nos elementos de nível superior de **AdaptiveText**


## <a name="update-or-replace-a-notification"></a>Atualizar ou substituir uma notificação

Desde o Windows 10, você sempre pode **substituir** uma notificação ao enviar uma nova notificação do sistema com a mesma **Marca** e **Grupo**. Então, qual é a diferença entre **substituir** a notificação do sistema e **atualizar** os dados da notificação?

| | Substituir | Atualizar |
| -- | -- | --
| **Posição na Central de Ações** | Move a notificação para o tipo da Central de ações. | Deixa a notificação sem alterações na Central de ações. |
| **Modificar o conteúdo** | Pode alterar completamente todo o conteúdos/layout da notificação do sistema | Pode alterar somente as propriedades que oferecem suporte à associação de dados (barra de progresso e texto de nível superior) |
| **Reaparece como pop-up** | Podem reaparecer como um pop-up de notificação do sistema se você deixar **SuppressPopup** definida como `false` (ou definida como true para enviá-la silenciosamente à Central de ações) | Não reaparece como um pop-up; os dados da notificação do sistema são atualizados silenciosamente na Central de ações |
| **Ignorado pelo usuário** | A notificação substituta sempre será enviada independentemente do usuário ignorar a notificação anterior | Se o usuário ignorou a notificação do sistema, ocorre falha na atualização dela |

Em geral, a **atualização é útil para...**

- Informações que mudam frequentemente em um curto período de tempo e não exigem a atenção imediata do usuário
- Alterações sutis de conteúdo de notificação do sistema, como alterar 50% para 65%

Muitas vezes, após a conclusão da sequência de atualizações (como o fim do download do arquivo), recomendamos a substituição como a etapa final, pois...

- A notificação final provavelmente tem alterações de layout drásticas, como a remoção da barra de progresso, adição de novos botões etc
- O usuário talvez tenha ignorado sua notificação de progresso pendente, pois eles não se preocupam em assistir o download, mas ainda querem ser notificados com um pop-up de notificação do sistema quando a operação é concluída


## <a name="related-topics"></a>Tópicos relacionados

- [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Conteúdo e documentação sobre notificações do sistema](adaptive-interactive-toasts.md)