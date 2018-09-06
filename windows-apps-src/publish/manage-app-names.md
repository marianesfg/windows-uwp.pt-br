---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: Gerenciar nomes de aplicativo
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, nomes de aplicativo, altere o nome do aplicativo, nome do aplicativo de atualização, nome do jogo, nome do produto
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3376172"
---
# <a name="manage-app-names"></a>Gerenciar nomes de aplicativo

O permite **Gerenciar nomes de aplicativo** exibir todos os nomes que você tenha reservado para seu aplicativo, reservar outros nomes (para outros idiomas ou para alterar o nome do aplicativo) e excluir nomes não é necessário. Você pode encontrar essa página no [Painel de controle do Windows Dev Center](https://partner.microsoft.com/dashboard) , expandindo a seção **gerenciamento de aplicativos** no menu de navegação à esquerda para qualquer um dos seus aplicativos.


## <a name="reserve-additional-names-for-your-app"></a>Reservar nomes adicionais para seu aplicativo

Você pode reservar vários nomes de aplicativo para usar no mesmo aplicativo. Isso é especialmente útil se você estiver oferecendo seu aplicativo em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode reservar um novo nome para alterar o nome de um aplicativo, conforme descrito abaixo.

Para reservar um novo nome de aplicativo, localize a caixa de texto na seção **reservar mais nomes** da página **Gerenciar nomes de aplicativo** . Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome do produto**. Você pode reservar vários nomes de aplicativo repetindo essas etapas, se desejado.

> [!NOTE]
> Para saber mais sobre como reservar nomes de aplicativo, e por que um determinado nome pode não estar disponível, consulte [Criar seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Excluir nomes de aplicativo

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu aplicativo, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que seu aplicativo deve ter pelo menos um nome reservado. Para remover completamente um aplicativo de painel, (e liberar todos os nomes que você tenha reservado para que o aplicativo), clique em **Excluir este aplicativo** na página **Visão geral do aplicativo** . Se você tiver um envio do aplicativo em andamento, é necessário excluir o envio primeiro. Observe que se o aplicativo já publicadas no armazenamento, você não pode excluí-lo do painel (que você pode usar a funcionalidade de **Mostrar/ocultar produtos** em sua página de **Visão geral** para ocultá-la). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renomear um aplicativo já publicado

Se o aplicativo já está na Loja e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o aplicativo. 

Você deve atualizar o pacote do aplicativo para substituir o nome antigo por um novo e carregue o pacote atualizado (s) ao seu envio.
- Primeiro, atualizar o arquivo Package.StoreAssociation.xml para usar o novo nome, seja manualmente ou usando o Visual Studio (**projeto > armazenamento > associar o aplicativo com o armazenamento...**). Para obter mais informações, consulte [pacote um aplicativo UWP com Visual Studio](../packaging/packaging-uwp-apps.md).
- Também é necessário atualizar o elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) no manifesto do aplicativo e atualize elementos gráficos ou texto que inclua o nome do aplicativo. 
  > [!IMPORTANT]
  > Certifique-se de atualizar o arquivo Package.StoreAssociation.xml antes de alterar **Pacote/Propriedades/DisplayName** no aplicativo do manifesto ou você poderá receber um erro.

Para atualizar uma listagem de armazenamento para que possa usar o novo nome, vá para a [página de listagem de armazenamento](create-app-store-listings.md) para esse idioma e selecione o nome na lista suspensa **nome do produto** . Certifique-se de examinar a descrição e outras partes da listagem para qualquer menção do nome e faça as atualizações, se necessário.

> [!NOTE]
> Se seu aplicativo tiver pacotes e/ou listagens de armazenamento em vários idiomas, você precisará atualizar os pacotes e/ou armazenar listagens para cada idioma em que o nome precisa ser atualizado.

Quando seu aplicativo tiver sido publicado com o novo nome, você pode excluir os nomes antigos que você não precisa usar.

> [!TIP]
> Cada aplicativo é exibida no painel usando o nome que você reservou para ele. Se você tiver seguido as etapas acima para renomear um aplicativo e você deseja que ele apareça no painel usando o novo nome, você deve excluir o nome original (clicando em **Excluir** na página **Gerenciar aplicativo nomes** ). 

 

 




