---
author: mcleanbyron
Description: "Neste passo a passo, você criará, executará e gerenciará seu primeiro experimento com testes A/B."
title: Criar e executar seu primeiro experimento com testes A/B
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: fb9e93747fa77031fe906d9ab7463fc41b73cdeb
ms.lasthandoff: 02/07/2017

---

# <a name="create-and-run-your-first-experiment-with-ab-testing"></a>Criar e executar seu primeiro experimento com testes A/B

Neste passo a passo, você fará o seguinte:
* Criará uma experimentação [projeto](run-app-experiments-with-a-b-testing.md#terms) no painel do Centro de Desenvolvimento que define diversas variáveis remotas que representam o texto e a cor de um botão de app.
* Criará um app com código que recupera os valores de variáveis remotas, usará esses dados para alterar a cor de fundo de um botão e registrará em log dados de eventos de exibição e conversão novamente no Centro de Desenvolvimento.
* Criará um experimento no projeto para testar se a mudança na cor de fundo de um botão de app aumenta com êxito o número de cliques do botão.
* Executará o app para coletar dados de experimento.
* Examinará os resultados do experimento no painel do Centro de Desenvolvimento, escolherá uma variação a ser habilitada para todos os usuários do app e concluirá o experimento.

Para obter uma visão geral de testes A/B teste com o Centro de Desenvolvimento, veja [Executar experimentos de app com teste A/B](run-app-experiments-with-a-b-testing.md).

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este passo a passo, você deve ter uma conta do Centro de Desenvolvimento do Windows e configurar seu computador de desenvolvimento conforme descrito em [Executar experimentos de app com teste A/B](run-app-experiments-with-a-b-testing.md).

## <a name="create-a-project-with-remote-variables-in-windows-dev-center"></a>Criar um projeto com variáveis remotas no Centro de Desenvolvimento do Windows

1. Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview).
2. Se já tiver um app no Centro de Desenvolvimento que você deseja usar para criar um experimento, selecione-o no painel. Se você ainda não tiver um app no seu painel, [crie um novo app reservando um nome](../publish/create-your-app-by-reserving-a-name.md) e, em seguida, selecione esse app no painel.
3. No painel de navegação, clique em **Serviços** e depois em **Experimentação**.
4. Na seção **Projetos** da próxima página, clique no botão **Novo projeto**.
5. Na página **Novo projeto**, insira o nome **Experimentos de clique de botão** para seu novo projeto.
6. Expanda a seção **Variáveis remotas** e clique em **Adicionar variável** quatro vezes. Agora, você terá quatro linhas de variáveis em branco.
  * Na primeira linha, digite **buttonText** para o tipo e o nome de variável **Botão cinza** na coluna **Valor padrão**.
  * Na segunda linha, digite **r** para o tipo e o nome de variável **128** na coluna **Valor padrão**.
  * Na terceira linha, digite **g** para o tipo e o nome de variável **128** na coluna **Valor padrão**.
  * Na quarta linha, digite **b** para o tipo e o nome de variável **128** na coluna **Valor padrão**.
