---
title: Criar um placar de líderes no Unity
description: Guia sobre a criação de seu próprio placar de líderes no Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity, placares de líderes
ms.openlocfilehash: a62cb2b3bd4afebcc7aa9060db60ac167f052977
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657821"
---
# <a name="leaderboards-data-in-unity"></a>Dados de placares de líderes no Unity

Para aqueles que desejam uma experiência de placar de líderes personalizados, este artigo fornecerá as ferramentas para implementar seu próprio placar de líderes indo sobre as APIs disponíveis para desenvolvedores do Unity. Depois de compreender como efetuar pull de dados de placar de líderes, você poderá aplicá-lo à interface do usuário de sua escolha.

[!IMPORTANT]
> Este artigo se aplica a uma versão do plug-in antes de uma atualização feita em maio de 2018 (versão 1804). Se você instalou o Plugin do Xbox Live depois desse período, ou ainda não baixou-lo, você pode ter uma versão mais recente que usa chamadas diferentes para coletar dados de placar de líderes. Além disso, você descobrirá que as capturas de tela desse plug-in não correspondem da versão mais recente. Em vez disso, consulte a [placares de líderes de unity mais recente do artigo transitório](unity-leaderboard-from-scratch.md).


## <a name="call-for-leaderboard-data"></a>Chamada para dados de placar de líderes

Para solicitar dados de placar de líderes do serviço Xbox Live, que você deve chamar  `void GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)`

Com êxito, fazer essa chamada, você precisará adquirir uma `XboxLiveUser` de [entrar](unity-prefabs-and-sign-in.md), têm uma [configurado stat](add-stats-and-leaderboards-in-unity.md) com o valor para pelo menos um player e o formulário um `LeaderboardQuery`. Se você não souber como um usuário entrar ou precisa inicializar uma estatística para o placar de líderes, você pode ler os artigos vinculados. Depois que você tiver uma inicializada uma estatística a maneira mais fácil para associá-la ao seu script de placar de líderes é incluir um dos pré-fabricados de estatística: `IntegerStat`, `DoubleStat`, ou `StringStat` como uma variável pública. Seu stat precisará ter sua propriedade de ID configurada no mínimo, como isso é o que usaremos para o **statName** quando chamamos para nossos dados de placar de líderes de parâmetro. Por fim, você precisará para formar um `LeaderboardQuery` objeto.
Um `LeaderboardQuery` tem alguns atributos que podem ser definidos que afetarão os dados retornados:

- **StatName(Required):** esta é a ID do stat o placar de líderes recuperará os dados, se isso não é definido não é definido como um valor de ID de estatísticas, nenhum dado será retornado.
- **SocialGroup:** , dependendo do valor dessa cadeia de caracteres seus dados de placar de líderes retornados serão filtrados para seus amigos, seus amigos Favoritos ou se não definido, recuperará um placar de líderes global não filtrada. O valor "" será retorna uma lista não filtrada, o valor "all" retornará uma lista com apenas seus amigos nele, "favorite" retornará uma lista com apenas seus amigos Favoritos presentes.
- **SkipResultToRank:** se definida, essa variável uint determinará o que os dados de placar de líderes de classificação será iniciado ao retornar. Classificações começam na posição 1.
- **SkipResultToMe:** se definido como true, esse valor booliano fará com que os dados de placar de líderes retornados para iniciar à `XboxLiveUser` usados no `GetLeaderboard()` chamar.
- **Ordem:** Enumerações do tipo `Microsoft.Xbox.Services.Leaderboard.SortOrder` têm dois valores possíveis, em ordem crescente e decrescente. Configurar esta variável para a sua consulta determinará a ordem de classificação de seu placar de líderes. Por padrão o placar de líderes retorna dados em ordem decrescente.
- **MaxItems:** Este uint determina o número máximo de linhas a serem retornadas por chamada ao `GetLeaderboard()`.

Que formam seu LeaderboardQuery pode parecer semelhante ao seguinte:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            StatName = stat.ID,
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

Usando esta consulta retornará cinco linhas de placar de líderes começando a 100th classificação individuais para a estatística determinada.

