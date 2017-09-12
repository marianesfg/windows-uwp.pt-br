---
author: jnHs
Description: "Você pode ajudar os clientes a descobrir seu aplicativo vinculando aos detalhes do aplicativo na Loja."
title: Link para seu app
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, link, protocolo da windows store, vincular a um aplicativo, vincular ao aplicativo
ms.openlocfilehash: 2d0750493926937a6326c5f72f568d4294b137c5
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2017
---
# <a name="link-to-your-app"></a>Link para seu app


Você pode ajudar os clientes a descobrir seu aplicativo vinculando aos detalhes do aplicativo na Loja.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtendo o link para os detalhes do seu aplicativo na Loja

Para obter a URL da listagem da Loja do aplicativo, navegue até a página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo**. A URL está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Quando um cliente clicar nesse link, ele abre a página de listagem baseada na Web do aplicativo. Em dispositivos Windows, o aplicativo da Loja também inicia e exiba a listagem do aplicativo.


## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>Vinculando aos detalhes de seu aplicativo na Loja com o selo da Windows Store

Você pode vincular diretamente aos detalhes do seu aplicativo com um selo personalizado para que os clientes saibam que seu aplicativo está na Windows Store.

Para criar o selo, visite a página [Selos da Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=534236). Você precisará ter a **ID da Loja** de 12 caracteres do aplicativo para usar esse formulário, além de gerar o selo e o link. Você pode encontrar a **ID da Loja** do aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento do aplicativo**.

> [!NOTE]
> Consulte [Diretrizes para marketing dos aplicativos](app-marketing-guidelines.md) para obter informações e requisitos relacionados ao uso do selo da Windows Store.


## <a name="linking-directly-to-your-app-in-the-windows-store"></a>Criando um link direto para seu aplicativo na Windows Store

Você pode criar um link que inicia a Windows Store e vai diretamente até a página de listagem do seu aplicativo sem abrir um navegador usando o esquema de URI **ms-windows-store:**.

Esses links são úteis quando você sabe que os usuários estão em um dispositivo Windows e você deseja que eles sejam encaminhados diretamente para a página de listagem da Loja. Por exemplo, você pode usar o link depois de verificar as cadeias de caracteres de agente do usuário em um navegador para confirmar se o sistema operacional do usuário oferece suporte à Loja quando você já está se comunicando por um aplicativo UWP.

Para usar o protocolo da Windows Store e vincular diretamente à listagem da Loja do seu aplicativo, acrescente a ID da Loja do aplicativo a este link:

`ms-windows-store://pdp/?ProductId=`

Para obter mais informações sobre como usar o protocolo da Windows Store, consulte [Iniciar o aplicativo da Windows Store](../launch-resume/launch-store-app.md).

 

 




