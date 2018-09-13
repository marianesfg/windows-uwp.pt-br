---
author: mcleanbyron
Description: After you define your experiment in the Dev Center dashboard and code your experiment in your app, you are ready to active your experiment and use the Dev Center dashboard to review the results of your experiment.
title: Gerenciar seu experimento no painel
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 073d5ec67bb012cbfe21c279ee97ec3c50b8458f
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3959132"
---
# <a name="manage-your-experiment-in-the-dashboard"></a>Gerenciar seu experimento no painel

Depois de [definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md) e [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md), você estará pronto para ativar seu experimento e usar o painel do Centro de Desenvolvimento para analisar os resultados de seu experimento. Depois de obter todos os dados de que precisa, você pode encerrar seu experimento e escolher se deseja continuar usando os valores variáveis de variação do controle em todos os seus aplicativos ou alternar para usar os valores variáveis em uma das outras variações.

> [!NOTE]
> Quando você ativa um experimento, o Centro de Desenvolvimento começa imediatamente a coletar dados de quaisquer aplicativos que sejam instrumentados para registrar os dados de seu experimento. No entanto, pode levar várias horas para que os dados do experimento apareçam no painel.

Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Ativar seu experimento

Quando estiver satisfeito com os parâmetros de seu experimento no painel e tiver atualizado o código do aplicativo, você estará pronto para ativar seu experimento para poder iniciar a coleta de dados de experimento do seu aplicativo. Quando o experimento está ativo, seu aplicativo pode recuperar os valores de variação e relatar os eventos de exibição e de conversão para o Centro de Desenvolvimento.

1. Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview).
2. Em **Seus aplicativos**, selecione o aplicativo com o experimento que você deseja ativar.
3. No painel de navegação, selecione **Serviços** e, em seguida, selecione **Experimentação**.
4. Na tabela de projetos na seção **Projetos**, expanda o projeto que contém seu experimento e siga um destes procedimentos:
  * Clique no link **Ativar** para o seu experimento. Seu experimento é adicionado à seção **Experimentos ativos** na parte superior da página.
  * Clique no nome do experimento, role até a parte inferior da página do experimento e clique em **Ativar**.

> [!IMPORTANT]
> Depois de ativar um experimento, você não pode mais modificar os parâmetros dele, a menos que tenha clicado na caixa de seleção **Experimento editável** quando criou o experimento. Recomendamos que você codifique o experimento em seu app antes de ativar seu experimento.

## <a name="review-the-results-of-your-experiment"></a>Analise os resultados de seu experimento

1. No Centro de Desenvolvimento, volte para a página **Experimentação** do seu aplicativo.
2. Na seção **Experimentos ativos**, clique no nome do seu experimento ativo para ir para a página do experimento.
3. Para um experimento ativo ou concluído, as primeiras duas seções nesta página fornecem os resultados de seu experimento:
  * A seção **Resumo dos resultados** lista suas metas para o experimento e a porcentagem de taxa de conversão para cada variação.
  * A seção **Detalhes dos resultados** fornece mais detalhes para cada variação de todas as metas em seu experimento, incluindo os modos de exibição, conversões, usuários exclusivos, taxa de conversão, delta %, confiança e importância. A *confiança* é uma medida estatística da confiabilidade de uma estimativa, que calcula a margem de erro. A *importância* é uma medida estatística, com base no tamanho da amostra, para determinar a probabilidade de que um resultado não seja devido à chance, mas em vez disso, seja atribuído a uma causa específica.

> [!NOTE]
> O Centro de Desenvolvimento relata apenas o primeiro evento de conversão de cada usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu app em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a ajudar a impedir que um usuário único com muitos eventos de conversão distorça os resultados do experimento de um grupo de amostra de usuários.


## <a name="complete-your-experiment"></a>Conclua seu experimento

1. No painel, retorne à página do experimento. Para instruções, veja a seção anterior.
2. Na seção **Resumo dos resultados**, siga um destes procedimentos:
  * Se você deseja finalizar o experimento e continuar a usar os valores variáveis na variação de controle em seu aplicativo, clique em **Manter**.
  * Se você deseja finalizar o experimento, mas alternar para usar os valores variáveis em uma variação diferente em seu aplicativo, clique em **Alternar** sob a variação para a qual deseja alternar.
3. Clique em **OK** para confirmar que você deseja finalizar o experimento.


## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
* [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
* [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Executar experimentos de app com teste A/B](run-app-experiments-with-a-b-testing.md)
