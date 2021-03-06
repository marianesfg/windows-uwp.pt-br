---
Description: Seja seu app gratuito ou não, você pode vender conteúdo, outros apps ou uma nova funcionalidade do app (como o desbloqueio do próximo nível de um jogo) no próprio app. Consulte a seguir como habilitar esses produtos em seu aplicativo.
title: Habilitar compras de produtos no aplicativo
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: uwp, complementos, compras no aplicativo, IAPs, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 44cc0674e98c2fdf1bf8ecd2fbf6f859dfe25e62
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371852"
---
# <a name="enable-in-app-product-purchases"></a>Habilitar compras de produtos no aplicativo

Seja seu app gratuito ou não, você pode vender conteúdo, outros apps ou uma nova funcionalidade do app (como o desbloqueio do próximo nível de um jogo) no próprio app. Consulte a seguir como habilitar esses produtos em seu aplicativo.

> [!IMPORTANT]
> Este artigo demonstra como usar os membros do namespace [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para habilitar compras de produto no aplicativo. Esse namespace não está sendo atualizado com os novos recursos e recomendamos que você use o namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) em vez disso. O **Windows.Services.Store** namespace oferece suporte a tipos de complemento mais recente, como gerenciado pelo Store consumíveis complementos e assinaturas e é projetado para ser compatível com tipos futuros de produtos e recursos suportados pelo parceiro Centro e a Store. O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Para obter informações sobre como habilitar as compras de produtos no aplicativo usando o namespace **Windows.Services.Store**, consulte [este artigo](enable-in-app-purchases-of-apps-and-add-ons.md).

> [!NOTE]
> Os produtos no aplicativo não podem ser oferecidos em uma versão de avaliação do aplicativo. Os clientes que usam uma versão de avaliação do aplicativo só poderão comprar um produto no aplicativo se comprarem a versão completa do seu aplicativo.

## <a name="prerequisites"></a>Pré-requisitos

-   Um aplicativo do Windows no qual devem ser adicionados os recursos que os clientes podem comprar.
-   Ao codificar e testar novos produtos no aplicativo pela primeira vez, você deve usar o objeto [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) em vez do objeto [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças, em vez de chamar o servidor ativo. Para fazer isso, você precisará personalizar o arquivo nomeado WindowsStoreProxy.xml em % userprofile %\\AppData\\local\\pacotes\\&lt;nome do pacote&gt;\\LocalState\\ Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu app pela primeira vez, mas também é possível carregar um arquivo personalizado em tempo de execução. Para obter mais informações, consulte [Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tópico também faz referência a exemplos de código fornecidos no [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Este exemplo é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Etapa 1: Inicializar as informações de licença para seu aplicativo

Quando seu app estiver em processo de inicialização, obtenha o objeto [LicenseInformation](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) para seu app inicializando [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) ou [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) para habilitar compras de um produto no aplicativo.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Etapa 2: Adicione as ofertas no aplicativo ao seu aplicativo

Para cada recurso a ser disponibilizado por meio de uma transação de produto no aplicativo, crie uma oferta e adicione-a ao seu app.

> [!IMPORTANT]
> Você deve adicionar todos os produtos no aplicativo que deseja apresentar para seus clientes antes de enviá-lo para a Loja. Para adicionar novos produtos no aplicativo depois, você deve atualizar o aplicativo e reenviar uma nova versão.

1.  **Criar um token de oferta no aplicativo**

    Você pode identificar cada produto no seu aplicativo por um token. Esse token é uma cadeia de caracteres que você define e usa no aplicativo e na Loja para identificar um produto no aplicativo específico. Dê (ao aplicativo) um nome exclusivo e significativo, para poder identificar o recurso correto que ele representa durante a codificação. Este são alguns exemplos de nomes:

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > O token de oferta no aplicativo que você usa em seu código deve corresponder a [ID do produto](../publish/set-your-add-on-product-id.md#product-id) valor especificado quando você [definem o complemento correspondente para o seu aplicativo no Partner Center](../publish/add-on-submissions.md).

2.  **O recurso em um bloco condicional do código**

    Coloque o código de cada recurso associado a um produto no aplicativo em um bloco de condições que testa se o cliente tem uma licença para usar esse recurso.

    Veja a seguir um exemplo que mostra como é possível codificar um recurso de produto chamado **featureName** em um bloco condicional específico da licença. A cadeia de caracteres **featureName** é o token que identifica esse produto de forma exclusiva no app, e também é usada para identificá-lo na Loja.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **Adicionar a compra da interface do usuário para esse recurso**

    Seu app também deve permitir que os clientes comprem o produto ou o recurso proposto para produto no aplicativo. O jeito de comprá-los é diferente da maneira como os clientes compraram o aplicativo completo na Loja.

    Veja aqui como testar se o cliente já possui um produto no aplicativo e, se não tiver, se ele pode visualizar a caixa de diálogo para fazer a compra. Substitua o comentário "mostrar a caixa de diálogo de compra" pelo código personalizado da caixa de diálogo de compra (como uma página com um botão "Compre este app!" ).

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Etapa 3: Alterar o código de teste para as chamadas finais

Esta etapa é fácil: basta mudar todas as referências a [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) para [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) no código do app. Não é mais preciso fornecer o arquivo WindowsStoreProxy.xml, então, remova-o do caminho do aplicativo (embora você possa salvá-lo para referência ao configurar a oferta no aplicativo, na próxima etapa).

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Etapa 4: Configurar a oferta de produto no aplicativo na Store

No Centro de parceiros, navegue até seu aplicativo e [criar um complemento](../publish/add-on-submissions.md) que corresponde à sua oferta de produto no aplicativo. Defina a ID do produto, o tipo, o preço e outras propriedades para o complemento. Lembre-se de configurá-lo com a mesma configuração que você definiu no WindowsStoreProxy.xml durante o teste.

  > [!NOTE]
  > O token de oferta no aplicativo que você usa em seu código deve corresponder a [ID do produto](../publish/set-your-add-on-product-id.md#product-id) valor especificado para o complemento correspondente no Partner Center.

## <a name="remarks"></a>Comentários

Se você tiver interesse em fornecer aos seus clientes opções de compra de produtos no aplicativo (itens que poderão ser comprados, usados e comprados novamente se desejado), vá para o tópico [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md).

Se você precisar usar recibos para verificar se o usuário fez uma compra no aplicativo, confira [Usar recibos para verificar compras de produto](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Tópicos relacionados


* [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md)
* [Gerenciar um vasto catálogo de produtos no aplicativo](manage-a-large-catalog-of-in-app-products.md)
* [Recebimentos de uso para verificar se a compras de produtos](use-receipts-to-verify-product-purchases.md)
* [Exemplo de Store (demonstra as avaliações e compras no aplicativo)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
