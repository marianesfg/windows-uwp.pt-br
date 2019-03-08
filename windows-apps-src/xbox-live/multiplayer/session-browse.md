---
title: Navegação de sessão para múltiplos jogadores
description: Saiba como implementar a navegação de sessão para múltiplos jogadores usando o Xbox Live para múltiplos jogadores.
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.date: 10/16/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 579c71ef9266fb9a1ee4ef0538d1beffec0bb4ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660671"
---
# <a name="multiplayer-session-browse"></a>Navegação de sessão para múltiplos jogadores

Sessão para múltiplos jogadores procurar é um novo recurso introduzido em novembro de 2016 que permite que um título para consultar uma lista de abrir sessões de jogos com vários participantes que atendem aos critérios especificados.

## <a name="what-is-session-browse"></a>O que é navegação de sessão?

Em um cenário de navegação de sessão, um player em um jogo é capaz de recuperar uma lista de sessões permite junções de jogos. Cada entrada de sessão nessa lista contém alguns metadados adicionais sobre o jogo, o que um jogador pode usar para ajudá-los a selecionar qual sessão para ingressar.  Eles também podem filtrar a lista de sessões com base nos metadados. Depois que o player visualiza uma sessão de jogo que atrai a eles, eles poderão ingressar na sessão.

Um player também pode criar uma nova sessão de jogo e usar a navegação de sessão para recrutar players adicionais em vez de depender de emparelhamento.

Navegação de sessão é diferente de cenários de cruzamento tradicionais em que o player seleciona automaticamente qual sessão jogo que deseja unir, enquanto no relacionamento de pessoas, o player normalmente clica em um botão "encontrar um jogo" que tenta colocar automaticamente o player em um sessão de jogo apropriado. Enquanto a navegação de sessão é um processo manual e mais lento que não pode selecionar sempre o jogo objetivamente melhor, ele fornece mais controle para o player e poderá ser visto como o jogo subjetiva melhor escolha.

É comum incluir ambos os cenários de procurar e cruzamento de sessão em jogos. Normalmente emparelhamento é usado para reproduzida comumente modos de jogo, enquanto a navegação de sessão é usada para jogos personalizados.

**Exemplo:** João pode estar interessado em jogo uma batalha hero arena estilo com vários participantes, mas quer jogar um jogo onde todos os jogadores selecionam seu hero aleatoriamente. Ele pode recuperar uma lista de sessões abertas de jogos e localizem aquelas que incluem "heroes aleatórios" em sua descrição, ou se a interface do usuário do jogo permite que ele, ele pode selecionar o modo de jogo "aleatórios" hero"e recuperar somente as sessões que estão marcadas para indicar que eles são"RandomHero"gam es.

Quando ele encontra um jogo que ele gosta, ele participa do jogo. Quando basta as pessoas se associar a sessão, o host da sessão de jogo pode iniciar o jogo.

### <a name="roles"></a>Funções

Um jogo em navegação de sessão talvez queira recrutar players para funções específicas. Por exemplo, um player talvez queira criar uma sessão de jogo que especifica que a sessão contém classes de violência não mais de 5, mas deve conter pelo menos 2 funções de healer e função do tanque de pelo menos 1.

Quando outro jogador aplica-se para a sessão, poderá pré-selecionar sua função e o serviço não permitirá-los ingressar na sessão, se não houver nenhum slot aberto para a função que foi selecionado.

Outro exemplo seria se um player deseja reservar 2 slots para seus amigos ingressar, o jogo pode especificar uma função de "friends", e apenas os jogadores que são amigos com o host de sessão podem preencher os 2 slots dedicados à função de "friends".

Para obter mais informações sobre as funções, consulte [funções para múltiplos jogadores](multiplayer-roles.md).



## <a name="how-does-session-browse-work"></a>Como funciona a navegação de sessão?

Navegação de sessão funciona principalmente sobre o uso de identificadores de pesquisa. Um identificador de pesquisa é um pacote de dados que contém uma referência para a sessão, bem como metadados adicionais sobre a sessão, ou seja, os atributos de pesquisa.

Quando cria um título de procurar uma nova sessão de jogo que é qualificada para a sessão, ele cria um identificador de pesquisa para a sessão. O identificador de pesquisa é armazenado no Multiplayer serviço de diretório (MPSD), que mantém os identificadores de pesquisa para o título.

