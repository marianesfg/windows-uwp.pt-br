---
author: mcleanbyron
Description: "Depois de definir seu experimento no painel do Centro de Desenvolvimento, você está pronto para codificá-lo no seu aplicativo."
title: "Codificar seu aplicativo para experimentação"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: d403e78b775af0f842ba2172295a09e35015dcc8
ms.openlocfilehash: 4e6706624e71c6d448a3d457c27d11c9f6ecc156

---

# Codificar seu aplicativo para experimentação

Depois de [definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md), você estará pronto para atualizar o código no seu aplicativo da Plataforma Universal do Windows (UWP) de forma a obter dados de variação para o experimento, usar esses dados para modificar o comportamento do recurso que você está testando e registrar eventos de exibição e eventos de conversão no Centro de Desenvolvimento.

As seções a seguir descrevem o processo geral de obtenção de variações para o seu experimento e de registro de eventos em log no Centro de Desenvolvimento. Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configurar seu projeto

Para começar, instale o SDK de Microsoft Store Engagement and Monetization no seu computador de desenvolvimento e adicione as referências necessárias ao seu projeto.

1. Instale o [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk).
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, expanda o nó do seu projeto, clique com botão direito no nó **Referências** e clique em **Adicionar Referência**.
3. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
4. Na lista de SDKs, marque a caixa de seleção ao lado de **SDK de Microsoft Store Engagement** e clique em **OK**.

## Adicionar código para obter configurações de variação

No seu projeto, localize o código do recurso que você deseja modificar no seu experimento. Adicione o código que recupera configurações para uma variação e use esses dados para modificar o comportamento do recurso que você está testando. O código específico necessário dependerá do seu aplicativo, mas consulte a seguir as etapas gerais. Para conhecer um exemplo de código completo, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Declare um objeto [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) que você usará para recuperar variações do seu experimento e um objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) que represente a atribuição de variação atual.
```CSharp
private readonly ExperimentClient experiment;
private ExperimentVariation variation;
```

2. Inicialize o objeto [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) e transmita ao construtor a chave de API que você recuperou da página **Experimentos** do painel. Para saber mais sobre a chave de API, veja [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md#generate-an-api-key). A chave de API mostrada abaixo é apenas para fins de exemplo.
```CSharp
experiment = new ExperimentClient("F48AC670-4472-4387-AB7D-D65B095153FB");
```

3. Obtenha a atribuição de variação em cache atual para o seu experimento chamando o método [GetVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.getvariationasync.aspx). Esse método retorna um objeto [ExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.aspx) que fornece acesso para a atribuição de variações por meio da propriedade [Variation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.variation.aspx).
```CSharp
ExperimentVariationResult result = await experiment.GetVariationAsync();
variation = result.Variation;
```

4. Verifique a propriedade [NeedsRefresh](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.needsrefresh.aspx) para determinar se a atribuição da variação em cache precisa ser atualizada. Se ela precisar ser atualizada, chame o método [RefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.refreshvariationasync.aspx) para verificar se há uma atribuição de variação atualizada do servidor e atualizar a variação em cache local.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
{
      result = await experiment.RefreshVariationAsync();

      // If the call succeeds, use the new result. Otherwise, use the
      // cached value we retrieved earlier.
      if (result.ErrorCode == EngagementErrorCode.Success)
      {
          variation = result.Variation;
      }
}
```

5. Use os métodos [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getdouble.aspx), [GetInteger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getinteger.aspx) ou [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) do objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) para obter as configurações para a atribuição de variações. Em cada método, o primeiro parâmetro é o nome da configuração que você deseja recuperar (inserido no painel do Centro de Desenvolvimento). O segundo parâmetro é o valor padrão que o método deverá retornar se não for capaz de recuperar o valor especificado do Centro de Desenvolvimento (por exemplo, se não houver conectividade de rede) e se uma versão em cache da variação não estiver disponível.

  O exemplo a seguir usa [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) para obter uma configuração denominada *buttonText* e especifica um valor de configuração padrão de **Botão Cinza**.
```CSharp
var buttonText = currentVariation.GetString("buttonText", "Grey Button");
```
4. No seu código, use os valores de configuração para modificar o comportamento do recurso que você está testando. Por exemplo, o seguinte código atribui o valor *buttonText* ao conteúdo de um botão.
```CSharp
button.Content = buttonText;
```

## Adicionar código para registrar em log eventos de exibição e conversão no Centro de Desenvolvimento

Em seguida, adicione o código que registra em log eventos de exibição e conversão no serviço de testes A/B do Centro de Desenvolvimento. Seu código deve registrar em log o evento de exibição quando o usuário começa a visualizar uma variação que faz parte do seu experimento e deve registrar o evento de conversão quando esse usuário atinge um objetivo para o seu experimento.

O código específico necessário dependerá do seu aplicativo, mas consulte a seguir as etapas gerais. Para conhecer um exemplo de código completo, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. No código que é executado quando o usuário começa a visualizar uma variação, chame o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) estático do objeto [StoreServicesCustomEvents](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.aspx). Transmita o nome do evento de exibição que você definiu no seu experimento no painel do Centro de Desenvolvimento e o objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) que representa a atribuição de variação atual (esse objeto fornece contexto sobre o evento para o Centro de Desenvolvimento). O exemplo a seguir registra um evento de exibição denominado **userViewedButton**.
```CSharp
StoreServicesCustomEvents.Log("userViewedButton", variation);
```
2. No código que é executado quando o usuário atinge um objetivo para uma das metas da experiência, chame novamente o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) e transmita o nome de um evento de conversão que você definiu no seu experimento e o objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx). O exemplo a seguir registra em log um evento de conversão denominado **userClickedButton**.
```CSharp
StoreServicesCustomEvents.Log("userClickedButton", variation);
```

## Próximas etapas

Depois de definir seu experimento no painel do Centro de Desenvolvimento e codificá-lo no seu aplicativo, você estará pronto para [executar e gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md).

## Tópicos relacionados

  * [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
  * [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


