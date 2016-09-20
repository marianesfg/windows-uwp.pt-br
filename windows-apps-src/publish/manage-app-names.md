---
author: jnHs
Description: "Exiba os nomes que você já reservou para seu aplicativo, reserve nomes adicionais (para outros idiomas ou para alterar o nome do aplicativo) e exclua nomes reservados de que você não precisa mais."
title: Gerenciar nomes de aplicativo
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.sourcegitcommit: 48952b5d4f2565d06ec79475f88fbabf93fd0f70
ms.openlocfilehash: e03e99b5de2142c2bcf46ee2aad4a76bb87ee4e5

---

# Gerenciar nomes de aplicativo


Você pode visualizar todos os nomes já reservados para seu aplicativo, reservar nomes adicionais (para outros idiomas ou para alterar o nome do aplicativo) e excluir nomes reservados de que você não precisa mais. Para fazer isso, acesse a página **Gerenciar nomes de aplicativo** na seção **Gerenciamento de aplicativos** para qualquer um dos seus aplicativos no painel do Centro de Desenvolvimento do Windows.

## Reservar nomes adicionais para seu aplicativo

Você pode reservar vários nomes de aplicativo para usar no mesmo aplicativo. Isso é especialmente útil se você estiver oferecendo seu aplicativo em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode usar isso para alterar o nome de um aplicativo que ainda não foi publicado.

Na seção **Reservar mais nomes** da página **Gerenciar nomes de aplicativo**, você verá uma caixa de texto. Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome**.

> 
            **Observação**  Para saber mais sobre como reservar nomes de aplicativo, e por que um determinado nome pode não estar disponível, consulte [Criar seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md).

Você pode continuar a reservar nomes de aplicativo adicionais aqui, se desejar.

## Excluir nomes de aplicativo

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu aplicativo, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que seu aplicativo precisa ter pelo menos um nome reservado. Para remover completamente um aplicativo do painel (o que também libera todos os nomes reservados desse aplicativo), você pode clicar em **Excluir este aplicativo** da página **Visão geral**.

## Renomear um aplicativo que já foi publicado

Se o seu aplicativo já está na Windows Store e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o aplicativo. Observe que você precisará atualizar seu pacote para incluir o novo nome para a Loja exibir o aplicativo com o novo nome. Certifique-se de usar o novo nome no elemento [**Package/Properties/DisplayName**](https://msdn.microsoft.com/library/windows/apps/dn423240) no manifesto do aplicativo e atualize elementos gráficos ou texto que inclua o nome do aplicativo. Você também vai querer revisar a descrição do aplicativo e alterar o nome caso ele seja mencionado lá.

Quando o aplicativo tiver sido publicado com o novo nome, você poderá excluir o nome antigo que não precisa mais usar.

 

 







<!--HONumber=Jun16_HO4-->


