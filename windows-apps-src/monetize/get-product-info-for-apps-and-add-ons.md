---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Saiba como usar o namespace Windows.Services.Store para obter informações de produto relacionados à Loja para o aplicativo atual ou um de seus complementos.
title: Obter informações do produto para apps e complementos
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp, compras no aplicativo, IAPs, complementos, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 9b923764c6374e403d2652db715f65a80c48bacf
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8325516"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Obter informações do produto para apps e complementos

Este artigo demonstra como usar métodos da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) a fim de acessar informações relacionadas à Microsoft Store para o aplicativo atual ou um de seus complementos.

Para obter um aplicativo de exemplo completo, consulte o [exemplo da Microsoft Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [este artigo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Pré-requisitos

Esses exemplos têm os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior.
* Você tenha [criado um envio de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) no Partner Center e esse aplicativo é publicado na loja. Opcionalmente, é possível configurar o app para que ele não possa ser descoberto na Loja enquanto você o testa. Para obter mais informações, consulte as [diretrizes para teste](in-app-purchases-and-trials.md#testing).
* Se você deseja obter informações de produto para um complemento para o aplicativo, você também deve [criar o complemento no Partner Center](../publish/add-on-submissions.md).

O código nestes exemplos pressupõe que:
* O código seja executado no contexto de uma [Página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contenha um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` e um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado nesses exemplos para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obter informações para o aplicativo atual

Para obter informações da Loja sobre o aplicativo atual, use o método [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync). Esse é um método assíncrono que retorna um objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que você pode usar para obter informações, como o preço.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Obter informações de complementos com IDs da Store conhecidos que estão associados ao app atual

Para obter informações de produto da Store para complementos associados ao app atual e cujas [IDs da Store](in-app-purchases-and-trials.md#store_ids) você já conhece, use o método [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). Trata-se de um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos complementos. Além das IDs da Store, você deve passar uma lista de sequências para esse método que identifiquem os tipos dos complementos. Para obter uma lista dos valores de sequência com suporte, consulte a propriedade [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> O método **GetStoreProductsAsync** retorna informações de produto para os complementos especificados que estão associados ao app, independentemente se os complementos estejam disponíveis para compra atualmente. Para recuperar informações de todos os complementos para o app atual que podem ser comprados no momento, use o método **GetAssociatedStoreProductsAsync** conforme descrito na [seção a seguir](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app).

Este exemplo recupera informações para complementos duráveis com as IDs da Store associadas ao app atual.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Obter informações para complementos que estão disponíveis para compra desde o app atual

Para obter informações da Store sobre os complementos atualmente disponíveis para compra desde o app atual, use o método [GetAssociatedStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync). Trata-se de um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos complementos disponíveis. Você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos de complementos que você deseja recuperar. Para obter uma lista dos valores de sequência com suporte, consulte a propriedade [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Se o app tiver vários complementos disponíveis para compra, você poderá usar como alternativa o método [GetAssociatedStoreProductsWithPagingAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) para usar a paginação para retornar os resultados do complemento.

O exemplo a seguir recupera informações para todos os complementos duráveis, complementos consumíveis gerenciados pela Store e complementos consumíveis gerenciados pelo desenvolvedor, disponíveis para compra desde o app atual.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Obter informações para complementos para o app atual que o usuário comprou

Para obter informações de produto da Store para complementos que o usuário atual comprou, use o método [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionasync). Trata-se de um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos complementos. Você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos de complementos que você deseja recuperar. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a propriedade [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

> [!NOTE]
> Se o aplicativo tiver muitos complementos, uma alternativa é usar o método [GetUserCollectionWithPagingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) para usar paginação e retornar os resultados dos complementos.

O exemplo a seguir recupera informações para complementos duráveis com as [IDs da Store](in-app-purchases-and-trials.md#store_ids) especificadas.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
