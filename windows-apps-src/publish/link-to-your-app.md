---
Description: Você pode ajudar os clientes a descobrir seu aplicativo vinculando-se à listagem do seu aplicativo na Microsoft Store.
title: Link para seu aplicativo
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, link, protocolo da windows store, vincular a um aplicativo, vincular ao aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: de22505cf42193932a5bbd951c983e02eea37bd7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259983"
---
# <a name="link-to-your-app"></a>Link para seu aplicativo


Você pode ajudar os clientes a descobrir seu aplicativo vinculando-se à listagem do seu aplicativo na Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtendo o link para os detalhes do seu aplicativo na Loja

Para obter a URL da listagem da Loja do aplicativo, navegue até a página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo**. A URL está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Quando um cliente clicar nesse link, ele abre a página de listagem baseada na Web do aplicativo. Em dispositivos Windows, o aplicativo da Loja também inicia e exiba a listagem do aplicativo.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vinculando à listagem de repositório do aplicativo com a notificação de Microsoft Store

Você pode vincular diretamente à listagem do seu aplicativo com um selo personalizado para permitir que os clientes saibam que seu aplicativo está na Microsoft Store.

Para criar seu crachá, visite a página [Microsoft Store crachás](https://developer.microsoft.com/store/badges) . Você precisará ter a **ID da Loja** de 12 caracteres do aplicativo para usar esse formulário, além de gerar o selo e o link. Você pode encontrar a **ID da Loja** do aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento do aplicativo**.

> [!NOTE]
> Consulte as [diretrizes de marketing do aplicativo](app-marketing-guidelines.md) para obter informações e requisitos relacionados ao uso da notificação de Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vinculando diretamente ao seu aplicativo no Microsoft Store

Você pode criar um link que inicia o Microsoft Store e vai diretamente para a página de listagem do seu aplicativo sem abrir um navegador usando o esquema **MS-Windows-Store:** URI.

Esses links são úteis quando você sabe que os usuários estão em um dispositivo Windows e você deseja que eles sejam encaminhados diretamente para a página de listagem da Loja. Por exemplo, você pode usar o link depois de verificar as cadeias de caracteres de agente do usuário em um navegador para confirmar se o sistema operacional do usuário oferece suporte à Loja quando você já está se comunicando por um aplicativo UWP.

Para usar este esquema de URI para vincular diretamente à listagem de armazenamento do seu aplicativo, acrescente a ID de loja do aplicativo a este link:

`ms-windows-store://pdp/?ProductId=`

Para obter mais informações sobre como usar o protocolo Microsoft Store, consulte [iniciar o aplicativo Microsoft](../launch-resume/launch-store-app.md).

 

 




