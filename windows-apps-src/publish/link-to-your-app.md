---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: Link para seu app
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, link, protocolo da windows store, vincular a um aplicativo, vincular ao aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: 0025321aa73a66cc0a976bd347e613de3c3c4765
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862267"
---
# <a name="link-to-your-app"></a>Link para seu app


Você pode ajudar os clientes descubram o seu aplicativo vinculando a listagem do seu aplicativo em que o Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtendo o link para os detalhes do seu aplicativo na Loja

Para obter a URL da listagem da Loja do aplicativo, navegue até a página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo**. A URL está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Quando um cliente clicar nesse link, ele abre a página de listagem baseada na Web do aplicativo. Em dispositivos Windows, o aplicativo da Loja também inicia e exiba a listagem do aplicativo.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vinculando a listagem de repositório do seu aplicativo com o emblema Microsoft Store

Você pode vincular diretamente a listagem do seu aplicativo com um emblema personalizado que permitem aos clientes souber que seu aplicativo é o Microsoft Store.

Para criar seu crachá, visite a página de [credenciais de repositório do Microsoft](http://go.microsoft.com/fwlink/p/?LinkID=534236) . Você precisará ter a **ID da Loja** de 12 caracteres do aplicativo para usar esse formulário, além de gerar o selo e o link. Você pode encontrar a **ID da Loja** do aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento do aplicativo**.

> [!NOTE]
> Consulte [as diretrizes de marketing App](app-marketing-guidelines.md) for info e requisitos relacionados ao uso do crachá Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vinculando diretamente para seu aplicativo no repositório do Microsoft

Você pode criar um link que inicia o Microsoft Store e vai diretamente para a página de listagem do seu aplicativo sem abrindo um navegador usando o **ms-windows-store:** esquema URI.

Esses links são úteis quando você sabe que os usuários estão em um dispositivo Windows e você deseja que eles sejam encaminhados diretamente para a página de listagem da Loja. Por exemplo, você pode usar o link depois de verificar as cadeias de caracteres de agente do usuário em um navegador para confirmar se o sistema operacional do usuário oferece suporte à Loja quando você já está se comunicando por um aplicativo UWP.

Para usar o esquema de URI para vincular diretamente para o repositório do seu aplicativo listando, acrescente ID do repositório do seu aplicativo neste link:

`ms-windows-store://pdp/?ProductId=`

Para obter mais informações sobre como usar o protocolo Microsoft Store, consulte [Iniciar o Microsoft app](../launch-resume/launch-store-app.md).

 

 




