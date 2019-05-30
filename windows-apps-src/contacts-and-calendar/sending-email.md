---
description: Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.
title: Enviar email
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contatos, email, envio
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 524e1f12c3da0d9d06e73d84e08e2d54efde9a7e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361215"
---
# <a name="send-email"></a>Enviar email

Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.

**Neste artigo**

-   [Iniciar a caixa de diálogo de email de composição](#launch-the-compose-email-dialog)
-   [Resumo e próximas etapas](#summary-and-next-steps)
-   [Tópicos relacionados](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Iniciar a caixa de redação de email

Crie um novo objeto [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) e defina os dados que você quer que sejam previamente populados na caixa de redação de email. Chame [**ShowComposeNewEmailAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) para mostrar a caixa de diálogo.

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> Os anexos que você adicionar a um email usando o [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) classe será exibida apenas no aplicativo Mail. Se os usuários tiverem qualquer outro programa de email configurado como esse programa de email padrão, a janela de redação será exibida sem o anexo. Esse é um problema conhecido.

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Este tópico mostrou como iniciar a caixa de redação de email. Para obter mais informações sobre como selecionar contatos para usar como destinatários de uma mensagem de email, consulte [Selecionar contatos](selecting-contacts.md). Veja [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) para selecionar um arquivo a ser usado como um anexo de email.

## <a name="related-topics"></a>Tópicos relacionados

* [Seleção de contatos](selecting-contacts.md)
* [Como continuar seu aplicativo do Windows Phone depois de chamar um seletor de arquivos](https://docs.microsoft.com/previous-versions/windows/apps/dn614994(v=win.10))
 

 
