---
author: mcleanbyron
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: "Saiba como usar o namespace Windows.Services.Store para obter informações de produto relacionados à Loja para o aplicativo atual ou um de seus complementos."
title: "Obter informações do produto para apps e complementos"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, compras no aplicativo, IAPs, complementos, Windows.Services.Store
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7e486c451174cd24429dc35cda07d22fe2b28745
ms.lasthandoff: 02/07/2017

---

# <a name="get-product-info-for-apps-and-add-ons"></a>Obter informações do produto para apps e complementos

Os aplicativos destinados ao Windows 10, versão 1607 ou posterior podem usar métodos da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para acessar informações relacionadas à Loja para o aplicativo atual ou um de seus complementos (também conhecidos como produtos no aplicativo ou IAPs). Os exemplos neste artigo a seguir demonstram como fazer isso para diferentes cenários. Para obter um exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Observação**&nbsp;&nbsp;Este artigo se refere a aplicativos direcionados ao Windows 10, versão 1607, ou posterior. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Pré-requisitos

Esses exemplos têm os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao Windows 10, versão 1607 ou posterior.
* Você criou um aplicativo no painel do Centro de Desenvolvimento do Windows, e esse aplicativo foi publicado e disponibilizado na Loja. Pode ser um aplicativo que você deseja liberar para os clientes ou pode ser um aplicativo básico que atenda aos requisitos mínimos [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que você esteja usando somente para testes. Para obter mais informações, consulte [diretrizes para teste](in-app-purchases-and-trials.md#testing).

O código nestes exemplos pressupõem que:
* O código seja executado no contexto de uma [Página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contenha um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` e um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

Para obter um aplicativo de exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Observação**&nbsp;&nbsp;Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado nesses exemplos para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obter informações para o aplicativo atual

Para obter informações da Loja sobre o aplicativo atual, use o método [GetStoreProductForCurrentAppAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getstoreproductforcurrentappasync.aspx). Esse é um método assíncrono que retorna um objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que você pode usar para obter informações, como o preço.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-products-with-known-store-ids"></a>Obter informações de produtos com IDs da Loja conhecidas

Para obter informações de produto da Loja para apps ou complementos cujas [IDs da Loja](in-app-purchases-and-trials.md#store_ids) você já conhece, use o método [GetStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706579.aspx). Isso é um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos aplicativos ou complementos. Além das IDs da Loja, você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos dos complementos. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a propriedade [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

O exemplo a seguir recupera informações para complementos duráveis com as IDs da Loja especificadas.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-the-current-app"></a>Obter informações para complementos que estão disponíveis para o app atual

Para obter informações da Loja sobre os complementos que estão disponíveis para o app atual, use o método [GetAssociatedStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706571.aspx). Trata-se de um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos complementos disponíveis. Você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos de complementos que você deseja recuperar. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a propriedade [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

>**Observação**&nbsp;&nbsp;Se o app tiver muitos complementos, uma alternativa é usar o método [GetAssociatedStoreProductsWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706572.aspx) para usar paginação e retornar os resultados dos complementos.

O exemplo a seguir recupera informações para todos os complementos duráveis, complementos consumíveis gerenciados pela Loja e complementos consumíveis gerenciadas pelo desenvolvedor.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-current-user-is-entitled-to-use"></a>Obter informações dos complementos para o app atual que o usuário atual tem o direito de usar

Para obter informações da Loja sobre os complementos que o usuário atual tem o direito de usar, utilize o método [GetUserCollectionAsync](https://msdn.microsoft.com/library/windows/apps/mt706580.aspx). Trata-se de um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos complementos. Você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos de complementos que você deseja recuperar. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a propriedade [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

>**Observação**&nbsp;&nbsp;Se o app tiver muitos complementos, uma alternativa é usar o método [GetUserCollectionWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706581.aspx) para usar paginação e retornar os resultados dos complementos.

O exemplo a seguir recupera informações para complementos duráveis com as IDs da Loja especificadas.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)

