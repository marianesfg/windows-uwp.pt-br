---
author: mcleanbyron
Description: Antes de executar um experimento em seu aplicativo UWP (Plataforma Universal do Windows) com os testes A/B, você deve definir seu experimento no painel do Centro de Desenvolvimento.
title: Definir seu experimento no painel do Centro de Desenvolvimento
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
---

# Definir seu experimento no painel do Centro de Desenvolvimento

Para executar um experimento em seu aplicativo UWP (Plataforma Universal do Windows) com os testes A/B, comece definindo seu experimento no painel do Centro de Desenvolvimento.

As seções a seguir descrevem o processo geral para definir um experimento no painel. Para um guia passo a passo que demonstra o processo de criação e execução de um experimento de ponta a ponta, veja [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Obter uma chave de API

Para começar, vá para a página **Experimentação** do painel do Centro de Desenvolvimento e obtenha uma *chave de API* para seu experimento.

Uma chave de API é uma ID exclusiva que associa seu aplicativo a um experimento em sua conta do Centro de Desenvolvimento. Cada experimento é associado a exatamente uma chave de API. No entanto, um aplicativo pode ter várias chaves de API, e cada chave de API pode ser associada a vários experimentos. Você pode usar chaves de API para ajudar a diferenciar entre conjuntos de experimentos distintos. Por exemplo, você pode ter um conjunto de experimentos liberado para os testadores de sua organização e outro conjunto de experimentos liberado somente para usuários externos do seu aplicativo.

Você deve usar essa chave de API para se conectar ao serviço de testes A/B no código do aplicativo. Para saber mais, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).

1. Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview).
2. Em **Seus aplicativos**, selecione o aplicativo para o qual você deseja criar um experimento.
3. No painel de navegação, selecione **Serviços** e, em seguida, selecione **Experimentação**.
4. A seção **Chaves de API** fornece uma chave de API padrão para o seu aplicativo, que é chamada de **Chave de API nº 1**. Se você quiser usar essa chave, opcionalmente, digite um nome amigável para essa chave e copie-o para usar no código de seu aplicativo. Para gerar uma nova chave de API, selecione **Nova chave de API** e insira um nome amigável para a chave de API.

## Criar um experimento

Em seguida, crie um novo experimento e defina as metas para o experimento.

1. Na seção **Experimentos**, clique no botão **Novo experimento**.
2. Na seção **Nome da chave de API**, escolha a chave de API que você deseja associar a este experimento. Se você tiver apenas uma chave de API, essa chave de API será selecionada por padrão.
3. No campo **Nome do experimento**, digite um nome que você pode usar para identificar facilmente o experimento. Depois que você criar um experimento, esse nome aparecerá na lista de experimentos de rascunho, ativos e concluídos na página **Experimentação**.
4. Se você quiser criar um experimento de teste, clique na caixa de seleção **Experimento de teste**. A diferença entre experimentos de teste e experimentos normais é que apenas os experimentos de teste podem ser alterados depois que são ativados.

  Os experimentos de teste destinam-se a ajudá-lo a testar todas as variações em um dispositivo cliente antes de liberar seu experimento para os clientes. Para se certificar de que uma variação seja enviada para os clientes conforme o esperado, você pode ativar um experimento de teste com a distribuição de 100% alocada para uma variação e 0% alocada para outras variações. Depois de verificar essa variação, você pode repetir o processo para outras variações.
  > **Observação**  Marque essa caixa de seleção apenas se você estiver criando um experimento de teste para validar parâmetros por meio de testes internos. Não marque essa caixa se estiver criando um experimento que você liberará para os clientes.

