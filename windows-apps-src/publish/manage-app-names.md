---
author: jnHs
Description: "Exiba os nomes que você já reservou para seu app, reserve nomes adicionais (para outros idiomas ou para alterar o nome do app) e exclua nomes reservados de que você não precisa mais."
title: Gerenciar nomes de app
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 5b34723eb6d336eeacb7437a926cae7f1d3ca871
ms.lasthandoff: 02/07/2017

---

# <a name="manage-app-names"></a>Gerenciar nomes de app


Você pode visualizar todos os nomes já reservados para seu app, reservar nomes adicionais (para outros idiomas ou para alterar o nome do app) e excluir nomes reservados de que você não precisa mais. Para fazer isso, acesse a página **Gerenciar nomes de app** na seção **Gerenciamento de apps** para qualquer um dos seus apps no painel do Centro de Desenvolvimento do Windows.

## <a name="reserve-additional-names-for-your-app"></a>Reservar nomes adicionais para seu app

Você pode reservar vários nomes de app para usar no mesmo app. Isso é especialmente útil se você estiver oferecendo seu app em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode usar isso para alterar o nome de um app que ainda não foi publicado.

Na seção **Reservar mais nomes** da página **Gerenciar nomes de app**, você verá uma caixa de texto. Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome**.

> **Observação**  Para saber mais sobre como reservar nomes de app, e por que um determinado nome pode não estar disponível, consulte [Criar seu app reservando um nome](create-your-app-by-reserving-a-name.md).

Você pode continuar a reservar nomes de app adicionais aqui, se desejar.

## <a name="delete-app-names"></a>Excluir nomes de app

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu app, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que seu app precisa ter pelo menos um nome reservado. Para remover completamente um app do painel (o que também libera todos os nomes reservados desse app), você pode clicar em **Excluir este app** da página **Visão geral**.

## <a name="rename-an-app-that-has-already-been-published"></a>Renomear um app que já foi publicado

Se o seu app já está na Windows Store e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o app. Observe que você precisará atualizar seu pacote para incluir o novo nome para a Loja exibir o app com o novo nome. Certifique-se de usar o novo nome no elemento [**Package/Properties/DisplayName**](https://msdn.microsoft.com/library/windows/apps/dn423240) no manifesto do app e atualize elementos gráficos ou texto que inclua o nome do app. Você também vai querer revisar a descrição do app e alterar o nome caso ele seja mencionado lá.

Quando o app tiver sido publicado com o novo nome, você poderá excluir o nome antigo que não precisa mais usar.

 

 





