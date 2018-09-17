---
author: mcleanbyron
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must define your experiment in the Dev Center dashboard.
title: Definir seu experimento no painel
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, testes A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: f690aaede800f76b2eb006355e982ea6b3509b78
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3988205"
---
# <a name="define-your-experiment-in-the-dashboard"></a>Definir seu experimento no painel

Após [criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) e [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md), você estará pronto para criar um experimento no projeto. Quando você cria o experimento, você definirá os objetivos e as variações as quais os usuários receberão.

Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>Criar seu experimento

1. Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview).
2. Em **Seus aplicativos**, selecione o aplicativo para o qual você deseja criar um experimento.
3. No painel de navegação, selecione **Serviços** e, em seguida, selecione **Experimentação**.
4. Na página **Experimentação**, identifique o projeto onde deseja adicionar um experimento na tabela de projetos e clique no link **Adicionar experimento** para aquele projeto.
5. No campo **Nome do experimento**, digite um nome que você pode usar para identificar facilmente o experimento. Depois de criar um experimento, esse nome aparecerá na lista de experimentos existentes na página **Experimentação** do seu aplicativo e na página do projeto.
6. Se você deseja editar o experimento enquanto ele está ativo, clique na caixa de seleção **Experimento editável**. Marque essa caixa de seleção apenas se você estiver criando um experimento para validar todas as variações por meio de testes internos. Para obter mais informações, consulte [Criar um experimento para testes internos](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments).
    > [!NOTE]
    > Não marque essa caixa se estiver criando um experimento que você liberará para os clientes (isto é, uma experiência que está associada com a ID do projeto que é usada em uma versão do seu app e está disponível para os clientes). Editar um experimento enquanto ele está ativo invalidará os resultados do experimento.

7. Na lista suspensa **Nome do projeto**, o projeto atual será selecionado automaticamente. Se você deseja adicionar o novo experimento em um projeto diferente, você pode selecionar esse projeto aqui. Caso contrário, deixe esta seleção sozinha.
8.   Anote o valor do [ID do projeto](run-app-experiments-with-a-b-testing.md#terms). Ao [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md), você deve fazer referência a esse ID em seu código para que receber dados de variação e relatar eventos de exibição e conversão no Centro de Desenvolvimento.
9. Na seção **Evento de visualização**, digite o nome do [evento de visualização](run-app-experiments-with-a-b-testing.md#terms) para o seu experimento no campo **Nome do evento de visualização**.
10. Na seção **Metas e eventos de conversão**, defina pelo menos uma meta para seu experimento:
  * No campo **Nome da meta**, digite um nome descritivo para sua meta. Depois que você executar um experimento, esse nome aparecerá no resumo dos resultados do experimento.
  * No campo **Nome do evento de conversão**, digite o nome do [evento de conversão](run-app-experiments-with-a-b-testing.md#terms) dessa meta.
  * No campo **Objetivo**, escolha **Maximizar** ou **Minimizar**, dependendo se você deseja maximizar ou minimizar as ocorrências do evento de conversão. Essas informações são usadas no resumo dos resultados do experimento.

> [!NOTE]
> O Centro de Desenvolvimento relata apenas o primeiro evento de conversão de cada modo de exibição do usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu aplicativo em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a impedir que um usuário único distorça os resultados do experimento de um grupo de amostra de usuários quando o objetivo é maximizar o número de usuários que executam uma conversão.

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>Defina as variáveis remotas e as variações de seu experimento

Em seguida, defina as [variáveis](run-app-experiments-with-a-b-testing.md#terms) remotas e as [variações](run-app-experiments-with-a-b-testing.md#terms) de seu experimento.

1. Na seção **Variáveis remotas e variações**, você verá duas variações do padrão, **Variação A (controle)** e **Variação B**. Se você quiser mais variações, clique em **Adicionar variação**. Opcionalmente, você pode renomear cada variação.
2. Por padrão, as variações são igualmente distribuídas para os usuários do aplicativo. Se você deseja escolher uma porcentagem de distribuição específica, desmarque a caixa de seleção **Distribuir igualmente** e digite as porcentagens na linha **Distribuição (%)**.
3. Adicione variáveis remotas às suas variações. No controle de lista suspensa na parte inferior desta seção, escolha cada variável que você deseja adicionar e clique em **Adicionar variável**.
    > [!NOTE]
    > As variáveis listadas neste controle são herdadas do projeto para o experimento. O valor padrão para a variável (conforme definido no projeto) é atribuído automaticamente para a variação de controle. Se você quiser criar novas variáveis que não estejam listadas aqui, vá para a página de projeto relacionados e adicione as variáveis lá.

4. Edite os valores de variáveis para cada variação exclusiva no experimento (ou seja, as variações que não sejam variações de controle).

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>Salve e ative seu experimento

Quando você terminar de inserir os campos obrigatórios do seu experimento, clique em **Salvar** para salvar seu experimento.

Se você estiver satisfeito com os parâmetros de seu experimento e estiver pronto para ativá-lo para poder iniciar a coleta de dados do experimento do seu aplicativo, clique em **Ativar**. Quando o experimento está ativo, seu aplicativo pode recuperar as variáveis de variação e relatar os eventos de exibição e de conversão para o Centro de Desenvolvimento. Para obter mais informações, veja [Executar e gerenciar o experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md).

> [!IMPORTANT]
> Um projeto pode conter apenas um experimento ativo de cada vez. Depois de ativar um experimento, você não pode mais modificar os parâmetros dele, a menos que tenha selecionado a caixa de seleção **Experimento editável** quando criou o experimento. Recomendamos que você codifique o experimento em seu aplicativo antes de ativar seu experimento.

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>Crie um experimento para testes internos

Você pode querer testar seu experimento com um público controlado (por exemplo, um conjunto de testadores internos) e confirmar se todas as variações estão funcionando conforme o esperado antes de você ativar o experimento para seus clientes. Você pode fazer isso criando um experimento que tem a opção **Experimento editável** selecionada.

Para testar seu experimento antes de liberá-lo aos clientes, siga este processo:

1. Crie dois projetos: um para a versão pública do seu aplicativo e outro para uma compilação particular do seu aplicativo que está disponível apenas para seu público de teste. As instruções a seguir se referem a esses projetos como o projeto público e projeto de teste, respectivamente.
2. Ao [codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md), faça referência a ID do projeto do seu projeto público na compilação pública do seu aplicativo. Na compilação privada do seu aplicativo, faça referência a ID do projeto do seu projeto de teste.
3. Crie um experimento no projeto de teste e selecione a opção **experimento editável** para o experimento.
4. Ative o experimento no projeto de teste. Aloque a distribuição de 100% para uma variação de e verifique se essa variação funciona conforme o esperado para os testadores. Repita o processo para outras variações.
5. Depois de verificar se as variações estão funcionando conforme o esperado, faça as alterações finais para o experimento no projeto de teste. Quando estiver pronto para liberar o experimento aos seus clientes, clone o experimento para o projeto público. Nesse experimento, não marque a opção **experimento editável**.
4. Certifique-se de que a distribuição de variação de destino está correta no experimento clonado.
5. Ative o experimento clonado para liberar o experimento para seus clientes.

## <a name="next-steps"></a>Próximas etapas

Depois de definir seu experimento no painel do Centro de Desenvolvimento e codificá-lo no seu aplicativo, você estará pronto para [executar e gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Criar um projeto e definir variáveis remotas no painel do Centro de Desenvolvimento](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
* [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
* [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Executar experimentos de app com teste A/B](run-app-experiments-with-a-b-testing.md)
