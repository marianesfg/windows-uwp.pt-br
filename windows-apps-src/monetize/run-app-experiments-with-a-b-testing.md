---
Description: Você pode usar o Partner Center para executar experimentos para seus aplicativos de plataforma Universal do Windows (UWP) com um teste a / B.
title: Executar experimentos de app com teste A/B
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: d4f5271d70cefea99a9caff04e7203e05043440c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599771"
---
# <a name="run-app-experiments-with-ab-testing"></a>Executar experimentos de app com teste A/B

Você pode usar o Centro de parceiros para definir as variáveis remotas que podem ser recuperadas em tempo de execução de seus aplicativos de plataforma Universal do Windows (UWP), e você pode testar as variações desses valores com os usuários a identificar os valores mais eficientes para o comportamento de condução usuário desejado. O aplicativo pode usar variáveis remotas para configurar experiências de aplicativo, como compras realizadas em aplicativo, fluxo de assinaturas, legendas e veiculações de anúncios.

A meta do teste A/B deve ser identificar uma variação dos valores variáveis remotos que deve resultar em taxas de conversão melhores (por exemplo, mais compras realizadas em aplicativo), oferecendo uma experiência de aplicativo mais atraente. Depois de ter identificado uma variação bem-sucedida, você pode imediatamente terminar o teste e habilitar essa variação para seu público-alvo do usuário inteira do Partner Center, sem a necessidade de republicar seu aplicativo.

## <a name="create-and-run-an-ab-test"></a>Criar e executar um teste A/B

Para criar e executar um teste A/B, siga estas etapas:

1. [Criar um projeto e definir variáveis remotas no Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Este projeto contém as variáveis e os valores de variáveis padrão para os experimentos.  
2. [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md). Use uma API no do Microsoft Store Services SDK para obter valores de variável remotas de projeto que você criou no Partner Center, usar esses dados para modificar o comportamento do recurso que você está testando e enviar eventos de exibição e eventos de conversão para o Partner Center.
3. [Definir seu experimento no Partner Center ](define-your-experiment-in-the-dev-center-dashboard.md). Crie um experimento no projeto que defina as metas exclusivas e as variações para o teste A/B.
4. [Executar e gerenciar seu teste no Partner Center ashboard](manage-your-experiment.md). Ativar seu experimento e usar a Central de parceiro para examinar os resultados do teste e concluir o teste.

Para obter um guia passo a passo que demonstra o processo de ponta a ponta, consulte [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="requirements"></a>Requisitos

A / B testando no Partner Center só tem suporte para aplicativos UWP.

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
| Experiment    |   Um conjunto de parâmetros que definem um teste A/B que os usuários receberão. Os experimentos são definidos no escopo de um projeto, e cada experimento consiste em: <p></p><ul><li>Um *evento de exibição* que indica quando o usuário começa a exibir uma variação que faz parte do seu experimento.</li><li>Uma ou mais metas com *eventos de conversão*, que indicam quando um objetivo foi atingido.</li><li>Uma ou mais *variações*, que definem os dados variáveis usados pelo seu experimento. A variação *control* usa os valores variáveis padrão definidos no projeto para o experimento. Além da variação de controle, experimentos normalmente têm pelo menos uma variação adicional com valores variáveis exclusivos do experimento. </li></ul>          |
| ID do projeto    |   Uma ID exclusiva que associa o seu aplicativo com um projeto em sua conta no Partner Center. Você deve usar essa ID para se conectar com o um teste a / B serviço no código do aplicativo para receber dados de variação e relatar eventos de exibição e conversão para o Partner Center. Para obter mais informações, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).<p></p><p>Cada projeto e todos os experimentos no projeto estão associados exatamente a uma ID de projeto. Você pode usar IDs de projeto para ajudar a diferenciar entre conjuntos de experimentos distintos. Por exemplo, você pode ter um conjunto de experimentos liberado para os testadores de sua organização e outro conjunto de experimentos liberado somente para usuários externos do seu aplicativo.  Um aplicativo poderá referenciar várias IDs de projeto se implementar vários experimentos.</p>         |
| Variação    |   Uma coleção de uma ou mais variáveis que você está testando em seu experimento. Cada experimento deve ter pelo menos uma variável e duas variações (incluindo o controle). Um experimento pode ter até cinco variações.           |
| Variável    |  Um valor que o aplicativo usa para inicializar uma propriedade ou algum outro valor no aplicativo. Durante um experimento, o valor da variável muda de variação para variação. Depois que você termina um experimento, é atribuído à variável o valor da variação que você optar por liberar para todos os usuários de seu aplicativo. As variáveis podem ter os seguintes tipos: cadeia de caracteres, booliano, duplo e inteiro.
| Exibir evento    |  Uma cadeia de caracteres arbitrária que representa uma atividade quando o usuário começa a exibir uma variação que faz parte do seu experimento. Normalmente, esse é o nome de um evento no seu código. O código do aplicativo enviará essa cadeia de caracteres do evento de exibição ao Partner Center quando o usuário começa exibindo uma variação. Para obter mais informações, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).
| Evento de conversão    |  Uma cadeia de caracteres arbitrária que representa um objetivo de uma meta de um experimento. Normalmente, esse é o nome de um evento no seu código. O código do aplicativo enviará essa cadeia de caracteres do evento de conversão ao Partner Center quando o usuário atingir um objetivo. Para obter mais informações, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).  

## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Seu aplicativo para experimentação de código](code-your-experiment-in-your-app.md)
* [Definir seu experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no Partner Center](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com um teste a / B](create-and-run-your-first-experiment-with-a-b-testing.md)
