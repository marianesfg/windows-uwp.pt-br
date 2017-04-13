---
author: Xansky
description: "Este tópico mostra como iniciar a caixa de diálogo de SMS para permitir que o usuário envie uma mensagem SMS. Você pode previamente preencher os campos de SMS com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar."
title: Enviar uma mensagem SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contatos, SMS, envio
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 3d2edd2e26633abacce07a2a6fe9f8c9d14b0afe
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="send-an-sms-message"></a>Enviar uma mensagem SMS

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este tópico mostra como iniciar a caixa de diálogo de SMS para permitir que o usuário envie uma mensagem SMS. Você pode previamente preencher os campos de SMS com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.

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

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Este tópico mostrou como iniciar a caixa de redação de SMS. Para obter informações sobre como selecionar contatos para usar como destinatários de uma mensagem SMS, consulte [Selecionar contatos](selecting-contacts.md). Baixe as [amostras de aplicativo Universal do Windows](http://go.microsoft.com/fwlink/p/?linkid=619979) do GitHub para ver mais exemplos de como enviar e receber mensagens SMS usando uma tarefa em segundo plano.

## <a name="related-topics"></a>Tópicos relacionados

* [Selecionar contatos](selecting-contacts.md)
