---
author: Xansky
description: "Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar."
title: Enviar email
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, email, send
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: b4b5b029c321256028993e283395a91bd0ed3d7c

---

# Enviar email

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.

**Neste artigo**

-   [Iniciar a caixa de redação de email](#launch-the-compose-email-dialog)
-   [Resumo e próximas etapas](#summary-and-next-steps)
-   [Tópicos relacionados](#related-topics)

## Iniciar a caixa de redação de email

Crie um novo objeto [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) e defina os dados que você quer que sejam previamente populados na caixa de redação de email. Chame [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) para mostrar a caixa de diálogo.

``` cs
private async void ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Email.EmailAttachment(
            attachmentFile.Name,
            stream);

        emailMessage.Attachments.Add(attachment);
    }

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
        
}
```

## Resumo e próximas etapas

Este tópico mostrou como iniciar a caixa de redação de email. Para saber mais sobre como selecionar contatos para usar como destinatários de uma mensagem de email, consulte [Selecionar contatos](selecting-contacts.md). Veja [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) para selecionar um arquivo a ser usado como um anexo de email.

## Tópicos relacionados

* [Selecionando contatos](selecting-contacts.md)
* [Como continuar seu aplicativo do Windows Phone após chamar um Seletor de arquivo](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 







<!--HONumber=Jun16_HO3-->


