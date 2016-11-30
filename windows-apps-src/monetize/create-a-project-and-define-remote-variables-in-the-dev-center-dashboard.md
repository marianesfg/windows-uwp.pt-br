---
author: mcleanbyron
Description: "Antes de executar um experimento em seu aplicativo UWP (Plataforma Universal do Windows) com os testes A/B, você deve criar um projeto e definir suas variáveis remotas no painel do Centro de Desenvolvimento."
title: "Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento"
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
translationtype: Human Translation
ms.sourcegitcommit: 32c1b379ee3913e267664e6d125fbc3daf480bb3
ms.openlocfilehash: 88a55c9ed64d5f52f959a1c68618dc5296dc24d6

---

# Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento

Para começar a usar experimentação, crie uma experimentação [projeto](run-app-experiments-with-a-b-testing.md#terms) para seu aplicativo no painel do Centro de Desenvolvimento e defina as variáveis remotas que seu aplicativo pode acessar.

As instruções a seguir descrevem as etapas principais para criar um projeto. Para um guia passo a passo detalhado que demonstra o processo de criação de um projeto e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Instruções

1. Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview).
2. Em **Seus aplicativos**, selecione o aplicativo para o qual você deseja criar um experimento.
3. No painel de navegação, selecione **Serviços** e, em seguida, selecione **Experimentação**.
4. Na página **Experimentação**, clique no botão **Novo projeto** na seção **Projetos**. Se você já criou um ou mais projetos, eles estarão listados na seção **Projetos**.
5. Na página **Novo projeto**, digite um nome para seu novo projeto.
6. Na seção **Variáveis remotas**, adicione as [variáveis](run-app-experiments-with-a-b-testing.md#terms) que deverão estar disponíveis para todos os experimentos nesse projeto e defina valores padrão para cada variável. Os valores padrão que você especificar aqui serão usados para o grupo de controle dos experimentos e para todos os usuários que não participam do experimento.
  1. Se a seção **Variáveis remotas** estiver recolhida, clique em **Mostrar** no cabeçalho da seção.
  2. Clique em **Adicionar variável** para criar uma cada nova variável que deverá estar disponível para qualquer experimento nesse projeto e digite o nome da variável e o valor padrão.
  3. Quando terminar de adicionar variáveis, clique em **Salvar**.
3. Na seção **Integração do SDK**, anote o valor da [ID do Projeto](run-app-experiments-with-a-b-testing.md#terms). Ao [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md), você deve fazer referência a essa ID de projeto em seu código para que receber dados de variação e relatar eventos de exibição e conversão no Centro de Desenvolvimento.

>**Observação**&nbsp;&nbsp;Não é possível editar, adicionar nem remover variáveis remotas enquanto um experimento do projeto está ativo. Essa limitação ajuda a proteger a integridade dos dados para o grupo de controle do experimento ativo.


## Próximas etapas

Depois de criar um projeto, você pode [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md) iniciar a recuperação dos valores de variáveis remotas em seu aplicativo e [criar um experimento no projeto](define-your-experiment-in-the-dev-center-dashboard.md).

## Tópicos relacionados

* [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
* [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Nov16_HO1-->


