---
author: andrewleader
Description: Learn how to use Universal Dismiss on your toast notifications.
title: Ignorar universal
label: Universal Dismiss
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, uwp, notificação do sistema, Central de ações na nuvem, ignorar universal, notificação, entre dispositivos, ignorar uma vez, ignorar em todos os locais
ms.localizationpriority: medium
ms.openlocfilehash: 40a9c446172b25f2430a3f75014c8e168a91c233
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5743756"
---
# <a name="universal-dismiss"></a>Ignorar universal

O recurso Ignorar universal, da Central de ações na nuvem, significa que, quando você ignorar uma notificação de um dispositivo, a mesma notificação também é ignorada em seus outros dispositivos.

> [!IMPORTANT]
> **Requer a Atualização de Aniversário**: você deve usar o SDK 14393 e executar o build 14393 ou posterior para usar o recurso de Ignorar universal.

O exemplo comum desse cenário é o de lembretes de calendário: você tem um aplicativo de calendário em ambos os dispositivos. Você recebe um lembrete em seu telefone e no computador. Em seguida, clica em ignorar no computador. Graças ao Ignorar Universal, o lembrete em seu telefone também é ignorado! **A habilitação do Ignorar universal exibe apenas uma linha de código.**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

Nesse cenário, o fato de chave é que **o mesmo aplicativo está instalado em vários dispositivos**, ou seja, **cada dispositivo já está recebendo notificações**. Um aplicativo de calendário é o exemplo icônico, desde que você normalmente tenha o mesmo aplicativo de calendário instalado em seu computador Windows e no telefone, e cada instância do aplicativo já envia lembretes em cada dispositivo. Ao adicionar o suporte para Ignorar universal, essas instâncias dos mesmos lembretes podem ser vinculadas em todos os dispositivos.


## <a name="how-to-enable-universal-dismiss"></a>Como habilitar o Ignorar universal

Como desenvolvedor, é extremamente fácil habilitar o Ignorar universal. Forneça uma ID que nos permite vincular cada notificação entre dispositivos para que, quando o usuário descartar uma notificação de um dispositivo, a notificação vinculada correspondente seja ignorada de outro dispositivo.

![Diagrama de RemoteId de Ignorar universal](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: um identificador que identifica exclusivamente uma notificação *em todos os dispositivos*.

É necessário apenas uma linha de código para adicionar RemoteId e habilitar o suporte para o Ignorar universal. Como a RemoteId é gerada depende de você. Entretanto, é preciso certificar-se de que ela identifica sua notificação exclusivamente em todos os dispositivos, e que o mesmo identificador pode ser gerado a partir de instâncias diferentes do seu aplicativo executado em dispositivos diferentes.

Por exemplo, no meu aplicativo planejador de dever de casa, gero a RemoteId dizendo que ela é do tipo "lembrete" e, em seguida, devo incluir a ID da conta online e o identificador online do item do dever de casa. Posso gerar consistentemente a mesma RemoteId, independentemente de qual dispositivo está enviando a notificação, desde que as IDs online sejam compartilhadas entre os dispositivos.

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

O código a seguir é executado no meu telefone e no aplicativo de computador, ou seja, a notificação em ambos os dispositivos terá a mesma RemoteId.

Isso é tudo o que você precisa fazer. Quando o usuário descarta (ou clica em) uma notificação, é possível verificar se ele tem uma RemoteId. Em caso afirmativo, ignoramos a RemoteId em todos os dispositivos do usuário.

**Problema conhecido**: a recuperação da **RemoteId** por meio da API `ToastNotificationHistory.GetHistory()` sempre retornará a cadeia de caracteres vazia em vez da **RemoteId** especificada. Não se preocupe, tudo está funcionando, está apenas recuperando o valor violado.

> [!NOTE]
> Se o usuário ou a empresa desabilita o [espelhamento de notificação](notification-mirroring.md) do aplicativo (ou desabilita totalmente o espelhamento de notificação), o recurso Ignorar universal não funcionará, pois não temos suas notificações na nuvem.


## <a name="supported-devices"></a>Dispositivos com suporte

Desde a Atualização de Aniversário, o recurso Ignorar universal tem suporte no Windows Mobile e Windows para computador. O Ignorar universal funciona em ambas as direções, de computador para computador, de computador para o telefone e de telefone para telefone.