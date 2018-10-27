---
author: Xansky
description: Saiba como usar o namespace Windows.Services.Store para implementar complementos de assinatura.
title: Habilitar complementos de assinatura para o app
keywords: windows 10, uwp, assinaturas, complementos, compras no aplicativo, IAPs, Windows.Services.Store
ms.author: mhopkins
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b89cad4c299f7326d0bb7d9ea4b8c6685f70f26c
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5707541"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Habilitar complementos de assinatura para o app

O aplicativo UWP pode oferecer compras no aplicativo de complementos de *assinatura* para seus clientes. Você pode usar assinaturas para vender produtos digitais em seu app (como os recursos do app ou conteúdo digital) com períodos de cobrança recorrentes automatizados.

> [!NOTE]
> Para habilitar a compra de complementos de assinatura no aplicativo, o projeto deve ser direcionada para o **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio (isso corresponde ao Windows 10, versão 1607), e ele deve usar as APIs no namespace **Windows.Services.Store** para implementar a experiência de compra realizada em aplicativo, em vez do namespace **ApplicationModel**. Para obter mais informações sobre as diferenças entre esses namespaces, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).

## <a name="feature-highlights"></a>Destaques do recurso

Os complementos de assinatura para aplicativos UWP dão suporte aos seguintes recursos:

