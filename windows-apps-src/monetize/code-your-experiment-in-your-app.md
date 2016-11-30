---
author: mcleanbyron
Description: Para executar um experimento em seu aplicativo UWP (Plataforma Universal do Windows) com os testes A/B, codifique o experimento em seu aplicativo.
title: "Codificar seu aplicativo para experimentação"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: ae0ddedf09913d42a036f48d2f60d7a62769b4bb

---

# Codificar seu aplicativo para experimentação

Depois que você [criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), você estará pronto para atualizar o código no aplicativo da UWP (Plataforma Universal do Windows) para:
* Receber valores de variáveis remotos do Centro de Desenvolvimento do Windows.
* Usar variáveis remotas para configurar as experiências de aplicativo para seus usuários.
* Registrar eventos no Centro de Desenvolvimento que indicam quando os usuários viram seu experimento e realizaram uma ação desejada (também chamada de um *conversão*).

Para adicionar esse comportamento ao seu aplicativo, você vai usar APIs fornecidas pelo Microsoft Store Services SDK.

As seções a seguir descrevem o processo geral de obtenção de variações para o seu experimento e de registro de eventos em log no Centro de Desenvolvimento. Depois de codificar seu aplicativo para experimentação, você poderá [definir um experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md). Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

>**Observação**&nbsp;&nbsp;Algumas APIs de experimentação no Windows Store Services SDK usam o [padrão assíncrono](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) para recuperar dados do Centro de Desenvolvimento. Isso significa que parte da execução desses métodos pode ocorrer depois que os métodos são invocados. Portanto, a interface do usuário do seu aplicativo pode permanecer responsiva enquanto as operações são concluídas. O padrão assíncrono requer que seu aplicativo use a palavra-chave **async** e o operador **await** ao chamar as APIs, como demonstrado pelos exemplos de código neste artigo. Por convenção, os métodos assíncronos terminam com **Async**.

## Configurar seu projeto

Para começar, instale o Microsoft Store Services SDK no seu computador de desenvolvimento e adicione as referências necessárias ao seu projeto.