Quando um título precisa recuperar uma lista de sessões, o título pode enviar uma consulta de pesquisa para MPSD, que retornará uma lista de identificadores de pesquisa que atendem aos critérios de pesquisa. O título, em seguida, pode usar a lista de sessões para exibir uma lista de jogos permite junções para o player.

Quando uma sessão está cheio ou, caso contrário, não pode ser unida, um título pode remover o identificador de pesquisa de MPSD para que a sessão não aparecerão em consultas de procura de sessão.

>[!NOTE]
> Identificadores de pesquisa destinam-se a ser usado ao exibir uma lista de sessões para ser apresentado ao usuário. Usando identificadores de pesquisa para o emparelhamento de plano de fundo não é válido e, em vez disso, considere o uso de [SmartMatch](multiplayer-manager/play-multiplayer-with-matchmaking.md)

## <a name="set-up-a-session-for-session-browse"></a>Configurar uma sessão de navegação de sessão

Para usar os identificadores de pesquisa para uma sessão, a sessão deve ter o seguinte conjunto de recursos como true:

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> O `userAuthorizationStyle` funcionalidade só é necessária para jogos da UWP, mas é recomendável implementá-las para o Xbox Live todos os jogos, incluindo jogos XDK, pois garante portabilidade futuras.

>[!NOTE]
> Definindo o `userAuthorizationStyle` padrões de recurso do `readRestriction` e `joinRestriction` da sessão para `local` em vez de `none`. Isso significa que os títulos devem usar identificadores de pesquisa ou transferir as alças para ingressar em uma sessão de jogo.

Você pode definir esses recursos no modelo de sessão quando você configura os serviços do Xbox Live.

Para navegação de sessão, você deve criar somente identificadores de pesquisa em sessões que serão usadas para o jogo real, não para sessões de corredor.

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>O que significa ser um proprietário de uma sessão?

Enquanto a sessão de jogo de muitos tipos, como SmartMatch ou um amigos apenas jogos, não exigem um proprietário, para sessões de navegação de sessão você poderá ter um proprietário. 

Ter uma sessão gerenciada pelo proprietário tem alguns benefícios para o proprietário. Proprietários podem remover outros membros da sessão ou alterar o status de propriedade de outros membros.

Para usar os proprietários de uma sessão, a sessão deve ter o seguinte conjunto de recursos como true:

* `hasOwners`

Se um proprietário de uma sessão tem um membro do Xbox Live bloqueado, esse membro não pode ingressar na sessão.

Ao usar [funções com vários participantes](multiplayer-roles.md), você pode defini-lo isso somente os proprietários podem atribuir funções a usuários.

Se todos os proprietários de deixar uma sessão, o serviço usa a ação na sessão com base a `ownershipPolicy.migration` política que está definida para a sessão. Se a política for "antiga", o player que tenha sido na sessão mais longa é definido para o novo proprietário. Se a política for "endsession" (o padrão se não for fornecido), o serviço encerra a sessão e remove todos os jogadores restantes da sessão.


## <a name="search-handles"></a>Identificadores de pesquisa

Um identificador de pesquisa é armazenado no MSPD como uma estrutura JSON. Além de conter uma referência para a sessão, os identificadores de pesquisa também contêm metadados adicionais para pesquisas, conhecido como atributos de pesquisa.

Uma sessão pode ter apenas um identificador de pesquisa criado para ele a qualquer momento.

Para criar um identificador de pesquisa para uma sessão no usando as APIs do Xbox Live, você primeiro crie uma `multiplayer::multiplayer_search_handle_request` do objeto e, em seguida, passa esse objeto para o `multiplayer::multiplayer_service::set_search_handle()` método.

### <a name="search-attributes"></a>Atributos de pesquisa

Atributos de pesquisa consiste dos seguintes componentes:

`tags` -As marcas são descritores de cadeia de caracteres que as pessoas podem usar para categorizar uma sessão de jogo, semelhante a uma hashtag. Marcas devem começar com uma letra, não podem conter espaços e devem ser menor que 100 caracteres.
Marcas de exemplo: "ProRankOnly", "norocketlaunchers", "cityMaps".

`strings` -Cadeias de caracteres são variáveis de texto, e os nomes de cadeia de caracteres devem começar com uma letra, não podem conter espaços e devem ser menor que 100 caracteres.

Metadados de cadeia de caracteres de exemplo: "Armas facas de"="+ pistols + rifles", "MapName" = "UrbanCityAssault", "Descrição"="divertidos jogo casual, novas pessoas boas-vindas".

