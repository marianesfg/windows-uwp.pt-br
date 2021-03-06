---
description: Como usar informações do calendário e contatos em seu aplicativo UWP.
title: Contatos e calendário
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, contatos, calendário, compromissos, mensagens de email
ms.localizationpriority: medium
ms.openlocfilehash: b2e3f0b1d93d2b2c32e117f61fd7514077ca3923
ms.sourcegitcommit: 90fe7a9a5bfa7299ad1b78bbef289850dfbf857d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756542"
---
# <a name="contacts-my-people-and-calendar"></a>Contatos, Minhas Pessoas e calendário


Permita que os usuários acessem seus contatos e compromissos para que possam compartilhar entre si conteúdo, emails, informações do calendário, mensagens ou qualquer outra funcionalidade que você desenvolver.

Para conhecer alguns métodos diferentes que o seu aplicativo pode usar para acessar contatos e compromissos, consulte os seguintes tópicos:

| Tópico | Descrição |
|-------|-------------|
| [Selecionar contatos](selecting-contacts.md) | Pelo namespace [<strong>Windows.ApplicationModel.Contacts</strong>](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), você tem várias opções para selecionar contatos. Vamos mostrar aqui como selecionar um único contato ou vários deles e também como configurar o seletor de contatos para recuperar somente as informações de contatos necessárias para o aplicativo. |
| [Enviar email](sending-email.md) | Mostra como iniciar a caixa de diálogo de email para permitir que o usuário envie uma mensagem de email. Você pode previamente preencher os campos de email com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar. |
| [Enviar uma mensagem SMS](sending-an-sms-message.md) | Este tópico mostra como iniciar a caixa de diálogo de SMS para permitir que o usuário envie uma mensagem SMS. Você pode previamente preencher os campos de SMS com dados antes de mostrar a caixa. A mensagem não será enviada até que o usuário toque no botão enviar. |
| [Gerenciar compromissos](managing-appointments.md) | Pelo namespace [<strong>Windows.ApplicationModel.Appointments</strong>](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments), você pode criar e gerenciar compromissos no aplicativo de calendário de um usuário. Aqui, mostraremos como criar um compromisso, adicioná-lo a um aplicativo de calendário, substituí-lo no aplicativo de calendário e depois removê-lo desse aplicativo de calendário. Também mostraremos como exibir um intervalo de tempo para um aplicativo de calendário e criar um objeto de recorrência de compromisso. |
| [Conectar seu aplicativo a ações em um cartão de visita](integrating-with-contacts.md) | Mostra como seu aplicativo pode aparecer ao lado de ações em um cartão de contato ou um minicartão de contato. Os usuários podem escolher seu aplicativo para executar uma ação, como abrir uma página de perfil, fazer uma chamada ou enviar uma mensagem. |
| [Adicionando o suporte para Minhas Pessoas a um aplicativo](my-people-support.md) | Mostra como adicionar o suporte para Minhas Pessoas a um aplicativo e como fixar e desafixar contatos na barra de tarefas. |
| [Compartilhamento de Minhas Pessoas](my-people-sharing.md) | Mostra como adicionar o suporte ao compartilhamento para Minhas Pessoas, que permite aos usuários compartilhar conteúdo com seus contatos fixados arrastando arquivos do Explorador de Arquivos para o bloco Minhas Pessoas fixado. |
| [Notificações de Minhas Pessoas](my-people-notifications.md) | Mostra como criar e usar notificações de Minhas Pessoas a um novo tipo de notificação do sistema enviado de um contato fixado. |

 

## <a name="related-topics"></a>Tópicos relacionados

* [Amostra da API de compromissos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Appointments)
* [Amostra da API do Gerenciador de Contatos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Contact%20manager%20API%20sample)
* [Amostra de aplicativo de Seletor de Contatos](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/ContactPicker)
* [Amostra de manipulação de ações de contato](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/Handling%20Contact%20Actions)
* [Exemplo de integração de cartão de visita](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
