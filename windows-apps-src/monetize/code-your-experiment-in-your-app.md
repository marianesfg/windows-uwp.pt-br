---
author: mcleanbyron
Description: Para executar um experimento em seu aplicativo UWP (Plataforma Universal do Windows) com os testes A/B, codifique o experimento em seu aplicativo.
title: "Codificar seu aplicativo para experimentação"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# Codificar seu aplicativo para experimentação

Depois que você [criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), você estará pronto para atualizar o código no aplicativo da UWP (Plataforma Universal do Windows) para:
* Receber valores de variáveis remotos do Centro de Desenvolvimento do Windows.
* Usar variáveis remotas para configurar as experiências de aplicativo para seus usuários.
* Registrar eventos no Centro de Desenvolvimento que indicam quando os usuários viram seu experimento e realizaram uma ação desejada (também chamada de um *conversão*).

As seções a seguir descrevem o processo geral de obtenção de variações para o seu experimento e de registro de eventos em log no Centro de Desenvolvimento. Depois de codificar seu aplicativo para experimentação, você poderá [definir um experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md). Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configurar seu projeto

Para começar, instale o Microsoft Store Services SDK no seu computador de desenvolvimento e adicione as referências necessárias ao seu projeto.

1. Instale o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk).
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, expanda o nó do seu projeto, clique com botão direito no nó **Referências** e clique em **Adicionar Referência**.
3. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
4. Na lista de SDKs, marque a caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.

## Adicionar código para obter dados de variação

No seu projeto, localize o código do recurso que você deseja modificar no seu experimento. Adicione o código que recupera dados para uma variação e use esses dados para modificar o comportamento do recurso que você está testando. O código específico necessário dependerá do seu aplicativo, mas consulte a seguir as etapas gerais. Para conhecer um exemplo de código completo, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Declare um objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que representa a atribuição de variação atual e um objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) que você usará para registrar eventos de exibição e conversão no Centro de Desenvolvimento.
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. Obtenha a atribuição de variação em cache atual para o seu experimento chamando o método estático [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) e passe a [ID do projeto](run-app-experiments-with-a-b-testing.md#terms) do seu experimento para o método. Esse método retorna um objeto [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) que fornece acesso para a atribuição de variações por meio da propriedade [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx).
  >**Observação**&nbsp;&nbsp;Obtenha uma ID de projeto ao [criar um projeto no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). A ID do projeto mostrada abaixo é apenas para fins de exemplo.

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. Verifique a propriedade [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) para determinar se a atribuição de variação em cache precisa ser atualizada com uma atribuição de variação remoto do servidor. Se ela precisar ser atualizada, chame o método estático [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) para verificar se há uma atribuição de variação atualizada do servidor e atualizar a variação em cache local.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
{
      result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

      // If the call succeeds, use the new result. Otherwise, use the cached value.
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
4. No seu código, use os valores de variável para modificar o comportamento do recurso que você está testando. Por exemplo, o seguinte código atribui o valor *buttonText* ao conteúdo de um botão.
```CSharp
button.Content = buttonText;
```

## Adicionar código para registrar em log eventos de exibição e conversão no Centro de Desenvolvimento

Em seguida, adicione o código que registra em log [eventos de exibição](run-app-experiments-with-a-b-testing.md#terms) e [conversão](run-app-experiments-with-a-b-testing.md#terms) no serviço de testes A/B do Centro de Desenvolvimento. Seu código deve registrar em log o evento de exibição quando o usuário começa a visualizar uma variação que faz parte do seu experimento e deve registrar o evento de conversão quando esse usuário atinge um objetivo para o seu experimento.

O código específico necessário dependerá do seu aplicativo, mas consulte a seguir as etapas gerais. Para conhecer um exemplo de código completo, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. No código que é executado quando o usuário começa a visualizar uma variação, inicialize o campo ```logger```para um objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) e chame o método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx). Transmita o objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que representa a atribuição de variação atual (esse objeto fornece contexto sobre o evento para o Centro de Desenvolvimento) e o nome do evento de exibição (que é o mesmo nome de um evento de exibição que você insere no painel do Centro de Desenvolvimento). O exemplo a seguir registra um evento de exibição denominado **userViewedButton**.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. No código que é executado quando o usuário atinge um objetivo para uma das metas da experiência, chame o método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) novamente e transmita o objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) e o nome de um evento de conversão (que é o mesmo nome de um evento de conversão que você insere no painel do Centro de Desenvolvimento). O exemplo a seguir registra em log um evento de conversão denominado **userClickedButton**.
```CSharp
logger.LogForVariation(variation, "userClickedButton");
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



<!--HONumber=Sep16_HO1-->


