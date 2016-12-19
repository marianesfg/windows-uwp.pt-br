---
author: mcleanbyron
Description: "Ofereça produtos consumíveis no aplicativo&\\#8212;itens que podem ser comprados, usados e comprados novamente&\\#8212;por meio da plataforma de comércio da Loja e proporcione aos seus clientes uma experiência de compra robusta e confiável."
title: "Habilitar compras de produtos consumíveis no aplicativo"
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: "exemplo de código de oferta no aplicativo"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: acb7218bed287f430950d4f8d3621831b269ae18

---

# <a name="enable-consumable-in-app-product-purchases"></a>Habilitar compras de produtos consumíveis no aplicativo


>**Observação**&nbsp;&nbsp;Este artigo demonstra como usar membros do namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Se seu aplicativo for destinado ao Windows 10, versão 1607 ou posterior, recomendamos que você use membros do namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para gerenciar complementos (também conhecidos como produtos no aplicativo ou IAPs), em vez do namespace **Windows.ApplicationModel.Store**. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).

Ofereça produtos consumíveis no aplicativo — itens que podem ser comprados, usados e comprados novamente — por meio da plataforma de comércio da Loja para proporcionar aos seus clientes uma experiência de compra robusta e confiável. Isso é especialmente útil para itens como moedas em jogos (ouro, moedas etc.) que podem ser comprados e então usados para comprar power-ups específicos.

## <a name="prerequisites"></a>Pré-requisitos

-   Este tópico trata dos relatórios de compra e atendimento de produtos no aplicativo consumíveis. Se você não tiver familiaridade com produtos no aplicativo, examine [Habilitar compras de produto no aplicativo](enable-in-app-product-purchases.md) para saber mais sobre informações de licença e como listar adequadamente produto nos aplicativo na Loja.
-   Ao codificar e testar novos produtos no aplicativo pela primeira vez, você deve usar o objeto [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) em vez do objeto [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças, em vez de chamar o servidor ativo. Para fazer isso, você precisa personalizar o arquivo chamado WindowsStoreProxy.xml em %userprofile%\\AppData\\local\\packages\\&lt;nome do pacote&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu app pela primeira vez, mas também é possível carregar um arquivo personalizado em tempo de execução. Para obter mais informações, consulte [Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tópico também faz referência a exemplos de código fornecidos no [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Essa amostra é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## <a name="step-1-making-the-purchase-request"></a>Etapa 1: Fazendo a solicitação de compra

A solicitação de compra inicial é feita com [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381), como acontece com qualquer outra compra feita na Loja. A diferença no caso dos produtos consumíveis no aplicativo é que, depois de uma compra bem-sucedida, um cliente não pode comprar o mesmo produto outra vez até que o aplicativo tenha notificado a Loja de que a compra anterior foi atendida com êxito. É de responsabilidade do seu aplicativo atender aos consumíveis comprados e notificar a Windows Store do atendimento.

O próximo exemplo mostra uma solicitação de compra de produto no aplicativo consumível. Você perceberá comentários de código que indicam quando seu app deve realizar seu atendimento local do produto consumível no aplicativo para dois cenários diferentes — quando a solicitação é bem-sucedida e quando ela não é bem-sucedida devido a uma compra não atendida do mesmo produto.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Etapa 2: Rastreando o atendimento local do consumível

Ao conceder ao seu cliente acesso ao produto consumível no aplicativo, é importante manter o controle de qual produto é atendido (*productId*) e a qual transação o atendimento está associado (*transactionId*).

>**Importante**&nbsp;&nbsp;Seu app é responsável por relatar o atendimento com precisão à Loja. Essa etapa é essencial para manter uma experiência de compras justa e confiável para os seus clientes.

O exemplo a seguir demonstra o uso das propriedades [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) da chamada a [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381) na etapa anterior para identificar o produto comprado para atendimento. Uma coleção é usada para armazenar as informações do produto em um local que, mais tarde, pode ser referenciado para confirmar se o atendimento local foi bem-sucedido.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

Este próximo exemplo mostra como usar a matriz do exemplo anterior para acessar pares de ID do produto/ID da transação que, mais tarde, serão usados para relatar o atendimento à Loja.

>**Importante**&nbsp;&nbsp;Seja qual for a metodologia usada pelo app para controlar e confirmar o atendimento, ele precisa demonstrar discernimento para garantir que os clientes não sejam cobrados por itens que não receberam.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Etapa 3: Relatando o atendimento do produto à Loja

Concluído o atendimento local, o app deve fazer uma chamada a [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/dn263380) que inclua a *productId* e a transação na qual a compra do produto está incluída.

>**Importante**&nbsp;&nbsp;Se um produto consumível atendido no aplicativo não for relatado à Loja, o usuário não poderá comprar esse produto novamente enquanto o atendimento da compra anterior não for relatado.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Etapa 4: Identificando compras não atendidas

Seu app pode usar o método [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) para verificar a qualquer momento se há produtos consumíveis não atendidos no aplicativo. Esse método deve ser chamado regularmente para verificar se existem consumíveis não atendidos devido a eventos de aplicativo não previstos, como uma interrupção na conectividade de rede ou encerramento do aplicativo.

O exemplo a seguir demonstra como [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) pode ser usado para enumerar consumíveis não atendidos e como o seu app pode iterar por essa lista para concluir o atendimento local.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>Tópicos relacionados

* [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md)
* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 



<!--HONumber=Dec16_HO1-->


