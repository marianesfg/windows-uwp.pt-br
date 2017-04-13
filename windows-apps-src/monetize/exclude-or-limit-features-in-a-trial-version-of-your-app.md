---
author: mcleanbyron
Description: "Se você permitir que os clientes usem seu app gratuitamente durante um período de avaliação, incentive-os a atualizar para a versão completa do app excluindo ou limitando alguns recursos durante o período de avaliação."
title: "Excluir ou limitar recursos em uma versão de avaliação"
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: "windows 10, uwp, compra no aplicativo, avaliação, IAP, Windows.ApplicationModel.Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: badb14e5c915db68fb262002a8ee3321b62e2778
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>Excluir ou limitar recursos em uma versão de avaliação


> [!NOTE]
> Este artigo demonstra como usar membros do namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Se seu aplicativo for destinado ao Windows 10, versão 1607 ou posterior, recomendamos que você use membros do namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para implementar uma versão de avaliação, em vez do namespace **Windows.ApplicationModel.Store**. Para obter mais informações, consulte [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md).

Se você permitir que os clientes usem seu app gratuitamente durante um período de avaliação, incentive-os a atualizar para a versão completa do app excluindo ou limitando alguns recursos durante o período de avaliação. Determine quais recursos devem ser limitados antes de começar a codificação, depois certifique-se de que o seu aplicativo permita que eles funcionem após a compra de uma licença completa. Você também pode habilitar recursos, como faixas ou marcas-d'água que são mostrados apenas durante a avaliação, antes de o cliente comprar o aplicativo.

Vamos examinar como adicionar isso a seu aplicativo.

## <a name="prerequisites"></a>Pré-requisitos

Um aplicativo do Windows no qual devem ser adicionados os recursos que os clientes podem comprar.

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>Etapa 1: Escolha os recursos que você deseja habilitar ou desabilitar durante o período de avaliação.

O estado da licença atual de seu app é armazenado como propriedades da classe [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157). Geralmente, você coloca as funções que dependem do estado da licença em um bloco condicional, conforme descrito na próxima etapa. Ao considerar esses recursos, verifique se você pode implementá-los de maneira que funcionem em todos os estados de licença.

Decida também como você gostaria de habilitar as alterações na licença do aplicativo durante sua execução. O aplicativo de avaliação pode conter todos os recursos, mas ter faixas de anúncios no aplicativo que a versão paga não tem. O aplicativo de avaliação também pode desabilitar determinados recursos ou exibir mensagens regulares solicitando a compra.

Analise sobre o tipo de aplicativo sendo criado e uma boa estratégia de avaliação ou expiração para ele. Para uma versão de avaliação de um jogo, uma boa estratégia é limitar a quantidade de conteúdo do jogo que um usuário pode jogar. Para uma versão de avaliação de um utilitário, você pode considerar definir uma data de expiração ou limitar os recursos que um possível comprador pode usar.

Nos aplicativos não destinados a jogos, a configuração de uma data de expiração funciona bem, pois os usuários podem desenvolver um bom entendimento do aplicativo como um todo. Veja aqui alguns cenários comuns de expiração e as opções para lidar com eles.

-   **A licença de avaliação expira enquanto o aplicativo está em execução**

    Se a avaliação expirar enquanto o aplicativo estiver em execução, o aplicativo poderá:

    -   Não fazer nada.
    -   Exibir uma mensagem para o cliente.
    -   Fechar.
    -   Solicitar que o cliente faça a compra.

    A prática recomendada é exibir uma mensagem com uma solicitação de compra do aplicativo e, se o cliente comprá-lo, continuar com todos os recursos habilitados. Se o usuário não se decidir pela compra, feche o aplicativo ou lembre periodicamente o usuário para comprá-lo.

-   **A licença de avaliação expira antes de o aplicativo ser iniciado**

    Se a avaliação expirar antes de o usuário iniciar o aplicativo, ele não será iniciado. Em vez disso, os usuários veem uma caixa de diálogo que lhes dá a opção de comprar o aplicativo na loja.

-   **O cliente compra o aplicativo enquanto ele está em execução**

    Se o cliente comprar o aplicativo enquanto ele estiver em execução, aqui estão algumas ações que o aplicativo poderá executar.

    -   Não fazer nada e manter o cliente no modo de avaliação enquanto o aplicativo não for reiniciado.
    -   Agradeça-os pela compra ou exiba uma mensagem.
    -   Habilitar silenciosamente os recursos disponibilizados pela licença completa (ou desabilitar os avisos de somente avaliação).

