---
author: jnHs
Description: "Exiba os nomes que você já reservou para seu aplicativo, reserve nomes adicionais (para outros idiomas ou para alterar o nome do aplicativo) e exclua nomes reservados de que você não precisa mais."
title: Gerenciar nomes de app
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 539ccc671ae3f66c6c17a077ac1c09d989d86fdc
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2017
---
# <a name="manage-app-names"></a>Gerenciar nomes de app


Você pode visualizar todos os nomes já reservados para seu aplicativo, reservar nomes adicionais (para outros idiomas ou para alterar o nome do aplicativo) e excluir nomes reservados de que você não precisa mais. Para fazer isso, acesse a página **Gerenciar nomes de aplicativo** na seção **Gerenciamento de aplicativos** para qualquer um dos seus aplicativos no painel do Centro de Desenvolvimento do Windows.

## <a name="reserve-additional-names-for-your-app"></a>Reservar nomes adicionais para seu aplicativo

Você pode reservar vários nomes de aplicativo para usar no mesmo aplicativo. Isso é especialmente útil se você estiver oferecendo seu aplicativo em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode reservar um novo nome para alterar o nome de um aplicativo.

Na seção **Reservar mais nomes** da página **Gerenciar nomes de aplicativo**, você verá uma caixa de texto. Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome do produto**.

> [!NOTE]
> Para saber mais sobre como reservar nomes de aplicativo, e por que um determinado nome pode não estar disponível, consulte [Criar seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md).

Você pode continuar a reservar nomes de aplicativo adicionais aqui, se desejar.

## <a name="delete-app-names"></a>Excluir nomes de aplicativo

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu aplicativo, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que seu app precisa ter pelo menos um nome reservado. Para remover completamente um aplicativo do painel (e liberar todos os nomes reservados para esse aplicativo), clique em **Excluir este aplicativo** da página **Visão geral do aplicativo**. Se você tiver um envio do aplicativo em andamento, é necessário excluir o envio primeiro. Se você já tiver publicado o aplicativo na Loja, não é possível excluí-lo do painel (mas você pode usar a funcionalidade **Mostrar/ocultar produtos** na página **Visão geral** para ocultá-lo). 

## <a name="rename-an-app-that-has-already-been-published"></a>Renomear um aplicativo já publicado

Se o aplicativo já está na Loja e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o aplicativo.

Você deve atualizar os pacotes do aplicativo a fim de incluir o novo nome para que a Loja mostre-o com o novo nome. Primeiro, atualize o arquivo Package.StoreAssociation.xml para usar o novo nome, manualmente ou usando o Visual Studio (**Projeto > Loja > Associar aplicativo à Loja...**). Para obter mais informações, consulte [Empacotando aplicativos UWP](../packaging/packaging-uwp-apps.md).

Também é necessário atualizar o elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-displayname) no manifesto do aplicativo e atualize elementos gráficos ou texto que inclua o nome do aplicativo. 

> [!IMPORTANT]
> Certifique-se de atualizar o arquivo Package.StoreAssociation.xml antes de alterar **Pacote/Propriedades/DisplayName** no aplicativo do manifesto ou você poderá receber um erro.

Você também deve revisar a listagem da Loja do aplicativo e alterar o nome caso seja mencionado lá. Quando o aplicativo tiver sido publicado com o novo nome, você poderá excluir o nome antigo que não precisa mais usar.

 

 




