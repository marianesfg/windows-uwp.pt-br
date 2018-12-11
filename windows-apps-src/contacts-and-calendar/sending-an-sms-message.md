---
description: Este tópico mostra como iniciar a caixa de diálogo de SMS para permitir que o usuário envie uma mensagem SMS. Você pode previamente preencher os campos de SMS com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.
title: Enviar uma mensagem SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contatos, SMS, envio
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 427eb1b895269727d82e42d5abc3ae1f1da1a35d
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874063"
---
# <a name="send-an-sms-message"></a>Enviar uma mensagem SMS

Este tópico mostra como iniciar a caixa de diálogo de SMS para permitir que o usuário envie uma mensagem SMS. Você pode previamente preencher os campos de SMS com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.

Para chamar esse código, declare os recursos de **bate-papo**, **smsSend**e **chatSystem** no manifesto do pacote. Esses são [recursos restritos](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities) , mas você pode usá-los em seu aplicativo. Você precisará da aprovação somente se você pretende publicar seu aplicativo para a loja. Consulte [taxas, locais e tipos de conta](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees).

## <a name="launch-the-compose-sms-dialog"></a>Iniciar a caixa de redação de SMS

Crie um novo objeto [**ChatMessage**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessage) e defina os dados que você quer que sejam previamente preenchidos na caixa de redação de email. Chame [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) para mostrar a caixa de diálogo.

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient,
    string messageBody,
    StorageFile attachmentFile,
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

Você pode usar o código a seguir para determinar se o dispositivo que está executando o aplicativo é capaz de enviar mensagens SMS.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Este tópico mostrou como iniciar a caixa de redação de SMS. Para obter informações sobre como selecionar contatos para usar como destinatários de uma mensagem SMS, consulte [Selecionar contatos](selecting-contacts.md). Baixe as [amostras de aplicativo Universal do Windows](http://go.microsoft.com/fwlink/p/?linkid=619979) do GitHub para ver mais exemplos de como enviar e receber mensagens SMS usando uma tarefa em segundo plano.

## <a name="related-topics"></a>Tópicos relacionados

* [Selecionar contatos](selecting-contacts.md)
