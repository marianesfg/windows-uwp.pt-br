---
description: Descreve o esquema de dados JSON estendido para produtos da Loja no namespace Windows.Services.Store.
title: Esquemas de dados de produtos da Loja
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp, ExtendedJsonData, produtos da Store, esquema
ms.localizationpriority: medium
ms.openlocfilehash: 77f63ce409a576b3c873d95df0d2e8d0f0933808
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372531"
---
# <a name="data-schemas-for-store-products"></a>Esquemas de dados de produtos da Loja

Quando você envia um produto como um app ou um complemento para a Loja, a Loja mantém um conjunto abrangente de dados para o produto e suas licenças. No código do seu app, é possível acessar programaticamente alguns desses dados usando as propriedades no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Por exemplo, você pode recuperar a descrição e o preço do app atual ou um complemento para o app atual usando as propriedades [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Description) e [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Price).

No entanto, a maioria dos dados de produtos na Loja não é exposta pela propriedades predefinidas no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Para acessar os dados completos de um produto em seu código, você poderá usar as seguintes propriedades gerais:

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

Essas propriedades retornam os dados completos de objeto correspondente como uma cadeia de caracteres formatada em JSON. Este artigo fornece o esquema completo para os dados retornados por essas propriedades.

> [!NOTE]
> Produtos na Loja são organizados em uma hierarquia de objetos de [produto](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) e [disponibilidade](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability). Para obter mais informações, consulte [Produtos, SKUs e disponibilidades](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Esquema para StoreProduct, StoreSku, StoreAvailability, and StoreCollectionData

O esquema a seguir descreve a cadeia de caracteres formatada em JSON retornada por [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData). As propriedades [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData) e [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) retornam somente partes do esquema definido nos caminhos `DisplaySkuAvailabilities\Sku`, `DisplaySkuAvailabilities\Availabilities` e `DisplaySkuAvailabilities\Sku\CollectionData`, respectivamente.

Para obter um exemplo de uma cadeia de caracteres formatada em JSON retornada por [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData), consulte [esta seção](#product-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>Exemplo

O exemplo a seguir demonstra uma cadeia de caracteres formatada em JSON retornada pela propriedade [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) para o app.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Esquema para StoreAppLicense e StoreLicense

O esquema a seguir descreve a cadeia de caracteres formatada em JSON retornada por [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData). A propriedade [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData) retorna somente as partes do esquema definidas no caminho `productAddOns`.

Para obter um exemplo de uma cadeia de caracteres formatada em JSON retornada por [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData), consulte [esta seção](#license-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>Exemplo

O exemplo a seguir demonstra uma cadeia de caracteres formatada em JSON retornada pela propriedade [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) para o app.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Esquema para StorePurchaseProperties

O esquema a seguir descreve a cadeia de caracteres formatada em JSON retornada por [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações sobre produtos para os aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras no aplicativo de aplicativos e complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
