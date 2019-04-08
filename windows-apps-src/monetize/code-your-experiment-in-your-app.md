---
Description: Para executar um experimento em seu aplicativo UWP (Plataforma Universal do Windows) com os testes A/B, codifique o experimento em seu aplicativo.
title: Codificar seu aplicativo para experimentação
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: edd0fbcf841dc9d8fa43873da95dc08b276a5418
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636741"
---
# <a name="code-your-app-for-experimentation"></a>Codificar seu aplicativo para experimentação

Depois que você [criar um projeto e definir variáveis remotas no Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), você está pronto para atualizar o código em seu aplicativo da plataforma Universal do Windows (UWP) para:
* Receba valores de variável remotas do Partner Center.
* Usar variáveis remotas para configurar as experiências de aplicativo para seus usuários.
* Registrar eventos que indicam quando os usuários exibiram seu experimento e realizou uma ação desejada Partner Center (também chamado de um *conversão*).

Para adicionar esse comportamento ao seu aplicativo, você vai usar APIs fornecidas pelo Microsoft Store Services SDK.

As seções a seguir descrevem o processo geral de obtenção de variações para seu experimento e log de eventos ao Partner Center. Depois que você codifique seu aplicativo para experimentação, você pode [definem um experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md). Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Algumas APIs no do Microsoft Store Services SDK experimentação usam o [padrão assíncrono](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) para recuperar dados do Partner Center. Isso significa que parte da execução desses métodos pode ocorrer depois que os métodos são invocados. Portanto, a interface do usuário do seu aplicativo pode permanecer responsiva enquanto as operações são concluídas. O padrão assíncrono requer que seu aplicativo use a palavra-chave **async** e o operador **await** ao chamar as APIs, como demonstrado pelos exemplos de código neste artigo. Por convenção, os métodos assíncronos terminam com **Async**.

## <a name="configure-your-project"></a>Configurar seu projeto

Para começar, instale o Microsoft Store Services SDK no seu computador de desenvolvimento e adicione as referências necessárias ao seu projeto.

1. [Instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Abra o projeto no Visual Studio.
3. No Gerenciador de Soluções, expanda o nó do seu projeto, clique com botão direito no nó **Referências** e clique em **Adicionar Referência**.
3. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
4. Na lista de SDKs, marque a caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.

> [!NOTE]
> Os exemplos de código neste artigo pressupõem que o arquivo de código tem instruções **using** para os namespaces **System.Threading.Tasks** e **Microsoft.Services.Store.Engagement**.

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Obter dados de variação e registrar o evento de exibição para o seu experimento

No seu projeto, localize o código do recurso que você deseja modificar no seu experimento. Adicione o código que recupera dados para uma variação, usar esses dados para modificar o comportamento do recurso que você está testando e, em seguida, faça logon o evento de exibição para o seu teste de um teste a / B service no Partner Center.

O código específico necessário dependerá de seu aplicativo, mas o exemplo a seguir demonstra o processo básico. Para obter um exemplo de código completo, consulte [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

As etapas a seguir descrevem as partes importantes desse processo em detalhes.

1. Declarar uma [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) objeto que representa a atribuição de variação atual e um [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) objeto que você usará para o modo de exibição de log e de conversão eventos Partner Center.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. Declare uma variável de cadeia de caracteres que é atribuída à [ID de projeto](run-app-experiments-with-a-b-testing.md#terms) do experimento que você deseja recuperar.
    > [!NOTE]
    > Você obter um projeto de ID quando você [criar um projeto no Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). A ID do projeto mostrada abaixo é apenas para fins de exemplo.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. Obtenha a atribuição de variação em cache atual para o seu experimento chamando o método estático [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) e passe a ID do projeto do seu experimento para o método. Esse método retorna um objeto [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) que fornece acesso para a atribuição de variações por meio da propriedade [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation).

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. Verifique a propriedade [IsStale](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) para determinar se a atribuição de variação em cache precisa ser atualizada com uma atribuição de variação remota do servidor. Se ela precisar ser atualizada, chame o método estático [GetRefreshedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) para verificar se há uma atribuição de variação atualizada do servidor e atualizar a variação em cache local.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. Use os métodos [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) ou [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) do objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) para obter os valores para a atribuição de variações. Em cada método, o primeiro parâmetro é o nome da variação que você deseja recuperar (Isso é o mesmo nome de uma variação que você insere no Partner Center). O segundo parâmetro é o valor padrão que o método deve retornar se não for capaz de recuperar o valor especificado do Partner Center (por exemplo, se não houver nenhuma conectividade de rede) e uma versão em cache da variação não está disponível.

    O exemplo a seguir usa [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) para obter uma variável denominada *buttonText* e especifica um valor de variável padrão de **Botão Cinza**.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. No seu código, use os valores de variável para modificar o comportamento do recurso que você está testando. Por exemplo, o seguinte código atribui o valor *buttonText* ao conteúdo de um botão no seu aplicativo. Este exemplo pressupõe que você já definiu esse botão em outro lugar no seu projeto.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. Por fim, faça logon o [eventos de exibição](run-app-experiments-with-a-b-testing.md#terms) para o seu teste para o um teste a / B service no Partner Center. Inicialize o campo ```logger``` para um objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) e chame o método [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation). Passe o [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) objeto que representa a atribuição de variação atual (esse objeto fornece contexto sobre o evento Partner Center) e o nome do evento de exibição para o seu teste. Isso deve corresponder ao nome de evento de exibição que você inserir para seu experimento no Partner Center. Seu código deve registrar o evento de exibição quando o usuário começa a exibir uma variação que faz parte do seu experimento.

    O exemplo a seguir mostra como registrar um evento de exibição denominado **userViewedButton**. Neste exemplo, o objetivo do experimento é fazer o usuário clicar em um botão no app. Portanto, o evento de exibição é registrado depois que o app recuperou os dados de variação (nesse caso, o texto do botão) e os atribuiu ao conteúdo do botão.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-partner-center"></a>Registrar eventos de conversão no Partner Center

Em seguida, adicione o código que registra [eventos de conversão](run-app-experiments-with-a-b-testing.md#terms) à testes a / B service no Partner Center. Seu código deve registrar um evento de conversão quando o usuário atinge um objetivo do seu experimento. O código específico necessário dependerá do seu aplicativo, mas consulte a seguir as etapas gerais. Para obter um exemplo de código completo, consulte [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. No código que é executado quando o usuário atinge um objetivo para uma das metas do experimento, chame novamente o método [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) e transmita o objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) e o nome de um evento de conversão do seu experimento. Isso deve corresponder a um dos nomes de eventos de conversão que você inserir para seu experimento no Partner Center.

    O exemplo a seguir registra um evento de conversão denominado **userClickedButton** no manipulador de eventos **Click** para um botão. Neste exemplo, o objetivo do experimento é fazer o usuário clicar no botão.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>Próximas etapas

Depois de codificar o experimento no seu app, você estará pronto para as seguintes etapas:
1. [Definir seu experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md). Crie um experimento que define os eventos do modo de exibição, eventos de conversão e variações exclusivas para seu teste A/B.
2. [Executar e gerenciar seu experimento no Partner Center](manage-your-experiment.md).


## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definir seu experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no Partner Center](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com um teste a / B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Executar testes de aplicativo com um teste a / B](run-app-experiments-with-a-b-testing.md)
