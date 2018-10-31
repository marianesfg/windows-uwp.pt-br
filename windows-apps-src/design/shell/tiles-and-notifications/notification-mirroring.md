---
author: andrewleader
Description: Learn how to use notification mirroring on your toast notifications.
title: Espelhamento de notificação
label: Notification mirroring
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, notificação do sistema, central de ações na nuvem, espelhamento de notificação, notificação, entre dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: c36b5ffcbb016e5b89fa1c960a7493767192296c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5838189"
---
# <a name="notification-mirroring"></a>Espelhamento de notificação

O espelhamento de notificação, fornecido pela Central de ações na nuvem, permite que você veja as notificações do seu telefone no computador.

> [!IMPORTANT]
> **Requer a Atualização de Aniversário**: você precisa usar a versão 14393 ou posterior para ver o espalhamento de notificação. Se você gostaria de recusar a notificação de espelhamento do aplicativo, é necessário direcionar o SDK 14393 para acessar as APIs de espelhamento.

Com o espelhamento de notificação e a Cortana, os usuários podem receber e agir em relação às notificações do telefone (Windows Mobile e Android) pelo computador. Como desenvolvedor, você não precisa fazer nada para habilitar o espelhamento de notificação: o espelhamento funciona automaticamente. Clique nos botões da notificação do sistema espelhada, como respostas rápidas de mensagem, e você será enviado novamente para o telefone, invocando uma tarefa em segundo plano ou iniciando seu aplicativo em primeiro plano.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

Os desenvolvedores obtêm dois grandes benefícios de espelhamento de notificação: as notificações espelhadas resultam em mais envolvimento do usuário com seu serviço, e eles também ajudam os usuários a descobrir seu aplicativo da área de trabalho da Microsoft Store! Os usuários talvez não saibam que você tem um aplicativo UWP incrível disponível para seu computador Windows 10. Quando os usuários recebem a notificação espelhada do telefone, os usuários podem clicar para ir até a Microsoft Store, onde eles podem instalar seu aplicativo da área de trabalho UWP notificação do sistema.

O espelhamento funciona no Windows Phone e no Android. Os usuários devem estar conectados à Cortana no telefone e no computador para que o espelhamento de notificação funcione.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>O que acontece se o aplicativo estiver instalado em ambos os dispositivos?

Se o usuário já tiver o aplicativo em seu computador, a notificação de telefone espelhada será silenciada automaticamente para que não vejam notificações duplicadas. As notificações espelhadas serão desativadas automaticamente com base nos seguintes critérios...

1. Existe um aplicativo no computador com um o **mesmo nome de exibição ou o mesmo PFN** (Nome da família de pacotes)
2. Esse aplicativo de computador enviou uma notificação do sistema

Se o aplicativo de computador ainda não enviou uma notificação do sistema, ainda mostraremos as notificações de telefone, pois provavelmente o usuário ainda não iniciou o aplicativo para computador.


## <a name="how-to-opt-out-of-mirroring"></a>Como recusar o espelhamento

Os usuários, as empresas e desenvolvedores de aplicativo UWP podem optar por desabilitar o espelhamento de notificação.

> [!NOTE]
> Ao desabilitar o espelhamento, também será desativado o [Ignorar universal](universal-dismiss.md).


### <a name="as-a-developer-opt-out-an-individual-notification"></a>Como desenvolvedor, recuse uma notificação individual

Ocasionalmente, você pode ter uma notificação específica de dispositivo a qual você não deseja que seja espelhada para outros dispositivos. Você pode impedir que uma notificação específica seja espelhada ao definir a propriedade **Mirroring** na notificação do sistema. No momento, essa propriedade de espelhamento pode ser definida somente em notificações locais (ela não pode ser especificada ao enviar uma notificação WNS por push).

**Problema conhecido**: a recuperação da propriedade de espelhamento por meio da API de `ToastNotificationHistory.GetHistory()` sempre retornará o valor padrão (**Permitido**) em vez da opção que você especificou. Não se preocupe, tudo está funcionando, está apenas recuperando o valor violado.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>Como desenvolvedor, recuse completamente

Alguns desenvolvedores podem optar por recusar completamente o espelhamento de notificação no aplicativo. Embora acreditamos que todos os aplicativos se beneficiam com o espelhamento, facilitamos a recusa. Apenas chame o método a seguir uma vez e seu aplicativo será retirado. Por exemplo, você pode colocar essa chamada no construtor do aplicativo em `App.xaml.cs`...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>Como uma empresa, como faço para recusar?

As empresas podem optar por desabilitar completamente o espelhamento de notificação. Para fazer isso, devem apenas editar a Política de Grupo para desativar o espelhamento de notificação.


### <a name="as-a-user-how-do-i-opt-out"></a>Como um usuário, como faço para recusar?

Os usuários são capazes de recusar em aplicativos individuais ou recusar completamente ao desativar o recurso. Você pode não desejar notificações específicas do aplicativo espelhadas em sua área de trabalho, portanto, é possível desabilitar esse aplicativo específico. Você pode encontrar essas opções nas configurações da Cortana do no seu telefone e no computador.