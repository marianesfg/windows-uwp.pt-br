---
author: mcleanbyron
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: "Use a API de ofertas segmentadas do Windows Store para reivindicar ofertas direcionadas disponíveis para um aplicativo."
title: "Gerencie ofertas direcionadas usando os serviços da loja"
ms.author: mcleans
ms.date: 05/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, serviços de loja, ofertas direcionadas para o Windows Store API, ofertas direcionadas"
ms.openlocfilehash: 684c37d4439f415ad607b7f3e6a166966cc9f835
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2017
---
# <a name="manage-targeted-offers-using-store-services"></a>Gerenciar ofertas direcionadas usando os serviços da Loja

Se você criar uma *oferta direcionada* na página **Participar > Ofertas direcionadas** do seu aplicativo no painel Centro de Desenvolvimento do Windows, use a *API de ofertas direcionadas da Windows Store* no código do seu aplicativo para implementar a experiência no aplicativo para a oferta de destino. Para obter mais informações sobre ofertas direcionadas e como criá-las no painel, consulte [Use ofertas direcionadas para maximizar o engajamento e as conversões](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

A API de ofertas direcionadas é uma API REST que você pode usar para executar essas tarefas:

* Obtenha as ofertas segmentadas disponíveis para o usuário atual, com base no fato de o usuário fazer parte do segmento de clientes ou não para a oferta segmentada.
* Se o usuário comprar a oferta direcionada, você pode enviar uma reivindicação à Loja para que você possa receber o bônus associado à oferta segmentada. Isso só é necessário se a oferta direcionada estiver associada a uma promoção patrocinada pela Microsoft que pague um bônus aos desenvolvedores para cada conversão bem-sucedida.

Para usar esta API no código do seu aplicativo, siga estas etapas:

1.  [Obtenha um token da conta da Microsoft](#obtain-a-microsoft-account-token) para o usuário atual conectado do seu aplicativo.
2.  [Obtenha as ofertas direcionadas para o usuário atual](#get-targeted-offers).
3.  [Compre o complemento que está associado uma oferta direcionada](#purchase-add-on).
3.  [Declaração da oferta direcionada](#claim-targeted-offer) (se a oferta direcionada estiver associada a uma promoção patrocinada pela Microsoft que pague um bônus aos desenvolvedores por cada conversão bem-sucedida).

> [!NOTE]
> A capacidade de associar uma oferta direcionada com uma promoção patrocinada pela Microsoft e então enviar uma reivindicação para a oferta atualmente não está disponível para todas as contas de desenvolvedor.

Para obter um exemplo de código completo que demonstre todas estas etapas, consulte o [exemplo de código](#code-example) no final deste artigo. As seções a seguir fornecem mais detalhes sobre cada etapa.

<span id="obtain-a-microsoft-account-token" />
## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtenha uma conta da Microsoft para o usuário atual

No código do seu aplicativo, obtenha um token da conta da Microsoft (MSA) para o usuário conectado no momento. Você deve passar esse token no cabeçalho de solicitação ```Authorization``` para cada um dos métodos na API de ofertas direcionadas da Windows Store. Este token é usado pela loja para recuperar as ofertas direcionadas disponíveis para o usuário atual.

Para obter o token MSA, use a classe [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) para solicitar um token usando o escopo ```devcenter_implicit.basic,wl.basic```. O exemplo a seguir demonstra como fazer isso. Este exemplo é um trecho do [exemplo completo](#code-example) e requer as instruções **using** fornecidas no exemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Para obter mais informações sobre como obter os tokens MSA, consulte [Gerenciador de contas da Web ](../security/web-account-manager.md).

<span id="get-targeted-offers" />
## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtenha as ofertas direcionadas para o usuário atual

Depois de ter um token MSA para o usuário atual, chame o método GET do URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para obter as ofertas direcionadas disponíveis para o usuário atual. Para obter mais informações sobre esse método REST, consulte [Obter ofertas direcionadas](get-targeted-offers.md).

Esse método retorna as IDs do produto dos complementos que estão associados às ofertas direcionadas disponíveis para o usuário atual. Com esta informação, você pode oferecer uma ou mais ofertas direcionadas como uma compra no aplicativo ao usuário. Este método também retorna uma ID de rastreamento que você pode usar para [enviar uma reivindicação](#claim-targeted-offer) para a loja para que você possa receber qualquer bônus que está associado a uma das ofertas direcionadas.

O exemplo a seguir demonstra como obter as ofertas direcionadas para o usuário atual. Este exemplo é um trecho do [exemplo completo](#code-example). Isso requer a biblioteca [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft e as classes adicionais, e as instruções **using** fornecidas no exemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="purchase-add-on" />
## <a name="purchase-the-add-on-that-is-associated-with-a-targeted-offer"></a>Compre o complemento que está associado uma oferta direcionada

Em seguida, ofereça uma das ofertas direcionadas para compra ao usuário. Se o usuário concordar em comprar a oferta direcionada, use um dos seguintes métodos para adquirir programaticamente o complemento associado à oferta segmentada. Use os valores de ID do produto que você obteve no passo anterior para identificar o complemento que deseja comprar.

* Se o seu aplicativo tiver como alvo o Windows 10, versão 1607 ou posterior, recomendamos que você use um dos métodos **RequestPurchaseAsync** no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store). Para obter mais informações sobre como usar esses métodos, consulte [Habilitar compras no aplicativo de aplicativos e complementos](enable-in-app-purchases-of-apps-and-add-ons.md).

* Se o seu aplicativo tiver como alvo uma versão anterior do Windows 10, use o método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) no namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para comprar o complemento. Para obter mais informações sobre como usar esse método, consulte [Habilitar compras de produto no aplicativo](enable-in-app-product-purchases.md).

Para obter um exemplos de código que demonstrem cada opção, consulte o [exemplo de código](#code-example) no final deste artigo.

> [!NOTE]
> Existem dois namespaces diferentes para implementar compras no aplicativo em um aplicativo UWP: [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) (introduzido no Windows 10, versão 1607) e [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) (disponível em todas as versões do Windows 10). Para obter mais informações sobre as diferenças entre esses namespaces, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).

<span id="claim-targeted-offer" />
## <a name="submit-a-claim-for-a-targeted-offer"></a>Enviar uma reivindicação para uma oferta direcionada

Se o usuário comprar uma oferta direcionada associada a uma promoção patrocinada pela Microsoft que paga um bônus aos desenvolvedores para cada conversão bem-sucedida, você deverá enviar uma reivindicação à Loja para que você possa receber seu bônus. Quando você enviar sua reivindicação, deverá fornecer um valor que represente a confirmação de compra. A maneira como você obtém essa confirmação de compra dependerá de o app usar APIs no namespace **ApplicationModel** ou no namespace **Windows.Services.Store** para comprar o complemento.

> [!NOTE]
> No momento, a capacidade de associar uma oferta direcionada com uma promoção patrocinada pela Microsoft e então enviar uma reivindicação para a oferta não está disponível para todas as contas de desenvolvedor.

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Apps que usam o namespace Windows.ApplicationModel.Store

1. Depois de comprar o complemento usando o método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) no namespace **Windows.ApplicationModel.Store**, verifique se você recebeu um [recibo de compra](use-receipts-to-verify-product-purchases.md). Esse recibo está disponível no objeto [PurchaseResults.ReceiptXml](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults#Windows_ApplicationModel_Store_PurchaseResults_ReceiptXml_), retornado pelo método **RequestProductPurchaseAsync**. Esse recibo representa a confirmação de compra que você usará na próxima etapa.

2. Envie uma mensagem POST para o URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para enviar uma reivindicação para a conversão da oferta direcionada. Para obter mais informações sobre esse método REST, consulte [Reivindicar uma oferta direcionada](claim-a-targeted-offer.md). No corpo da solicitação, passe os seguintes dados:

  * Campo *storeOffer*: passe um dos objetos recebidos no corpo da resposta do método [Get targeted offers](get-targeted-offers.md) (esse objeto deve incluir os campos *ofertas* e *trackingId*).

  * Campo *receipt*: passe a cadeia de caracteres do recibo recuperado na etapa anterior (isso está disponível no objeto **PurchaseResults.ReceiptXml** retornado pelo método **RequestProductPurchaseAsync**).

O exemplo a seguir demonstra como comprar e reivindicar uma oferta direcionada usando as APIs no namespace **ApplicationModel**. Este exemplo é um trecho do [exemplo completo](#code-example). Isso requer a biblioteca [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft e as classes adicionais, e as instruções **using** fornecidas no exemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnAnyVersionWindows10)]

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Apps que usam o namespace Windows.Services.Store

1. Após a compra do complemento usando um dos métodos **RequestPurchaseAsync** no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), obtenha a *ID do pedido* da compra ao seguir estas etapas. Esse valor representa a confirmação de compra.

  1. Chame o método [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetUserCollectionAsync_Windows_Foundation_Collections_IIterable_System_String__) para obter a coleção de objetos [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representam os complementos adquiridos pelo usuário.

  2. Neste conjunto, obtenha o objeto **StoreProduct** que representa o complemento comprado para a oferta direcionada.

  3. Obtenha o valor da propriedade [ExtendedJsonData](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) desse objeto **StoreProduct**. Essa propriedade retorna uma cadeia de caracteres formatada em JSON que contém os dados completos relacionados à Loja para o complemento.

  4. Analise essa cadeia de caracteres JSON e recupere o valor do campo *orderId* na cadeia de caracteres.

2. Envie uma mensagem POST para o URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para enviar uma reivindicação para a conversão da oferta direcionada. Para obter mais informações sobre esse método REST, consulte [Reivindicar uma oferta direcionada](claim-a-targeted-offer.md). No corpo da solicitação, passe os seguintes objetos:

  * Campo *storeOffer*: passe um dos objetos recebidos no corpo da resposta do método [Get targeted offers](get-targeted-offers.md) (esse objeto deve incluir os campos *ofertas* e *trackingId*).

  * Campo *receipt*: passe o valor de *orderId* recuperado na etapa anterior.

O exemplo a seguir demonstra como comprar e reivindicar uma oferta direcionada usando as APIs no namespace **Windows.Services.Store**. Este exemplo é um trecho do [exemplo completo](#code-example). Isso requer a biblioteca [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft e as classes adicionais, e as instruções **using** fornecidas no exemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnWindows1607AndLater)]

<span id="code-example" />
## <a name="complete-code-example"></a>Código de exemplo completo

O exemplo de código a seguir demonstra as seguintes tarefas:

* Obtenha um token MSA token para o usuário atual.
* Obtenha todas as ofertas direcionadas para o usuário atual usando o método [Obter ofertas direcionadas](get-targeted-offers.md).
* Compre o complemento que está associado uma oferta direcionada.
* Reivindique a oferta direcionada usando o método [Reivindicar uma oferta segmentada](claim-a-targeted-offer.md).

Este exemplo exige a biblioteca [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft. O exemplo usa esta biblioteca para serializar e desserializar dados formatados em JSON.

> [!NOTE]
> Existem dois namespaces diferentes para implementar compras no aplicativo em um aplicativo UWP: [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) (introduzido no Windows 10, versão 1607) e [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) (disponível em todas as versões do Windows 10). Este exemplo usa [código adaptável de versão](../debug-test-perf/version-adaptive-code.md) para usar ambos os namespaces no mesmo app para adquirir um complemento e enviar uma reivindicação para a oferta direcionada. Para usar esse código, a versão do sistema operacional de destino do seu projeto deve ser **1Windows 10 Anniversary Edition (10.0; Build 14394)** ou posterior, embora a versão mínima do sistema operacional possa ser uma versão anterior se você quiser que seu app seja executado em versões anteriores do Windows 10. Para obter mais informações sobre as diferenças entre esses namespaces, consulte [Compras no app e avaliações](in-app-purchases-and-trials.md).

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Tópicos relacionados

* [Usar ofertas direcionadas para maximizar o envolvimento e as conversões](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Receber as ofertas direcionadas](get-targeted-offers.md)
* [Reivindicar uma oferta direcionada](claim-a-targeted-offer.md)
