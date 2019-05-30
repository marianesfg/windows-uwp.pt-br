---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Saiba como usar o namespace Windows.Services.Store para trabalhar com complementos consumíveis.
title: Habilitar compras de complementos consumíveis
keywords: windows 10, uwp, consumíveis, complementos, compras no aplicativo, IAPs, Windows.Services.Store
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 601d4d0a2cfe7e6d024e9cc07fefcdb2be688a36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371836"
---
# <a name="enable-consumable-add-on-purchases"></a>Habilitar compras de complementos consumíveis

Este artigo demonstra como usar métodos da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para gerenciar o atendimento de complementos consumíveis do usuário em seus aplicativos UWP. Use complementos consumíveis para itens que podem ser comprados, usados e comprados novamente. Isso é especialmente útil para itens como moedas em jogos (ouro, moedas etc.) que podem ser comprados e então usados para comprar power-ups específicos.

> [!NOTE]
> O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [este artigo](enable-consumable-in-app-product-purchases.md).

## <a name="overview-of-consumable-add-ons"></a>Visão geral dos complementos consumíveis

Os aplicativos podem oferecer dois tipos de complementos consumíveis que variam dependendo da maneira como o atendimento é gerenciado:

* **Consumível gerenciado pelo desenvolvedor**. Para esse tipo de produto consumível, você é responsável por controlar o saldo do usuário de itens do usuário que o complemento representa e por relatar a compra do complemento como providenciada para a Store depois que o usuário consome todos os itens. O usuário não pode comprar o complemento novamente até que seu aplicativo tenha informado a compra anterior do complemento como providenciada.

  Por exemplo, se o seu complemento representar 100 moedas em um jogo e o usuário consumir 10 moedas, seu aplicativo ou o serviço deverá manter o novo saldo restante de 90 moedas para o usuário. Depois que o usuário tiver consumido todas as 100 moedas, seu aplicativo deverá declarar o complemento como providenciado e, em seguida, o usuário poderá comprar o complemento de 100 moedas novamente.

* **Consumível gerenciado pela Store**. Para esse tipo de produto consumível, a Store mantém o controle do saldo de itens do usuário que o complemento representa. Quando o usuário consome todos os itens, você é responsável por relatar esses itens como providenciados para a Store, e a Store atualiza o saldo do usuário. O usuário pode adquirir o complemento quantas vezes desejar (não é necessário consumir os itens primeiro). Seu aplicativo pode consultar o saldo atual na Store para o usuário a qualquer momento.

  Por exemplo, se o complemento representar uma quantidade inicial de 100 moedas em um jogo e o usuário consumir 50 moedas, o aplicativo relatará para a Store que 50 unidades do complemento foram providenciadas, e a Store atualizará o saldo restante. Se o usuário comprar o complemento novamente para adquirir mais 100 moedas, ele agora terá 150 moedas no total.
    > [!NOTE]
    > Os consumíveis gerenciados pela Microsoft Store foram introduzidos no Windows 10, versão 1607.

Para oferecer um complemento consumível a um usuário, siga este processo geral:

1. Permita que os usuários [comprem o complemento](enable-in-app-purchases-of-apps-and-add-ons.md) do seu aplicativo.
3. Quando o usuário consumir o complemento (por exemplo, gastar moedas em um jogo), [declare o complemento como providenciado](enable-consumable-add-on-purchases.md#report_fulfilled).

A qualquer momento, você também pode [obter o saldo restante](enable-consumable-add-on-purchases.md#get_balance) de um consumível gerenciado pela Store.

## <a name="prerequisites"></a>Pré-requisitos

Esses exemplos têm os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior.
* Você tem [criou um envio de aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-submissions) no Partner Center e esse aplicativo é publicado na Store. Opcionalmente, é possível configurar o app para que ele não possa ser descoberto na Store enquanto você o testa. Para obter mais informações, consulte as [diretrizes para teste](in-app-purchases-and-trials.md#testing).
* Você tem [criou um complemento consumível para o aplicativo](../publish/add-on-submissions.md) no Partner Center.

O código nestes exemplos pressupõe que:
* O código seja executado no contexto de uma [Página](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) que contenha um [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` e um [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O app seja um app de usuário único executado somente no contexto do usuário que o iniciou. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

Para obter um app de exemplo completo, consulte o [exemplo da Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado nesses exemplos para configurar o objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>Declarar um complemento consumível como providenciado

Depois que o usuário [comprar o complemento](enable-in-app-purchases-of-apps-and-add-ons.md) do seu aplicativo e ele consumir o complemento, seu aplicativo deverá declarar o complemento como providenciado chamando o método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Você deve passar as seguintes informações para esse método:

* A [ID da Store](in-app-purchases-and-trials.md#store-ids) do complemento que você deseja relatar como providenciado.
* As unidades do complemento a ser relatado como providenciado.
  * Para um consumível gerenciado pelo desenvolvedor, especifique 1 para o parâmetro *quantity*. Isso alerta a Store que o consumível foi providenciado, e o cliente pode comprar o consumível novamente. O usuário não pode comprar o consumível novamente até que seu aplicativo tenha notificado a Store que ele foi providenciado.
  * Para um consumível gerenciado pela Store, especifique o número real de unidades que foram consumidas. A Store atualizará o saldo restante do consumível.
* A ID de rastreamento do atendimento. Trata-se de uma GUID fornecida pelo desenvolvedor que identifica a transação específica à qual a operação de atendimento está associada para fins de controle. Para obter mais informações, consulte os comentários em [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

Este exemplo demonstra como relatar um consumível gerenciado pela Store como providenciado.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Obter o saldo restante de um consumível gerenciado pela Store

Este exemplo demonstra como usar o método [GetConsumableBalanceRemainingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) para obter o saldo restante de um complemento consumível gerenciado pela Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações sobre produtos para os aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras no aplicativo de aplicativos e complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
