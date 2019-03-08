---
title: O que há de novo para o Xbox Live SDK – novembro de 2016
description: O que há de novo para o Xbox Live SDK – novembro de 2016
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a9efeec63399916444de9ba33d4e587a914f2a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658561"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>O que há de novo para o Xbox Live SDK – novembro de 2016

Consulte a [What's New - agosto de 2016](1608-whats-new.md) artigo para o que foi adicionado na versão de agosto de 2016.

## <a name="xbox-services-api"></a>API de serviços do Xbox

### <a name="simplified-achievements"></a>Realizações simplificadas

* [Simplificado realizações](../achievements-2017/simplified-achievements.md) são uma forma nova e mais simples de configurar e usar conquistas.  Você não precisa enviar eventos e ter os serviços do Xbox Live a decidir se sua realização é desbloqueada.  O título é responsável por essa decisão.
* Se você ter sido parte do piloto inicial realizações simplificado, também adicionamos suporte offline.
* Há uma nova amostra chamada SimplifiedAchievements que mostra como é fácil usar.

### <a name="multiplayer"></a>Multijogadores

* [Navegação de sessão](../multiplayer/session-browse.md) é uma nova maneira para os usuários a encontrar um jogo com vários participantes.  Navegação de sessão permite que os jogadores procurar uma lista de abrir sessões de jogos com vários participantes que atendem aos critérios especificados.
* O [Multiplayer Manager](../multiplayer/multiplayer-manager.md) agora tem recursos de preenchimento automático.  Se habilitada, com vários participantes Manager encontrará membros por meio de emparelhamento para preencher os slots abertos durante o jogo.
* Uma versão de pré-produção do [XIM (Xbox integrado com vários participantes)](../multiplayer/xbox-integrated-multiplayer.md) agora está disponível para o desenvolvimento do XDK.  XIM é uma interface independente para adicionar facilmente com vários participantes comunicação de rede e bate-papo em tempo real ao seu jogo com a potência de serviços do Xbox Live.

### <a name="social-manager"></a>Gerenciador de social

* Inúmeros [Social Manager](../social-platform/intro-to-social-manager.md) alterações na API:
    * Gerenciador social deixou o namespace experimental. Agradecimentos especiais a aqueles que foram pioneiros e forneceu comentários!
    * `xbox_social_user`
        * `string_t` objetos foram alterados para `char*` objetos
        * Registro de presença personalizada com limite de seis `social_manager_presence_title_record` por `social_manager_presence_record`
    * `social_event`
        * Retorna um `std::vector<xbox_user_id_container>` em vez de `std::vector<xbox_social_user>`
        * Esse vetor pode ser passado para a nova API, `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` API retorna um `std::vector<xbox_social_user*>`. Esses ponteiros ficam invalidados na próxima chamada para `social_manager::do_work()`
        * `get_copy_of_users` retornar um `std::vector<xbox_social_user>` como uma cópia dos usuários atuais do social_user_group ao chamador. Chamar essa função pode afetar `do_work` tempo de conclusão.
* Social Manager agora nunca falhará após a inicialização. Social Manager tentará novamente RTA automaticamente na desconexão. O `error` e `rta_disconnect_error` eventos foram preteridos e removidos
* Título pode especificar os alocadores de memória personalizado. Consulte a documentação do novo: [Memória de Gerenciador social e de desempenho](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Xbox One UWP
* APIs de TCUI adiciona suporte a vários usuários para aplicativos da UWP do Xbox One.  O C++ XSAPI adiciona novos Windows::System::User ^ parâmetros especificam o usuário e a API do WinRT XSAPI adiciona APIs ForUserAsync.
* Exemplos UWP atualizados para dar suporte a vários usuários Xbox

### <a name="other"></a>Outro

* C + + c++ /CLI WinRT suporte adicionado.   Mais detalhes podem ser encontrados [aqui](../introduction-to-xbox-live-apis.md)
* Nova funcionalidade para adicionar e remover seus próprios retornos de chamada de registro em log.  O nível de diagnóstico será passado para o retorno de chamada para que você pode ajustar seu comportamento.  Ver `add_logging_handler` e `remove_logging_handler` no `microsoft::xbox::services::system::xbox_live_services_settings` namespace

## <a name="documentation"></a>Documentação
* Há nova documentação sobre o [Manager Multiplayer](../multiplayer/multiplayer-manager.md), [XIM](../multiplayer/xbox-integrated-multiplayer.md), e [conceitos para múltiplos jogadores](../multiplayer/multiplayer-concepts.md) do Xbox Live.
* O [Xbox Live Introdução](../get-started-with-partner/get-started-with-xbox-live-partner.md) seções foram reescritas.  Se você estiver criando um novo título do Xbox Live habilitado ou se estiver curioso sobre a incorporação de outras funcionalidades do Xbox Live em seu jogo, você pode ver os novos documentos [aqui](../get-started-with-partner/get-started-with-xbox-live-partner.md).

## <a name="tools"></a>Ferramentas
* O Xbox Live rastreamento analisador que pode ser encontrado no [ https://aka.ms/XboxLiveTools ](https://aka.ms/XboxLiveTools) agora tem um modo CERT.  
* Também no analisador de rastreamento de Xbox Live é o suporte a console.  Se você passar em um rastreamento do Fiddler que contém as solicitações HTTP para várias do console, serão gerados relatórios separados para cada um.