* Você pode escolher entre períodos de assinatura de um mês, três meses, seis meses, um ano ou dois anos.
* Você pode adicionar períodos de avaliação gratuita de uma semana ou de um mês à sua assinatura.
* O SDK do Windows [fornece APIs](#code-examples) que você pode usar em seu app para obter informações sobre os complementos de assinatura disponíveis para o aplicativo e habilitar a compra de um complemento de assinatura. Também fornecemos APIs REST que você pode chamar dos seus serviços para [gerenciar assinaturas para um usuário](#manage-subscriptions).
* Você pode exibir relatórios analíticos que fornecem o número de aquisições de assinatura, os assinantes ativos e as assinaturas canceladas em um determinado período de tempo.
* Os clientes podem gerenciar suas assinaturas na página [http://account.microsoft.com/services](http://account.microsoft.com/services) da conta da Microsoft. Os clientes podem usar essa página para exibir todas as assinaturas que eles adquiriram, cancelar uma assinatura e alterar a forma de pagamento associada à sua assinatura.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Etapas para habilitar um complemento de assinatura para o app

Para habilitar a compra de complementos de assinatura em seu app, siga estas etapas.

1. [Crie um envio de complemento](../publish/add-on-submissions.md) para sua assinatura no painel do Centro de Desenvolvimento e publique o envio. Conforme você segue o processo de envio de complemento, preste atenção às seguintes propriedades:

    * [Tipo de produto](../publish/set-your-add-on-product-id.md#product-type): selecione **Assinatura**.

    * [Período de assinatura](../publish/enter-add-on-properties.md#subscription-period): escolha o período de cobrança recorrente para sua assinatura. Você não poderá alterar o período de assinatura depois de publicar o complemento.

        Cada complemento de assinatura dá suporte a um único período de assinatura e período de avaliação. Você deve criar um complemento de assinatura diferente para cada tipo de assinatura que deseja oferecer em seu app. Por exemplo, se você quiser oferecer uma assinatura mensal sem avaliação, uma assinatura mensal com uma versão de avaliação de um mês, uma assinatura anual sem avaliação e uma assinatura anual com uma versão de avaliação de um mês, precisará criar quatro complementos de assinatura.

    * [Período de avaliação](../publish/enter-add-on-properties.md#free-trial): considere a escolha de um período de avaliação de uma semana ou de um mês para sua assinatura para permitir que os usuários experimentem o conteúdo da assinatura antes de comprá-la. Você não poderá alterar ou remover o período de assinatura depois de publicar o complemento da assinatura.

        Para adquirir uma avaliação gratuita de sua assinatura, um usuário precisará comprar sua assinatura por meio do processo padrão de compra no aplicativo, incluindo uma forma de pagamento válida. Eles não são cobrados durante o período de avaliação. No final do período de avaliação, a assinatura é automaticamente convertida na assinatura completa e o instrumento de pagamento do usuário será cobrado pelo primeiro período da assinatura paga. Se o usuário optar por cancelar sua assinatura durante o período de avaliação, a assinatura permanecerá ativa até o final do período de avaliação. Alguns períodos de avaliação não estão disponíveis para todos os períodos de assinatura.

        > [!NOTE]
        > Cada cliente pode adquirir uma avaliação gratuita para um complemento de assinatura somente uma vez. Depois que um cliente adquire uma avaliação gratuita de uma assinatura, a Store impede que o mesmo cliente adquira a mesma assinatura de avaliação gratuita novamente.

    * [Visibilidade](../publish/set-add-on-pricing-and-availability.md#visibility): se você estiver criando um complemento de teste que usará somente para testar a experiência de compra no aplicativo para a sua assinatura, nós recomendamos que você selecione uma das opções **Oculto na Store**. Caso contrário, você pode selecionar a melhor opção de visibilidade para seu cenário.

    * [Preço](../publish/set-add-on-pricing-and-availability.md?#pricing): escolha o preço de sua assinatura nesta seção. Você não poderá elevar o preço da assinatura depois de publicar o complemento. No entanto, você poderá reduzir o preço mais tarde.
        > [!IMPORTANT]
        > Por padrão, quando você criar qualquer complemento, o preço será inicialmente definido como **Grátis**. Como você não pode aumentar o preço de um complemento de assinatura depois de concluir o envio de complemento, escolha o preço de sua assinatura aqui.

2. Em seu app, use as APIs no namespace [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) para determinar se o usuário atual já adquiriu o complemento de assinatura e então oferecê-lo para venda ao usuário como uma compra no aplicativo. Consulte os [exemplos de código](#code-examples) neste artigo para obter mais detalhes.

3. Teste a implementação de compra no aplicativo de sua assinatura em seu app. Será necessário baixar seu app uma vez da Store para seu dispositivo de desenvolvimento para usar sua licença para teste. Para obter mais informações, consulte nossas [diretrizes de teste](in-app-purchases-and-trials.md#testing) para compras no aplicativo.  

4. Criar e publicar um envio de aplicativo que inclui o pacote do app atualizado, incluindo seu código testado. Para obter mais informações, consulte [Envios de aplicativo](../publish/app-submissions.md).

<span id="code-examples"/>

## <a name="code-examples"></a>Exemplos de código

Os exemplos de código nesta seção demonstram como usar as APIs no namespace [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) para obter informações sobre os complementos de assinatura para o app atual e solicitar a compra de um complemento de assinatura em nome do usuário atual.

Esses exemplos têm os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior.
* Você [criou um envio de aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-submissions) no painel do Centro de Desenvolvimento do Windows e esse app foi publicado e disponibilizado na Microsoft Store. Opcionalmente, é possível configurar o app para que ele não possa ser descoberto na Store enquanto você o testa. Para obter mais informações, consulte [diretrizes para teste](in-app-purchases-and-trials.md#testing).
* Você [criou um complemento de assinatura para o app](../publish/add-on-submissions.md) no painel do Centro de Desenvolvimento.

O código nestes exemplos pressupõe que:
* O arquivo de código tenha instruções **using** para **Windows.Services.Store** e namespaces **System.Threading.Tasks**.
* O app seja um app de usuário único executado somente no contexto do usuário que o iniciou. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Se você tiver um aplicativo da área de trabalho que utilize a [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado nestes exemplos para configurar o objeto [**StoreContext**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext). Para obter mais informações, consulte [Como usar a classe StoreContext em um aplicativo da área de trabalho que usa a Ponte de Desktop](in-app-purchases-and-trials.md#desktop).

### <a name="purchase-a-subscription-add-on"></a>Comprar um complemento de assinatura

Este exemplo demonstra como solicitar a compra de um complemento de assinatura conhecido para o seu app em nome do cliente atual. Este exemplo também mostra como tratar o caso em que a assinatura tem um período de avaliação.

1. O código primeiro determina se o cliente já tem uma licença ativa da assinatura. Se o cliente já tiver uma licença ativa, seu código deverá desbloquear os recursos de assinatura conforme necessário (como isso é proprietário do seu app, é identificado com um comentário no exemplo).
2. Em seguida, o código obtém o objeto [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) que representa a assinatura que você deseja comprar em nome do cliente. O código presume que você já conheça a [ID da Store](in-app-purchases-and-trials.md#store-ids) do complemento da assinatura que deseja comprar e que você atribuiu esse valor à variável *subscriptionStoreId*.
3. O código determina então se uma avaliação está disponível para a assinatura. Opcionalmente, seu aplicativo pode usar essas informações para exibir detalhes sobre a avaliação disponível ou a assinatura completa para o cliente.
4. Por fim, o código chama o método [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) para solicitar a compra da assinatura. Se houver uma avaliação disponível para a assinatura, a avaliação será oferecida ao cliente para compra. Caso contrário, a assinatura completa será oferecida para compra.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Obter informações sobre complementos de assinatura para o app atual

Este exemplo de código demonstra como obter informações para todos os complementos de assinatura que estão disponíveis em seu app. Para obter essas informações, primeiro use o método [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) para obter a coleção de objetos [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) que representam cada um dos complementos disponíveis para o app. Em seguida, obtenha a [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) para cada produto e use as propriedades [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.IsSubscription) e [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.SubscriptionInfo) para acessar as informações da assinatura.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>Gerenciar assinaturas de seus serviços

Depois que seu app atualizado estiver na Store e os clientes puderem comprar seu complemento de assinatura, talvez haja cenários onde será necessário gerenciar a assinatura para um cliente. Nós fornecemos APIs REST que você pode chamar de seus serviços para realizar as seguintes tarefas de gerenciamento de assinatura:

* [Obtenha as assinaturas que um usuário tem um direito de usar](get-subscriptions-for-a-user.md). Se sua assinatura fizer parte de um serviço de plataforma cruzada, você poderá chamar essa API para determinar se o usuário especificado tem direito à sua assinatura e o status da assinatura no contexto de seu aplicativo UWP. Então você poderá usar essas informações para atualizar o status da assinatura em outras plataformas compatíveis com o seu serviço.

* [Alterar o estado de cobrança de uma assinatura para um determinado usuário](change-the-billing-state-of-a-subscription-for-a-user.md). Use esta API para cancelar, estender ou desabilitar a renovação automática de uma assinatura.

## <a name="cancellations"></a>Cancelamentos

Os clientes podem usar a página [http://account.microsoft.com/services](http://account.microsoft.com/services) da conta da Microsoft para exibir todas as assinaturas que eles adquiriram, cancelar uma assinatura e alterar a forma de pagamento associada à assinatura. Quando um cliente cancela uma assinatura usando esta página, ele continuam a ter acesso à assinatura durante o período de cobrança atual. Ele não obtém reembolso de qualquer parte do período de cobrança atual. No final do período de cobrança atual, a assinatura é desativada.

Você também pode cancelar uma assinatura em nome de um usuário usando a API REST para [alterar o estado de cobrança de uma assinatura para um determinado usuário](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="subscription-renewals-and-grace-periods"></a>Períodos de carência e renovações de assinatura

Em algum momento durante cada período de cobrança, tentaremos cobrar o cartão de crédito do cliente pelo próximo período de cobrança. Se a cobrança falhar, a assinatura do cliente entrará no estado de *cobrança*. Isso significa que suas assinaturas ainda estarão ativas pelo restante do período de cobrança atual, mas nós tentaremos periodicamente cobrar do cartão de crédito para renovarmos automaticamente a assinatura. Esse estado pode durar até duas semanas, até o final do período de cobrança e a data de renovação para o próximo período de cobrança atual.

Não oferecemos períodos de carência para cobrança de assinatura. Se não pudermos cobrar o cartão de crédito do cliente até o final do período de cobrança atual, a assinatura será cancelada e o cliente não terá mais acesso à assinatura após o período de cobrança atual.

## <a name="unsupported-scenarios"></a>Cenários sem suporte

No momento, os cenários a seguir não têm suporte para complementos de assinatura.

* No momento, não há suporte para a venda de assinaturas para clientes diretamente por meio da Store. As assinaturas só estão disponíveis para compras no aplicativo de produtos digitais.
* Os clientes não podem trocar de períodos de assinatura usando a página [http://account.microsoft.com/services](http://account.microsoft.com/services) para a conta da Microsoft. Para alternar para um período de assinatura diferente, os clientes devem cancelar sua assinatura atual e então comprar uma assinatura com um período de assinatura diferente do seu aplicativo.
* No momento, a troca de camada não tem suporte para complementos de assinatura (por exemplo, alternar um cliente de uma assinatura básica para uma assinatura premium com mais recursos).
* No momento, não há suporte a [vendas](../publish/put-apps-and-add-ons-on-sale.md) e [códigos promocionais](../publish/generate-promotional-codes.md) para complementos de assinatura.


## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
