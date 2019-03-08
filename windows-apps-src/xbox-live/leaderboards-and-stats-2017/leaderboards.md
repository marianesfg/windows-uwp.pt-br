---
title: Placares de líderes
description: Saiba como usar o Xbox Live placares de líderes para comparar os jogadores.
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.date: 09/28/2018
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8fd7e30b99418fda614a888d9269548cdc57a88a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662021"
---
# <a name="leaderboards"></a>Placares de líderes

## <a name="introduction"></a>Introdução

Conforme descrito em [visão geral da plataforma de dados](../data-platform/data-platform.md), placares de líderes são uma ótima maneira para incentivar a concorrência entre os jogadores e manter os jogadores envolvidos na tentativa de atingir a melhor pontuação de anterior, bem como de seus amigos.

Placar de líderes para [estatísticas em destaque](stats2017.md#configured-stats-and-featured-leaderboards) são sempre exibidos no Hub de jogo de um título e, às vezes, exibido como parte da interface do usuário para um título quando ele é fixado à página inicial. Você também pode usar suas estatísticas de em destaque configurado para criar um placar de líderes dentro de seu título.

## <a name="choosing-good-leaderboards"></a>Escolhendo um bom placares de líderes

Conforme discutido em [Player Stats](player-stats.md), um placar de líderes corresponde a um estado que você definiu.  Você deve escolher placares de líderes que correspondem a uma conquista que um jogador pode trabalhar para melhorar.

Por exemplo, o melhor tempo de visão geral em um jogo de corrida é um bom placar de líderes, porque os jogadores vai querer trabalhar para melhorar o seu tempo de obter uma visão melhor.  Outros exemplos são a taxa de eliminação/morte para atiradores ou tamanho máximo da caixa de combinação em um jogo de combater.

## <a name="when-to-display-leaderboards"></a>Quando exibir o placar de líderes

Você tem a capacidade de exibir o placar de líderes a qualquer momento em seu título.  Você deve escolher um horário quando um placar de líderes não irá interferir com o jogo ou o fluxo do seu título.  Entre os turnos e depois que as correspondências são os dois bons momentos.

## <a name="how-to-display-leaderboards"></a>Como exibir o placar de líderes

Há várias opções para exibir o placar de líderes fornecidos no SDK do Xbox Live.  Se você estiver usando o Unity com o programa de criadores do Xbox Live, você pode começar a usar, usando um placar de líderes pré-fabricado para exibir seus dados de placar de líderes.  Consulte a [configurar o Xbox Live no Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) artigo para obter informações específicas.

Se você está codificando o Xbox Live SDK diretamente, em seguida, continue lendo para saber mais sobre as APIs que você pode usar.

## <a name="programming-guide"></a>Guia de programação

Há várias APIs de placar de líderes, você pode usar para obter o estado atual de um placar de líderes.  Todas as APIs são assíncronas e não bloqueiam.  Faça uma solicitação para obter dados de placar de líderes e continuar o processamento normal de jogo.  Quando os resultados do placar de líderes são retornados do serviço, você pode exibir os resultados no momento apropriado.

Você deve solicitar os dados de placar de líderes de serviço, um pouco à frente de quando você deseja exibi-lo, para que os jogadores não estão bloqueados aguardando o placar de líderes exibir.

## <a name="leaderboards-2013-apis"></a>APIs de 2013 do placar de líderes

Você pode ver o `leaderboard_service` namespace para todas as APIs de placar de líderes de 2013 de estatísticas.

<table>

<tr>
<td>C++ API</td><td>Descrição</td>
</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
        const string_t& scid,
        const string_t& name
        );
```

</td>

<td>Versão mais básica da API.  Isso retornará os valores de placar de líderes para o placar de líderes determinado, começando do player no topo do placar de líderes.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
        _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName
        ) 
```

</td>

<td>WinRT C# de código – obter um placar de líderes para um único placar de líderes considerando uma ID de configuração de serviço e um nome de placar de líderes.</td>

