---
Description: Exiba os nomes que você já reservou para seu aplicativo, reserve nomes adicionais (para outros idiomas ou para alterar o nome do aplicativo) e exclua nomes reservados de que você não precisa mais.
title: Gerenciar nomes de aplicativo
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: o Windows 10, uwp, nomes de aplicativo, altere o nome do aplicativo, o nome do aplicativo de atualização, o nome do jogo, nome do produto
ms.localizationpriority: medium
ms.openlocfilehash: 786bfd7cbb4129274a2ff19bc33084824d296bcd
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63827518"
---
# <a name="manage-app-names"></a>Gerenciar nomes de aplicativo

O **gerenciar nomes de aplicativo** permite exibir todos os nomes que você reservou para seu aplicativo, reservar nomes adicionais (para outras linguagens ou para alterar o nome do seu aplicativo) e exclua os nomes que você não precisa. Você pode encontrar essa página no [Partner Center](https://partner.microsoft.com/dashboard) expandindo o **gerenciamento de aplicativo** seção no menu de navegação à esquerda de qualquer um dos seus aplicativos.

> [!IMPORTANT]
> Você pode reservar nomes adicionais para um aplicativo, e você pode optar por usar uma da versão publicada do seu aplicativo em vez daquele que você reservou quando criou seu aplicativo pela primeira vez no Partner Center. No entanto, lembre-se de que o primeiro nome reservado para o seu produto será usado em algumas de sua equipe de TI da [detalhes de identidade](view-app-identity-details.md), como o **nome da família de pacotes (PFN)**. Esses valores podem ser visíveis para alguns usuários e não pode ser alterado, portanto, certifique-se de que o nome que você pode reservar primeiro é apropriado para esse uso.


## <a name="reserve-additional-names-for-your-app"></a>Reservar nomes adicionais para seu aplicativo

Você pode reservar vários nomes de aplicativo para usar no mesmo aplicativo. Isso é especialmente útil se você estiver oferecendo seu aplicativo em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode reservar um novo nome para alterar o nome de um aplicativo, conforme descrito abaixo.

Para reservar um novo nome de aplicativo, localize a caixa de texto na **reservar nomes mais** seção o **gerenciar nomes de aplicativo** página. Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome do produto**. Você pode reservar vários nomes de aplicativo, repetindo essas etapas, se desejado.

> [!NOTE]
> Para saber mais sobre como reservar nomes de aplicativo, e por que um determinado nome pode não estar disponível, consulte [Criar seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Excluir nomes de aplicativo

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu aplicativo, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que seu aplicativo deve ter pelo menos um nome reservado. Para completamente remover um aplicativo do Partner Center (e todos os nomes que você reservou para o aplicativo de versão), clique em **excluir este aplicativo** da **visão geral do aplicativo** página. Se você tiver um envio do aplicativo em andamento, é necessário excluir o envio primeiro. Observe que, se você já publicou o aplicativo para a Store, você não pode excluí-lo do Partner Center (embora você possa usar o **produtos Mostrar/ocultar** funcionalidades em seus **visão geral** página para ocultá-la). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renomear um aplicativo que já foi publicado

Se o aplicativo já está na Loja e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o aplicativo. 

Você deve atualizar o pacote (s) de seu aplicativo para substituir o nome antigo pelo novo e carregar o pacote atualizado (s) seu envio.
- Primeiro, atualize o arquivo Package.StoreAssociation.xml para usar o novo nome, manualmente ou usando o Visual Studio (**Projeto > Loja > Associar aplicativo à Loja...**). Para obter mais informações, consulte [empacotar um aplicativo UWP com o Visual Studio](../packaging/packaging-uwp-apps.md).
- Também é necessário atualizar o elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) no manifesto do aplicativo e atualize elementos gráficos ou texto que inclua o nome do aplicativo. 
  > [!IMPORTANT]
  > Certifique-se de atualizar o arquivo Package.StoreAssociation.xml antes de alterar **Pacote/Propriedades/DisplayName** no aplicativo do manifesto ou você poderá receber um erro.

Para atualizar uma listagem da Store para que ele use o novo nome, vá para o [página de listagem de Store](create-app-store-listings.md) para o idioma e selecione o nome da **nome do produto** lista suspensa. Certifique-se de examinar a descrição e outras partes da listagem de qualquer menções do nome e fazer atualizações, se necessário.

> [!NOTE]
> Se seu aplicativo tiver pacotes e/ou em listagens de Store em vários idiomas, você precisará atualizar os pacotes e/ou Store listagens para cada idioma no qual o nome precisa ser atualizado.

Depois que seu aplicativo tiver sido publicado com o novo nome, você pode excluir todos os nomes mais antigos que você não precisa mais usar.

> [!TIP]
> Cada aplicativo aparece no Partner Center usando o nome que você reservou para ele. Se você tiver seguido as etapas acima para renomear um aplicativo, e você gostaria que ele apareça no Partner Center usando o novo nome, você deve excluir o nome original (clicando **excluir** sobre o **gerenciar nomes de aplicativo** página). 

 

 




