---
title: Introdução ao Gerenciador do Social
description: Saiba mais sobre a API de Manager Social do Xbox Live para acompanhar online com seus amigos.
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.date: 03/26/2018
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dff3dcfd79fe43ff8af1513a4358bd0ff98b8d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609001"
---
# <a name="introduction-to-social-manager"></a>Introdução ao Gerenciador do Social

## <a name="description"></a>Descrição

Xbox Live fornece um gráfico de redes sociais avançado que títulos podem usar para vários cenários.

Usando as APIs de redes sociais no Xbox Live API (XSAPI) para obter e manter informações sobre um grafo social é complexo e manter essas informações atualizadas pode ser complicado.  Não fazer isso corretamente, isso pode resultar em problemas de desempenho, dados obsoletos ou sendo limitado devido ao chamar os serviços do Xbox Live social com mais frequência do que o necessário.

O Gerenciador de Social resolve esse problema por:

* Criando uma API simple para chamar
* Criando informações atualizadas usando o serviço de atividade em tempo real em segundo plano
* Os desenvolvedores podem chamar a API do Gerenciador de Social forma síncrona sem qualquer sobrecarga extra sobre o serviço

O Gerenciador de Social mascara a complexidade de lidar com várias assinaturas RTA e atualização de dados para usuários e permite que os desenvolvedores obter facilmente o gráfico atualizado que desejam criar cenários interessantes.

