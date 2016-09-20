---
author: mcleanbyron
Description: "Depois de definir seu experimento no painel do Centro de Desenvolvimento e codificar seu experimento em seu aplicativo, você estará pronto para ativar seu experimento e usar o painel do Centro de Desenvolvimento para analisar os resultados de seu experimento."
title: Gerenciar seu experimento no painel do Centro de Desenvolvimento
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 24ca106cc83c4495657972f463c556585cdfcb45

---

# Gerenciar seu experimento no painel do Centro de Desenvolvimento

Depois de [definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md) e [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md), você estará pronto para ativar seu experimento e usar o painel do Centro de Desenvolvimento para analisar os resultados de seu experimento. Depois de obter todos os dados de que precisa, você pode encerrar seu experimento e escolher se deseja continuar usando as configurações de variação do controle em todos os seus aplicativos ou alternar para usar as configurações em uma de suas variações.

> 
            **Observação** Quando você ativa um experimento, o Centro de Desenvolvimento começa imediatamente a coletar dados de quaisquer aplicativos que sejam instrumentados para registrar os dados de seu experimento. No entanto, pode levar várias horas para que os dados do experimento apareçam no painel.

Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Ativar seu experimento

Quando estiver satisfeito com os parâmetros de seu experimento no painel e tiver atualizado o código do aplicativo, você estará pronto para ativar seu experimento para poder iniciar a coleta de dados de experimento do seu aplicativo. Quando o experimento está ativo, seu aplicativo pode recuperar as configurações de variação e relatar os eventos de exibição e de conversão para o Centro de Desenvolvimento.

1. Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview).
2. Em **Seus aplicativos**, selecione o aplicativo com o experimento que você deseja ativar.
3. No painel de navegação, selecione **Serviços** e, em seguida, selecione **Experimentação**.
4. A seção **Experimentos** lista os experimentos de rascunho, ativos e concluídos para o aplicativo atual. Clique no filtro **Rascunho** e, em seguida, clique em **Ativar** para o experimento que você deseja ativar.

> 
            **Importante**  Depois de ativar um experimento, você não pode mais modificar os parâmetros dele, a menos que ele seja um experimento de teste (você clicou na caixa de seleção **Experimento de teste** quando criou o experimento). Recomendamos que você codifique o experimento em seu aplicativo antes de ativar seu experimento.


## Analise os resultados de seu experimento

1. No Centro de Desenvolvimento, volte para a página **Experimentação** do seu aplicativo.
2. Na seção **Experimentos**, clique no filtro **Ativo** e, em seguida, clique no nome do seu experimento ativo para ir para a página do experimento.
3. Para um experimento ativo ou concluído, as primeiras duas seções nesta página fornecem os resultados de seu experimento:
  * A seção **Resumo dos resultados** lista suas metas para o experimento e a porcentagem de taxa de conversão para cada variação.
  * A seção **Detalhes dos resultados** fornece mais detalhes para cada meta em seu experimento, incluindo os modos de exibição, conversões, taxa de conversão, delta %, confiança e importância. A *confiança* é uma medida estatística da confiabilidade de uma estimativa, que calcula a margem de erro. A *importância* é uma medida estatística, com base no tamanho da amostra, para determinar a probabilidade de que um resultado não seja devido à chance, mas em vez disso, seja atribuído a uma causa específica.

  >
            **Observação** O Centro de Desenvolvimento relata apenas o primeiro evento de conversão de cada usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu aplicativo em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a ajudar a impedir que um usuário único com muitos eventos de conversão distorça os resultados do experimento de um grupo de amostra de usuários.


## Conclua seu experimento

1. No painel, retorne à página do experimento. Para instruções, veja a seção anterior.
2. Na seção **Resumo dos resultados**, siga um destes procedimentos:
  * Se você deseja finalizar o experimento e continuar a usar as configurações na variação de controle em seu aplicativo, clique em **Manter**.
  * Se você deseja finalizar o experimento, mas alternar para usar as configurações em uma variação diferente em seu aplicativo, clique em **Alternar** sob a variação para a qual deseja alternar.
3. Clique em **OK** para confirmar que você deseja finalizar o experimento.


## Tópicos relacionados

  * [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
  * [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


