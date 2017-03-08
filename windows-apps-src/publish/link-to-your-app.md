---
author: jnHs
Description: "Você pode ajudar os clientes a descobrir seu app vinculando aos detalhes do app na Loja."
title: Link para seu app
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 59f19dbf0cd66679805a9fcf3054427a22fb0e8f
ms.lasthandoff: 02/07/2017

---

# <a name="link-to-your-app"></a>Link para seu app


Você pode ajudar os clientes a descobrir seu app vinculando aos detalhes do app na Loja.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtendo o link para os detalhes do seu app na Loja


Você pode encontrar o link para os detalhes do seu app na Loja na página [Identidade do app](view-app-identity-details.md), na seção **Gerenciamento de app** de cada app em seu painel.

Este link está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**

Quando um cliente clicar nesse link, ele abrirá a página de listagem baseada na web do seu app. Se o app estiver disponível para o dispositivo do cliente, o app da Loja também será iniciado e exibirá a listagem do app.

> **Observação**  Dependendo das versões de sistema operacional que você almeja, pode haver mais de um link aqui. Todos os apps mostrarão a URL para Windows 10, que funcionará em qualquer sistema operacional. Você pode ver links adicionais para o Windows 8.1 e versões anteriores e/ou Windows Phone 8.1 e versões anteriores, que funcionarão somente nas versões de sistema operacional especificadas.

 

## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>Vinculando aos detalhes de seu app na Loja com o selo da Windows Store


Você pode vincular diretamente aos detalhes do seu app com um selo personalizado para que os clientes saibam que seu app está na Windows Store.

Para criar o selo, visite a página [Selos da Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=534236). Você precisará ter a ID da Loja do seu app para usar esse formulário e gerar o selo e o link. Esse ID é formado pelos 12 últimos caracteres da **URL para Windows 10** mostrada na página [Identidade do app](view-app-identity-details.md) na seção **Gerenciamento de app**.

> **Observação**  Consulte [Diretrizes para marketing de apps](app-marketing-guidelines.md) para saber mais sobre como usar o selo da Windows Store.

 

## <a name="linking-directly-to-your-app-in-the-windows-store"></a>Criando um link direto para seu app na Windows Store


Você pode criar um link que inicia a Windows Store e vai diretamente até a página de listagem do seu app sem abrir um navegador usando o esquema de URI **ms-windows-store:**.

Esses links são úteis quando você sabe que os usuários estão em um dispositivo Windows e deseja que ele chegue diretamente na página de detalhes na Loja, por exemplo, após a verificação de cadeias de caracteres de agente de usuário em um navegador para confirmar o sistema operacional do usuário, ou quando você já está se comunicando via app UWP, talvez você queira aplicar esse protocolo.

Para usar o protocolo da Windows Store e vincular diretamente à listagem da Loja do seu app, acrescente a ID da Loja do app a este link:

`ms-windows-store://pdp/?ProductId=`

Para obter mais informações sobre como usar o protocolo da Windows Store, consulte [Iniciar o aplicativo da Windows Store](../launch-resume/launch-store-app.md).

 

 