</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ uint32_t skipToRank,
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>Essa API fornece alguns mais flexibilidade, você pode especificar a classificação (posição) que você deseja exibir, bem como um valor máximo de itens a serem retornados.  Por exemplo, você usaria essa API se você quisesse exibir o placar de líderes começando na posição 1000.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_ uint32 skipToRank,
         _In_ uint32 maxItems
        ) 
```

</td>

<td>WinRT C# de código – obter uma página de resultados do placar de líderes para um placar de líderes único forneça uma ID de configuração de serviço e um nome de placar de líderes, placar de líderes de resultados serão iniciada na classificação "skipToRank".</td>

</tr>

<tr>

<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard_skip_to_xuid(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ const string_t& skipToXuid = string_t(),
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>

Use essa opção se você quiser ignorar o placar de líderes para um determinado usuário.  Um `XUID` é um identificador exclusivo para cada usuário do Xbox.  Você pode obter para o usuário conectado, ou qualquer um dos seus amigos e passá-lo para essa função.

</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardWithSkipToUserAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_opt_ Platform::String^ skipToXboxUserId,
         _In_ uint32 maxItems
        )
```

</td>

<td>WinRT C# de código – obter um placar de líderes começando em um player especificado, independentemente do jogador classificação ou pontuação, ordenadas por classificação de percentil do player</td>

</tr>

</table>

## <a name="2013-c-example"></a>Exemplo de C++ de 2013

Ao usar a camada da API do C++, em seguida, você pode definir um retorno de chamada a ser invocado quando os resultados do placar de líderes são retornados do serviço.  Mostramos um exemplo abaixo.

Se você estiver familiarizado com o `pplx::task` que está sendo retornado a partir dessas APIs, este é um objeto de tarefa assíncrona da Microsoft Parallel Programming PPL (biblioteca).  Você pode aprender mais sobre isso em [ https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks ](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks).

A seção a seguir mostra como você pode recuperar os resultados do placar de líderes e usá-los.

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. Criar uma tarefa assíncrona para recuperar os resultados do placar de líderes

A primeira etapa é chamar o serviço de placares de líderes para recuperar os resultados para um placar de líderes específico.

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2. Configurar um retorno de chamada

Você pode configurar uma [tarefa de continuação](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations) a ser chamado depois que os resultados do placar de líderes são retornados.  Você fazer isso da seguinte maneira abaixo.

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

Essa tarefa de continuação é chamada no contexto do objeto que invocou originalmente e recebe o ```leaderboard_result``` que podem ser exibido de maneira que atenda às seu título.


### <a name="3-display-leaderboard"></a>3. Exibir o placar de líderes

Os dados de placar de líderes estão contidos no ```leaderboard_result``` e os campos são auto-explicativa.  Veja abaixo um exemplo.

```cpp
auto leaderboard = result.payload();

for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
{
    string_t colValues;
    for (auto columnValue : row.column_values())
    {
        colValues = colValues + L" ";
        colValues = colValues + columnValue;
    }
    m_console->Format(L"%18s %8d %14f %10s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
}

```

## <a name="2013-winrt-c-example"></a>WinRT 2013 C# exemplo

Ao usar o WinRT C# camada, você não precisará fazer um retorno de chamada separado de tarefas e será simplesmente a necessidade de usar o `await` palavra-chave ao chamar o serviço de placar de líderes.

### <a name="1-access-the-leaderboardservice"></a>1. Acesso a LeaderboardService

O `LeaderboardService` podem ser recuperados do `XboxLiveContext` criado ao entrar em um usuário no jogo, você precisará dele para chamar o placar de líderes dados.

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2. Chamar o LeaderboardService

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3. Recuperar dados de placar de líderes

`GetLeaderboardAsync()` Retorna um `LeaderboardResult` que conterá as estatísticas de preencher o placar de líderes nomeado.

`LeaderboardResult` tem várias funções e propriedades para facilitar a leitura dos dados de placar de líderes.