Para examinar a memória de Gerenciador Social e o desempenho características dar uma olhada [Social Manager-memória e visão geral do desempenho](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>Recursos

O Gerenciador de Social fornece os seguintes recursos:

* API simplificada de redes sociais
* Grafo social atualizado
* O controle sobre o nível de detalhes de informações exibidas
* Reduzir o número de chamadas para serviços do Xbox Live
  * Isso diretamente correlacionado à redução de latência geral na aquisição de dados
* Thread-safe
* Manter com eficiência dados atualizados

## <a name="core-concepts"></a>Conceitos básicos

**Grafo social**: Um *grafo social* é criado para um usuário local no dispositivo. Isso cria uma estrutura que mantém informações sobre todos os amigos um usuários atualizado.

> [!NOTE]
> No Windows pode haver apenas um usuário local

**Usuário do Social Xbox**: Uma *usuário social do Xbox* é um conjunto completo de dados sociais associados a um usuário de um grupo

**Grupo de usuários do Xbox Social**: Um grupo é uma coleção de usuários que é usada para coisas como preencher a interface do usuário. Há dois tipos de grupos

* **Filtrar grupos**: Um grupo de filtro usa um local (chamado) do usuário *grafo social* e retorna um conjunto atualizado de forma consistente de usuários com base nos parâmetros de filtro especificado
* **Grupos de usuários**: Um grupo de usuários usa uma lista de usuários e retorna uma exibição atualizada consistentemente desses usuários. Esses usuários podem estar fora da lista de amigos do usuário.

Para manter uma *grupo de usuários social* até a função date `social_manager::do_work()` deve ser chamado a cada quadro.

## <a name="api-overview"></a>Visão geral da API

Você usará com mais frequência as principais classes a seguir:

### <a name="social-manager"></a>Gerenciador de social

* Nome da classe C++ API: social_manager
* WinRT (C#) nome de classe de API: [SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

Essa é uma classe singleton que pode ser usada para obter **grupos de usuário social Xbox** que são os modos de exibição descritos acima.

O Gerenciador de Social manterá xbox grupos de usuários social atualizado e pode filtrar grupos de usuários pela presença ou a relação para o usuário.  Por exemplo, um grupo de usuário social do xbox que contém todos os amigos do usuário que estão online e reproduzir o título atual pode ser criado.  Isso seria mantido atualizado como amigos iniciar ou parassem a execução do título.

### <a name="xbox-social-user-group"></a>Grupo do usuário social Xbox

* Nome da classe C++ API: xbox_social_user_group
* WinRT (C#) nome de classe de API: [XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

Um grupo de usuários que atendem a certos critérios, conforme descrito acima. Grupos de usuário social Xbox expõem o tipo de um grupo são, quais usuários estão sendo rastreados ou o filtro definido é neles, e o usuário local que o grupo pertence.

Você pode encontrar uma descrição completa das APIs no Gerenciador de Social a [referência de API do Xbox Live](https://aka.ms/xboxliveuwpdocs).
Você também pode encontrar as APIs do WinRT no [Microsoft.Xbox.Services.Social.Manager.Namespace documentação](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01)

## <a name="usage"></a>Uso

### <a name="creating-a-social-user-group-from-filters"></a>Criando um grupo de usuários social de filtros

Nesse cenário, você deseja uma lista de usuários de um filtro, como todas as pessoas que esse usuário está seguindo ou marcado como favorito.

#### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando a API do C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando o C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}

```

#### <a name="events-returned"></a>Eventos retornados

`local_user_added`(C++) | `LocalUserAdded`(C#)-Dispara quando o carregamento do grafo social de usuários é concluído. Indica se os erros ocorreram durante a inicialização

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#)-Dispara quando a criação do grupo do usuário social

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#)-Dispara quando os usuários são carregados no

#### <a name="additional-details"></a>Detalhes adicionais

O exemplo acima mostra como inicializar o Gerenciador de Social para um usuário, crie um grupo de usuário social para que o usuário e mantê-lo atualizado.

As opções de filtragem podem ser vistas na `presence_filter` e `relationship_filter` enums

No loop de jogo, o `do_work` função atualiza os modos de exibição criados com o instantâneo mais recente dos usuários no grupo.

Os usuários no modo de exibição podem ser obtidos chamando o `xbox_social_user_group::get_users()` que retorna uma lista de `xbox_social_user` objetos.  O `xbox_social_user` contém as informações de redes sociais, como o nome de jogador, uri gamerpic, etc.

### <a name="create-and-update-a-social-user-group-from-list"></a>Criar e atualizar um grupo de usuários social de lista

Nesse cenário, você deseja que as informações de redes sociais de uma lista de usuários como usuários em uma sessão para múltiplos jogadores.

#### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando a API do C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_list(
    xboxLiveContext->user(),
    userList  // this is a std::vector<string_t> (list of xuids)
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando o C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromList(
     xboxLiveContext.User,
     userList //this is a IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
    );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Eventos retornados

`local_user_added`(C++) | `LocalUserAdded`(C#)-Dispara quando o carregamento do grafo social de usuários é concluído. Indica se os erros ocorreram durante a inicialização

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#)-Dispara quando a criação do grupo do usuário social

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#)-Dispara quando os usuários são carregados no

### <a name="updating-social-user-group-from-list"></a>Atualizando o grupo de usuário do Social de lista

Você também pode alterar a lista de usuários controladas no grupo de usuários social chamando update_social_user_group()

#### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando a API do C++

```cpp
//#include "Social.h"

socialManager->update_social_user_group(
    xboxSocialUserGroup,
    newUserList    // std::vector<string_t> (list of xuids)
    );

    while(true)
    {
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
    }
```

#### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando o C# API

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.UpdateSocialUserGroup(
     xboxSocialUserGroup,
     newUserList //IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Eventos retornados

`social_user_group_updated`(C++) | `SocialUserGroupUpdated`(C#)-Dispara quando a atualização de grupo do usuário social está concluída.

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#)-Dispara quando os usuários são carregados no. Se os usuários adicionados por meio da lista já estão no gráfico, esse evento não será disparado

### <a name="using-social-manager-events"></a>Usando eventos do Gerenciador de Social

Gerenciador de social também lhe dirá o que aconteceu na forma de eventos.  Você pode usar esses eventos para atualizar a interface do usuário ou executar outra lógica.

#### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando a API do C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();
socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::no_extra_detail
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

socialManager->do_work();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    auto events = socialManager->do_work();
    for(auto evt : events)
    {
        auto affectedUsersFromGraph = socialUserGroup->get_users_from_xbox_user_ids(evt.users_affected());
        // TODO: render the changes to the friends list using game UI, passing in affectedUsersFromGraph
    }
}
```

##### <a name="source-example-using-the-c-api"></a>Exemplo de código-fonte usando o C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

socialManager.DoWork();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    IReadOnlyList<SocialEvent> Events = socialManager.DoWork();
    IReadOnlyList<XboxSocialUser> affectedUsersFromGraph;
    foreach(SocialEvent managerEvent in Events)
    {
        affectedUsersFromGraph = socialUserGroup.GetUsersFromXboxUserIds(managerEvent.UsersAffected);
    }
}

```

#### <a name="events-returned"></a>Eventos retornados

`local_user_added`(C++) | `LocalUserAdded`(C#)-Dispara quando o carregamento do grafo social de usuários é concluído. Indica se os erros ocorreram durante a inicialização

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#)-Dispara quando a criação do grupo do usuário social

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#)-Dispara quando os usuários são carregados no

#### <a name="additional-details"></a>Detalhes adicionais

Este exemplo mostra algumas das controle adicional oferecido.  Em vez de contar com os filtros de grupo do usuário social para fornecer uma lista atualizada de usuário durante o loop do jogo, o grafo social é inicializado fora do loop do jogo.  Em seguida, o título baseia-se na `events` retornado pelo `socialManager->do_work()` função.  `events` é uma lista dos `social_event`e cada `social_event` contém uma alteração para o grafo social que ocorreram durante o último quadro.  Por exemplo `profiles_changed`, `users_added`, etc.  Mais informações podem ser encontradas no `social_event` documentação da API.

### <a name="cleanup"></a>Cleanup

#### <a name="cleaning-up-social-user-groups"></a>Limpando grupos de usuários Social

```cpp
//#include "Social.h"

socialManger->destroy_social_user_group(
    socialUserGroup
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.DestroySocialUserGroup(
     socialUserGroup
     );
```

Limpa o grupo de usuários social que foi criado. Chamador também deve remover todas as referências que eles têm a qualquer grupo de usuário de redes sociais criado como agora ele contém dados obsoletos.

#### <a name="cleaning-up-local-users"></a>Limpeza de usuários locais

```cpp
//#include "Social.h"

socialManger->remove_local_user(
    xboxLiveContext->user()
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.RemoveLocalUser(
     xboxLiveContext.User
     );
```

Remover usuário local remove o grafo social de usuários carregado, bem como quaisquer grupos do usuário social que foram criados usando esse usuário.

#### <a name="events-returned"></a>Eventos retornados

`local_user_removed`(C++) | `LocalUserRemoved`(C#)-Dispara quando um usuário local foi removido com êxito