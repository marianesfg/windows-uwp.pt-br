---
title: Funções com vários participantes
description: Saiba mais sobre como usar funções para definir funções de player no Xbox Live com vários participantes.
ms.date: 06/29/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox one, para múltiplos jogadores, as funções
ms.localizationpriority: medium
ms.openlocfilehash: ac5e7758bd8e068681d1c8dab2d47d11374c2616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662081"
---
# <a name="roles"></a>Funções

Para algumas sessões de jogos, convém especificar que certos membros têm certas funções do jogo, como suporte, medic, violência, etc. Você também poderá wat para reservar slots de jogos para os jogadores que preencherão uma função específica do jogo. Ao usar o recurso de funções do Xbox Live, o serviço pode controlar quais os jogadores são atribuídos a quais funções de jogo e impor um número máximo de players que pode selecionar uma função específica do jogo.

O uso mais comum de funções é determinar o jogos funções específicas para a sessão de jogo. Por exemplo, você pode ter um modo de jogo que requer entre classes de suporte de 1 e 2, pelo menos 1 classe de tanque/pesado e classes de violência não mais do que 5.

Outro cenário possível, convém especificar que uma sessão de jogo pode ter exatamente 4 jogadores, até 8 spectators e 1 anunciante.

Você também pode usar funções para reservar os slots para amigos ao preencher os slots restantes por outros meios, como navegação de sessão.

## <a name="role-types"></a>Tipos de função

Um tipo de função representa um grupo de definições de função. Cada função deve ser definida como parte de um tipo de função. Tipos de função são definidos no documento de sessão para múltiplos jogadores.

Um membro só pode ser atribuído uma função de um tipo de função específica. Por exemplo, se um tipo de função de "class" inclui healers, tanques e danos, em seguida, um membro pode apenas ser atribuído a uma dessas funções.

Você pode definir vários tipos de função e um membro pode ser atribuído a uma função de cada tipo de função. No cenário anterior, um membro pode ter escolhido a função healer, mas também pode ser atribuído a um Esquadrão função de líder, se a função de líder de Esquadrão é definida em um tipo de função separada.

> **Importante:** O Xbox Live SDK atualmente suporta apenas um tipo de função única e uma única função por membro.

## <a name="role-type-properties"></a>Propriedades de tipo de função

Quando você define um tipo de função, você deve especificar as seguintes informações para tipos de função:

* O nome do tipo de função. O nome deve ser minúscula e alfanuméricos e não mais do que 100 caracteres.
* Se as funções definidas no tipo de função serão o proprietário gerenciado ou não.
* Se as propriedades das funções definidas no tipo de função podem ser alteradas durante a vida útil da sessão.
* As definições de função que estão incluídos no tipo de função.

Se um tipo de função é gerenciado de proprietário, que significa que somente os membros que são proprietários da sessão podem atribuir funções desse tipo para membros. Se o tipo de função não é proprietário gerenciado, os membros podem atribuir funções a mesmos.

Você só pode especificar que um tipo de função é gerenciado em sessões que têm a capacidade de "hasOwners" conjunto de proprietário.

> O Xbox Live SDK não oferece suporte atualmente proprietários atribuindo funções a outros membros.

## <a name="role-properties"></a>Propriedades de função

Quando você define uma função, você deve especificar as seguintes informações para cada função:

* O nome da função. O nome deve ser minúscula e alfanuméricos e não mais do que 100 caracteres.
* O número máximo de membros que têm permissão para preencher a função. Deve ser maior que zero.
* O número de membros que deve preencher a função de destino. O destino deve ser maior que zero e menor ou igual ao número máximo de membros permitido para preencher a função.

Quando um membro de sessão é atribuído uma função, essa informação é registrada nas funções de membro no documento de sessão para múltiplos jogadores.

O serviço impõe o número máximo de membros que podem ser atribuídos a uma função, mas não impõe o número de destino.

## <a name="create-roles"></a>Criar funções

