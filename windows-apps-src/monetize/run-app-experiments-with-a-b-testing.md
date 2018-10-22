---
author: Xansky
Description: You can use the Windows Dev Center dashboard to run experiments for your Universal Windows Platform (UWP) apps with A/B testing.
title: Executar experimentos de app com testes A/B
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 90ff6ba9a3767570aa9e885d491b38053a1d5d52
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5402131"
---
# <a name="run-app-experiments-with-ab-testing"></a>Executar experimentos de aplicativo com teste A/B

Você pode usar o painel do Centro de Desenvolvimento do Windows para definir variáveis remotas que pode recuperar em tempo de execução dos aplicativos da Plataforma Universal do Windows (UWP) e pode testar variações desses valores com os usuários para identificar os valores mais efetivos para conseguir o comportamento desejado do usuário. O aplicativo pode usar variáveis remotas para configurar experiências de aplicativo, como compras realizadas em aplicativo, fluxo de assinaturas, legendas e veiculações de anúncios.

A meta do teste A/B deve ser identificar uma variação dos valores variáveis remotos que deve resultar em taxas de conversão melhores (por exemplo, mais compras realizadas em aplicativo), oferecendo uma experiência de aplicativo mais atraente. Depois de ter identificado uma variação bem-sucedida, você poderá imediatamente finalizar o experimento e habilitar essa variação para todo o seu público de usuários no painel do Centro de Desenvolvimento, sem precisar republicar o aplicativo.

## <a name="create-and-run-an-ab-test"></a>Criar e executar um teste A/B

Para criar e executar um teste A/B, siga estas etapas:

1. [Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Este projeto contém as variáveis e os valores de variáveis padrão para os experimentos.  
2. [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md). Use uma API no Microsoft Store Services SDK para obter valores de variáveis remotas do projeto criado no painel. Use esses dados para modificar o comportamento do recurso que você está testando e envie eventos de exibição e de conversão para o Centro de Desenvolvimento.
3. [Defina seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md). Crie um experimento no projeto que defina as metas exclusivas e as variações para o teste A/B.
4. [Execute e gerencie seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md). Ative o experimento e use o painel para analisar os resultados do experimento e conclua o experimento.

Para obter um guia passo a passo que demonstra o processo de ponta a ponta, consulte [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="requirements"></a>Requisitos

Testes A/B no Centro de Desenvolvimento do Windows têm suporte apenas para aplicativos UWP.

Antes de poder executar experimentos com testes A/B, você deve configurar seu computador de desenvolvimento:

* Siga as instruções [aqui](../get-started/get-set-up.md) para configurar seu computador de desenvolvimento para o desenvolvimento da UWP.
* [Instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk). Além da API para experimentos, esse SDK também fornece APIs para outros recursos, como a exibição de anúncios e o direcionamento dos seus clientes ao Hub de Feedback para coletar comentários sobre o seu aplicativo.

## <a name="best-practices"></a>Práticas recomendadas

Para obter os resultados mais úteis, convém seguir estas recomendações ao executar experimentos com testes A/B:

* Considere executar experimentos com apenas duas variações, com uma distribuição aleatória dividida em 50/50 para atribuições de variação.
* Execute experimentos por pelo menos de 2 a 4 semanas para coletar dados suficientes que são estatisticamente significativos e acionáveis.

<span id="terms" />

## <a name="related-terms"></a>Termos relacionados

|  Termo  |  Definição  |
|--------|--------------|
| Projeto    |   Uma coleção de variáveis remotas com valores padrão que o aplicativo pode acessar usando o Microsoft Store Services SDK. Um projeto também pode conter um ou mais experimentos que compartilham as mesmas variáveis remotas.  |
| Experimento    |   Um conjunto de parâmetros que definem um teste A/B que os usuários receberão. Os experimentos são definidos no escopo de um projeto, e cada experimento consiste em: <p></p><ul><li>Um *evento de exibição* que indica quando o usuário começa a exibir uma variação que faz parte do seu experimento.</li><li>Uma ou mais metas com *eventos de conversão*, que indicam quando um objetivo foi atingido.</li><li>Uma ou mais *variações*, que definem os dados variáveis usados pelo seu experimento. A variação *control* usa os valores variáveis padrão definidos no projeto para o experimento. Além da variação de controle, experimentos normalmente têm pelo menos uma variação adicional com valores variáveis exclusivos do experimento. </li></ul>          |
| ID do projeto    |   Uma ID exclusiva que associa seu aplicativo a um projeto em sua conta do Centro de Desenvolvimento. Você deve usar essa ID para se conectar ao serviço de teste A/B no código do aplicativo para receber dados de variação e relatar eventos de exibição e conversão no Centro de Desenvolvimento. Para obter mais informações, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).<p></p><p>Cada projeto e todos os experimentos no projeto estão associados exatamente a uma ID de projeto. Você pode usar IDs de projeto para ajudar a diferenciar entre conjuntos de experimentos distintos. Por exemplo, você pode ter um conjunto de experimentos liberado para os testadores de sua organização e outro conjunto de experimentos liberado somente para usuários externos do seu aplicativo.  Um aplicativo poderá referenciar várias IDs de projeto se implementar vários experimentos.</p>         |
| Variação    |   Uma coleção de uma ou mais variáveis que você está testando em seu experimento. Cada experimento deve ter pelo menos uma variável e duas variações (incluindo o controle). Um experimento pode ter até cinco variações.           |
| Variável    |  Um valor que o aplicativo usa para inicializar uma propriedade ou algum outro valor no aplicativo. Durante um experimento, o valor da variável muda de variação para variação. Depois que você termina um experimento, é atribuído à variável o valor da variação que você optar por liberar para todos os usuários de seu aplicativo. As variáveis podem ter os seguintes tipos: cadeia de caracteres, booliano, duplo e inteiro.
| Exibir evento    |  Uma cadeia de caracteres arbitrária que representa uma atividade quando o usuário começa a exibir uma variação que faz parte do seu experimento. Normalmente, esse é o nome de um evento no seu código. O código do seu aplicativo enviará essa cadeia de caracteres de evento de exibição para o Centro de Desenvolvimento, quando o usuário começar a exibir uma variação. Para obter mais informações, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).
| Evento de conversão    |  Uma cadeia de caracteres arbitrária que representa um objetivo de uma meta de um experimento. Normalmente, esse é o nome de um evento no seu código. O código do seu aplicativo enviará essa cadeia de caracteres de evento de conversão para o Centro de Desenvolvimento quando o usuário atingir um objetivo. Para obter mais informações, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).  

## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
* [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
