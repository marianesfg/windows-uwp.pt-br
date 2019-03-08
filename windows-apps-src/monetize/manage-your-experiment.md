---
Description: Depois de definir seu experimento no Partner Center e o teste de código em seu aplicativo, você estará pronto para o Active Directory seu experimento e usar o Partner Center para analisar os resultados do teste.
title: Gerenciar seu experimento no Partner Center
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 6e5c0d0ca1b1d771df2b224cc41ec5a37e267bc9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594921"
---
# <a name="manage-your-experiment-in-partner-center"></a>Gerenciar seu experimento no Partner Center

Depois que você [definem seu experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md) e [seu aplicativo para experimentação de código](code-your-experiment-in-your-app.md), você está pronto para ativar seu experimento e usar o Partner Center para analisar os resultados do teste. Depois de obter todos os dados de que precisa, você pode encerrar seu experimento e escolher se deseja continuar usando os valores variáveis de variação do controle em todos os seus aplicativos ou alternar para usar os valores variáveis em uma das outras variações.

> [!NOTE]
> Quando você ativar um experimento, Partner Center começa imediatamente a coleta de dados de todos os aplicativos que são instrumentados para registrar dados para o seu teste. No entanto, pode levar várias horas para dados de teste sejam exibidos no Partner Center.

Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Ativar seu experimento

Quando estiver satisfeito com os parâmetros do seu experimento no Partner Center e você atualizou o código do aplicativo, você está pronto para ativar o seu teste para que você possa começar a coleta de dados de teste do seu aplicativo. Quando o teste está ativo, seu aplicativo pode recuperar valores de variação e relatar eventos de exibição e conversão para o Partner Center.

1. Entre no [Partner Center](https://partner.microsoft.com/dashboard).
2. Em **Seus aplicativos**, selecione o aplicativo com o experimento que você deseja ativar.
3. No painel de navegação, selecione **Serviços** e, em seguida, selecione **Experimentação**.
4. Na tabela de projetos na seção **Projetos**, expanda o projeto que contém seu experimento e siga um destes procedimentos:
  * Clique no link **Ativar** para o seu experimento. Seu experimento é adicionado à seção **Experimentos ativos** na parte superior da página.
  * Clique no nome do experimento, role até a parte inferior da página do experimento e clique em **Ativar**.

> [!IMPORTANT]
> Depois de ativar um experimento, você não pode mais modificar os parâmetros dele, a menos que tenha clicado na caixa de seleção **Experimento editável** quando criou o experimento. Recomendamos que você codifique o experimento em seu aplicativo antes de ativar seu experimento.

## <a name="review-the-results-of-your-experiment"></a>Analise os resultados de seu experimento

1. No Centro de parceiros, retorne para o **experimentação** página do seu aplicativo.
2. Na seção **Experimentos ativos**, clique no nome do seu experimento ativo para ir para a página do experimento.
3. Para um experimento ativo ou concluído, as primeiras duas seções nesta página fornecem os resultados de seu experimento:
  * A seção **Resumo dos resultados** lista suas metas para o experimento e a porcentagem de taxa de conversão para cada variação.
  * A seção **Detalhes dos resultados** fornece mais detalhes para cada variação de todas as metas em seu experimento, incluindo os modos de exibição, conversões, usuários exclusivos, taxa de conversão, delta %, confiança e importância. A *confiança* é uma medida estatística da confiabilidade de uma estimativa, que calcula a margem de erro. A *importância* é uma medida estatística, com base no tamanho da amostra, para determinar a probabilidade de que um resultado não seja devido à chance, mas em vez disso, seja atribuído a uma causa específica.

> [!NOTE]
> Partner Center relata apenas o primeiro evento de conversão para cada usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu aplicativo em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a ajudar a impedir que um usuário único com muitos eventos de conversão distorça os resultados do experimento de um grupo de amostra de usuários.


## <a name="complete-your-experiment"></a>Conclua seu experimento

1. No Partner Center, retorne para sua página de teste. Para instruções, veja a seção anterior.
2. Na seção **Resumo dos resultados**, siga um destes procedimentos:
  * Se você deseja finalizar o experimento e continuar a usar os valores variáveis na variação de controle em seu aplicativo, clique em **Manter**.
  * Se você deseja finalizar o experimento, mas alternar para usar os valores variáveis em uma variação diferente em seu aplicativo, clique em **Alternar** sob a variação para a qual deseja alternar.
3. Clique em **OK** para confirmar que você deseja finalizar o experimento.


## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Seu aplicativo para experimentação de código](code-your-experiment-in-your-app.md)
* [Definir seu experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Criar e executar seu primeiro experimento com um teste a / B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Executar testes de aplicativo com um teste a / B](run-app-experiments-with-a-b-testing.md)