`numbers` -Números são variáveis numéricas e os nomes de número devem começar com uma letra, não podem conter espaços e devem ser menor que 100 caracteres. As APIs do Xbox Live recuperar valores de número como o tipo float.

Metadados de número de exemplo: "MinLevel" = 25, "MaxRank" = 10.

>**Observação:** A capitalização de marcas e valores de cadeia de caracteres é preservada no serviço, mas você deve usar a função ToLower () quando você consulta para marcas. Isso significa que as marcas e valores de cadeia de caracteres são atualmente todos tratados como letras minúsculas, mesmo se eles contiverem caracteres em letras maiusculas.

Nas APIs do Live Xbox, você pode definir os atributos de pesquisa usando o `set_tags()`, `set_stringsmetadata()`, e `set_numbers_metadata()` métodos de um `multiplayer_search_handle_request` objeto.


### <a name="additional-details"></a>Detalhes adicionais

Quando você recupera um identificador de pesquisa, os resultados também incluem dados úteis adicionais sobre a sessão, tal como se a sessão for fechada, existem quaisquer restrições de associação na sessão, etc.

No Xbox Live APIs, esses detalhes, juntamente com os atributos de pesquisa, são incluídos no `multiplayer_search_handle_details` que são retornados após uma consulta de pesquisa.

### <a name="remove-a-search-handle"></a>Remover um identificador de pesquisa

Quando você deseja remover uma sessão de navegação de sessão, como quando a sessão está completa, ou se a sessão for fechada, você pode excluir o identificador de pesquisa.

As APIs Xbox Live, você pode usar o `multiplayer_service::clear_search_handle()` método para remover um identificador de pesquisa.

### <a name="example-create-a-search-handle-with-metadata"></a>Exemplo: Criar um identificador de pesquisa com metadados

O código a seguir mostra como criar um identificador de pesquisa para uma sessão por meio das APIs de vários jogadores C++ Xbox Live.

```cpp
auto searchHandleReq = multiplayer_search_handle_request(sessionBrowseRef);

searchHandleReq.set_tags(std::vector<string_t> val);
searchHandleReq.set_numbers_metadata(std::unordered_map<string_t, double> metadata);
searchHandleReq.set_strings_metadata(std::unordered_map<string_t, string_t> metadata);

auto result = xboxLiveContext->multiplayer_service().set_search_handle(searchHandleReq)
.then([](xbox_live_result<void> result)
{
  if (result.err())
  {
    // handle error
  }
});
```


## <a name="create-a-search-query-for-sessions"></a>Criar uma consulta de pesquisa para sessões

Ao recuperar uma lista de identificadores de pesquisa, você pode usar uma consulta de pesquisa para restringir os resultados para as sessões que atendem a critérios específicos.