> [!WARNING]
> Definir SkipResultToRank maior do que o número de jogadores contidos no placar de líderes fará com que os dados de placar de líderes por retornar com zero linhas.

Agora que temos todas as partes juntas, chamamos o `GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)` função, onde usaremos nosso assinado `XboxLiveUser` (provavelmente uma `XboxLiveUserInfo.User`) obtido de entrada, como nosso usuário e o `LeaderboardQuery` que acabamos de criar como nossa consulta.

O placar de líderes de chamada de função pode parecer semelhante ao seguinte:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

public void LoadLeaderboard()
    {
        // check to make sure you have a valid stat
        if (this.stat == null)
        {
            // TO DO: Display "Stat not specified" error message!
            return;
        }

        // check to make sure you have an XboxLiveUser
        if (this.xboxLiveUser == null)
        {
            if (XboxLiveUserManager.Instance.UserForSingleUserMode != null
                && XboxLiveUserManager.Instance.UserForSingleUserMode.User != null)
            {
                // set the XboxLiveUser if one is available
                this.xboxLiveUser = XboxLiveUserManager.Instance.UserForSingleUserMode.User;
            }
            else // if you don't have a user signed in display an error message and exit. 
            {
                // TO DO: Display "User Not logged In" error message
                return;
            }
        }

        LeaderboardQuery query;

        query = new LeaderboardQuery
        {
            StatName = this.stat.ID,
            MaxItems = this.maxItems,

        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
    }
```

Agora, você talvez tenha observado que nosso placar de líderes ao recuperar a função `GetLeaderboard()` retorna void e, portanto, não retorna os dados de placar de líderes que estamos procurando. Na verdade, estamos recuperará os dados de placar de líderes em uma função de evento discutido na próxima seção.

## <a name="receive-the-leaderboard-data"></a>Receber os dados de placar de líderes

Para recuperar os dados de placar de líderes, você precisará adicionar uma função de escuta para o `StatsManagerComponent` instância para o título. Você deve adicionar a seguinte linha de código para o `Awake()` função do seu código: `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`.

```csharp
private void Awake()
    {
        this.EnsureEventSystem();
        XboxLiveServicesSettings.EnsureXboxLiveServicesSettings();

        StatsManagerComponent.Instance.GetLeaderboardCompleted += this.GetLeaderboardCompleted;
    }
```

O `StatsManagerComponent` escuta para eventos de conclusão de placar de líderes. Ao executar esta linha de código, você adicionará uma função para a lista de funções a ser chamado quando ocorre um evento de conclusão de placar de líderes. Neste exemplo, se a função é chamada `MyGetLeaderBoardCompletedFunction()`, você pode nomear a função quanto quiser em seu próprio script. A função "MyGetLeaderboardCompletedFunction" precisará usam dois parâmetros, um objeto que representa o remetente e um `Microsoft.Xbox.Services.Client.StatEventArgs` parâmetro. O shell de sua função pode ter esta aparência:

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

A primeira coisa de que essa função deve fazer é verificar se há erros que podem ser encontradas no `StatEventArgs` statArgs de parâmetro. StatArgs contém um `StatisticEvent` chamou EventData, que contém o `System.Exception` chamado ErrorInfo. Você o encontrará no statArgs.EventData.ErrorInfo se ocorreu um erro ao recuperar os dados de placar de líderes. Você pode adicionar um simple se a instrução para o código anterior para verificar se há erros.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }
    }
```

Depois de confirmar que não há nenhum erro, armazenar os resultados da solicitação placar de líderes que são encontradas no `statArgs.EventData.EventArgs.Result`. `Result` é um `LeaderBoardResult` objeto que contém os dados necessários para popular o placar de líderes. Em nosso código de exemplo, extrairá esses dados e enviá-lo para outra função chamada LoadResult.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

O `LeaderboardResult` resultado que enviamos para a função LoadResult terá todos os dados, precisamos tanto ler os dados de placar de líderes que foi retornados como fazer chamadas adicionais para recuperar as classificações que ainda não foi retornadas pela chamada original. Convém armazenar os resultados em uma variável de classe para o seu script de placar de líderes da seguinte forma:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

Isso é importante porque o `LeaderboardResult` contém uma propriedade de HasNext que determina se há uma seção posterior do placar de líderes que pode ser recuperado. O `LeaderboardResult` também armazena uma variável para o total de linhas que compõem o placar de líderes. Essas propriedades serão importantes para o placar de líderes de navegação. Para extrair dados de sua `LeaderBoardResult` simplesmente implementar um para o loop usando a `LeaderboardResults` lista de `LeaderboardRow` chamado `Rows`. Nosso código de exemplo simplesmente vamos para concatenar os valores em cada `LeaderboardRow` em uma cadeia de caracteres a ser exibido.

```csharp
using Microsoft.Xbox.Services.Leaderboard

void LoadResult(LeaderboardResult result)
{

    this.leaderboardData = result;

    this.leaderBoardText.text = "" // leaderBoardText is a UnityEngine.UI text box.

    foreach (LeaderboardRow row in this.leaderboardData.Rows)
    {
        leaderBoardText.text += string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", row.Rank, row.Gamertag, this.stat.DisplayName, row.Values[0]);
    }
}
```

Em nosso exemplo, usamos as propriedades de classificação, nome de jogador e valores de `LeaderBoardResult` para preencher nossa cadeias de caracteres, bem como o DisplayName do associado com o placar de líderes de stat.

Tenho certeza de que você poderá fazer algo mais criativo com todos esses dados de placar de líderes.

## <a name="navigating-the-leaderboard-data"></a>Navegando pelos dados o placar de líderes

Nos casos mais comuns não carregará cada classificação única em seu placar de líderes ao mesmo tempo e precisará ser capaz de navegar as classificações para exibir as diferentes seções de placar de líderes para o usuário. Digamos que você tenha um placar de líderes com cem classificados jogadores. Em sua chamada inicial para `GetLeaderboard()` você recuperar dez `LeaderboardRows` e exibi-las para o player. O player talvez queira ver mais de dez principais jogadores. A maneira mais fácil de obter o próximo conjunto de dez usuários é determinar ou não a `LeaderboardResult` armazenados no seu último consulta tem mais linhas a serem recuperadas e em seguida, chamando `GetLeaderboard()` com esse LeaderboardResult `NextQuery` propriedade. O código a seguir recuperará o próximo conjunto de placar de líderes de dados.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.NextQuery;
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
        }
        else
        {
            //TO DO: Display an error message or go back to the beginning of the leaderboard as the situation demands.
        }
    }