|Propriedade  |Descrição  |
|---------|---------|
|IAsyncOperation público<LeaderboardResult> GetNextAsync (uint maxItems);     |Recupera o próximo conjunto de fileiras até o número do parâmetro maxItems. Isso é, essencialmente, outra chamada para `GetLeaderboard()`         |
|public LeaderboardQuery GetNextQuery();     |Recupera o LeaderboardQuery que poderia ser usado para fazer a chamada de placar de líderes para recuperar o próximo conjunto de dados.         |
|Public bool HasNext {get;}    |Designa se houver mais linhas de placar de líderes para recuperar         |
|IReadOnlyList público<LeaderboardRow> linhas {get;}     | Linhas que contêm dados de placar de líderes por classificação        |
|IReadOnlyList público<LeaderboardColumn> colunas {get;}     | A lista de colunas que compõem o placar de líderes        |
|uint público TotalRowCount {get;}     | Quantidade total de linhas no placar de líderes        |
|cadeia de caracteres pública DisplayName {get;}     | Nome a ser exibido para o placar de líderes       |

Placar de líderes dados serão fornecidos uma página por vez. Você pode executar um loop por meio de `LeaderboardResult` linhas e colunas para recuperar os dados.  
Use o `HasNext` boolean e `GetNextAsync()` função para recuperar páginas posteriores de placar de líderes de dados.

```csharp
if (boardResult != null)
{
    foreach (LeaderboardRow row in boardResult.Rows)
    {
        Debug.Write(string.Format("Rank: {0} | Gamertag: {1} | {2}\n", row.Rank, row.Gamertag, row.Values.ToString()));
    }
}
```

## <a name="leaderboard-2017"></a>Leaderboard 2017

Para fazer chamadas para o serviço de placar de líderes de 2017 estatísticas que você usará o `StatisticManager` apis de placar de líderes em vez do `LeaderboardService` placar de líderes apis.  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_leaderboard (
     _In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ leaderboard::leaderboard_query query
     ) 
```  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_social_leaderboard (_In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ const string_t &socialGroup,
     _In_ leaderboard::leaderboard_query query
)
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetLeaderboard`  

```csharp
public void GetLeaderboard(
    XboxLiveUser user,
    string statName,
    LeaderboardQuery query
    )
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetSocialLeaderboard`  

```csharp
public void GetSocialLeaderboard(
    XboxLiveUser user,
    string statName,
    string socialGroup,
    LeaderboardQuery query
    )
```  

## <a name="2017-c-example"></a>Exemplo de C++ 2017

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1. Obter uma instância Singleton a stats_manager

Antes de chamar o `stats_manager` funções, você precisará definir uma variável para a instância do Singleton.

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2. Criar um LeaderboardQuery

O `leaderboard_query` ditará a quantidade, em ordem, e o ponto de dados inicial retornado da chamada placar de líderes.

Um `leaderboard_query` tem alguns atributos que podem ser definidos que afetarão os dados retornados:

|Propriedade |Descrição  |
|---------|---------|
|m_skipResultToRank     |Essa variável uint determinará o que os dados de placar de líderes de classificação será iniciado ao retornar. Classificações começam na posição 1.         |
|m_skipResultToMe     |Se definido como true, esse valor booliano fará com que os dados de placar de líderes retornados para iniciar à `XboxLiveUser` usado no `get_leaderboard()` chamar.  |
|m_order     |Enumerações do tipo `xbox::services::leaderboard::sort_order` têm dois valores possíveis, em ordem crescente e decrescente. Configurar esta variável para a sua consulta determinará a ordem de classificação de seu placar de líderes.        |
|m_maxItems     |Este uint determina o número máximo de linhas a serem retornadas por chamada para `get_leaderboard` ou `get_social_leaderboard()`.         |