A sintaxe de consulta de pesquisa é um [OData](https://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) estilo de sintaxe, com apenas os seguintes operadores com suporte:

 Operador | Descrição
 --- | ---
 eq | é igual a
 ne | Não é igual a
 gt | Maior que
 ge | Maior que ou igual
 lt | Menor que
 le | Menor ou igual
 e | AND lógico
 ou | lógico ou (consulte a observação abaixo)

Você também pode usar expressões lambda e o `tolower` canônica. Não há outras funções do OData têm suporte no momento.

Ao procurar por valores de cadeia de caracteres ou marcas, você deve usar a função 'tolower' na consulta de pesquisa, pois o serviço dá suporte apenas no momento ao pesquisar cadeias de caracteres em letras minúsculas.

O serviço Xbox Live retorna apenas os 100 primeiros resultados que correspondem à consulta de pesquisa. Seu jogo deve permitir que os jogadores refinar sua consulta de pesquisa, se os resultados forem muito amplas.

>[!NOTE]
>  ORs lógicos têm suporte em consultas de cadeia de caracteres de filtro; No entanto, somente um OR é permitida e deve ser na raiz da sua consulta. Você não pode ter vários ORs em sua consulta, nem você pode criar uma consulta que resultaria em ou não sendo na parte superior a maioria dos nível da estrutura de consulta.

### <a name="search-handle-query-examples"></a>Exemplos de consulta do identificador de pesquisa

Em uma chamada restful, "Filtro" é onde você especifica uma cadeia de caracteres de idioma de filtro OData que será executada em sua consulta em relação a todos os identificadores de pesquisa.  
Nas APIs de 2015 para múltiplos jogadores, você pode especificar a cadeia de caracteres de filtro de pesquisa na *searchFilter* parâmetro do `multiplayer_service.get_search_handles()` método.  

Atualmente, há suporte para os seguintes cenários de filtro:

 Filtrar por | Cadeia de caracteres de filtro de pesquisa
 --- | ---
 Um único membro xuid '1234566' | "memberXuids/sessão/any(d:d eq '1234566')"
 Um único proprietário xuid '1234566' | "session/ownerXuids/any(d:d eq '1234566')"
 Uma cadeia de caracteres 'forzacarclass' igual a 'classb' | "tolower(strings/forzacarclass) eq 'classb'"
 Um número 'forzaskill' igual a 6 | "números/forzaskill eq 6"
 Um número 'halokdratio' maior que 1,5 | "números/halokdratio gt 1.5"
 Uma marca 'coolpeopleonly' | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 Sessões que não contêm a marca 'cursingallowed' | "tags/any(d:tolower(d) ne 'cursingallowed')"
 As sessões que não contêm um número 'classificação' que são igual a 0 | "números/classificação ne 0"
 As sessões que não contêm uma cadeia de caracteres 'forzacarclass' que é igual a 'classa' | "tolower(strings/forzacarclass) ne 'classa'"
 Uma marca coolpeopleonly e um número 'halokdratio' igual a 7.5 | "tags/any(d:tolower(d) eq 'coolpeopleonly') eq true and numbers/halokdratio eq 7.5"
 Um número 'halodkratio' maior que ou igual à 1.5, um número 'classificação' menor que 60 e um número 'customnumbervalue' menor ou igual a 5 | "números/halokdratio ge 1.5 e números/classificação lt 60 e números/customnumbervalue le 5"
 Uma id de medalha '123456' | "achievementIds/any(d:d eq '123456')"
 O código de idioma 'en' | "idioma eq 'en'"
 Horário agendado, retorna agendados vezes menor ou igual ao tempo especificado | "sessão/scheduledTime le ' 2009-06-15T13:45:30.0900000Z'"
 Hora de lançamento, lançada retorna todas as vezes menor do que o tempo especificado | "session/postedTime lt '2009-06-15T13:45:30.0900000Z'"
 Estado de sessão do registro | "sessão/registrationState eq 'registrado'"
 Em que o número de membros da sessão é igual a 5 | "sessão/membersCount eq 5"
 Em que a contagem de destino do membro de sessão é maior que 1 | "sessão/targetMembersCount gt 1"
 Em que a contagem máxima de membros da sessão for menor que 3 | "sessão/maxMembersCount lt 3"
 Em que a diferença entre a contagem de destino do membro de sessão e o número de membros da sessão é menor que ou igual a 5 | "le de sessão/targetMembersCountRemaining 5"
 Em que a diferença entre a contagem máxima de membros da sessão e o número de membros da sessão é maior que 2 | "sessão/maxMembersCountRemaining gt 2"
 Em que a diferença entre a contagem de destino do membro de sessão e o número de membros da sessão é menor ou igual a 15.</br> Se a função não tem um destino especificado, essa consulta filtra em relação a diferença entre a contagem máxima de membros da sessão e o número de membros da sessão. | "sessão/precisa le 15"
 Função "confirmada" do tipo de função "lfg" em que o número de membros com essa função é igual a 5 | "/ funções/lfg/confirmar/contagem de sessões eq 5"
 Função "confirmada" do tipo de função "lfg" em que o destino dessa função é maior que 1.</br> Se a função não tem um destino especificado, o máximo da função é usado em vez disso. | "gt/funções/lfg/confirmar/destino da sessão de 1"
 Função "confirmado" da função do tipo "lfg" em que a diferença entre o destino da função e o número de membros com essa função é menor ou igual a 15.</br> Se a função não tem um destino especificado, essa consulta filtra em relação a diferença entre o máximo da função e o número de membros com essa função. | "sessão/funções/lfg/confirmar/necessidades le 15"
 Todos os identificadores de pesquisa que apontam para uma sessão que contém uma palavra-chave específica | "session/keywords/any(d:tolower(d) eq 'level2')"
 Todos os identificadores de pesquisa que apontam para uma sessão que pertence a um determinado scid | "session/scid eq '151512315'"
 Todos os identificadores de pesquisa que apontam para uma sessão que usa um nome de modelo específico | "sessão/templateName eq 'mytemplate1'"
 Todos os identificadores que têm a marca 'elite' ou tem um número 'armas' maiores que 15 e 'clan' igual a 'roxo' da cadeia de caracteres de pesquisa | "tags/any(a:tolower(a) eq 'elite') ou número/armas gt 15 e a cadeia de caracteres/clan eq 'roxo'"

### <a name="refreshing-search-results"></a>Atualizando resultados da pesquisa

 Seu jogo deve evitar a atualização automática de uma lista de sessões, mas em vez disso, fornecem a interface do usuário que permite que um jogador atualizar manualmente a lista (possivelmente depois de refinar os critérios de pesquisa para filtrar melhor os resultados).

 Se um jogador tenta ingressar em uma sessão, mas que a sessão está cheio ou fechado, o seu jogo deve atualizar os resultados da pesquisa.

 Número excessivo de atualizações de pesquisa podem levar a limitação do serviço, portanto, o título deve limitar a taxa na qual a consulta pode ser atualizada.

 Para reduzir o volume de chamadas de serviço, os identificadores de pesquisa incluem propriedades de sessão personalizado que podem ser usadas para armazenar e consultar os atributos de sessão rápida mudança. Esses atributos não devem ser armazenados em atributos de pesquisa.

### <a name="example-query-for-search-handles"></a>Exemplo: consulta para identificadores de pesquisa

 O código a seguir mostra como consultar identificadores de pesquisa. A API retorna uma coleção de `multiplayer_search_handle_details` objetos que representam todos os identificadores de pesquisa que correspondem à consulta.

```cpp
 auto result = multiplayer_service().get_search_handles(scid, template, orderBy, orderAscending, searchFilter)
 .then([](xbox_live_result<std::vector<multiplayer_search_handle_details>> result)
 {
   if (result.err())
   {
      // handle error
   }
   else
   {
      // parse result.payload
   }
 });

 /* Payload element properties

 multiplayer_search_handle_details
 {
   string_t& handle_id();
   multiplayer_session_reference& session_reference();
   std::vector<string_t>& session_owner_xbox_user_ids();
   std::vector<string_t>& tags();
   std::unordered_map<string_t, double>& numbers_metadata();
   std::unordered_map<string_t, string_t>& strings_metadata();
   std::unordered_map<string_t, multiplayer_role_type>& role_types();
 }
 */
```

## <a name="join-a-session-by-using-a-search-handle"></a>Ingressar em uma sessão usando um identificador de pesquisa

Depois de recuperar um identificador de pesquisa para uma sessão que você deseja ingressar, o título deve usar `MultiplayerService::WriteSessionByHandleAsync()` ou `multiplayer_service::write_session_by_handle()` para serem adicionados à sessão.

> [!NOTE]
> O `WriteSessionAsync()` e `write_session()` métodos não podem ser usados para ingressar em uma sessão de navegação de sessão.

O código a seguir demonstra como ingressar em uma sessão depois de recuperar um identificador de pesquisa.

```cpp
void Sample::BrowseSearchHandles()
{
    auto context = m_liveResources->GetLiveContext();
    context->multiplayer_service().get_search_handles(...)
    .then([this](xbox_live_result<std::vector<multiplayer_search_handle_details>> searchHandles)
    {
        if (searchHandles.err())
        {
            LogErrorFormat( L"BrowseSearchHandles failed: %S\n", searchHandles.err_message().c_str() );
        }
        else
        {
            m_searchHandles = searchHandles.payload();

            // Join the game session

            auto handleId = m_searchHandles.at(0).handle_id();
            auto sessionRef = multiplayer_session_reference(m_searchHandles.at(0).session_reference());
            auto gameSession = std::make_shared<multiplayer_session>(m_liveResources->GetLiveContext()->xbox_live_user_id(), sessionRef);
            gameSession->join(web::json::value::null(), false, false, false);

            context->multiplayer_service().write_session_by_handle(gameSession, multiplayer_session_write_mode::update_existing, handleId)
            .then([this, sessionRef](xbox_live_result<std::shared_ptr<multiplayer_session>> writeResult)
            {
                if (!writeResult.err())
                {
                    // Join the game session via MPM
                    m_multiplayerManager->join_game(sessionRef.session_name(), sessionRef.session_template_name());
                }
            });
        }
    });
}
```
