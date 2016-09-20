---
author: mcleanbyron
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: "Saiba como usar o namespace Windows.Services.Store para trabalhar com complementos consumíveis."
title: "Habilitar compras de complementos consumíveis"
keywords: "exemplo de código de oferta no aplicativo"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 1e9ecad5abb9addbe41b38d0b56b84404716f2a8

---

# Habilitar compras de complementos consumíveis

Os aplicativos destinados ao Windows 10, versão 1607 ou posterior podem usar métodos da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para gerenciar o atendimento de complementos consumíveis do usuário em seus aplicativos UWP (complementos também são conhecidos como produtos no aplicativo ou IAPs). Use complementos consumíveis para itens que podem ser comprados, usados e comprados novamente. Isso é especialmente útil para coisas como moedas em jogos (ouro, moedas, etc.) que podem ser compradas e então usadas para comprar power-ups específicos.

>**Observação**&nbsp;&nbsp;Este artigo se refere a aplicativos direcionados ao Windows 10, versão 1607, ou posterior. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## Visão geral dos complementos consumíveis

Aplicativos destinados ao Windows 10, versão 1607, ou posterior podem oferecer dois tipos de complementos consumíveis que variam dependendo da maneira como o atendimento é gerenciado:

* **Consumível gerenciado pelo desenvolvedor**. Para esse tipo de produto consumível, você é responsável por controlar o saldo do usuário de itens do usuário que o complemento representa e por relatar a compra do complemento como providenciada para a Loja depois que o usuário consome todos os itens. O usuário não pode comprar o complemento novamente até que seu aplicativo tenha informado a compra anterior do complemento como providenciada.

  Por exemplo, se o seu complemento representar 100 moedas em um jogo e o usuário consumir 10 moedas, seu aplicativo ou o serviço deverá manter o novo saldo restante de 90 moedas para o usuário. Depois que o usuário tiver consumido todas as 100 moedas, seu aplicativo deverá declarar o complemento como providenciado e, em seguida, o usuário poderá comprar o complemento de 100 moedas novamente.

* **Consumível gerenciado pela Loja**. Para esse tipo de produto consumível, a Loja mantém o controle do saldo de itens do usuário que o complemento representa. Quando o usuário consome todos os itens, você é responsável por relatar esses itens como providenciados para a Loja, e a Loja atualiza o saldo do usuário. Seu aplicativo pode consultar o saldo atual para o usuário a qualquer momento. Depois que o usuário consome todos os itens, ele pode comprar o complemento novamente.

  Por exemplo, se o complemento representar uma quantidade inicial de 100 moedas em um jogo e o usuário consumir 10 moedas, o aplicativo relatará para a Loja que 10 unidades do complemento foram providenciadas, e a Loja atualizará o saldo restante. Depois que o usuário tiver consumido todas as 100 moedas, o usuário poderá comprar o complemento de 100 moedas novamente.

  >**Observação**&nbsp;&nbsp;Os consumíveis gerenciados pela Loja estão disponíveis a partir do Windows 10, versão 1607. A capacidade de criar um produto consumível gerenciado pela Loja no painel do Centro de Desenvolvimento do Windows estará disponível em breve.

Para oferecer um complemento consumível a um usuário, siga este processo geral:

1. Permita que os usuários [comprem o complemento](enable-in-app-purchases-of-apps-and-add-ons.md) do seu aplicativo.
3. Quando o usuário consumir o complemento (por exemplo, gastar moedas em um jogo), [declare o complemento como providenciado](enable-consumable-add-on-purchases.md#report_fulfilled).

A qualquer momento, você também pode [obter o saldo restante](enable-consumable-add-on-purchases.md#get_balance) de um consumível gerenciado pela Loja.

## Pré-requisitos

Esses exemplos têm os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao Windows 10, versão 1607 ou posterior.
* Você criou um aplicativo no painel do Centro de Desenvolvimento do Windows com complementos consumíveis (também conhecidos como produtos no aplicativo ou IAPs), e esse aplicativo foi publicado e disponibilizado na Loja. Pode ser um aplicativo que você deseja liberar para os clientes ou pode ser um aplicativo básico que atenda aos requisitos mínimos [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que você esteja usando somente para testes. Para obter mais informações, consulte [diretrizes para teste](in-app-purchases-and-trials.md#testing).

O código nestes exemplos pressupõem que:
* O código seja executado no contexto de uma [Página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contenha um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` e um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

Para obter um aplicativo de exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="report_fulfilled" />
## Declarar um complemento consumível como providenciado

Depois que o usuário [comprar o complemento](enable-in-app-purchases-of-apps-and-add-ons.md) do seu aplicativo e ele consumir o complemento, seu aplicativo deverá declarar o complemento como providenciado chamando o método [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Você deve passar as seguintes informações para esse método:

* A [ID da Loja](in-app-purchases-and-trials.md#store_ids) do complemento que você deseja relatar como providenciado.
* As unidades do complemento a ser relatado como providenciado.
  * Para um consumível gerenciado pelo desenvolvedor, especifique 1 para o parâmetro *quantity*. Isso alerta a Loja que o consumível foi providenciado, e o cliente pode comprar o consumível novamente. O usuário não pode comprar o consumível novamente até que seu aplicativo tenha notificado a Loja que ele foi providenciado.
  * Para um consumível gerenciado pela Loja, especifique o número real de unidades que foram consumidas. A Loja atualizará o saldo restante do consumível.
* A ID de rastreamento do atendimento. Trata-se de uma GUID fornecida pelo desenvolvedor que identifica a transação específica à qual a operação de atendimento está associada para fins de controle. Para obter mais informações, consulte os comentários em [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx).

Este exemplo demonstra como relatar um consumível gerenciado pela Loja como providenciado.

```csharp
private StoreContext context = null;

public async void ConsumeAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // This is an example for a Store-managed consumable, where you specify the actual number
    // of units that you want to report as consumed so the Store can update the remaining
    // balance. For a developer-managed consumable where you maintain the balance, specify 1
    // to just report the add-on as fulfilled to the Store.
    uint quantity = 10;
    string addOnStoreId = "9NBLGGH4TNNR";
    Guid trackingId = Guid.NewGuid();

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.ReportConsumableFulfillmentAsync(
        addOnStoreId, quantity, trackingId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "The fulfillment was successful. Remaining balance: " +
                result.BalanceRemaining;
            break;

        case StoreConsumableStatus.InsufficentQuantity:
            textBlock.Text = "The fulfillment was unsuccessful because the user " +
            "doesn't have enough remaining balance." + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "The fulfillment was unsuccessful due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "The fulfillment was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The fulfillment was unsuccessful due to an unknown error.";
            break;
    }
}
```

<span id="get_balance" />
## Obtenha o saldo restante de um consumível gerenciado pela Loja

Este exemplo demonstra como usar o método [GetConsumableBalanceRemainingAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getconsumablebalanceremainingasync.aspx) da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para obter o saldo restante de um complemento consumível gerenciado pela Loja.

```csharp
private StoreContext context = null;

public async void GetRemainingBalance(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    string addOnStoreId = "9NBLGGH4TNNR";

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.GetConsumableBalanceRemainingAsync(addOnStoreId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "Remaining balance: " + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "Could not retrieve balance due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "Could not retrieve balance due to a server error.";
            break;

        default:
            textBlock.Text = "Could not retrieve balance due to an unknown error.";
            break;
    }
}
```

## Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações do produto para aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