`leaderboard_query` tem várias função de conjunto, você pode usar para atribuir um valor para essas propriedades. O código a seguir mostra como configurar a sua `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

Essa consulta retornará dez linhas de placar de líderes começando a 100th classificação individuais.

> [!WARNING]
> Definir SkipResultToRank maior do que o número de jogadores contidos no placar de líderes fará com que os dados de placar de líderes por retornar com zero linhas.

### <a name="3-call-getleaderboard"></a>3. Chamar get_leaderboard

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> O `statName` usado em de `GetLeaderboard()` chamada deve ser igual ao nome de um stat configurado para seu título na [Partner Center](https://partner.microsoft.com/dashboard), que diferencia maiusculas de minúsculas.

### <a name="4-read-the-leaderboard-data"></a>4. Ler os dados de placar de líderes

Para ler os dados de placar de líderes, você precisará chamar o `stats_manager::do_work()` função que retornará uma lista de `stat_event` valores. Placar de líderes dados estará contidos em um `stat_event` do tipo `stat_event_type::get_leaderboard_complete`. Quando você se deparar com um evento desse tipo na lista de `stat_event`s, você pode examinar as `leaderboard_result` contido no `stat_event` para acessar os dados.

Exemplo `do_work()` manipulador

```cpp
void Sample::UpdateStatsManager()
{
    // Process events from the stats manager
    // This should be called each frame update

    auto statsEvents = m_statsManager->do_work();
    std::wstring text;

    for (const auto& evt : statsEvents)
    {
        switch (evt.event_type())
        {
            case stat_event_type::local_user_added: 
                text = L"local_user_added"; 
                break;

            case stat_event_type::local_user_removed: 
                text = L"local_user_removed"; 
                break;

            case stat_event_type::stat_update_complete: 
                text = L"stat_update_complete"; 
                break;

            case stat_event_type::get_leaderboard_complete: //leaderboard data is read here
                text = L"get_leaderboard_complete";
                auto getLeaderboardCompleteArgs = std::dynamic_pointer_cast<leaderboard_result_event_args>(evt.event_args());
                ProcessLeaderboards(evt.local_user(), getLeaderboardCompleteArgs->result());
                break;
        }

        stringstream_t source;
        source << _T("StatsManager event: ");
        source << text;
        source << _T(".");
        m_console->WriteLine(source.str().c_str());
    }
}
```

Lendo os dados de placar de líderes do resultado de placar de líderes  

```cpp
void Sample::PrintLeaderboard(const xbox::services::leaderboard::leaderboard_result& leaderboard)
{
    if (leaderboard.rows().size() > 0)
    {
        m_console->Format(L"%16s %6s %12s %12s\n", L"Gamertag", L"Rank", L"Percentile", L"Values");
    }

    for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
    {
        string_t colValues;
        for (auto columnValue : row.column_values())
        {
            colValues = colValues + L" ";
            colValues = colValues + columnValue;
        }
        m_console->Format(L"%16s %6d %12f %12s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
    }
}
```  

Recupere mais páginas de dados do placar de líderes.  

```cpp
void Sample::ProcessLeaderboards(
    _In_ std::shared_ptr<xbox::services::system::xbox_live_user> user,
    _In_ xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result
    )
{
    if (!result.err())
    {
        auto leaderboardResult = result.payload();
        PrintLeaderboard(leaderboardResult);

        // Keep processing if there is more data in the leaderboard
        if (leaderboardResult.has_next())
        {
            if (!leaderboardResult.get_next_query().err())
            {               
                auto lbQuery = leaderboardResult.get_next_query().payload();
                if (lbQuery.social_group().empty())
                {
                    m_statsManager->get_leaderboard(user, lbQuery.stat_name(), lbQuery);
                }
                else
                {
                    m_statsManager->get_social_leaderboard(user, lbQuery.stat_name(), lbQuery.social_group(), lbQuery);
                }
            }
        }
    }
}
```  

## <a name="2017-winrt-c-example"></a>2017 WinRT C# exemplo

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1. Obter uma instância singleton a StatisticManager

Antes de chamar o `StatisticManager` funções, você precisará definir uma variável para a instância do Singleton.

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2. Criar um LeaderboardQuery

O `LeaderboardQuery` ditará a quantidade, em ordem, e o ponto de dados inicial retornado da chamada placar de líderes.  

```csharp
public sealed class LeaderboardQuery : __ILeaderboardQueryPublicNonVirtuals
    {
        [Overload("CreateInstance1")]
        public LeaderboardQuery();

        public bool HasNext { get; }
        public string SocialGroup { get; }
        public string StatName { get; }
        public SortOrder Order { get; set; }
        public uint MaxItems { get; set; }
        public uint SkipResultToRank { get; set; }
        public bool SkipResultToMe { get; set; }
    }
```

Um `LeaderboardQuery` tem alguns atributos que podem ser definidos que afetarão os dados retornados:

|Propriedade |Descrição  |
|---------|---------|
|SkipResultToRank     |Essa variável uint determinará o que os dados de placar de líderes de classificação será iniciado ao retornar. Classificações começam na posição 1.         |
|SkipResultToMe     |Se definido como true, esse valor booliano fará com que os dados de placar de líderes retornados para iniciar à `XboxLiveUser` usado no `GetLeaderboard()` chamar.  |
|Ordem     |Enumerações do tipo `Microsoft.Xbox.Services.Leaderboard.SortOrder` têm dois valores possíveis, em ordem crescente e decrescente. Configurar esta variável para a sua consulta determinará a ordem de classificação de seu placar de líderes.        |
|MaxItems     |Este uint determina o número máximo de linhas a serem retornadas por chamada para `GetLeaderboard()` ou `GetSocialLeaderboard()`.         |

Que formam seu `LeaderboardQuery` pode se parecer com o seguinte:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

Essa consulta retornará cinco linhas de placar de líderes começando a 100th classificação individuais.

> [!WARNING]
> Definir SkipResultToRank maior do que o número de jogadores contidos no placar de líderes fará com que os dados de placar de líderes por retornar com zero linhas.

### <a name="3-call-getleaderboard"></a>3. Call GetLeaderboard()

Agora você pode chamar `GetLeaderboard()` com seus `XboxLiveUser`, o nome do seu estatística e um `LeaderboardQuery`.

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> O `statName` usado em de `GetLeaderboard()` chamada deve ser igual ao nome de um stat configurado para seu título na [Partner Center](https://partner.microsoft.com/dashboard), que diferencia maiusculas de minúsculas.

### <a name="4-read-leaderboard-data"></a>4. Placar de líderes de leitura de dados

Para ler os dados de placar de líderes, você precisará chamar o `StatisticManager.DoWork()` função que retornará uma lista de `StatisticEvent` valores. Placar de líderes dados estará contidos em um `StatisticEvent` do tipo `GetLeaderboardComplete`. Quando você se deparar com um evento desse tipo na lista de `StatisticEvent`s, você pode examinar as `LeaderboardResult` contido no `StatisticEvent` para acessar os dados.

```csharp
IReadOnlyList<StatisticEvent> statEvents = statManager.DoWork(); //In practice this should be called every update frame

foreach(StatisticEvent statEvent in statEvents)
{
    if(statEvent.EventType == StatisticEventType.GetLeaderboardComplete
        && statEvent.ErrorCode == 0)
    {
        LeaderboardResultEventArgs leaderArgs = (LeaderboardResultEventArgs)statEvent.EventArgs;
        LeaderboardResult leaderboardResult = leaderArgs.Result;
        foreach(LeaderboardRow leaderRow in leaderboardResult.Rows)
        {
            Debug.WriteLine(string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", leaderRow.Rank, leaderRow.Gamertag, "test", leaderRow.Values[0]));
        }
    }
}
```

Em seu código de título `StatisticManager.DoWork()` deve ser usado para lidar com todos os eventos de Gerenciador de estatística de entrada e não apenas para o placar de líderes. 

> [!NOTE]
> Para recuperar o `LeaderboardResultEventArgs` você precisará converter o `StatisticEvent.EventArgs` como um `LeaderboardResultEventArgs` variável.

### <a name="5-retrieve-more-leaderboard-data"></a>5. Recuperar mais dados de placar de líderes

Para recuperar páginas posteriores dos dados de placar de líderes, você precisará usar o `LeaderboardResult.HasNext` propriedade e o `LeaderboardResult.GetNextQuery()` função para recuperar o `LeaderboardQuery` que trará a próxima página de dados.

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```