1. [Instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, expanda o nó do seu projeto, clique com botão direito no nó **Referências** e clique em **Adicionar Referência**.
3. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
4. Na lista de SDKs, marque a caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.

>**Observação**&nbsp;&nbsp;Os exemplos de código neste artigo pressupõem que o arquivo de código **usou** instruções para os namespaces **Tasks** e **Microsoft.Services.Store.Engagement**.

## Obter dados de variação e registrar o evento de exibição para o seu experimento

No seu projeto, localize o código do recurso que você deseja modificar no seu experimento. Adicione o código que recupera os dados para uma variação, use esses dados para modificar o comportamento do recurso que você está testando e, em seguida, registre o evento de exibição do seu experimento no serviço de teste A/B no Centro de Desenvolvimento.

O código específico necessário dependerá de seu aplicativo, mas o exemplo a seguir demonstra o processo básico. Para conhecer um exemplo de código completo, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;

// Assign this variable to the project ID for your experiment from Dev Center.
// The project ID shown below is for example purposes only.
private string projectId = "F48AC670-4472-4387-AB7D-D65B095153FB";

private async Task InitializeExperiment()
{
    // Get the current cached variation assignment for the experiment.
    var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
    variation = result.ExperimentVariation;

    // Refresh the cached variation assignment if necessary.
    if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
    {
        result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

        if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
        {
            variation = result.ExperimentVariation;
        }
    }

    // Get the remote variable named "buttonText" and assign the value
    // to the button.
    var buttonText = variation.GetString("buttonText", "Grey Button");
    await button.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            button.Content = buttonText;
        });

    // Log the view event named "userViewedButton" to Dev Center.
    if (logger == null)
    {
        logger = StoreServicesCustomEventLogger.GetDefault();
    }

    logger.LogForVariation(variation, "userViewedButton");
}
```

As etapas a seguir descrevem as partes importantes desse processo em detalhes.

1. Declare um objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que representa a atribuição de variação atual e um objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) que você usará para registrar eventos de exibição e conversão no Centro de Desenvolvimento.
  ```CSharp
  private StoreServicesExperimentVariation variation;
  private StoreServicesCustomEventLogger logger;
  ```

1. Declare uma variável de string que é atribuída à [ID de projeto](run-app-experiments-with-a-b-testing.md#terms) do experimento que você deseja recuperar.
  >**Observação**&nbsp;&nbsp;Obtenha uma ID de projeto ao [criar um projeto no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). A ID do projeto mostrada abaixo é apenas para fins de exemplo.

  ```CSharp
  private string projectId = "F48AC670-4472-4387-AB7D-D65B095153FB";
  ```

2. Obtenha a atribuição de variação em cache atual para o seu experimento chamando o método estático [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) e passe a ID do projeto do seu experimento para o método. Esse método retorna um objeto [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) que fornece acesso para a atribuição de variações por meio da propriedade [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx).

   ```CSharp
   var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
   variation = result.ExperimentVariation;
   ```

4. Verifique a propriedade [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) para determinar se a atribuição de variação em cache precisa ser atualizada com uma atribuição de variação remoto do servidor. Se ela precisar ser atualizada, chame o método estático [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) para verificar se há uma atribuição de variação atualizada do servidor e atualizar a variação em cache local.

  ```CSharp
  if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
  {
        result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

        if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
        {
            variation = result.ExperimentVariation;
        }
  }
  ```

5. Use os métodos [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx), [GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx) ou [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) do objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) para obter os valores para a atribuição de variações. Em cada método, o primeiro parâmetro é o nome da variação que você deseja recuperar (esse é o mesmo nome de uma variação que você insere no painel do Centro de Desenvolvimento). O segundo parâmetro é o valor padrão que o método deverá retornar se não for capaz de recuperar o valor especificado do Centro de Desenvolvimento (por exemplo, se não houver conectividade de rede) e se uma versão em cache da variação não estiver disponível.

  O exemplo a seguir usa [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) para obter uma variável denominada *buttonText* e especifica um valor de variável padrão de **Botão Cinza**.

  ```CSharp
  var buttonText = variation.GetString("buttonText", "Grey Button");
  ```

4. No seu código, use os valores de variável para modificar o comportamento do recurso que você está testando. Por exemplo, o seguinte código atribui o valor *buttonText* ao conteúdo de um botão no seu aplicativo. Este exemplo pressupõe que você já definiu esse botão em outro lugar no seu projeto.

  ```CSharp
  await button.Dispatcher.RunAsync(
      CoreDispatcherPriority.Normal,
      () =>
      {
          button.Content = buttonText;
      });
  ```

5. Por fim, registre o [evento de exibição](run-app-experiments-with-a-b-testing.md#terms) do seu experimento no serviço de teste A/B no Centro de Desenvolvimento. Inicialize o campo ```logger``` para um objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) e chame o método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx). Transmita o objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que representa a atribuição de variação atual (esse objeto fornece contexto sobre o evento para o Centro de Desenvolvimento) e o nome do evento de exibição do seu experimento. Isso deve corresponder ao nome do evento de visualização que você digita para o seu experimento no painel do Centro de Desenvolvimento. Seu código deve registrar o evento de exibição quando o usuário começa a exibir uma variação que faz parte do seu experimento.

  O exemplo a seguir mostra como registrar um evento de exibição denominado **userViewedButton**. Neste exemplo, o objetivo do experimento é fazer o usuário clicar em um botão no aplicativo. Portanto, o evento de exibição é registrado depois que o aplicativo recuperou os dados de variação (nesse caso, o texto do botão) e os atribuiu ao conteúdo do botão.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

## Registrar eventos de conversão no Centro de Desenvolvimento

Em seguida, adicione o código que registra [eventos de conversão](run-app-experiments-with-a-b-testing.md#terms) no serviço de testes A/B do Centro de Desenvolvimento. Seu código deve registrar um evento de conversão quando o usuário atinge um objetivo do seu experimento. O código específico necessário dependerá do seu aplicativo, mas consulte a seguir as etapas gerais. Para conhecer um exemplo de código completo, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. No código que é executado quando o usuário atinge um objetivo para uma das metas do experimento, chame novamente o método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) e transmita o objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) e o nome de um evento de conversão do seu experimento. Isso deve coincidir com um dos nomes de evento de conversão que você digita para o seu experimento no painel do Centro de Desenvolvimento.

  O exemplo a seguir registra um evento de conversão denominado **userClickedButton** no manipulador de eventos **Click** para um botão. Neste exemplo, a meta do experimento é fazer o usuário clicar no botão.

  ```CSharp
  private void button_Click(object sender, RoutedEventArgs e)
  {
      if (logger == null)
      {
          logger = StoreServicesCustomEventLogger.GetDefault();
      }

      logger.LogForVariation(variation, "userClickedButton");
  }
  ```

## Próximas etapas

Depois de codificar o experimento no seu aplicativo, você estará pronto para as seguintes etapas:
1. [Defina seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md). Crie um experimento que define os eventos do modo de exibição, eventos de conversão e variações exclusivas para seu teste A/B.
2. [Execute e gerencie seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md).


## Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Nov16_HO1-->