Funções e tipos de função geralmente são definidos na [modelo de sessão](service-configuration/session-templates.md). O serviço dá suporte a função e definição de tipo de função durante a criação de sessão, no entanto, o Xbox Live SDK não faz isso.

### <a name="define-role-types-and-roles-in-a-session-template"></a>Definir funções e tipos de função em um modelo de sessão

Quando você cria um modelo de sessão durante a configuração do Xbox Live, você pode definir funções e tipos de função.

As informações de função e tipo de função são especificadas como um elemento de "roleTypes" de nível básico no modelo de sessão, no seguinte formato:

```json
"roleTypes": {
  "myroletype1": { // must be lowercase alpha-numeric.
    "ownerManaged": true, // Can only be true on sessions with the "hasOwners" capability set. If true, only the owner of the session can assign this role to members.
    "mutableRoleSettings": ["max", "target"], // Which role settings for roles in this role type can be modified throughout the life of the session. Exclude role settings to lock them.
    "roles": {
      "role1": { // must be lowercase alpha-numeric.
        "max": 3, // Max number of members assigned to this role at a time, enforced by MPSD.
        "target": 2 // Target number of members to assign this role to. Like max, but not enforced (can be exceeded).
      },
      "role2": {
        ...
      }
  },
  "myroletype2": {
    ...
  }
},
```

## <a name="retrieve-role-information-for-a-multiplayer-session"></a>Recuperar informações de função para uma sessão para múltiplos jogadores

Você pode obter as informações sobre a função de tipos, funções e quantos membros são atribuídos a cada função de uma sessão para múltiplos jogadores, ou de um identificador de pesquisa com vários participantes.

No Xbox Live SDK, informações sobre os tipos de função e funções são armazenadas dentro de uma estrutura de mapa. O uso de APIs C++ do `unordered_map` classe e as APIs do WinRT usam o `IMapView` classe.

### <a name="get-the-role-information-from-a-search-handle"></a>Obter as informações de função de um identificador de pesquisa

No `multiplayer_search_handle_details` objeto retornado de uma solicitação de pesquisa, você pode obter as informações de tipo de função indexando o `role_types` mapa com o nome do tipo de função que você está interessado.

Isso retorna um `multiplayer_role_type` objeto. Você pode obter as funções de indexação a `roles` mapear, que retorna um `multiplayer_role_info` objeto.

O `multiplayer_role_info` objeto contém informações sobre a função, incluindo `max_members_count`, `member_xbox_user_ids`, `members_count`, e `target_count`.

### <a name="get-the-role-information-from-a-search-handle"></a>Obter as informações de função de um identificador de pesquisa

O fluxo para obter informações sobre a função de uma sessão é semelhante ao fluxo para obter informações de um identificador de pesquisa, mas algumas classes diferentes são usados.

No `multiplayer_session` do objeto, você pode obter as informações de tipo de função fazendo referência a `session_role_types` objeto, que é um `multiplayer_session_role_types` classe. No objeto, você pode indexar o `role_types` mapa com o nome do tipo de função que você está interessado.

Isso retorna um `multiplayer_role_type` objeto. Você pode obter as funções de indexação a `roles` mapear, que retorna um `multiplayer_role_info` objeto.

O `multiplayer_role_info` objeto contém informações sobre a função, incluindo `max_members_count`, `member_xbox_user_ids`, `members_count`, e `target_count`.

## <a name="change-mutable-role-settings"></a>Alterar configurações de função mutável

Se um tipo de função indicar que algumas configurações de função podem ser alteradas (mutáveis), você pode usar o `multiplayer_role_type.set_roles()` método para modificar as configurações mutáveis. Somente os membros que são marcados como proprietários de sessão podem alterar configurações de função.

## <a name="assign-a-role-to-a-member"></a>Atribuir uma função a um membro

Atualmente, somente um membro pode atribuir sua própria função no Xbox Live SDK. No `multiplayer_session` do objeto, você pode chamar o `set_current_user_role_info(role_type, role_name)` método para especificar o tipo de função e a função para o membro atual.

Se a função já estiver cheio, quando você tentar gravar a sessão para o serviço, MPSD rejeitará a gravação.
