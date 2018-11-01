---
author: normesta
description: Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.
title: Enviar email
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contatos, email, envio
ms.author: normesta
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0a28809210f71bf523e3cc5f9c8da1db9fbcc90c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5883913"
---
# <a name="send-email"></a>Enviar email

Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar.

**Neste artigo**

-   [Iniciar a caixa de redação de email](#launch-the-compose-email-dialog)
-   [Resumo e próximas etapas](#summary-and-next-steps)
-   [Tópicos relacionados](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Iniciar a caixa de redação de email

Crie um novo objeto [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) e defina os dados que você quer que sejam previamente populados na caixa de redação de email. Chame [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) para mostrar a caixa de diálogo.

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
> Anexos que você adicionar a um email usando a classe [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) aparecerá somente no aplicativo Mail. Se os usuários têm qualquer outro programa de email configurado como o programa de email padrão, a janela de redigir será exibida sem o anexo. Isso é um problema conhecido.

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Este tópico mostrou como iniciar a caixa de redação de email. Para obter mais informações sobre como selecionar contatos para usar como destinatários de uma mensagem de email, consulte [Selecionar contatos](selecting-contacts.md). Veja [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) para selecionar um arquivo a ser usado como um anexo de email.

## <a name="related-topics"></a>Tópicos relacionados

* [Selecionando contatos](selecting-contacts.md)
* [Como continuar seu aplicativo do Windows Phone após chamar um Seletor de arquivo](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 
