---
author: Xansky
Description: In this walkthrough, you will create, run, and manage your first experiment with A/B testing.
title: Criar e executar seu primeiro experimento
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, testes comparativos, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: e5a4c3607486a7163648c7aa5a0e1d03d37e421f
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5985007"
---
# <a name="create-and-run-your-first-experiment"></a>Criar e executar seu primeiro experimento

Neste passo a passo, você fará o seguinte:
* Crie um [projeto](run-app-experiments-with-a-b-testing.md#terms) de experimentação no Partner Center que define diversas variáveis remotas que representam o texto e a cor de um botão de aplicativo.
* Criar um aplicativo com o código que recupera os valores de variáveis remotas, usará esses dados para alterar a cor de plano de fundo de um botão e registra o modo de exibição e dados de evento de conversão de volta para o Partner Center.
* Criará um experimento no projeto para testar se a mudança na cor de fundo de um botão de aplicativo aumenta com êxito o número de cliques do botão.
* Executará o aplicativo para coletar dados de experimento.
* Examine os resultados do experimento no Partner Center, escolha uma variação para habilitar para todos os usuários do aplicativo e conclua o experimento.

Para obter uma visão geral da / B teste com o Partner Center, veja [Executar experimentos de aplicativo com um teste a / B](run-app-experiments-with-a-b-testing.md).

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este passo a passo, você deve ter uma conta do Partner Center e você deve configurar seu computador de desenvolvimento, conforme descrito em [Executar experimentos de aplicativo com um teste a / B](run-app-experiments-with-a-b-testing.md).

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>Criar um projeto com variáveis remotas no Partner Center

1. Entre no [Partner Center](https://partner.microsoft.com/dashboard).
2. Se você já tiver um aplicativo no Partner Center que você deseja usar para criar um experimento, selecione esse aplicativo no Partner Center. Se você ainda não tiver um aplicativo no Partner Center, [criar um novo aplicativo reservando um nome](../publish/create-your-app-by-reserving-a-name.md) e, em seguida, selecione-o no Partner Center.
3. No painel de navegação, clique em **Serviços** e depois em **Experimentação**.
4. Na seção **Projetos** da próxima página, clique no botão **Novo projeto**.
5. Na página **Novo projeto**, insira o nome **Experimentos de clique de botão** para seu novo projeto.
6. Expanda a seção **Variáveis remotas** e clique em **Adicionar variável** quatro vezes. Agora, você terá quatro linhas de variáveis em branco.
  * Na primeira linha, digite **buttonText** para o tipo e o nome de variável **Botão cinza** na coluna **Valor padrão**.
  * Na segunda linha, digite **r** para o tipo e o nome de variável **128** na coluna **Valor padrão**.
  * Na terceira linha, digite **g** para o tipo e o nome de variável **128** na coluna **Valor padrão**.
  * Na quarta linha, digite **b** para o tipo e o nome de variável **128** na coluna **Valor padrão**.
7. Clique em **Salvar** e anote o valor da [ID do projeto](run-app-experiments-with-a-b-testing.md#terms) que consta na seção **integração do SDK**. Na próxima seção, você atualizará o código do aplicativo e fará referência a esse valor em seu código.

## <a name="code-the-experiment-in-your-app"></a>Codificar o experimento no seu aplicativo

1. No Visual Studio, crie um novo projeto de plataforma Universal do Windows usando Visual c#. Especifique o nome **SampleExperiment** para o projeto.
2. No Gerenciador de Soluções, expanda o nó do seu projeto, clique com botão direito no nó **Referências** e clique em **Adicionar Referência**.
3. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
4. Na lista de SDKs, marque a caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.
5. No **Gerenciador de Soluções**, clique duas vezes em MainPage.xaml para abrir o designer da página principal no aplicativo.
6. Arraste um elemento **Button** de **Toolbox** até a página.
7. Clique duas vezes no botão do designer para abrir o arquivo de código e adicionar um manipulador de eventos para o evento **Click**.  
8. Substitua o conteúdo inteiro do arquivo de código pelo código a seguir. Atribuir o ```projectId``` variável para o valor de [ID do projeto](run-app-experiments-with-a-b-testing.md#terms) que você obteve do Partner Center na seção anterior.
    [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

9. Salve o arquivo de código e compile o projeto.

## <a name="create-the-experiment-in-partner-center"></a>Criar o experimento no Partner Center

1. Retornar à página de projeto de **Experimentos de clique de botão** no Partner Center.
2. Na seção **Experimentos**, clique no botão **Novo experimento**.
3. Na seção **Detalhes do experimento**, digite o nome **Otimizar cliques de botão** no campo **Nome do experimento**.
4. Na seção **Evento de visualização**, digite **userViewedButton** no campo **Nome do evento de visualização**. Observe que esse nome corresponde à cadeia de caracteres de evento de visualização ao qual você conectou o código que você adicionou na seção anterior.
5. Na seção **Metas e eventos de conversão**, insira os seguintes valores:
  * No campo **Nome da meta**, digite **Aumentar Cliques de Botão**.
  * No campo **Nome do evento de conversão**, digite o nome **userClickedButton**. Observe que esse nome corresponde à cadeia de caracteres de evento de conversão ao qual você conectou o código que você adicionou na seção anterior.
  * No campo **Objetivo**, escolha **Maximizar**.
6. Na seção **Variações e variáveis remotas**, confirme se a caixa de seleção **Distribuir igualmente** está marcada para que as variações sejam distribuídas igualmente ao seu aplicativo.
7. Adicione variáveis ao seu experimento:
    1. Clique no controle de lista suspensa, escolha **buttonText**e clique em **Adicionar variável**. A cadeia de caracteres **Botão cinza** deve ser exibida automaticamente na coluna **Variação A** (esse valor é derivado das configurações do projeto). Na coluna **Variação B**, digite **Botão azul**.
    2. Clique novamente no controle de lista suspensa, escolha **r**e clique em **Adicionar variável**. A cadeia de caracteres **128** deve aparecer automaticamente na coluna **Variação A**. Na coluna **Variação B**, digite **1**.
    3. Clique novamente no controle de lista suspensa, escolha **g**e clique em **Adicionar variável**. A cadeia de caracteres **128** deve aparecer automaticamente na coluna **Variação A**. Na coluna **Variação B**, digite **1**.  
    4. Clique novamente no controle de lista suspensa, escolha **b**e clique em **Adicionar variável**. A cadeia de caracteres **128** deve aparecer automaticamente na coluna **Variação A**. Na coluna **Variação B**, digite **255**.  
8. Clique em **Salvar** e depois em **Ativar**.

> [!IMPORTANT]
> Depois de ativar um experimento, você não pode mais modificar os parâmetros dele, a menos que tenha clicado na caixa de seleção **Experimento editável** quando criou o experimento. Em geral, recomendamos codificar o experimento no seu app antes de ativar esse experimento.

## <a name="run-the-app-to-gather-experiment-data"></a>Execute o aplicativo para coletar dados de experimento

1. Execute o aplicativo **SampleExperiment** criado anteriormente.
2. Confirme que você está vendo um botão cinza ou azul. Clique no botão e, em seguida, feche o aplicativo.
3. Repita as etapas acima várias vezes no mesmo computador para confirmar que o aplicativo está mostrando a mesma cor de botão.

## <a name="review-the-results-and-complete-the-experiment"></a>Examine os resultados e conclua o experimento

Aguarde várias horas depois de concluir a seção anterior e, em seguida, siga estas etapas para analisar os resultados do seu experimento e concluir o processo.

> [!NOTE]
> Assim que você ativa um experimento, Partner Center começa imediatamente a coletar dados de quaisquer aplicativos que sejam instrumentados para registrar os dados de seu experimento. No entanto, pode levar várias horas para dados do experimento apareçam no Partner Center.

1. No Partner Center, retorne à página **experimentação** para seu aplicativo.
2. Na seção **Experimentos ativos**, clique em **Otimizar cliques de botão** para acessar a página desse experimento.
3. Confirme que os resultados mostrados nas seções **Resumo dos resultados** e **Detalhes dos resultados** correspondem ao que você espera ver. Para obter mais detalhes sobre essas seções, consulte [Gerenciar seu experimento no Partner Center](manage-your-experiment.md#review-the-results-of-your-experiment).
    > [!NOTE]
    > Partner Center relata apenas o primeiro evento de conversão para cada usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu app em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a ajudar a impedir que um usuário único com muitos eventos de conversão distorça os resultados do experimento de um grupo de amostra de usuários.

4. Agora, você está pronto para finalizar o experimento. Na seção **Resumo dos resultados**, na coluna **Variação B**, clique em **Alternar**. Isso alterna todos os usuários do seu aplicativo para o botão azul.
5. Clique em **OK** para confirmar que você deseja finalizar o experimento.
6. Execute o aplicativo **SampleExperiment** criado na seção anterior.
7. Confirme que você está vendo um botão azul. Observe que pode levar até dois minutos para que o seu aplicativo receba uma atribuição de variação atualizada.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar o aplicativo para experimentação](code-your-experiment-in-your-app.md)
* [Definir seu experimento no Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gerenciar seu experimento no Partner Center](manage-your-experiment.md)
* [Executar experimentos de app com teste A/B](run-app-experiments-with-a-b-testing.md)