Se quiser detectar a mudança de licença e tomar alguma providência no seu aplicativo, adicione um manipulador de eventos para isso, conforme descrito na próxima etapa.

## <a name="step-2-initialize-the-license-info"></a>Etapa 2: Inicializar as informações de licença

Quando seu app estiver sendo inicializado, obtenha o objeto [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) do app, conforme mostrado neste exemplo. Pressupomos que **licenseInformation** seja uma variável global ou um campo global do tipo **LicenseInformation**.

Por enquanto, você receberá informações de licença simuladas usando [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766), em vez de [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Antes de enviar a versão do seu app à **Loja**, você deve substituir todas as referências a **CurrentAppSimulator** em seu código por **CurrentApp**.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTest)]

Em seguida, adicione um manipulador de eventos para receber as notificações quando a licença for alterada durante a execução do app. A licença do app poderá ser alterada se o período de avaliação expirar ou o cliente comprar o app por meio de uma Loja, por exemplo.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTestWithEvent)]

## <a name="step-3-code-the-features-in-conditional-blocks"></a>Etapa 3: Codificar recursos em blocos condicionais

Quando o evento de alteração da licença for gerado, o app deverá chamar a API de Licença para determinar se o status de avaliação foi alterado. O código nesta etapa mostra como estruturar o manipulador desse evento. Nesse ponto, se um usuário comprou o aplicativo, é uma prática recomendada fornecer comentários para o usuário informando que o status de licença foi alterado. Você pode precisar solicitar que o usuário reinicie o aplicativo, caso este tenha sido codificado assim. Mas faça essa transição de maneira mais transparente e suave possível.

Este exemplo mostra como avaliar o status de licença do app para que você possa habilitar ou desabilitar um recurso do app de forma adequada.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#ReloadLicense)]

## <a name="step-4-get-an-apps-trial-expiration-date"></a>Etapa 4: Obter uma data de expiração da avaliação do app

Inclua o código para determinar a data de expiração da avaliação do app.

O código neste exemplo define uma função para obter a data de expiração da licença de avaliação do aplicativo. Se a licença ainda for válida, exiba a data de expiração com o número de dias que restam até a expiração da avaliação.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#DisplayTrialVersionExpirationTime)]

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>Etapa 5: Testar os recursos usando chamadas simuladas à API de Licença

Agora, teste seu app usando dados simulados. **CurrentAppSimulator** obtém informações específicas do teste de um arquivo XML denominado WindowsStoreProxy.xml, localizado em %UserProfile%\\AppData\\local\\packages\\&lt;nome do pacote&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Você pode editar o WindowsStoreProxy.xml para alterar as datas de expiração simuladas do app e seus recursos. Teste todas as configurações possíveis de expiração e licença para verificar se tudo funciona conforme o esperado. Para obter mais informações, consulte [Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).

Se esse caminho e esse arquivo não existirem, você deverá criá-los ou fornecê-los durante a instalação ou em tempo de execução. Se você tentar acessar a propriedade [CurrentAppSimulator.LicenseInformation](https://msdn.microsoft.com/library/windows/apps/hh779768) sem o WindowsStoreProxy.xml estar presente nesse local específico, receberá um erro.

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>Etapa 6: Substituir os métodos da API de Licença simulada pela API real

Depois de testar seu app com o servidor de licenças simuladas, e antes de enviá-lo a uma Loja para certificação, substitua **CurrentAppSimulator** por **CurrentApp**, conforme mostrado no código de exemplo a seguir.

> [!IMPORTANT]
> Seu app deverá usar o objeto **CurrentApp** quando você o enviar a uma Loja; caso contrário, haverá falha na certificação.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseRetailWithEvent)]

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>Etapa 7: Descrever para os clientes como funciona a avaliação gratuita

Lembre-se de explicar como o app se comportará durante e após o período de avaliação gratuita, assim os clientes não serão surpreendidos pelo comportamento do app.

Para saber mais sobre a descrição de seu aplicativo, consulte [Criar descrições de aplicativos](https://msdn.microsoft.com/library/windows/apps/mt148529).

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Definir a disponibilidade e o preço do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 
