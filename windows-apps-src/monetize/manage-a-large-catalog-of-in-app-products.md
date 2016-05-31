---
author: mcleanbyron
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Se o seu aplicativo oferecer um catálogo abrangente de produtos no aplicativo, você também poderá seguir o processo descrito neste tópico para ajudar a gerenciar seu catálogo.
title: Gerenciar um catálogo abrangente de produtos no aplicativo
---

# Gerenciar um catálogo abrangente de produtos no aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Se o seu aplicativo oferecer um catálogo abrangente de produtos no aplicativo, você também poderá seguir o processo descrito neste tópico para ajudar a gerenciar seu catálogo. Você criará algumas entradas de produtos para faixas de preços específicos, em que cada uma poderá representar centenas de produtos em um catálogo.

Para habilitar esse recurso, use a sobrecarga de método [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) que especifica a oferta definida em aplicativo associada a um produto no aplicativo listado na Loja. Além de especificar uma oferta e uma associação ao produto durante a chamada, seu aplicativo deve também passar um objeto [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384) que contenha os detalhes da oferta de um catálogo abrangente. Se esses detalhes não forem fornecidos, os detalhes para o produto listado serão usados no lugar.

A Loja usará somente a *offerId* da solicitação de compra nos [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) resultantes. Esse processo não modifica diretamente as informações originalmente fornecidas ao [listar o produto no aplicativo na Loja](https://msdn.microsoft.com/library/windows/apps/mt148551).

**Observação**  A partir do Windows 10, a Loja não terá limite no número de listagens de produtos por conta de desenvolvedor. Em versões anteriores, a Loja tem um limite de 200 listagens de produtos por conta de desenvolvedor, e o processo descrito neste tópico poderá ser usado para contornar essa limitação.

## Pré-requisitos

-   Esse tópico abrange o suporte da Loja para a representação de várias ofertas no aplicativo usando um único produto no aplicativo listado na Loja. Se você não tiver familiaridade com compras no próprio aplicativo, revise [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md) para saber mais sobre informações de licença e como listar adequadamente sua compra no próprio aplicativo na Windows Store.
-   Ao codificar e testar novas ofertas no aplicativo pela primeira vez, use o objeto [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) em vez do objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças em vez de chamar o servidor ativo. Para fazer isso, você precisa personalizar o arquivo chamado "WindowsStoreProxy.xml" em %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu aplicativo pela primeira vez, mas também é possível carregar um arquivo personalizado no tempo de execução. Para saber mais, veja **CurrentAppSimulator**.
-   Este tópico também faz referência a exemplos de código fornecidos no [Exemplo da Loja](http://go.microsoft.com/fwlink/p/?LinkID=627610). Este exemplo é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## Fazer a solicitação de compra para o produto no aplicativo

A solicitação de compra para um produto específico em um catálogo abrangente é efetuada da mesma maneira como qualquer outra solicitação de compra em um aplicativo. Quando o aplicativo chama a nova sobrecarga do método [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382), o aplicativo fornece *OfferId* e um objeto [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390) populado com o nome do produto no aplicativo.

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## Relatar o atendimento da oferta no aplicativo

O aplicativo precisará relatar o atendimento do produto para a Loja quando a oferta tiver sido atendida localmente. Em um cenário de catálogo abrangente, se o aplicativo não relatar o atendimento da oferta, o usuário não poderá comprar qualquer oferta no aplicativo usando a mesma lista de produtos da Loja.

Como mencionado antes, a Loja usa apenas a informação da oferta fornecida para preencher os [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392), e não cria uma associação persistente entre uma oferta de catálogo abrangente e uma lista de produtos da Loja. Como resultado, você precisa rastrear os direitos do usuário para produtos, e fornecer um contexto específico do produto (como o nome do item comprado ou os detalhes sobre ele) para o usuário fora da operação [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382).

O seguinte código demonstra a chamada de atendimento, e um padrão de mensagem de IU onde a informação sobre a oferta específica é inserida. Na ausência dessas informações de produto específicas, o exemplo usará informações do produto [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163).

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## Tópicos relacionados

* [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md)
* [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md)
* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)


<!--HONumber=May16_HO2-->


