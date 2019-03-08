---
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Saiba como usar o namespace Windows.Services.Store para implementar uma versão de avaliação do seu aplicativo.
title: Implementar uma versão de avaliação do aplicativo
keywords: windows 10, uwp, avaliação, compras no aplicativo, Windows.Services.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 796266565965a62d3f168b48893d62e1cdd7df44
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646161"
---
# <a name="implement-a-trial-version-of-your-app"></a>Implementar uma versão de avaliação do aplicativo

Se você [configurar seu aplicativo como uma avaliação gratuita no Partner Center](../publish/set-app-pricing-and-availability.md#free-trial) para que os clientes podem usar seu aplicativo gratuitamente durante um período de avaliação, você pode atrair seus clientes para atualizar para a versão completa do seu aplicativo, excluindo ou limitar a alguns recursos durante o período de avaliação. Determine quais recursos devem ser limitados antes de começar a codificação, depois certifique-se de que seu aplicativo permita que eles funcionem apenas após a compra de uma licença completa. Você também pode habilitar recursos, como faixas ou marcas-d'água, que são mostrados apenas durante a avaliação, antes de o cliente comprar o aplicativo.

Este artigo mostra como usar membros da classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para determinar se o usuário tem uma licença da versão de avaliação do seu aplicativo e ser notificado se o estado da licença mudar enquanto seu aplicativo estiver em execução. 

> [!NOTE]
> O namespace **Windows.Services.Store** foi introduzido no Windows 10, versão 1607 e pode ser usada somente em projetos para **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio. Se seu aplicativo for direcionado para uma versão anterior do Windows 10, use o namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) em vez do **Windows.Services.Store**. Para obter mais informações, consulte [este artigo](exclude-or-limit-features-in-a-trial-version-of-your-app.md).

## <a name="guidelines-for-implementing-a-trial-version"></a>Diretrizes para a implementação de uma versão de avaliação

O estado da licença atual de seu aplicativo é armazenado como propriedades da classe [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Geralmente, você coloca as funções que dependem do estado da licença em um bloco condicional, conforme descrito na próxima etapa. Ao considerar esses recursos, verifique se você pode implementá-los de maneira que funcionem em todos os estados de licença.

Decida também como você gostaria de habilitar as alterações na licença do aplicativo durante sua execução. O aplicativo de avaliação pode conter todos os recursos, mas ter faixas de anúncios no aplicativo que a versão paga não tem. O aplicativo de avaliação também pode desabilitar determinados recursos ou exibir mensagens regulares solicitando a compra.

Analise sobre o tipo de aplicativo sendo criado e uma boa estratégia de avaliação ou expiração para ele. Para uma versão de avaliação de um jogo, uma boa estratégia é limitar a quantidade de conteúdo do jogo que um usuário pode jogar. Para uma versão de avaliação de um utilitário, você pode considerar definir uma data de expiração ou limitar os recursos que um possível comprador pode usar.

Nos aplicativos não destinados a jogos, a configuração de uma data de expiração funciona bem, pois os usuários podem desenvolver um bom entendimento do aplicativo como um todo. Veja aqui alguns cenários comuns de expiração e as opções para lidar com eles.

-   **Licença de avaliação expirar enquanto o aplicativo está em execução**

    Se a avaliação expirar enquanto o aplicativo estiver em execução, o aplicativo poderá:

    -   Não fazer nada.
    -   Exibir uma mensagem para o cliente.
    -   Feche.
    -   Solicitar que o cliente faça a compra.

    A prática recomendada é exibir uma mensagem com uma solicitação de compra do aplicativo e, se o cliente comprá-lo, continuar com todos os recursos habilitados. Se o usuário não se decidir pela compra, feche o aplicativo ou lembre periodicamente o usuário para comprá-lo.

-   **Licença de avaliação expira antes que o aplicativo é iniciado**

    Se a avaliação expirar antes de o usuário iniciar o aplicativo, ele não será iniciado. Em vez disso, os usuários veem uma caixa de diálogo que lhes dá a opção de comprar o aplicativo na loja.

-   **Cliente adquire o aplicativo enquanto ele está em execução.**

    Se o cliente comprar o aplicativo enquanto ele estiver em execução, aqui estão algumas ações que o aplicativo poderá executar.

    -   Não fazer nada e manter o cliente no modo de avaliação enquanto o aplicativo não for reiniciado.
    -   Agradeça-os pela compra ou exiba uma mensagem.
    -   Habilitar silenciosamente os recursos disponibilizados pela licença completa (ou desabilitar os avisos de somente avaliação).

Lembre-se de explicar como o app se comportará durante e após o período de avaliação gratuita, assim os clientes não serão surpreendidos pelo comportamento do app. Para saber mais sobre a descrição de seu aplicativo, consulte [Criar descrições de aplicativos](https://msdn.microsoft.com/library/windows/apps/mt148529).

## <a name="prerequisites"></a>Pré-requisitos

Este exemplo tem os seguintes pré-requisitos:
* Um projeto do Visual Studio para um aplicativo da Plataforma Universal do Windows (UWP) destinado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior.
* Você criou um aplicativo no Partner Center que é configurado como um [avaliação gratuita](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) sem limite de tempo e este aplicativo é publicado na Store. Opcionalmente, é possível configurar o app para que ele não possa ser descoberto na Store enquanto você o testa. Para obter mais informações, consulte as [diretrizes para teste](in-app-purchases-and-trials.md#testing).

O código neste exemplo pressupõe que:
* O código seja executado no contexto de uma [Página](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) que contenha um [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) denominado ```workingProgressRing``` e um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) denominado ```textBlock```. Esses objetos sejam usados para indicar que uma operação assíncrona está ocorrendo e exibir mensagens de saída, respectivamente.
* O arquivo de código tenha uma instrução **using** para o namespace **Windows.Services.Store**.
* O aplicativo seja um aplicativo de usuário único executado somente no contexto do usuário que iniciou o aplicativo. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Se você tiver um aplicativo da área de trabalho que utilize o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), talvez seja necessário adicionar outro código não mostrado neste exemplo para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemplo de código

Quando seu aplicativo estiver inicializando, obtenha o objeto [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) para seu aplicativo e manipule o evento [OfflineLicensesChanged](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.offlinelicenseschanged) para receber notificações quando a licença for alterada enquanto o aplicativo estiver em execução. Por exemplo, a licença do aplicativo pode ser alterada quando o período de avaliação expira ou o cliente compra o aplicativo por meio de uma Loja. Quando a licença for alterada, obtenha a nova licença e habilite ou desabilite um recurso do seu aplicativo adequadamente.

Nesse ponto, se um usuário comprou o aplicativo, é uma prática recomendada fornecer comentários para o usuário informando que o status de licença foi alterado. Você pode precisar solicitar que o usuário reinicie o aplicativo, caso este tenha sido codificado assim. Mas faça essa transição de maneira mais transparente e suave possível.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ImplementTrial](./code/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs#ImplementTrial)]

Para obter um app de exemplo completo, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliação](in-app-purchases-and-trials.md)
* [Obter informações sobre produtos para os aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras no aplicativo de aplicativos e complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Exemplo de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
