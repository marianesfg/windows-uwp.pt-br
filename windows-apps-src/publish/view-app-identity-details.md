---
Description: Exibir detalhes relacionados à identidade exclusiva atribuída ao seu aplicativo, a Microsoft Store e obter um link para a listagem de Store do seu aplicativo.
title: Exibir detalhes de identidade do aplicativo
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: acc39dfecaf4360f9f5b7db9f3c1fe34021f35c0
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788222"
---
# <a name="view-app-identity-details"></a>Exibir detalhes de identidade do aplicativo


Você pode exibir os detalhes relacionados à identidade exclusiva atribuída ao seu aplicativo, a Microsoft Store na sua **identidade de aplicativo** páginas. Você também pode obter um link para Store seu aplicativo listando nesta página.

Para encontrar essas informações, navegue para um dos seus aplicativos, depois expanda **Gerenciamento de aplicativo** no menu de navegação esquerdo. Selecione a **Identidade do aplicativo** para exibir esses detalhes.


## <a name="values-to-include-in-your-app-package-manifest"></a>Valores a serem incluídos no manifesto do pacote do aplicativo

Os valores a seguir devem ser incluídos em seu manifesto de pacote. Se você [usar o Microsoft Visual Studio para criar seus pacotes](../packaging/packaging-uwp-apps.md), e estiver conectado à mesma conta da Microsoft que você associou com sua conta de desenvolvedor, esses detalhes serão incluídos automaticamente. Se você estiver criando o pacote manualmente, é necessário adicionar estes itens.

-   **Nome/de identidade de pacote**
-   **Identidade/pacote/publicador**
-   **Package/Properties/PublisherDisplayName**

Para saber mais, consulte [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) na [Referência de esquemas de manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Juntos, esses elementos declaram a identidade do seu aplicativo, estabelecendo a "família do pacote" a qual todos os seus pacotes pertencem. Pacotes individuais terão detalhes adicionais, como arquitetura e versão.


## <a name="additional-values-for-package-family"></a>Valores adicionais de família de pacote

Os valores seguintes são valores adicionais que se referem à família de pacote do seu aplicativo, mas que não estão incluídos em seu manifesto.

-   **Nome de família (PFN) do pacote**: Esse valor é usado com determinadas APIs do Windows.
-   **SID de pacote**: Você precisará desse valor para enviar notificações do WNS para seu aplicativo. Para saber mais, consulte a [Visão geral do WNS (Serviços de Notificação por Push do Windows)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Link com os detalhes do seu aplicativo

O link direto para a página do seu aplicativo pode ser compartilhado para ajudar os clientes a encontrar o aplicativo na Loja. Este link está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**. Quando um cliente clicar nesse link, ele abre a página de listagem baseada na Web do aplicativo. Em dispositivos Windows, o aplicativo da Loja também inicia e exiba a listagem do aplicativo.

O **ID da Loja** de seu aplicativo também é mostrado nesta seção. Esse ID da Loja pode ser usado para [gerar notificações da Loja](https://go.microsoft.com/fwlink/p/?LinkId=534236) ou identificar seu aplicativo de outra forma.

O **link de protocolo da Loja** pode ser usado para vincular diretamente ao seu aplicativo na Loja sem abrir um navegador, como quando você vincula em um aplicativo. Para saber mais, consulte [Link para o aplicativo](link-to-your-app.md).



 

 




