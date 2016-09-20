---
author: mcleanbyron
Description: "Você pode usar o painel do Centro de Desenvolvimento do Windows para executar experimentos para os seus aplicativos UWP (Plataforma Universal do Windows) com testes A/B."
title: Executar experimentos de aplicativo com testes A/B
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 88fd0516e3c10b657884b93377480b62c1758992

---

# Executar experimentos de aplicativo com testes A/B

Você pode usar o painel do Centro de Desenvolvimento do Windows para executar experimentos para os seus aplicativos UWP (Plataforma Universal do Windows) com testes A/B.

Em um teste A/B, você faz experimentos com atribuições de variável de programas nos seus aplicativos por meio do Centro de Desenvolvimento. Ao desenvolver a lógica do aplicativo com base em variáveis de problemas compatíveis com testes A/B, você pode habilitar variações do seu aplicativo para segmentos aleatórios do seu público de usuários. O objetivo do seu teste A/B é identificar uma variação que provavelmente resultará em maiores taxas de conversão (por exemplo, mais compras no aplicativo).

Depois de ter identificado uma variação que melhor atende às suas metas de negócios, você pode imediatamente finalizar o experimento e habilitar essa variação para todo o seu público de usuários no painel do Centro de Desenvolvimento, sem precisar republicar o aplicativo.

## Criar e executar um teste A/B

Para criar e executar um teste A/B, siga estas etapas:

1. 
            [Defina seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md). Cada experimento consiste em:
  * Um *evento de exibição* que indica quando o usuário começa a exibir uma variação que faz parte do seu experimento.
  * Uma ou mais metas com *eventos de conversão*, que indicam quando um objetivo foi atingido.
  * Uma ou mais *variações*, que definem as configurações usadas pelo seu experimento.
2. 
            [Codifique seu aplicativo para experimentação](code-your-experiment-in-your-app.md). Use uma API no SDK de Microsoft Store Engagement and Monetization para obter configurações de variação para o experimento. Use esses dados para modificar o comportamento do recurso que você está testando e envie eventos de exibição e de conversão para o Centro de Desenvolvimento.
3. 
            [Execute e gerencie seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md). Use o painel para analisar os resultados do experimento e conclua o experimento.

Para obter um guia passo a passo que demonstra o processo de ponta a ponta, consulte [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Requisitos

Testes A/B no Centro de Desenvolvimento do Windows têm suporte apenas para aplicativos UWP.

Antes de poder executar experimentos com testes A/B, você deve configurar seu computador de desenvolvimento:

* Siga as instruções [aqui](../get-started/get-set-up.md) para configurar seu computador de desenvolvimento para o desenvolvimento da UWP.
* Instale o [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Além da API para experimentos, esse SDK também fornece APIs para outros recursos, como a exibição de anúncios e o direcionamento dos seus clientes ao Hub de Feedback para coletar comentários sobre o seu aplicativo. Para saber mais sobre esse SDK, consulte [Monetizar seu aplicativo com o SDK de Store Engagement and Monetization](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).

## Práticas recomendadas

Para obter os resultados mais úteis, convém seguir estas recomendações ao executar experimentos com testes A/B:

* Considere executar experimentos com apenas duas variações, com uma distribuição aleatória dividida em 50/50 para atribuições de variação.
* Execute experimentos por pelo menos de 2 a 4 semanas para coletar dados suficientes que são estatisticamente significativos e acionáveis.

## Tópicos relacionados

* [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
* [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
* [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