5. No campo **Exibir nome do evento**, digite o nome do *evento de exibição* para seu experimento. O evento de exibição é uma cadeia de caracteres arbitrária que representa uma atividade quando o usuário começa a exibir uma variação que faz parte do seu experimento. O código do seu aplicativo enviará essa cadeia de caracteres de evento de exibição para o Centro de Desenvolvimento, quando o usuário começar a exibir uma variação. Para saber mais, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).
6. Na seção **Metas e eventos de conversão**, defina pelo menos uma meta para seu experimento:
  * No campo **Nome da meta**, digite um nome descritivo para sua meta. Depois que você executar um experimento, esse nome aparecerá no resumo dos resultados do experimento.
  * No campo **Nome do evento de conversão**, digite o nome do *evento de conversão* dessa meta. Um evento de conversão é uma cadeia de caracteres arbitrária que representa um objetivo para essa meta. O código do seu aplicativo enviará essa cadeia de caracteres de evento de conversão para o Centro de Desenvolvimento quando o usuário atingir um objetivo. Para saber mais, veja [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md).
  * No campo **Objetivo**, escolha **Maximizar** ou **Minimizar**, dependendo se você deseja maximizar ou minimizar as ocorrências do evento de conversão. Essas informações são usadas no resumo dos resultados do experimento.

  >**Observação** O Centro de Desenvolvimento relata apenas o primeiro evento de conversão de cada modo de exibição do usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu aplicativo em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a impedir que um usuário único distorça os resultados do experimento de um grupo de amostra de usuários quando o objetivo é maximizar o número de usuários que executam uma conversão.

## Definir as variações e as configurações para o experimento

Em seguida, defina as variações e as configurações de seu experimento.

* Uma *variação* é uma coleção de uma ou mais configurações que você está testando em seu experimento. Cada experimento deve ter pelo menos uma configuração e duas variações (incluindo o controle). Um experimento pode ter até cinco variações.
* Uma *configuração* é um valor que seu aplicativo usa para inicializar uma variável de programa. Durante o experimento, o valor da configuração muda de variação para variação. Depois que você termina o experimento, é atribuído à configuração o valor da variação que você optar por liberar para todos os usuários de seu aplicativo. As configurações podem ter os seguintes tipos: cadeia de caracteres, booliano, duplo e inteiro.

Para definir as variações e as configurações de seu experimento:
1. Na seção **Variações e configurações**, você verá duas variações do padrão, **Variação A (controle)** e **Variação B**. Se você quiser mais variações, clique em **Adicionar variação**. Opcionalmente, você pode renomear cada variação.
2. Em seguida, crie as configurações para suas variações. Clique em **Adicionar configuração** para criar cada nova configuração e digite o nome da configuração e o valor da configuração em cada variação.
3. Por padrão, as variações são igualmente distribuídas para os usuários do aplicativo. Se você deseja escolher uma porcentagem de distribuição específica, desmarque a caixa de seleção **Distribuir igualmente** e digite as porcentagens na linha **Distribuição (%)**.

## Salve seu experimento

Quando você terminar de inserir os campos obrigatórios do seu experimento, clique em **Salvar** para salvar seu experimento.

> **Importante** Depois de salvar um experimento, você não pode alterar a chave de API desse experimento, mesmo que ainda não o tenha ativado.

Se você estiver satisfeito com os parâmetros de seu experimento e estiver pronto para ativá-lo para poder iniciar a coleta de dados do experimento do seu aplicativo, clique em **Ativar**. Quando o experimento está ativo, seu aplicativo pode recuperar as configurações de variação e relatar os eventos de exibição e de conversão para o Centro de Desenvolvimento.

> **Importante**  Depois de ativar um experimento, você não pode mais modificar os parâmetros dele, a menos que ele seja um experimento de teste (você clicou na caixa de seleção **Experimento de teste** quando criou o experimento). Recomendamos que você codifique o experimento em seu aplicativo antes de ativar seu experimento.

## Próximas etapas

Depois de definir seu experimento no painel do Centro de Desenvolvimento, você está pronto para as seguintes etapas:
1. [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md). Use uma API no SDK de Microsoft Store Engagement and Monetization para obter configurações de variação para o experimento. Use esses dados para modificar o comportamento do recurso que você está testando e envie eventos de exibição e de conversão para o Centro de Desenvolvimento.
2. [Executar e gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md). Use o painel para analisar os resultados do experimento e conclua o experimento.

## Tópicos relacionados

  * [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
  * [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
  * [Criar e executar seu primeiro experimento com testes A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=May16_HO2-->


