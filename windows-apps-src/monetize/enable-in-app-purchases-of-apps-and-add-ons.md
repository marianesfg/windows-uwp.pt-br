---
author: mcleanbyron
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Saiba como usar o namespace Windows.Services.Store para comprar um aplicativo ou um dos seus complementos.
title: Habilitar compras nos aplicativos e complementos
keywords: "exemplo de código de oferta no aplicativo"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 05a93f3124324d7308f5494ad14a15bfd6a4e698

---

# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Habilitar compras nos aplicativos e complementos

Os aplicativos destinados ao Windows 10, versão 1607 ou posterior, podem usar membros no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para solicitar a compra do aplicativo atual ou um de seus complementos (também conhecidos como produtos no aplicativo ou IAPs) para o usuário. Por exemplo, se o usuário tem uma versão de avaliação do aplicativo, você pode usar esse processo para adquirir uma licença completa para o usuário. Como alternativa, você pode usar esse processo para adquirir um complemento, como um novo nível de jogo para o usuário.

Para solicitar a compra de um aplicativo ou complemento, o [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) fornece vários métodos diferentes:
* Se você souber o [ID da Loja](in-app-purchases-and-trials.md#store_ids) do aplicativo ou complemento, poderá usar o método [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx).
* Se você já tiver um objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx), [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) ou [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) que represente o aplicativo ou complemento, use os métodos **RequestPurchaseAsync** desses objetos.

Cada método apresenta uma interface do usuário de compra padrão para o usuário e, em seguida, é concluído assincronamente após a conclusão da transação. O método retorna um objeto que indica se a transação foi bem-sucedida.

>**Observação**&nbsp;&nbsp;Este artigo se refere a aplicativos direcionados ao Windows 10, versão 1607, ou posterior. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Pré-requisitos

Este exemplo tem os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao Windows 10, versão 1607 ou posterior.
* Você criou um aplicativo no painel do Centro de Desenvolvimento do Windows, e esse aplicativo foi publicado e disponibilizado na Loja. Pode ser um aplicativo que você deseja liberar para os clientes ou pode ser um aplicativo básico que atenda aos requisitos mínimos [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que você esteja usando somente para testes. Para obter mais informações, consulte [diretrizes para teste](in-app-purchases-and-trials.md#testing).

O código neste exemplo pressupõe que:
* O código seja executado no contexto de uma [Página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contenha um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` e um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

>**Observação**&nbsp;&nbsp;Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado neste exemplo para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemplo de código

Este exemplo demonstra como usar o método [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para comprar um app ou complemento com uma [ID da Loja](in-app-purchases-and-trials.md#store_ids) conhecida.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

Para obter um app de exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


