---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: Link para seu app
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, link, protocolo da windows store, vincular a um aplicativo, vincular ao aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: d0d0a9fc862734611167e5118392126cb42687f2
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6268273"
---
# <a name="link-to-your-app"></a>Link para seu app


Você pode ajudar os clientes a descobrir seu aplicativo vinculando aos detalhes do seu aplicativo na Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtendo o link para os detalhes do seu aplicativo na Loja

Para obter a URL da listagem da Loja do aplicativo, navegue até a página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo**. A URL está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Quando um cliente clicar nesse link, ele abre a página de listagem baseada na Web do aplicativo. Em dispositivos Windows, o aplicativo da Loja também inicia e exiba a listagem do aplicativo.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vinculação da listagem da Store do seu aplicativo com o selo da Microsoft Store

Você pode vincular diretamente aos detalhes do seu aplicativo com um selo personalizado para permitir que os clientes saibam que seu aplicativo está na Microsoft Store.

Para criar o selo, visite a página de [selos da Microsoft Store](http://go.microsoft.com/fwlink/p/?LinkID=534236) . Você precisará ter a **ID da Loja** de 12 caracteres do aplicativo para usar esse formulário, além de gerar o selo e o link. Você pode encontrar a **ID da Loja** do aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento do aplicativo**.

> [!NOTE]
> Consulte [diretrizes de marketing de aplicativo](app-marketing-guidelines.md) para obter informações e requisitos relacionados ao uso do selo Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincular diretamente ao seu aplicativo na Microsoft Store

Você pode criar um link que inicia o Microsoft Store e vai diretamente para a página de listagem do seu aplicativo sem abrir um navegador usando o **ms-windows-store:** esquema de URI.

Esses links são úteis quando você sabe que os usuários estão em um dispositivo Windows e você deseja que eles sejam encaminhados diretamente para a página de listagem da Loja. Por exemplo, você pode usar o link depois de verificar as cadeias de caracteres de agente do usuário em um navegador para confirmar se o sistema operacional do usuário oferece suporte à Loja quando você já está se comunicando por um aplicativo UWP.

Para usar esse esquema de URI para vincular diretamente à listagem da loja do seu aplicativo, acrescente a ID da loja do aplicativo a este link:

`ms-windows-store://pdp/?ProductId=`

Para obter mais informações sobre como usar o protocolo de Microsoft Store, consulte [Iniciar o aplicativo da Microsoft](../launch-resume/launch-store-app.md).

 

 




