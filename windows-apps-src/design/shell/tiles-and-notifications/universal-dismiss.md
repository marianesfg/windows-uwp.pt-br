---
Description: Saiba como usar a ignorar Universal em suas notificações do sistema.
title: Ignorar universal
label: Universal Dismiss
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, uwp, notificação do sistema, Central de ações na nuvem, ignorar universal, notificação, entre dispositivos, ignorar uma vez, ignorar em todos os locais
ms.localizationpriority: medium
ms.openlocfilehash: 0dc87e8856e35d60660c2643b70b820b2857b488
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605091"
---
# <a name="universal-dismiss"></a>Ignorar universal

O recurso Ignorar universal, da Central de ações na nuvem, significa que, quando você ignorar uma notificação de um dispositivo, a mesma notificação também é ignorada em seus outros dispositivos.

> [!IMPORTANT]
> **Requer a atualização de aniversário do**: Você deve ter como destino do SDK do 14393 e estar executando a build 14393 ou superior para usar ignorar Universal.

O exemplo comum desse cenário é o de lembretes de calendário: você tem um aplicativo de calendário em ambos os dispositivos. Você recebe um lembrete em seu telefone e no computador. Em seguida, clica em ignorar no computador. Graças ao Ignorar Universal, o lembrete em seu telefone também é ignorado! **Habilitando a ignorar Universal requer apenas uma linha de código!**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

Nesse cenário, o fato de chave é que **o mesmo aplicativo está instalado em vários dispositivos**, ou seja, **cada dispositivo já está recebendo notificações**. Um aplicativo de calendário é o exemplo icônico, desde que você normalmente tenha o mesmo aplicativo de calendário instalado em seu computador Windows e no telefone, e cada instância do aplicativo já envia lembretes em cada dispositivo. Ao adicionar o suporte para Ignorar universal, essas instâncias dos mesmos lembretes podem ser vinculadas em todos os dispositivos.


## <a name="how-to-enable-universal-dismiss"></a>Como habilitar o Ignorar universal

Como desenvolvedor, é extremamente fácil habilitar o Ignorar universal. Forneça uma ID que nos permite vincular cada notificação entre dispositivos para que, quando o usuário descartar uma notificação de um dispositivo, a notificação vinculada correspondente seja ignorada de outro dispositivo.

![Diagrama de RemoteId de Ignorar universal](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: Um identificador que identifica exclusivamente uma notificação *em todos os dispositivos*.

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

**Problema conhecido**: Recuperando a **RemoteId** por meio de `ToastNotificationHistory.GetHistory()` API sempre retornará a cadeia de caracteres vazia em vez de **RemoteId** especificado. Não se preocupe, tudo está funcionando, está apenas recuperando o valor violado.

> [!NOTE]
> Se o usuário ou a empresa desabilita o [espelhamento de notificação](notification-mirroring.md) do aplicativo (ou desabilita totalmente o espelhamento de notificação), o recurso Ignorar universal não funcionará, pois não temos suas notificações na nuvem.


## <a name="supported-devices"></a>Dispositivos com suporte

Desde a Atualização de Aniversário, o recurso Ignorar universal tem suporte no Windows Mobile e Windows para computador. O Ignorar universal funciona em ambas as direções, de computador para computador, de computador para o telefone e de telefone para telefone.