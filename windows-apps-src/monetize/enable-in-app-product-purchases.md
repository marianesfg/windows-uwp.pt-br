---
author: mcleanbyron
Description: "Seja seu app gratuito ou não, você pode vender conteúdo, outros apps ou uma nova funcionalidade do app (como o desbloqueio do próximo nível de um jogo) no próprio app. Veja a seguir como habilitar esses produtos no seu aplicativo."
title: Habilitar compras de produtos no aplicativo
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: "exemplo de código de oferta no aplicativo"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 1cd748cd1b6ca7e85cfb86daba367540af25db88

---

# <a name="enable-in-app-product-purchases"></a>Habilitar compras de produtos no aplicativo

>**Observação**&nbsp;&nbsp;Este artigo demonstra como usar membros do namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Se seu aplicativo for destinado ao Windows 10, versão 1607 ou posterior, recomendamos que você use membros do namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para gerenciar complementos (também conhecidos como produtos no aplicativo ou IAPs), em vez do namespace **Windows.ApplicationModel.Store**. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).

Seja seu app gratuito ou não, você pode vender conteúdo, outros apps ou uma nova funcionalidade do app (como o desbloqueio do próximo nível de um jogo) no próprio app. Veja a seguir como habilitar esses produtos no seu aplicativo.

> **Observação**&nbsp;&nbsp;Produtos no aplicativo não podem ser oferecidos em uma versão de avaliação do app. Os clientes que usam uma versão de avaliação do aplicativo só poderão comprar um produto no aplicativo se comprarem a versão completa do seu aplicativo.

## <a name="prerequisites"></a>Pré-requisitos

-   Um aplicativo do Windows no qual devem ser adicionados os recursos que os clientes podem comprar.
-   Ao codificar e testar novos produtos no aplicativo pela primeira vez, você deve usar o objeto [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) em vez do objeto [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças, em vez de chamar o servidor ativo. Para fazer isso, você precisa personalizar o arquivo chamado WindowsStoreProxy.xml em %userprofile%\\AppData\\local\\packages\\&lt;nome do pacote&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu app pela primeira vez, mas também é possível carregar um arquivo personalizado em tempo de execução. Para obter mais informações, consulte [Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Este tópico também faz referência a exemplos de código fornecidos no [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Essa amostra é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Etapa 1: Inicie as informações de licença do aplicativo

Quando seu app estiver em processo de inicialização, obtenha o objeto [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) para seu app inicializando [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) ou [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) para habilitar compras de um produto no aplicativo.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Etapa 2: Adicione as ofertas de produto no aplicativo ao seu app

Para cada recurso a ser disponibilizado por meio de uma transação de produto no aplicativo, crie uma oferta e adicione-a ao seu app.

> **Importante**&nbsp;&nbsp;Você deve adicionar todos os produtos no aplicativo que deseja apresentar aos clientes antes de enviar o app à Loja. Para adicionar novos produtos no aplicativo depois, você deve atualizar o aplicativo e reenviar uma nova versão.

1.  **Crie um token de oferta no aplicativo**

    Você pode identificar cada produto no seu aplicativo por um token. Esse token é uma cadeia de caracteres que você define e usa no aplicativo e na Loja para identificar um produto no aplicativo específico. Dê (ao aplicativo) um nome exclusivo e significativo, para poder identificar o recurso correto que ele representa durante a codificação. Este são alguns exemplos de nomes:

    -   "SpaceMissionLevel4"

    -   "ContosoCloudSave"

    -   "RainbowThemePack"

2.  **Codifique o recurso em um bloco de condições**

    Coloque o código de cada recurso associado a um produto no aplicativo em um bloco de condições que testa se o cliente tem uma licença para usar esse recurso.

    Veja a seguir um exemplo que mostra como é possível codificar um recurso de produto chamado **featureName** em um bloco condicional específico da licença. A cadeia de caracteres **featureName** é o token que identifica esse produto de forma exclusiva no app, e também é usada para identificá-lo na Loja.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **Adicione a interface do usuário de compra para este recurso**

    Seu app também deve permitir que os clientes comprem o produto ou o recurso proposto para produto no aplicativo. O jeito de comprá-los é diferente da maneira como os clientes compraram o aplicativo completo na Loja.

    Veja aqui como testar se o cliente já possui um produto no aplicativo e, se não tiver, se ele pode visualizar a caixa de diálogo para fazer a compra. Substitua o comentário "mostrar a caixa de diálogo de compra" pelo código personalizado da caixa de diálogo de compra (como uma página com um botão "Compre este app!" ).

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Etapa 3: Mude o código de teste para as chamadas finais

Esta etapa é fácil: basta mudar todas as referências a [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) para [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) no código do app. Não é mais preciso fornecer o arquivo WindowsStoreProxy.xml, então, remova-o do caminho do aplicativo (embora você possa salvá-lo para referência ao configurar a oferta no aplicativo, na próxima etapa).

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Etapa 4: Configurar o produto no aplicativo na Loja

No painel do Centro de Desenvolvimento, defina a ID do produto, o tipo, o preço e outras propriedades para seu produto no aplicativo. Lembre-se de configurá-lo com a mesma configuração que você definiu no WindowsStoreProxy.xml durante o teste. Para obter mais informações, consulte [Envios de IAP](https://msdn.microsoft.com/library/windows/apps/mt148551).

## <a name="remarks"></a>Comentários

Se você tiver interesse em fornecer aos seus clientes opções de compra de produtos no aplicativo (itens que poderão ser comprados, usados e comprados novamente se desejado), vá para o tópico [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md).

Se você precisar usar recibos para verificar se o usuário fez uma compra no aplicativo, confira [Usar recibos para verificar compras de produto](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Tópicos relacionados


* [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md)
* [Gerenciar um catálogo abrangente de produtos no aplicativo](manage-a-large-catalog-of-in-app-products.md)
* [Usar recibos para verificar compras de produtos](use-receipts-to-verify-product-purchases.md)
* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)



<!--HONumber=Dec16_HO1-->


