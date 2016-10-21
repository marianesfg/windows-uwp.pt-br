---
author: Xansky
description: "Como usar informações do calendário e contatos em seu aplicativo UWP."
title: "Contatos e calendário"
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
translationtype: Human Translation
ms.sourcegitcommit: 5c0f6ef1f1a346a66ca554a415d9f24c8a314ae1
ms.openlocfilehash: da73790ca9aec3fa16295eac4880c7b80db033ab

---

# Contatos e calendário

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Permita que os usuários acessem seus contatos e compromissos para que possam compartilhar entre si conteúdo, emails, informações do calendário, mensagens ou qualquer outra funcionalidade que você desenvolver.

Para conhecer alguns métodos diferentes que o seu aplicativo pode usar para acessar contatos e compromissos, consulte os seguintes tópicos:

| Tópico | Descrição |
|-------|-------------|
| [Selecionar contatos](selecting-contacts.md) | Pelo namespace [<strong>Windows.ApplicationModel.Contacts</strong>](https://msdn.microsoft.com/library/windows/apps/BR225002), você tem várias opções para selecionar contatos. Vamos mostrar aqui como selecionar um único contato ou vários deles e também como configurar o seletor de contatos para recuperar somente as informações de contatos necessárias para o aplicativo. |
| [Enviar email](sending-email.md) | Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar. |
| [Enviar uma mensagem SMS](sending-an-sms-message.md) | Este tópico mostra como iniciar a caixa de diálogo de SMS para permitir que o usuário envie uma mensagem SMS. Você pode previamente preencher os campos de SMS com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar. |
| [Gerenciar compromissos](managing-appointments.md) | Pelo namespace [<strong>Windows.ApplicationModel.Appointments</strong>](https://msdn.microsoft.com/library/windows/apps/Dn263359), você pode criar e gerenciar compromissos no aplicativo de calendário de um usuário. Aqui, mostraremos como criar um compromisso, adicioná-lo a um aplicativo de calendário, substituí-lo no aplicativo de calendário e depois removê-lo desse aplicativo de calendário. Também mostraremos como exibir um intervalo de tempo para um aplicativo de calendário e criar um objeto de recorrência de compromisso. |
| [Conectar seu aplicativo a ações em um cartão de contato](integrating-with-contacts.md) | Mostra como seu aplicativo pode aparecer ao lado de ações em um cartão de contato ou um minicartão de contato. Os usuários podem escolher seu aplicativo para executar uma ação, como abrir uma página de perfil, fazer uma chamada ou enviar uma mensagem. |
| [Fornecer feeds sociais ao aplicativo Pessoas](integrating-social-feeds-into-contact-cards.md) | Integre dados de feeds sociais de seu banco de dados ao aplicativo Pessoas. Seus dados de feed aparecerão nas páginas <strong>Novidades</strong> do aplicativo Pessoas ou na página <strong>Perfil</strong> de um contato. |

 

## Tópicos relacionados

* [Amostra da API de compromissos](http://go.microsoft.com/fwlink/p/?linkid=309836)
* [Amostra da API do Gerenciador de Contatos](http://go.microsoft.com/fwlink/p/?LinkID=310079)
* [Amostra de aplicativo de Seletor de Contatos](http://go.microsoft.com/fwlink/p/?linkid=231575)
* [Amostra de manipulação de ações de contato](http://go.microsoft.com/fwlink/p/?LinkID=320151)
* [Exemplo de integração de cartão de contato](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
* [Exemplo de informações sociais](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)



<!--HONumber=Aug16_HO5-->


