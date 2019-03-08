---
title: Autenticação para projetos do XDK
description: Saiba como conectar usuários Xbox Live no título de um Kit de desenvolvimento do Xbox (XDK).
ms.assetid: 713bb2e3-80c5-4ac9-8697-257525f243d3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 597b3becfa2083955d8bd4e0adc91e4ae9b827a1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613491"
---
# <a name="authentication-for-xdk-projects"></a>Autenticação para projetos do XDK

Para tirar proveito dos recursos do Xbox Live em jogos, um usuário precisa criar um perfil do Xbox Live para se identificar na comunidade do Xbox Live.  Serviços do Xbox Live manter o controle de jogo relacionado atividades usando o perfil Xbox Live, como o gamertag do usuário e o jogador imagem, que são amigos de jogos do usuário, o que o usuário de jogos tem desempenhado, quais realizações desbloqueou o usuário, onde o usuário se encontra Placar de líderes para um determinado, jogo, etc.

Em um alto nível, você pode usar as APIs do Xbox Live, seguindo estas etapas:
1. Identificar o usuário interagir
2. Criar um contexto Xbox Live, com base no usuário
3. Usar o contexto do Xbox Live para acessar serviços do Xbox Live
4. Quando o usuário ou sair do jogo sinais horizontalmente, liberar o objeto de XboxLiveContext definindo-a para nulo

### <a name="creating-an-xboxliveuser-object"></a>Criar um objeto XboxLiveUser
A maioria das atividades do Xbox Live está relacionada aos usuários do Xbox Live.  Como desenvolvedor de jogos, você precisa primeiro criar um objeto de XboxLiveUser para representar o usuário local.

C++:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>( user );
```

WinRT:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
Microsoft::Xbox::Services::XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext( user );
```
