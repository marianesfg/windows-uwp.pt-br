---
title: Responder ao feedback dos clientes
description: "Você pode responder diretamente aos comentários que os clientes deixam no Hub de Feedback."
author: JnHs
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 04983b80-2a18-4ace-93d3-e8c33c04bfb9
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ce908bd14065143f71062bf5792d256c7ac775e9
ms.lasthandoff: 02/08/2017

---

# <a name="respond-to-customer-feedback"></a>Responder ao feedback dos clientes

Você pode usar o [Relatório de comentários](feedback-report.md) para analisar os comentários que os clientes do Windows 10 deixaram sobre o app no Hub de Feedback e, em seguida, responder diretamente a esse comentário. Você pode postar as respostas no Hub de Feedback para que todos possam ver (como comentários individuais, ou atualizando o status de uma parte dos comentários e adicionando uma descrição) para informar aos clientes sobre novos recursos ou correções de bugs, ou para solicitar comentários mais específicos sobre como melhorar o app. Você também pode enviar a resposta como um email diretamente para o cliente que deixou os comentários.

> **Dica** Você pode incentivar os clientes a fazerem comentários usando a Feedback API no [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) para adicionar um controle que deixe os clientes [iniciarem o Hub de Feedback diretamente pelo app UWP](../monetize/launch-feedback-hub-from-your-app.md). Lembre-se de que qualquer cliente que tenha baixado seu app em um dispositivo Windows 10 que dê suporte a comentários Hub tem a capacidade de deixar comentários sobre ele diretamente por meio do app de Hub de Feedback. Por isso, você poderá ver comentários de clientes nesse relatório, mesmo que não tenha solicitado comentários especificamente dentro de seu app.

Para dar uma resposta a qualquer comentário, clique no link **Respond to feedback** exibido no comentário no **Relatório de comentários**.

O Centro de Desenvolvimento do Windows dá suporte a três opções para responder aos clientes que fazem comentários sobre o app. Independentemente da opção que você escolher, tenha em mente que há um limite de 1.000 caracteres para cada resposta.

## <a name="public-comments-in-feedback-hub"></a>Comentários públicos no Hub de Feedback

Por padrão, a botão de opção de **Comentário** será selecionado depois que você clicar em **Respond to feedback**. Para postar uma resposta pública aos comentários do cliente, deixe esse botão selecionado. Digite o comentário na caixa e clique em **Enviar**.

O comentário que você inseriu será exibido como um comentário no Hub de Feedback, com os comentários enviados por outros clientes. O nome do fornecedor e o nome do app serão exibidos com o comentário para identificar você como o desenvolvedor. Não há limite quanto ao número de comentários que você pode escrever para um comentário, mas observe que você não poderá editar nem excluir comentários depois de enviá-los. Os cinco comentários mais recentes para um comentário serão mostrados no **Relatório de comentários** (bem como no Hub de Feedback). Quando houver mais de cinco comentários, você poderá clicar em **Mostrar todos os comentários** para ver todos eles no Hub de Feedback.

## <a name="private-responses-via-email"></a>Respostas particulares por email

Se preferir não postar uma resposta pública, você poderá marcar a caixa **Send comment as email** para enviar uma resposta privada diretamente para o cliente (se ele tiver fornecido um endereço de email e não tiver optado por não receber respostas por email). Quando você faz isso, a Microsoft envia um email para o cliente em seu nome. O email conterá os comentários originais, bem como a resposta escrita por você.

Depois de marcar a caixa **Send comment as email**, digite o comentário e clique em **Enviar**. Você deve fornecer um endereço de email no campo **Email de contato de suporte** ao usar essa opção. Por padrão, usamos o endereço de email que você forneceu nas informações de contato da conta. Se preferir usar um endereço de email diferente, você poderá atualizar o campo **Email de contato de suporte** para usar um diferente. O cliente que recebe a resposta será capaz de responder diretamente a esse endereço de email.

## <a name="public-status-updates-and-descriptions-in-feedback-hub"></a>Atualizações de status público e descrições no Hub de Feedback

Uma terceira opção para uma resposta pública é definir o status em comentário para informar os clientes que você está trabalhando no problema ou o corrigiu. Quando você atualiza o status de um comentário, ele é exibido com os comentários no Hub de Feedback.

Para usar essa opção, selecione o botão de opção **Atualizar status**. Em seguida, selecione uma das seguintes opções:

- **Investigando**: Você está ciente de um problema e o está examinando.
- **Trabalhando nele**: Você está em meio ao processo de correção de um problema ou adição de um recurso solicitado.
- **Concluído**: Você publicou uma atualização para corrigir o problema ou adicionar o recurso solicitado.

Com a atualização do status, você pode inserir um comentário para fornecer mais informações, como uma estimativa de quando acha que um problema será corrigido ou obter mais informações sobre as alterações mais recentes. Esta descrição será exibida na parte superior da lista de comentários (e o Relatório de comentários exibirá o status e a descrição atuais).

O uso da opção **Atualizar status** permitirá alterar o status sempre que você quiser (além de fornecer descrições atualizadas para cada alteração de status). Sempre que você alterar o status de um comentário, ele será atualizado no Hub de Feedback de maneira que os clientes que veem a resposta vejam o status mais recente.

## <a name="guidelines-for-responses"></a>Diretrizes para respostas
Não importa qual método usa para responder aos comentários do cliente, você deve seguir essas diretrizes para todas as respostas.
- As respostas devem ter, no máximo, 1000 caracteres.
- Você não deve oferecer qualquer tipo de compensação, incluindo itens de apps digitais, aos usuários por comentários públicos.
- Não inclua qualquer conteúdo de marketing ou anúncios em sua resposta. Lembre-se de que a pessoa que fez comentários já é seu cliente.
- Não promova outros apps ou serviços em sua resposta.
- Sua resposta deve estar relacionada diretamente ao app específico e aos comentários.
- Não inclua nenhum comentário profano, agressivo, pessoal ou mal-intencionado em sua resposta. Seja sempre educado e tenha em mente que clientes felizes provavelmente serão os maiores defensores do seu app.

> **Observação** Os clientes poderão denunciar um desenvolvedor para a Microsoft, se receberem uma resposta de comentário inadequada. Eles também podem recusar receber respostas de comentários por email.

Sua relação com seus clientes cabe a você. A Microsoft não se envolve em controvérsias entre desenvolvedores e clientes. No entanto, se você acha que o conteúdo de um comentário do cliente ao seu produto seja inapropriada, envie um [tíquete de suporte](http://go.microsoft.com/fwlink/p/?LinkID=401178).

