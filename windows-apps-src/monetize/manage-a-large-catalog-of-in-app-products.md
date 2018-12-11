---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Se o seu app oferecer um catálogo abrangente de produtos no aplicativo, você também poderá seguir o processo descrito neste tópico para ajudar a gerenciar seu catálogo.
title: Gerenciar um catálogo abrangente de produtos no app
ms.date: 08/25/2017
ms.topic: article
keywords: windows 10, uwp, compras no aplicativo, IAPs, complementos, catálogo, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: 2335e09253570d09c33422d2f5ba4179697e4ea7
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8899431"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>Gerenciar um catálogo abrangente de produtos no aplicativo

Se o seu app oferecer um catálogo abrangente de produtos no aplicativo, você também poderá seguir o processo descrito neste tópico para ajudar a gerenciar seu catálogo. Em versões anteriores ao Windows 10, a Loja tem um limite de 200 listagens de produtos por conta de desenvolvedor, e o processo descrito neste tópico poderá ser usado para contornar essa limitação. Começando com Windows 10, a loja não tem limite para o número de listagens de produtos por conta de desenvolvedor, e o processo descrito neste artigo não é mais necessário.

> [!IMPORTANT]
> Este artigo demonstra como usar membros do namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Esse namespace não está sendo atualizado com os novos recursos e recomendamos que você use o namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) em vez disso. O namespace **Windows.Services.Store** dá suporte a tipos de complemento mais recentes, como complementos consumíveis gerenciado pela loja e assinaturas e foi projetado para ser compatível com futuros tipos de produtos e recursos compatíveis com o Partner Center e o armazenamento. O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).

Para habilitar esse recurso, você criará algumas entradas de produtos para faixas de preços específicos, em que cada uma poderá representar centenas de produtos em um catálogo. Use a sobrecarga do método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) que especifica a oferta definida em aplicativo associada a um produto no aplicativo listado na Loja. Além de especificar uma oferta e uma associação ao produto durante a chamada, seu aplicativo deve também passar um objeto [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384) que contenha os detalhes da oferta de um catálogo abrangente. Se esses detalhes não forem fornecidos, os detalhes para o produto listado serão usados no lugar.

A Loja usará somente a *offerId* da solicitação de compra nos [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) resultantes. Esse processo não modifica diretamente as informações originalmente fornecidas ao [listar o produto no aplicativo na Loja](../publish/add-on-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

-   Esse tópico abrange o suporte da Loja para a representação de várias ofertas no aplicativo usando um único produto no aplicativo listado na Loja. Se você não tiver familiaridade com compras no próprio aplicativo, revise [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md) para saber mais sobre informações de licença e como listar adequadamente sua compra no próprio aplicativo na Windows Store.
-   Ao codificar e testar novas ofertas no aplicativo pela primeira vez, use o objeto [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) em vez do objeto [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças, em vez de chamar o servidor ativo. Para fazer isso, você precisa personalizar o arquivo chamado WindowsStoreProxy.xml em %userprofile%\\AppData\\local\\packages\\&lt;nome do pacote&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu app pela primeira vez, mas também é possível carregar um arquivo personalizado em tempo de execução. Para obter mais informações, consulte [Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tópico também faz referência a exemplos de código fornecidos no [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Este exemplo é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## <a name="make-the-purchase-request-for-the-in-app-product"></a>Fazer a solicitação de compra para o produto no aplicativo

A solicitação de compra para um produto específico em um catálogo abrangente é efetuada da mesma maneira como qualquer outra solicitação de compra em um aplicativo. Quando o aplicativo chama a nova sobrecarga do método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), o aplicativo fornece *OfferId* e um objeto [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263390) populado com o nome do produto no aplicativo.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>Relatar o atendimento da oferta no aplicativo

O aplicativo precisará relatar o atendimento do produto para a Loja quando a oferta tiver sido atendida localmente. Em um cenário de catálogo abrangente, se o aplicativo não relatar o atendimento da oferta, o usuário não poderá comprar qualquer oferta no aplicativo usando a mesma lista de produtos da Loja.

Como mencionado antes, a Loja usa apenas a informação da oferta fornecida para preencher os [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392), e não cria uma associação persistente entre uma oferta de catálogo abrangente e uma lista de produtos da Loja. Como resultado, você precisa rastrear os direitos do usuário para produtos, e fornecer um contexto específico do produto (como o nome do item comprado ou os detalhes sobre ele) para o usuário fora da operação [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync).

O seguinte código demonstra a chamada de atendimento, e um padrão de mensagem de IU onde a informação sobre a oferta específica é inserida. Na ausência dessas informações de produto específicas, o exemplo usará informações do produto [ListingInformation](https://msdn.microsoft.com/library/windows/apps/br225163).

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>Tópicos relacionados

* [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md)
* [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md)
* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384)
