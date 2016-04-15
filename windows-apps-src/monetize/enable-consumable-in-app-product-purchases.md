---
Description: Ofereça produtos consumíveis no aplicativo&\#8212;itens que podem ser comprados, usados e comprados novamente&\#8212;por meio da plataforma de comércio da Loja para proporcionar aos seus clientes uma experiência de compra robusta e confiável.
title: Habilitar compras de produtos consumíveis no aplicativo
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: oferta no aplicativo
keywords: consumíveis
keywords: compra no aplicativo
keywords: produto no aplicativo
keywords: como oferecer suporte no aplicativo
keywords: amostra de código de compra no aplicativo
keywords: amostra de código de oferta no aplicativo
---

# Habilitar compras de produtos consumíveis no aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Ofereça produtos consumíveis no aplicativo — itens que podem ser comprados, usados e comprados novamente — por meio da plataforma de comércio da Loja para proporcionar aos seus clientes uma experiência de compra robusta e confiável. Isso é especialmente útil para coisas como moedas em jogos (ouro, moedas etc.) que podem ser compradas e então usadas para comprar power-ups específicos.

## Pré-requisitos

-   Este tópico trata dos relatórios de compra e atendimento de produtos no aplicativo consumíveis. Se você não tiver familiaridade com produtos no aplicativo, examine [Habilitar compras de produto no aplicativo](enable-in-app-product-purchases.md) para saber mais sobre informações de licença e como listar adequadamente produto nos aplicativo na Loja.
-   Ao codificar e testar novos produtos no aplicativo pela primeira vez, você deve usar o objeto [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) em vez do objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças em vez de chamar o servidor ativo. Para fazer isso, você precisa personalizar o arquivo chamado "WindowsStoreProxy.xml" em %userprofile%\\AppData\\local\\packages\\&lt;nome do pacote&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu aplicativo pela primeira vez, mas também é possível carregar um arquivo personalizado em tempo de execução. Para saber mais, consulte **CurrentAppSimulator**.
-   Este tópico também faz referência a exemplos de código fornecidos na [Amostra da Loja](http://go.microsoft.com/fwlink/p/?LinkID=627610). Essa amostra é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## Etapa 1: Fazendo a solicitação de compra

A solicitação de compra inicial é feita com o [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) como com qualquer outra compra feita na Loja. A diferença no caso dos produtos consumíveis no aplicativo é que, depois de uma compra bem-sucedida, um cliente não pode comprar o mesmo produto outra vez até que o aplicativo tenha notificado a Loja de que a compra anterior foi atendida com êxito. É de responsabilidade do seu aplicativo atender aos consumíveis comprados e notificar a Windows Store do atendimento.

O próximo exemplo mostra uma solicitação de compra de produto no aplicativo consumível. Você perceberá comentários de código que indicam quando seu aplicativo deve realizar seu atendimento local do produto consumível no aplicativo para dois cenários diferentes — quando a solicitação de compra é bem-sucedida e quando ela não é bem-sucedida devido a uma compra não atendida do mesmo produto.

```CSharp
PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1");
switch (purchaseResults.Status)
{
    case ProductPurchaseStatus.Succeeded:
        product1TempTransactionId = purchaseResults.TransactionId;

        // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;

    case ProductPurchaseStatus.NotFulfilled:
        product1TempTransactionId = purchaseResults.TransactionId;

        // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
        // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;
}
```

## Etapa 2: Rastreando atendimento local do consumível

Ao conceder o acesso para o seu cliente ao produto consumível no aplicativo, é importante manter o controle de qual produto é atendido (*productId*) e com qual transação o atendimento está associado (*transactionId*).

**Importante**  O aplicativo é responsável por comunicar o atendimento com precisão à Loja. Essa etapa é essencial para manter uma experiência de compras justa e confiável para os seus clientes.

O exemplo a seguir demonstra o uso das propriedades [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) da chamada [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) na etapa anterior para identificar o produto comprado para atendimento. Uma matriz é usada para armazenar as informações do produto em um local que, mais tarde, pode ser referenciado para confirmar se o atendimento local foi bem-sucedido.

```CSharp
private void GrantFeatureLocally(string productId, Guid transactionId)
{
    if (!grantedConsumableTransactionIds.ContainsKey(productId))
    {
        grantedConsumableTransactionIds.Add(productId, new List<Guid>());
    }
    grantedConsumableTransactionIds[productId].Add(transactionId);

    // Grant the user their content. You will likely increase some kind of gold/coins/some other asset count.
}
```

Este próximo exemplo mostra como usar a matriz do exemplo anterior para acessar pares de ID do produto/ID da transação que, mais tarde, serão usados para comunicar o atendimento à Windows Store.

**Importante**  Seja qual for a metodologia usada pelo aplicativo para controlar e confirmar o atendimento, ele precisa demonstrar discernimento para garantir que os clientes não sejam cobrados por itens que não receberam.

```CSharp
private Boolean IsLocallyFulfilled(string productId, Guid transactionId)
{
    return grantedConsumableTransactionIds.ContainsKey(productId) &amp;&amp; grantedConsumableTransactionIds[productId].Contains(transactionId);
}
```

## Etapa 3: Comunicando o atendimento do produto à Windows Store

Concluído o atendimento local, o aplicativo deve fazer uma chamada [**ReportConsumableFulfillmentAsync**](https://msdn.microsoft.com/library/windows/apps/dn263380) que inclui a *productId* e a transação na qual a compra do produto está incluída.

**Importante**  Se um produto consumível no aplicativo atendido não for comunicado à Loja, o usuário não poderá comprar esse produto novamente enquanto o atendimento da compra anterior não for comunicado.

```CSharp
FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync("product2", product2TempTransactionId);
```

## Etapa 4: Identificando compras não atendidas

Seu aplicativo pode usar o método [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) para verificar a qualquer momento se há produtos consumíveis no aplicativo não atendidos. Esse método deve ser chamado regularmente para verificar se existem consumíveis não atendidos devido a eventos de aplicativo não previstos, como uma interrupção na conectividade de rede ou encerramento do aplicativo.

O exemplo a seguir demonstra como [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) pode ser usado para enumerar consumíveis não atendidos e como o seu aplicativo pode iterar através dessa lista para concluir o atendimento local.

```CSharp
private async void GetUnfulfilledConsumables()
{
    products = await CurrentApp.GetUnfulfilledConsumablesAsync();

    foreach (UnfulfilledConsumable product in products)
    {
        logMessage += "\nProduct Id: " + product.ProductId + " Transaction Id: " + product.TransactionId;
        // This is where you would pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
    // To indicate local fulfillment to the Windows Store.
    }
}
```

## Tópicos relacionados

* [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md)
* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 






<!--HONumber=Mar16_HO1-->


