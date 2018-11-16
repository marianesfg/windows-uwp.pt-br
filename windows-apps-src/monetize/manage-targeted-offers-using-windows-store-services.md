---
author: Xansky
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Use a API de ofertas segmentadas da Microsoft Store para reivindicar ofertas direcionadas disponíveis para o usuário atual do seu app.
title: Gerencie ofertas direcionadas usando os serviços da loja
ms.author: mhopkins
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, uwp, serviços da Store, API de ofertas direcionadas da Microsoft Store, ofertas direcionadas
ms.localizationpriority: medium
ms.openlocfilehash: dbfefefdb7f7b96dbe99b35656b610b393ab3afa
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6968090"
---
# <a name="manage-targeted-offers-using-store-services"></a>Gerenciar ofertas direcionadas usando os serviços da Loja

Se você criar uma *oferta direcionada* no **participar > ofertas direcionadas** página do seu aplicativo no Partner Center, use a *API de ofertas direcionadas da Microsoft Store* no código do seu aplicativo para recuperar informações que ajuda você a implementar a experiência no aplicativo para o oferta direcionada. Para obter mais informações sobre ofertas direcionadas e como criá-las no painel, consulte [Use ofertas direcionadas para maximizar o engajamento e as conversões](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

A API de ofertas direcionadas é uma API REST simples que você pode usar para obter as ofertas segmentadas disponíveis para o usuário atual, com base no fato de o usuário fazer parte do segmento de clientes ou não para a oferta segmentada. Para usar esta API no código do seu aplicativo, siga estas etapas:

1.  [Obtenha um token da conta da Microsoft](#obtain-a-microsoft-account-token) para o usuário atual conectado do seu aplicativo.
2.  [Obtenha as ofertas direcionadas para o usuário atual](#get-targeted-offers).
3.  Implemente a experiência de compra realizada em aplicativo para o complemento associado a uma das ofertas direcionadas. Para obter mais informações sobre como implementar compras realizadas em aplicativo, consulte [este artigo](enable-in-app-purchases-of-apps-and-add-ons.md).

Para obter um exemplo de código completo que demonstre todas estas etapas, consulte o [exemplo de código](#code-example) no final deste artigo. As seções a seguir fornecem mais detalhes sobre cada etapa.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtenha uma conta da Microsoft para o usuário atual

No código do seu aplicativo, obtenha um token da Conta da Microsoft (MSA) para o usuário conectado no momento. Você deve passar esse token no cabeçalho de solicitação ```Authorization``` para a API de ofertas direcionadas da Microsoft Store. Este token é usado pela Store para recuperar as ofertas direcionadas disponíveis para o usuário atual.

Para obter o token MSA, use a classe [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) para solicitar um token usando o escopo ```devcenter_implicit.basic,wl.basic```. O exemplo a seguir demonstra como fazer isso. Este exemplo é um trecho do [exemplo completo](#code-example) e requer as instruções **using** fornecidas no exemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Para obter mais informações sobre como obter os tokens MSA, consulte [Gerenciador de contas da Web ](../security/web-account-manager.md).

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtenha as ofertas direcionadas para o usuário atual

Depois de ter um token MSA para o usuário atual, chame o método GET do URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para obter as ofertas direcionadas disponíveis para o usuário atual. Para obter mais informações sobre esse método REST, consulte [Obter ofertas direcionadas](get-targeted-offers.md).

Esse método retorna as IDs do produto dos complementos que estão associados às ofertas direcionadas disponíveis para o usuário atual. Com esta informação, você pode oferecer uma ou mais ofertas direcionadas como uma compra no aplicativo ao usuário.

O exemplo a seguir demonstra como obter as ofertas direcionadas para o usuário atual. Este exemplo é um trecho do [exemplo completo](#code-example). Isso requer a biblioteca [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft e as classes adicionais, e as instruções **using** fornecidas no exemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>Código de exemplo completo

O exemplo de código a seguir demonstra as seguintes tarefas:

* Obtenha um token MSA token para o usuário atual.
* Obtenha todas as ofertas direcionadas para o usuário atual usando o método [Obter ofertas direcionadas](get-targeted-offers.md).
* Compre o complemento que está associado a uma oferta direcionada.

Este exemplo exige a biblioteca [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft. O exemplo usa esta biblioteca para serializar e desserializar dados formatados em JSON.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Tópicos relacionados

* [Usar ofertas direcionadas para maximizar o envolvimento e as conversões](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Receber as ofertas direcionadas](get-targeted-offers.md)
