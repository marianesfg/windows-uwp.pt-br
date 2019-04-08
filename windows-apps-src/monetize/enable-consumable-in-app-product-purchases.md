---
Description: Oferecem produtos de consumo no aplicativo &\#8212; itens que podem ser adquiridos, usados e adquiridas novamente &\#8212; por meio do Store plataforma de comércio para fornecer a experiência de seus clientes com uma compra, que é robusto e confiável.
title: Habilitar compras de produtos consumíveis no aplicativo
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: uwp, consumíveis, complementos, compras no aplicativo, IAPs, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5588558eff3e9c9b2954f0726995765a2862c43b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655641"
---
# <a name="enable-consumable-in-app-product-purchases"></a>Habilitar compras de produtos consumíveis no aplicativo

Ofereça produtos consumíveis no aplicativo — itens que podem ser comprados, usados e comprados novamente — por meio da plataforma de comércio da Loja para proporcionar aos seus clientes uma experiência de compra robusta e confiável. Isso é especialmente útil para itens como moedas em jogos (ouro, moedas etc.) que podem ser comprados e então usados para comprar power-ups específicos.

> [!IMPORTANT]
> Este artigo demonstra como usar os membros do namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para habilitar compras de produto consumível no aplicativo. Esse namespace não está sendo atualizado com os novos recursos e recomendamos que você use o namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) em vez disso. O **Windows.Services.Store** namespace oferece suporte a tipos de complemento mais recente, como gerenciado pelo Store consumíveis complementos e assinaturas e é projetado para ser compatível com tipos futuros de produtos e recursos suportados pelo parceiro Centro e a Store. O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Para obter informações sobre como habilitar as compras de produtos consumíveis no aplicativo usando o namespace **Windows.Services.Store**, consulte [este artigo](enable-consumable-add-on-purchases.md).

## <a name="prerequisites"></a>Pré-requisitos

-   Este tópico trata dos relatórios de compra e atendimento de produtos no aplicativo consumíveis. Se você não tiver familiaridade com produtos no aplicativo, examine [Habilitar compras de produto no aplicativo](enable-in-app-product-purchases.md) para saber mais sobre informações de licença e como listar adequadamente produto nos aplicativo na Loja.
-   Ao codificar e testar novos produtos no aplicativo pela primeira vez, você deve usar o objeto [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) em vez do objeto [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças, em vez de chamar o servidor ativo. Para fazer isso, você precisará personalizar o arquivo nomeado WindowsStoreProxy.xml em % userprofile %\\AppData\\local\\pacotes\\&lt;nome do pacote&gt;\\LocalState\\ Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu app pela primeira vez, mas também é possível carregar um arquivo personalizado em tempo de execução. Para obter mais informações, consulte [Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tópico também faz referência a exemplos de código fornecidos no [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Este exemplo é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## <a name="step-1-making-the-purchase-request"></a>Etapa 1: Fazendo a solicitação de compra

A solicitação de compra inicial é feita com [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), como acontece com qualquer outra compra feita na Loja. A diferença no caso dos produtos consumíveis no aplicativo é que, depois de uma compra bem-sucedida, um cliente não pode comprar o mesmo produto outra vez até que o aplicativo tenha notificado a Loja de que a compra anterior foi atendida com êxito. É de responsabilidade do seu aplicativo atender aos consumíveis comprados e notificar a Windows Store do atendimento.

O próximo exemplo mostra uma solicitação de compra de produto no aplicativo consumível. Você perceberá comentários de código que indicam quando seu app deve realizar seu atendimento local do produto consumível no aplicativo para dois cenários diferentes — quando a solicitação é bem-sucedida e quando ela não é bem-sucedida devido a uma compra não atendida do mesmo produto.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Etapa 2: Acompanhamento de cumprimento de local do consumíveis

Ao conceder ao seu cliente acesso ao produto consumível no aplicativo, é importante manter o controle de qual produto é atendido (*productId*) e a qual transação o atendimento está associado (*transactionId*).

> [!IMPORTANT]
> O aplicativo é responsável pelo atendimento de relatórios precisos junto à Windows Store. Essa etapa é essencial para manter uma experiência de compras justa e confiável para os seus clientes.

O exemplo a seguir demonstra o uso das propriedades [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) da chamada a [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) na etapa anterior para identificar o produto comprado para atendimento. Uma coleção é usada para armazenar as informações do produto em um local que, mais tarde, pode ser referenciado para confirmar se o atendimento local foi bem-sucedido.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

Este próximo exemplo mostra como usar a matriz do exemplo anterior para acessar pares de ID do produto/ID da transação que, mais tarde, serão usados para relatar o atendimento à Loja.

> [!IMPORTANT]
> Seja qual for a metodologia que seu aplicativo use para controlar e confirmar o atendimento, o aplicativo precisa demonstrar discernimento para garantir que os clientes não sejam cobrados por itens que não receberam.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Etapa 3: Relatório de cumprimento de produto para a Store

Concluído o atendimento local, o app deve fazer uma chamada a [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) que inclua a *productId* e a transação na qual a compra do produto está incluída.

> [!IMPORTANT]
> A falha em gerar relatórios de produtos no aplicativo consumíveis atendidos para a Loja fará com que o usuário não possa comprar o produto novamente enquanto o atendimento da compra anterior não for relatado.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Etapa 4: Identificação de compras não cumpridas

Seu app pode usar o método [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) para verificar a qualquer momento se há produtos consumíveis não atendidos no aplicativo. Esse método deve ser chamado regularmente para verificar se existem consumíveis não atendidos devido a eventos de aplicativo não previstos, como uma interrupção na conectividade de rede ou encerramento do aplicativo.

O exemplo a seguir demonstra como [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) pode ser usado para enumerar consumíveis não atendidos e como o seu app pode iterar por essa lista para concluir o atendimento local.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>Tópicos relacionados

* [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md)
* [Exemplo de Store (demonstra as avaliações e compras no aplicativo)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 
