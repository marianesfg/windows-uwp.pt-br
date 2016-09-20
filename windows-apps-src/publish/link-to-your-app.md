---
author: jnHs
Description: "Você pode ajudar os clientes a descobrir seu aplicativo vinculando aos detalhes do aplicativo na Loja."
title: Link para seu aplicativo
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.sourcegitcommit: d36f14604bd8c2ce0d5778a67f5b5b9460d9fbf3
ms.openlocfilehash: 891b99b52f7e3b347c0e2f9d298f144313cc7b2d

---

# Link para seu aplicativo


Você pode ajudar os clientes a descobrir seu aplicativo vinculando aos detalhes do aplicativo na Loja.

## Obtendo o link para os detalhes do seu aplicativo na Loja


Você pode encontrar o link para os detalhes do seu aplicativo na Loja na página [Identidade do aplicativo](view-app-identity-details.md), na seção **Gerenciamento de aplicativo** de cada aplicativo em seu painel.

Este link está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**

Quando um cliente clicar nesse link, ele abrirá a página de listagem baseada na web do seu aplicativo. Se o aplicativo estiver disponível para o dispositivo do cliente, o aplicativo da Loja também será iniciado e exibirá a listagem do aplicativo.

> 
            **Observação**  Dependendo das versões de sistema operacional que você almeja, pode haver mais de um link aqui. Todos os aplicativos mostrarão a URL para Windows 10, que funcionará em qualquer sistema operacional. Você pode ver links adicionais para o Windows 8.1 e versões anteriores e/ou Windows Phone 8.1 e versões anteriores, que funcionarão somente nas versões de sistema operacional especificadas.

 

## Vinculando aos detalhes de seu aplicativo na Loja com o selo da Windows Store


Você pode vincular diretamente aos detalhes do seu aplicativo com um selo personalizado para que os clientes saibam que seu aplicativo está na Windows Store.

Para criar o selo, visite a página [Selos da Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=534236). Você precisará ter a ID da Loja do seu aplicativo para usar esse formulário e gerar o selo e o link. Esse ID é formado pelos 12 últimos caracteres da **URL para Windows 10** mostrada na página [Identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo**.

> 
            **Observação**  Consulte [Diretrizes para marketing de aplicativos](app-marketing-guidelines.md) para saber mais sobre como usar o selo da Windows Store.

 

## Criando um link direto para seu aplicativo na Windows Store


Você pode criar um link que inicia a Windows Store e vai diretamente até a página de listagem do seu aplicativo sem abrir um navegador usando o esquema de URI **ms-windows-store:**.

Esses links são úteis quando você sabe que os usuários estão em um dispositivo Windows e deseja que ele chegue diretamente na página de detalhes na Loja, por exemplo, após a verificação de cadeias de caracteres de agente de usuário em um navegador para confirmar o sistema operacional do usuário, ou quando você já está se comunicando via aplicativo UWP, talvez você queira aplicar esse protocolo.

Para usar o protocolo da Windows Store e vincular diretamente à listagem da Loja do seu aplicativo, acrescente a ID da Loja do aplicativo a este link:

`ms-windows-store://pdp/?ProductId=`

Para obter mais informações sobre como usar o protocolo da Windows Store, consulte [Iniciar o aplicativo da Windows Store](../launch-resume/launch-store-app.md).

 

 







<!--HONumber=Jun16_HO4-->


