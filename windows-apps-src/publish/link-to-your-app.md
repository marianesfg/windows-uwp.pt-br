---
Description: Você pode ajudar os clientes descobrir seu aplicativo por meio da vinculação a listagem do seu aplicativo em que a Microsoft Store.
title: Link para seu aplicativo
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, link, protocolo da windows store, vincular a um aplicativo, vincular ao aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: 56bc051c3c5a935f3b6b26e478731fcde9c06902
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661531"
---
# <a name="link-to-your-app"></a>Link para seu aplicativo


Você pode ajudar os clientes descobrir seu aplicativo por meio da vinculação a listagem do seu aplicativo em que a Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtendo o link para os detalhes do seu aplicativo na Loja

Para obter a URL da listagem da Loja do aplicativo, navegue até a página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo**. A URL está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Quando um cliente clicar nesse link, ele abre a página de listagem baseada na Web do aplicativo. Em dispositivos Windows, o aplicativo da Loja também inicia e exiba a listagem do aplicativo.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vinculando a listagem de Store do seu aplicativo com o selo do Microsoft Store

Você pode vincular diretamente para a listagem do seu aplicativo com uma notificação personalizada para que clientes saibam que seu aplicativo estiver em que a Microsoft Store.

Para criar seu selo, visite o [de notificações do Microsoft Store](https://go.microsoft.com/fwlink/p/?LinkID=534236) página. Você precisará ter a **ID da Loja** de 12 caracteres do aplicativo para usar esse formulário, além de gerar o selo e o link. Você pode encontrar a **ID da Loja** do aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento do aplicativo**.

> [!NOTE]
> Ver [diretrizes de marketing no aplicativo](app-marketing-guidelines.md) para informações e requisitos relacionados ao uso do selo do Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincular diretamente ao seu aplicativo em que a Microsoft Store

Você pode criar um link que inicia o Microsoft Store e vai diretamente para a página de listagem do seu aplicativo sem abrir um navegador usando o **ms-windows-store:** Esquema de URI.

Esses links são úteis quando você sabe que os usuários estão em um dispositivo Windows e você deseja que eles sejam encaminhados diretamente para a página de listagem da Loja. Por exemplo, você pode usar o link depois de verificar as cadeias de caracteres de agente do usuário em um navegador para confirmar se o sistema operacional do usuário oferece suporte à Loja quando você já está se comunicando por um aplicativo UWP.

Para usar este esquema URI para vincular diretamente à Store do seu aplicativo listando, acrescente a ID do aplicativo Store este link:

`ms-windows-store://pdp/?ProductId=`

Para obter mais informações sobre como usar o protocolo da Microsoft Store, consulte [inicie o aplicativo Microsoft](../launch-resume/launch-store-app.md).

 

 




