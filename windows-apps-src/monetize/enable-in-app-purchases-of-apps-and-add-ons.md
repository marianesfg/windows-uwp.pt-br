---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Saiba como usar o namespace Windows.Services.Store para comprar um aplicativo ou um dos seus complementos.
title: Habilitar compras no aplicativo e complementos no aplicativo
keywords: windows 10, uwp, complementos, compras no aplicativo, IAPs, Windows.Services.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0825732698da415f976ce1429aaa5af41b681bff
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372577"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Habilitar compras no aplicativo e complementos no aplicativo

Este artigo demonstra como usar membros no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para solicitar a compra do aplicativo atual ou um de seus complementos para o usuário. Por exemplo, se o usuário tem uma versão de avaliação do aplicativo, você pode usar esse processo para adquirir uma licença completa para o usuário. Como alternativa, você pode usar esse processo para adquirir um complemento, como um novo nível de jogo para o usuário.

Para solicitar a compra de um aplicativo ou complemento, o namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) fornece vários métodos diferentes:
* Se você souber o [ID da Loja](in-app-purchases-and-trials.md#store_ids) do aplicativo ou complemento, poderá usar o método [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext).
* Se você já tiver um objeto [**StoreProduct**, **StoreSku** ou **StoreAvailability**](in-app-purchases-and-trials.md#products-skus) que represente o aplicativo ou complemento, use os métodos **RequestPurchaseAsync** desses objetos. Para obter exemplos de diferentes maneiras de recuperar um **StoreProduct** em seu código, consulte [Obter informações de produto para apps e complementos](get-product-info-for-apps-and-add-ons.md).

Cada método apresenta uma interface do usuário de compra padrão para o usuário e, em seguida, é concluído assincronamente após a conclusão da transação. O método retorna um objeto que indica se a transação foi bem-sucedida.

> [!NOTE]
> O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [este artigo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Pré-requisitos

Este exemplo tem os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior.
* Você tem [criou um envio de aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-submissions) no Partner Center e esse aplicativo é publicado na Store. Opcionalmente, é possível configurar o app para que ele não possa ser descoberto na Store enquanto você o testa. Para obter mais informações, consulte as [diretrizes para teste](in-app-purchases-and-trials.md#testing).
* Se você quiser habilitar compras no aplicativo para um complemento para o aplicativo, você deve também [criar o complemento no Partner Center](../publish/add-on-submissions.md).

O código neste exemplo pressupõe que:
* O código seja executado no contexto de uma [Página](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) que contenha um [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring) denominado ```workingProgressRing``` e um [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado neste exemplo para configurar o objeto [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemplo de código

Este exemplo demonstra como usar o método [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) da classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) para comprar um app ou complemento com uma [ID da Loja](in-app-purchases-and-trials.md#store-ids) conhecida. Para obter um app de exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

## <a name="video"></a>Vídeo

Assista ao vídeo a seguir para obter uma visão geral de como implementar compras no aplicativo em seu app.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações sobre produtos para os aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
