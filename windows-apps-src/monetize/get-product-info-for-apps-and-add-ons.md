---
author: mcleanbyron
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: "Saiba como usar o namespace Windows.Services.Store para obter informações de produto relacionados à Loja para o aplicativo atual ou um de seus complementos."
title: "Obter informações do produto para aplicativos e complementos"
translationtype: Human Translation
ms.sourcegitcommit: 962bee0cae8c50407fe1509b8000dc9cf9e847f8
ms.openlocfilehash: 8471dfd24b189ff6ca4f50c4461b72ac5d81659d

---

# Obter informações do produto para aplicativos e complementos

Os aplicativos destinados ao Windows 10, versão 1607 ou posterior podem usar métodos da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para acessar informações relacionadas à Loja para o aplicativo atual ou um de seus complementos (também conhecidos como produtos no aplicativo ou IAPs). Os exemplos neste artigo a seguir demonstram como fazer isso para diferentes cenários. Para obter um exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Observação**&nbsp;&nbsp;Este artigo se refere a aplicativos direcionados ao Windows 10, versão 1607, ou posterior. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## Pré-requisitos

Esses exemplos têm os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao Windows 10, versão 1607 ou posterior.
* Você criou um aplicativo no painel do Centro de Desenvolvimento do Windows, e esse aplicativo foi publicado e disponibilizado na Loja. Pode ser um aplicativo que você deseja liberar para os clientes ou pode ser um aplicativo básico que atenda aos requisitos mínimos [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) que você esteja usando somente para testes. Para obter mais informações, consulte [diretrizes para teste](in-app-purchases-and-trials.md#testing).

O código nestes exemplos pressupõem que:
* O código seja executado no contexto de uma [Página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contenha um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` e um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

Para obter um aplicativo de exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Observação**&nbsp;&nbsp;Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado nesses exemplos para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## Obter informações para o aplicativo atual

Para obter informações da Loja sobre o aplicativo atual, use o método [GetStoreProductForCurrentAppAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getstoreproductforcurrentappasync.aspx). Isso é um método assíncrono que retorna um objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que você pode usar para obter informações, como o preço.

```csharp
private StoreContext context = null;

public async void GetAppInfo()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
        // If your app is a desktop app that uses the Desktop Bridge, you
        // may need additional code to configure the StoreContext object.
        // For more info, see https://aka.ms/storecontext-for-desktop.
    }

    // Get app store product details. Because this might take several moments,   
    // display a ProgressRing during the operation.
    workingProgressRing.IsActive = true;
    StoreProductResult queryResult = await context.GetStoreProductForCurrentAppAsync();
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    if (queryResult.Product == null)
    {
        // The Store catalog returned an unexpected result.
        textBlock.Text = "Something went wrong, the product was not returned.";
        return;
    }

    // Display the price of the app.
    textBlock.Text = $"The price of this app is: {queryResult.Product.Price.FormattedBasePrice}";
}
```

## Obter informações de produtos com IDs de Loja conhecidas

Para obter informações da Loja sobre aplicativos ou complementos cujas [IDs da Loja](in-app-purchases-and-trials.md#store_ids) você já conhece, use o método [GetStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706579.aspx). Isso é um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos aplicativos ou complementos. Além das IDs da Loja, você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos dos complementos. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a propriedade [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

O exemplo a seguir recupera informações para complementos duráveis com as IDs da Loja especificadas.

```csharp
private StoreContext context = null;

public async void GetProductInfo()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
        // If your app is a desktop app that uses the Desktop Bridge, you
        // may need additional code to configure the StoreContext object.
        // For more info, see https://aka.ms/storecontext-for-desktop.
    }

    // Specify the kinds of add-ons to retrieve.
    string[] productKinds = { "Durable" };
    List<String> filterList = new List<string>(productKinds);

    // Specify the Store IDs of the products to retrieve.
    string[] storeIds = new string[] { "9NBLGGH4TNMP", "9NBLGGH4TNMN" };

    workingProgressRing.IsActive = true;
    StoreProductQueryResult queryResult =
        await context.GetStoreProductsAsync(filterList, storeIds);
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    foreach (KeyValuePair<string, StoreProduct> item in queryResult.Products)
    {
        // Access the Store info for the product.
        StoreProduct product = item.Value;

        // Use members of the product object to access info for the product...
    }
}
```

## Obter informações para complementos que estão disponíveis para o aplicativo atual

Para obter informações da Loja sobre os complementos que estão disponíveis para o aplicativo atual, use o método [GetAssociatedStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706571.aspx). Trata-se de um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos complementos disponíveis. Você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos de complementos que você deseja recuperar. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a propriedade [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

O exemplo a seguir recupera informações para todos os complementos duráveis, complementos consumíveis gerenciados pela Loja e complementos consumíveis gerenciadas pelo desenvolvedor.

```csharp
private StoreContext context = null;

public async void GetAddOnInfo()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
        // If your app is a desktop app that uses the Desktop Bridge, you
        // may need additional code to configure the StoreContext object.
        // For more info, see https://aka.ms/storecontext-for-desktop.
    }

    // Specify the kinds of add-ons to retrieve.
    string[] productKinds = { "Durable", "Consumable", "UnmanagedConsumable" };
    List<String> filterList = new List<string>(productKinds);

    workingProgressRing.IsActive = true;
    StoreProductQueryResult queryResult = await context.GetAssociatedStoreProductsAsync(filterList);
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    foreach (KeyValuePair<string, StoreProduct> item in queryResult.Products)
    {
        // Access the Store product info for the add-on.
        StoreProduct product = item.Value;

        // Use members of the product object to access listing info for the add-on...
    }
}
```

>**Observação**&nbsp;&nbsp;Se o aplicativo tiver muitos complementos, como alternativa, você poderá usar o método [GetAssociatedStoreProductsWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706572.aspx) para usar paginação e retornar os resultados dos complementos.


## Obter informações dos complementos para o aplicativo atual que o usuário atual tem o direito de usar

Para obter informações da Loja sobre os complementos que o usuário atual tem o direito de usar, utilize o método [GetUserCollectionAsync](https://msdn.microsoft.com/library/windows/apps/mt706580.aspx). Trata-se de um método assíncrono que retorna uma coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam cada um dos complementos. Você deve passar uma lista de cadeias de caracteres para esse método que identifiquem os tipos de complementos que você deseja recuperar. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a propriedade [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

O exemplo a seguir recupera informações para complementos duráveis com as IDs da Loja especificadas.

```csharp
private StoreContext context = null;

public async void GetUserCollection()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
        // If your app is a desktop app that uses the Desktop Bridge, you
        // may need additional code to configure the StoreContext object.
        // For more info, see https://aka.ms/storecontext-for-desktop.
    }

    // Specify the kinds of add-ons to retrieve.
    string[] productKinds = { "Durable" };
    List<String> filterList = new List<string>(productKinds);

    workingProgressRing.IsActive = true;
    StoreProductQueryResult queryResult = await context.GetUserCollectionAsync(filterList);
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    foreach (KeyValuePair<string, StoreProduct> item in queryResult.Products)
    {
        StoreProduct product = item.Value;

        // Use members of the product object to access info for the product...
    }
}
```

>**Observação**&nbsp;&nbsp;Se o aplicativo tiver muitos complementos, como alternativa, você poderá usar o método [GetUserCollectionWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706581.aspx) para usar paginação e retornar os resultados dos complementos.

## Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Nov16_HO1-->


