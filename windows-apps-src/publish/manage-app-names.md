---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: Gerenciar nomes de aplicativo
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
keywords: o Windows 10, uwp, nomes de aplicativo, alterar o nome do aplicativo, atualização de nome do aplicativo, jogo, nome do produto
ms.localizationpriority: medium
ms.openlocfilehash: b35db620956e99791d03fb2d25dea8682d4ffaac
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7295700"
---
# <a name="manage-app-names"></a>Gerenciar nomes de aplicativo

Permite o **Gerenciar nomes de aplicativo** exibir todos os nomes que reservou para seu aplicativo, reservar nomes adicionais (para outros idiomas ou para alterar o nome do aplicativo) e excluir nomes que você não precisa. Você pode encontrar essa página no [Partner Center](https://partner.microsoft.com/dashboard) ao expandir a seção de **gerenciamento de aplicativo** no menu de navegação esquerdo para qualquer um dos seus aplicativos.

> [!IMPORTANT]
> Você pode reservar nomes adicionais para um aplicativo, e você pode optar por usar um na versão publicada do seu aplicativo em vez da reservado ao criar seu aplicativo no Partner Center. No entanto, lembre-se que o primeiro nome reservado para seu produto será usado em alguns dos seu TI do [detalhes de identidade](view-app-identity-details.md), como o **Nome da família de pacote (PFN)**. Esses valores pode estar visíveis para alguns usuários e não pode ser alterado, portanto, certifique-se de que o nome reservado primeiro é apropriado para esse uso.


## <a name="reserve-additional-names-for-your-app"></a>Reservar nomes adicionais para seu aplicativo

Você pode reservar vários nomes de aplicativo para usar no mesmo aplicativo. Isso é especialmente útil se você estiver oferecendo seu aplicativo em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode reservar um novo nome para alterar o nome de um aplicativo, conforme descrito a seguir.

Para reservar um novo nome de aplicativo, encontre a caixa de texto na seção **reservar mais nomes** da página **Gerenciar nomes de aplicativo** . Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome do produto**. Você pode reservar vários nomes de aplicativo por repetir essas etapas, se desejado.

> [!NOTE]
> Para saber mais sobre como reservar nomes de aplicativo, e por que um determinado nome pode não estar disponível, consulte [Criar seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Excluir nomes de aplicativo

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu aplicativo, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que seu aplicativo deve ter pelo menos um nome reservado. Para remover completamente um aplicativo do Partner Center (e liberar todos os nomes reservados para esse aplicativo), clique em **Excluir este aplicativo** na página **Visão geral do aplicativo** . Se você tiver um envio do aplicativo em andamento, é necessário excluir o envio primeiro. Observe que se você já tiver publicado o aplicativo para a loja, você não pode excluí-lo do Partner Center (embora você pode usar a funcionalidade de **Mostrar/ocultar produtos** em sua página de **Visão geral** para ocultá-lo). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renomear um aplicativo já publicado

Se o aplicativo já está na Loja e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o aplicativo. 

Você deve atualizar os pacotes do aplicativo para substituir o nome antigo pelo novo e carregar os pacotes atualizados para seu envio.
- Primeiro, atualize o arquivo storeassociation para usar o novo nome, seja manualmente ou usando o Visual Studio (**projeto > loja > associar aplicativo à loja...**). Para obter mais informações, consulte o [pacote de um aplicativo UWP com Visual Studio](../packaging/packaging-uwp-apps.md).
- Também é necessário atualizar o elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) no manifesto do aplicativo e atualize elementos gráficos ou texto que inclua o nome do aplicativo. 
  > [!IMPORTANT]
  > Certifique-se de atualizar o arquivo Package.StoreAssociation.xml antes de alterar **Pacote/Propriedades/DisplayName** no aplicativo do manifesto ou você poderá receber um erro.

Para atualizar uma listagem da loja para que ele usa o novo nome, vá para a [página de listagem de loja](create-app-store-listings.md) para o idioma e selecione o nome na lista suspensa de **nome do produto** . Certifique-se de revisar a descrição e outras partes da listagem para qualquer citações do nome e fazer atualizações, se necessário.

> [!NOTE]
> Se seu aplicativo tiver pacotes e/ou listagens da loja em vários idiomas, você precisará atualizar os pacotes e/ou armazenamento listagens para cada idioma em que o nome precisa ser atualizado.

Quando seu aplicativo tiver sido publicado com o novo nome, você pode excluir todos os nomes mais antigos que você não precisa mais usar.

> [!TIP]
> Cada aplicativo aparecerá no Partner Center usando o nome que você reservou para ele. Se você tiver seguido as etapas acima para renomear um aplicativo, e você quiser que ele seja exibido no Partner Center usando o novo nome, você deve excluir o nome original (clicando em **Excluir** na página **Gerenciar nomes de aplicativo** ). 

 

 