7. Clique em **Salvar** e anote o valor da [ID do projeto](run-app-experiments-with-a-b-testing.md#terms) que consta na seção **integração do SDK**. Na próxima seção, você atualizará o código do app e fará referência a esse valor em seu código.

## <a name="code-the-experiment-in-your-app"></a>Codificar o experimento no seu app

1. No Visual Studio 2015, crie um novo projeto da Plataforma Universal do Windows usando Visual C#. Especifique o nome **SampleExperiment** para o projeto.
2. No Gerenciador de Soluções, expanda o nó do seu projeto, clique com botão direito no nó **Referências** e clique em **Adicionar Referência**.
3. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
4. Na lista de SDKs, marque a caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.
5. No **Gerenciador de Soluções**, clique duas vezes em MainPage.xaml para abrir o designer da página principal no app.
6. Arraste um elemento **Button** de **Toolbox** até a página.
7. Clique duas vezes no botão do designer para abrir o arquivo de código e adicionar um manipulador de eventos para o evento **Click**.  
8. Substitua o conteúdo inteiro do arquivo de código pelo código a seguir. Atribua a variável ```projectId``` ao valor da [ID do projeto](run-app-experiments-with-a-b-testing.md#terms) que você obteve no painel do Centro de Desenvolvimento na seção anterior.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

10. Salve o arquivo de código e compile o projeto.

## <a name="create-the-experiment-in-windows-dev-center"></a>Criar o experimento no Centro de Desenvolvimento do Windows

1. Volte para a página do projeto de **Experimentos de clique de botão** no painel do Centro de Desenvolvimento do Windows.
2. Na seção **Experimentos**, clique no botão **Novo experimento**.
5. Na seção **Detalhes do experimento**, digite o nome **Otimizar cliques de botão** no campo **Nome do experimento**.
6. Na seção **Evento de visualização**, digite **userViewedButton** no campo **Nome do evento de visualização**. Observe que esse nome corresponde à cadeia de caracteres de evento de visualização ao qual você conectou o código que você adicionou na seção anterior.
7. Na seção **Metas e eventos de conversão**, insira os seguintes valores:
  * No campo **Nome da meta**, digite **Aumentar Cliques de Botão**.
  * No campo **Nome do evento de conversão**, digite o nome **userClickedButton**. Observe que esse nome corresponde à cadeia de caracteres de evento de conversão ao qual você conectou o código que você adicionou na seção anterior.
  * No campo **Objetivo**, escolha **Maximizar**.
8. Na seção **Variações e variáveis remotas**, confirme se a caixa de seleção **Distribuir igualmente** está marcada para que as variações sejam distribuídas igualmente ao seu app.
9. Adicione variáveis ao seu experimento:
  9. Clique no controle de lista suspensa, escolha **buttonText**e clique em **Adicionar variável**. A cadeia de caracteres **Botão cinza** deve ser exibida automaticamente na coluna **Variação A** (esse valor é derivado das configurações do projeto). Na coluna **Variação B**, digite **Botão azul**.
  9. Clique novamente no controle de lista suspensa, escolha **r**e clique em **Adicionar variável**. A cadeia de caracteres **128** deve aparecer automaticamente na coluna **Variação A**. Na coluna **Variação B**, digite **1**.
  9. Clique novamente no controle de lista suspensa, escolha **g**e clique em **Adicionar variável**. A cadeia de caracteres **128** deve aparecer automaticamente na coluna **Variação A**. Na coluna **Variação B**, digite **1**.  
  9. Clique novamente no controle de lista suspensa, escolha **b**e clique em **Adicionar variável**. A cadeia de caracteres **128** deve aparecer automaticamente na coluna **Variação A**. Na coluna **Variação B**, digite **255**.  
10. Clique em **Salvar** e depois em **Ativar**.

> **Importante**&nbsp;&nbsp;Depois de ativar um experimento, você não poderá mais modificar os parâmetros dele, a menos que você tenha clicado na caixa de seleção **Experimento editável** quando criou o experimento. Em geral, recomendamos codificar o experimento no seu app antes de ativar esse experimento.

## <a name="run-the-app-to-gather-experiment-data"></a>Execute o app para coletar dados de experimento

1. Execute o app **SampleExperiment** criado anteriormente.
2. Confirme que você está vendo um botão cinza ou azul. Clique no botão e, em seguida, feche o app.
3. Repita as etapas acima várias vezes no mesmo computador para confirmar que o app está mostrando a mesma cor de botão.

## <a name="review-the-results-and-complete-the-experiment"></a>Examine os resultados e conclua o experimento

Aguarde várias horas depois de concluir a seção anterior e, em seguida, siga estas etapas para analisar os resultados do seu experimento e concluir o processo.

> **Observação**&nbsp;&nbsp; Assim que você ativa um experimento, o Centro de Desenvolvimento começa imediatamente a coletar dados de quaisquer apps que sejam instrumentados para registrar dados para o seu experimento. No entanto, pode levar várias horas para que os dados do experimento apareçam no painel.

1. No Centro de Desenvolvimento, volte para a página **Experimentação** do seu app.
2. Na seção **Experimentos ativos**, clique em **Otimizar cliques de botão** para acessar a página desse experimento.
3. Confirme que os resultados mostrados nas seções **Resumo dos resultados** e **Detalhes dos resultados** correspondem ao que você espera ver. Para saber mais sobre essas seções, veja [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Observação**&nbsp;&nbsp;O Centro de Desenvolvimento relata apenas o primeiro evento de conversão de cada usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu app em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a ajudar a impedir que um usuário único com muitos eventos de conversão distorça os resultados do experimento de um grupo de amostra de usuários.
4. Agora, você está pronto para finalizar o experimento. Na seção **Resumo dos resultados**, na coluna **Variação B**, clique em **Alternar**. Isso alterna todos os usuários do seu app para o botão azul.
5. Clique em **OK** para confirmar que você deseja finalizar o experimento.
6. Execute o app **SampleExperiment** criado na seção anterior.
7. Confirme que você está vendo um botão azul. Observe que pode levar até dois minutos para que o seu app receba uma atribuição de variação atualizada.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar seu app para experimentação](code-your-experiment-in-your-app.md)
* [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
* [Executar experimentos de app com teste A/B](run-app-experiments-with-a-b-testing.md)

