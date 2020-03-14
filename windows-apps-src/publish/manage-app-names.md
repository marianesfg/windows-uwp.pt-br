---
Description: Exiba os nomes que você já reservou para seu aplicativo, reserve nomes adicionais (para outros idiomas ou para alterar o nome do aplicativo) e exclua nomes reservados de que você não precisa mais.
title: Gerenciar nomes de aplicativo
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP, nomes de aplicativos, alterar nome do aplicativo, atualizar nome do aplicativo, nome do jogo, nome do produto
ms.localizationpriority: medium
ms.openlocfilehash: 38cedf40d4ecf997f6fbced2186cd5b27c6d5e4f
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210352"
---
# <a name="manage-app-names"></a>Gerenciar nomes de aplicativo

O **gerenciar nomes de aplicativos** permite exibir todos os nomes que você reservou para seu aplicativo, reservar nomes adicionais (para outros idiomas ou alterar o nome do aplicativo) e excluir nomes desnecessários. Você pode encontrar essa página no [Partner Center](https://partner.microsoft.com/dashboard) expandindo a seção **Gerenciamento de aplicativos** no menu de navegação à esquerda para qualquer um de seus aplicativos.

> [!IMPORTANT]
> Você pode reservar nomes adicionais para um aplicativo e pode optar por usar um deles na versão publicada do seu aplicativo, em vez do que você reservou quando criou o aplicativo pela primeira vez no Partner Center. No entanto, lembre-se de que o primeiro nome que você reserva para o produto será usado em alguns dos [detalhes de identidade](view-app-identity-details.md), como o **nome da família de pacotes (PFN)** . Esses valores podem ser visíveis para alguns usuários e não podem ser alterados, portanto certifique-se de que o nome reservado primeiro seja apropriado para esse uso.


## <a name="reserve-additional-names-for-your-app"></a>Reservar nomes adicionais para seu aplicativo

Você pode reservar vários nomes de aplicativo para usar no mesmo aplicativo. Isso é especialmente útil se você estiver oferecendo seu aplicativo em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode reservar um novo nome para alterar o nome de um aplicativo, conforme descrito abaixo.

Para reservar um novo nome de aplicativo, localize a caixa de texto na seção **reservar mais nomes** da página **gerenciar nomes de aplicativos** . Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome do produto**. Você pode reservar vários nomes de aplicativo repetindo essas etapas, se desejar.

> [!NOTE]
> Para saber mais sobre como reservar nomes de aplicativo, e por que um determinado nome pode não estar disponível, consulte [Criar seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Excluir nomes de aplicativo

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu aplicativo, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que seu aplicativo deve ter pelo menos um nome reservado. Para remover completamente um aplicativo do Partner Center (e liberar todos os nomes que você reservou para esse aplicativo), clique em **excluir este aplicativo** na página **visão geral do aplicativo** . Se você tiver um envio do aplicativo em andamento, é necessário excluir o envio primeiro. Observe que, se você já publicou o aplicativo na loja, não poderá excluí-lo do Partner Center (embora você possa usar a funcionalidade **Mostrar/ocultar produtos** na página de **visão geral** para ocultá-lo). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renomear um aplicativo que já foi publicado

Se o aplicativo já está na Loja e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o aplicativo. 

Você deve atualizar os pacotes do seu aplicativo para substituir o nome antigo pelo novo e carregar os pacotes atualizados para o seu envio.
- Primeiro, atualize o arquivo Package. StoreAssociation. xml para usar o novo nome, seja manualmente ou usando o Visual Studio (**Project > Store > associar o aplicativo à Store...** ). Para obter mais informações, consulte [empacotar um aplicativo UWP com o Visual Studio](/windows/msix/package/packaging-uwp-apps).
- Também é necessário atualizar o elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) no manifesto do aplicativo e atualize elementos gráficos ou texto que inclua o nome do aplicativo. 
  > [!IMPORTANT]
  > Certifique-se de atualizar o arquivo Package.StoreAssociation.xml antes de alterar **Pacote/Propriedades/DisplayName** no aplicativo do manifesto ou você poderá receber um erro.

Para atualizar uma listagem de repositório para que ela use o novo nome, vá para a [página de listagem de armazenamento](create-app-store-listings.md) para esse idioma e selecione o nome na lista suspensa nome do **produto** . Certifique-se de examinar sua descrição e outras partes da listagem em busca de qualquer menção do nome e fazer atualizações, se necessário.

> [!NOTE]
> Se seu aplicativo tiver pacotes e/ou armazenar listagens em vários idiomas, você precisará atualizar os pacotes e/ou armazenar as listagens para cada idioma no qual o nome precisa ser atualizado.

Depois que o aplicativo tiver sido publicado com o novo nome, você poderá excluir os nomes mais antigos que não precisa mais usar.

> [!TIP]
> Cada aplicativo aparece no Partner Center usando o primeiro nome que você reservou para ele. Se você seguiu as etapas acima para renomear um aplicativo e deseja que ele apareça no Partner Center usando o novo nome, você deve excluir o nome original (clicando em **excluir** na página **gerenciar nomes de aplicativo** ). 

 

 