```

Mover para trás no seu placar de líderes é um pouco mais difícil, pois não há nenhuma função para efetuar pull anterior X número de classificações de seu placar de líderes. Para recuperar as classificações anteriores, que você terá que escrever sua própria lógica. Um método seria armazenar sua `MaxItems` por `LeaderboardQuery` e calcular a qual você precisa ignorar ao uso de classificação a `SkipToRank` atributo do seu `LeaderboardQuery`. Esse código pode ter esta aparência:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetPreviousLeader()
{
    if(leaderboardData == null || leaderboardData.Rows.Count < 1)
    {
        return;
    }

    uint topRank = leaderboardData.Rows[0].Rank; //get the first rank of the leaderboard.
    uint targetRank = topRank - this.maxItems > 0 ? topRank - this.maxItems : 0; //set your targetRank equal to the current top rank of your leaderboard minus the configured number of rows to display at a time.

    LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
    {
        StatName = stat.ID,
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
}
```

O cenário mais comum final é que um jogador simplesmente talvez queira ver seus especial no placar de líderes. Isso é feito facilmente por meio da chamada a `GetLeaderboard()` função com uma consulta em que o `SkipResultToMe` atributo é definido como true.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

    void GetRankForTag()
    {

        LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
        {
            StatName = stat.ID,
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
    }
```

Se você desejar se aprofundar em um exemplo mais detalhado do placar de líderes sempre possam ler o script Leaderboard.cs na pasta XboxLive Plugin em ativos >> XboxLive >> Scripts >> Leaderboard.cs.