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
keywords: Windows 10, uwp, nomes de aplicativo, altere o nome do aplicativo, o nome do aplicativo de atualização, o nome de jogo, o nome do produto
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2829902"
---
# <a name="manage-app-names"></a>Gerenciar nomes de aplicativo

O permite **Gerenciar nomes de aplicativo** que você exibir todos os nomes que você tiver reservada para seu aplicativo, reservar nomes adicionais (para outros idiomas ou alterar o nome do seu aplicativo) e exclua os nomes não é necessário. Você pode encontrar essa página no [Painel de controle do Windows Dev Center](https://partner.microsoft.com/dashboard) expandindo a seção de **gerenciamento de aplicativo** , no menu de navegação à esquerda para qualquer um dos seus aplicativos.


## <a name="reserve-additional-names-for-your-app"></a>Reservar nomes adicionais para seu aplicativo

Você pode reservar vários nomes de aplicativo para usar no mesmo aplicativo. Isso é especialmente útil se você estiver oferecendo seu aplicativo em vários idiomas e quiser usar nomes diferentes para diferentes idiomas. Você também pode reservar um novo nome para alterar o nome de um aplicativo, conforme descrito abaixo.

Para reservar um novo nome de aplicativo, encontre a caixa de texto na seção **reservar mais nomes** da página **Gerenciar os nomes de aplicativo** . Digite o nome que você gostaria de reservar e, em seguida, clique em **Verificar disponibilidade**. Se o nome estiver disponível, clique em **Reservar nome do produto**. Você pode reservar vários nomes de aplicativo, repetindo essas etapas, se desejado.

> [!NOTE]
> Para saber mais sobre como reservar nomes de aplicativo, e por que um determinado nome pode não estar disponível, consulte [Criar seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Excluir nomes de aplicativo

Se você não quiser usar um nome que já reservou anteriormente, libere-o, excluindo-o aqui. Tenha certeza de você que fazer isso, já que depois disso o nome se tornará imediatamente disponível para outra pessoa reservar e usar.

Para excluir um dos nomes reservados do seu aplicativo, encontre o nome que você não deseja mais usar e, em seguida, clique em **Excluir**. Na caixa de diálogo de confirmação, clique em **Excluir** novamente para confirmar.

Observe que o seu aplicativo deve ter pelo menos um nome reservado. Para completamente remova um aplicativo de seu painel (e liberar todos os nomes que você tiver reservado para esse aplicativo), clique em **Excluir este aplicativo** da página de **Visão geral de aplicativo** . Se você tiver um envio do aplicativo em andamento, é necessário excluir o envio primeiro. Observe que se você já tiver publicado o aplicativo para o repositório, você não é possível excluí-lo do seu painel (embora você pode usar a funcionalidade de **produtos de Mostrar/ocultar** na sua página de **Visão geral** para ocultá-lo). 


## <a name="rename-an-app-that-has-already-been-published"></a>Renomear um aplicativo já publicado

Se o aplicativo já está na Loja e você deseja renomeá-lo, é possível fazer isso reservando um novo nome para ele (seguindo as etapas descritas acima) e, em seguida, criando um novo envio para o aplicativo. 

Você deve atualizar pacote (s) do seu aplicativo para substituir o nome antigo pelo novo e carregar o pacote atualizado (s) ao envio.
- Primeiro, atualize o arquivo de Package.StoreAssociation.xml para usar o novo nome, seja manualmente ou usando o Visual Studio (**Project > repositório > App associar com o repositório …**). Para obter mais informações, consulte o [pacote de um aplicativo UWP com o Visual Studio](../packaging/packaging-uwp-apps.md).
- Também é necessário atualizar o elemento [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) no manifesto do aplicativo e atualize elementos gráficos ou texto que inclua o nome do aplicativo. 
  > [!IMPORTANT]
  > Certifique-se de atualizar o arquivo Package.StoreAssociation.xml antes de alterar **Pacote/Propriedades/DisplayName** no aplicativo do manifesto ou você poderá receber um erro.

Para atualizar uma listagem de repositório para que ele usa o novo nome, vá para a [página de listagem de repositório](create-app-store-listings.md) para o idioma e selecione o nome na lista suspensa **nome do produto** . Certifique-se de que revise a descrição e outras partes da listagem para qualquer menções do nome e faça atualizações, se necessário.

> [!NOTE]
> Se seu aplicativo tiver pacotes e/ou listagens de armazenamento em vários idiomas, você precisará atualizar os pacotes e/ou armazenar listagens para cada idioma no qual o nome precisa ser atualizado.

Depois que o seu aplicativo tiver sido publicado com o novo nome, você pode excluir quaisquer nomes mais antigos que não precisar mais usar.

> [!TIP]
> Cada aplicativo aparece no seu painel usando o nome que você reservada para ela. Se você ter seguido as etapas acima para renomear um aplicativo, e você gostaria que ele apareça em seu painel usando o novo nome, você deve excluir o nome original (clicando em **Excluir** na página **Gerenciar aplicativo nomes** ). 

 

 




