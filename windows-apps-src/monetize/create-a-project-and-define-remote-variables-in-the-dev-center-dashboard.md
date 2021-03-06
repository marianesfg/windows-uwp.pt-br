---
Description: Antes de executar um teste em seu aplicativo de plataforma Universal do Windows (UWP) com um teste a / B, você deve criar um projeto e definir as variáveis remotas no Partner Center.
title: Criar um projeto experimental no Partner Center
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: acfd654f02cb7fb727d35271175e59966e2abdc4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650791"
---
# <a name="create-an-experiment-project-in-partner-center"></a>Criar um projeto experimental no Partner Center

Para obter uma introdução experimentação, crie uma experimentação [projeto](run-app-experiments-with-a-b-testing.md#terms) para seu aplicativo no Partner Center e defina as variáveis remotas que seu aplicativo pode acessar.

As instruções a seguir descrevem as etapas principais para criar um projeto. Para um guia passo a passo detalhado que demonstra o processo de criação de um projeto e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="instructions"></a>Instruções

1. Entre no [Partner Center](https://partner.microsoft.com/dashboard).
2. Em **Seus aplicativos**, selecione o aplicativo para o qual você deseja criar um experimento.
3. No painel de navegação, selecione **Serviços** e, em seguida, selecione **Experimentação**.
4. Na página **Experimentação**, clique no botão **Novo projeto** na seção **Projetos**. Se você já criou um ou mais projetos, eles estarão listados na seção **Projetos**.
5. Na página **Novo projeto**, digite um nome para seu novo projeto.
6. Na seção **Variáveis remotas**, adicione as [variáveis](run-app-experiments-with-a-b-testing.md#terms) que deverão estar disponíveis para todos os experimentos nesse projeto e defina valores padrão para cada variável. Os valores padrão que você especificar aqui serão usados para o grupo de controle dos experimentos e para todos os usuários que não participam do experimento.
  1. Se a seção **Variáveis remotas** estiver recolhida, clique em **Mostrar** no cabeçalho da seção.
  2. Clique em **Adicionar variável** para criar uma cada nova variável que deverá estar disponível para qualquer experimento nesse projeto e digite o nome da variável e o valor padrão.
  3. Quando terminar de adicionar variáveis, clique em **Salvar**.
3. Na seção **Integração do SDK**, anote o valor da [ID do Projeto](run-app-experiments-with-a-b-testing.md#terms). Quando você [seu aplicativo para experimentação de código](code-your-experiment-in-your-app.md), você deve fazer referência a essa ID de projeto em seu código para que você possa receber dados de variação e relatar eventos de exibição e conversão para o Partner Center.

> [!NOTE]
> Não é possível editar, adicionar nem remover variáveis remotas enquanto um experimento do projeto está ativo. Essa limitação ajuda a proteger a integridade dos dados para o grupo de controle do experimento ativo.


## <a name="next-steps"></a>Próximas etapas

Depois de criar um projeto, você pode [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md) iniciar a recuperação dos valores de variáveis remotas em seu aplicativo e [criar um experimento no projeto](define-your-experiment-in-the-dev-center-dashboard.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Seu aplicativo para experimentação de código](code-your-experiment-in-your-app.md)
* [Definir seu experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no Partner Center](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com um teste a / B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Executar testes de aplicativo com um teste a / B](run-app-experiments-with-a-b-testing.md)
