---
title: Usando o jogo bate-papo 2
description: Saiba como usar o Xbox Live jogo bate-papo 2 para adicionar a comunicação de voz ao seu jogo.
ms.date: 03/20/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, jogo bate-papo 2, jogo bate-papo, comunicação de voz
ms.localizationpriority: medium
ms.openlocfilehash: 43fbd7cec037df735686aa60bc6cd57217875e33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639251"
---
# <a name="using-game-chat-2-c"></a>Usando o jogo bate-papo 2 (C++)

Este é um breve passo a passo sobre como usar a API do C++ Game bate-papo do 2. Desenvolvedores que desejam acessar o jogo de bate-papo 2 por meio de jogos C# deve ver [Use jogo bate-papo 2 WinRT projeções](using-game-chat-2-winrt.md).

1. [Pré-requisitos](#prerequisites)
2. [Inicialização](#initialization)
3. [Configurar usuários](#configuring-users)
4. [Quadros de dados de processamento](#processing-data-frames)
5. [Alterações de estado de processamento](#processing-state-changes)
6. [Bate-papo do texto](#text-chat)
7. [Acessibilidade](#accessibility)
8. [UI](#ui)
9. [Um silenciament](#muting)
10. [Reputação ruim mudo automático](#bad-reputation-auto-mute)
11. [Privilégio e privacidade](#privilege-and-privacy)
12. [Limpeza](#cleanup)
13. [Modelo de falha](#failure-model)
14. [Como configurar cenários populares](#how-to-configure-popular-scenarios)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar a codificar com 2 de bate-papo do jogo, você deve ter configurado AppXManifest do seu aplicativo para declarar a funcionalidade do dispositivo "microfone". Recursos de AppXManifest são descritos mais detalhadamente em suas respectivas seções da documentação plataforma; o trecho a seguir mostra o nó de capacidade do dispositivo "microfone" deve existir sob o nó de pacote e os recursos ou então bate-papo será bloqueado:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

Compilar o jogo de bate-papo 2 requer a inclusão do cabeçalho GameChat2.h principal. Para vincular corretamente, seu projeto também deve incluir GameChat2Impl.h pelo menos uma unidade de compilação (um cabeçalho pré-compilado comum é recomendado, pois essas implementações de função de stub são pequeno e fácil para o compilador gere como "embutido").

A interface de 2 de bate-papo do jogo não exige um projeto para escolher entre a compilação com C + + c++ /CLI CX versus C++ tradicionais; ele pode ser usado com qualquer um. A implementação também não gera exceções como um meio de relatórios para que você pode consumi-lo facilmente em projetos sem exceções se preferencial de erros não fatais. A implementação, no entanto, lançar exceções como um meio de relatório de erros fatais (consulte [modelo falha](#failure) para obter mais detalhes).

## <a name="initialization"></a>Inicialização

Você começa a interagir com a biblioteca por inicializar a instância singleton de 2 de bate-papo de jogo com parâmetros que se aplicam ao tempo de vida de inicialização do singleton.

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>Configurar usuários

Depois que a instância será inicializada, você deve adicionar os usuários locais para a instância de 2 de bate-papo do jogo. Neste exemplo, o usuário A representará um usuário local.

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

Você também deve adicionar os usuários remotos e os identificadores que serão usados para representar o "ponto de extremidade remoto" que o usuário está em. Um "ponto de extremidade" é uma instância do aplicativo em execução em um dispositivo remoto. Neste exemplo, o usuário B está em X do ponto de extremidade, os usuários C e D são em X do ponto de extremidade de Y. ponto de extremidade é atribuído arbitrariamente identificador "1" e Y do ponto de extremidade é atribuído arbitrariamente identificador "2". Informar o jogo de bate-papo 2 dos usuários remotos com essas chamadas:

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

Agora você pode configurar a relação de comunicação entre cada usuário remoto e o usuário local. Neste exemplo, suponha que o usuário A e B estão na mesma equipe e a comunicação bidirecional é permitida. `c_communicationRelationshipSendAndReceiveAll` é uma constante definida em GameChat2.h para representar a comunicação bidirecional. Defina a relação do usuário para o usuário B com:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Agora, suponha que os usuários C e D são 'spectators' e devem ter permissão para escutar o usuário A, mas não falar. `c_communicationRelationshipSendAll` é uma constante definida em GameChat2.h para representar essa comunicação unidirecional. Defina as relações com:

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

Se não houver usuários remotos que foram adicionados para a instância singleton, mas ainda não foi configurados para se comunicar com quaisquer usuários locais - a qualquer momento que é okey! Isso pode ser esperado em cenários em que os usuários determinam as equipes ou podem alterar os canais falantes arbitrariamente. 2 de bate-papo jogo armazenará em cache apenas informações (por exemplo, relações de privacidade e reputação) para usuários que foram adicionados à instância, por isso é útil informar ao jogo 2 de bate-papo de todos os usuários possíveis, mesmo se eles não falam para todos os usuários locais em um ponto específico no tempo.

Por fim, suponha que o usuário D saiu do jogo e devem ser removido da instância local do jogo 2 bate-papo. Isso pode ser feito com a chamada a seguir:

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Chamar `chat_manager::remove_user()` podem invalidar o objeto de usuário. Se você estiver usando [manipulação de áudio em tempo real](real-time-audio-manipulation.md), consulte o [tempos de vida de usuário de bate-papo](real-time-audio-manipulation.md#chat-user-lifetimes) documentação para obter mais informações. Caso contrário, o objeto de usuário é invalidado imediatamente quando `chat_manager::remove_user()` é chamado. Uma restrição sutil em quando os usuários podem ser removidos é detalhada na [alterações de estado de processamento](#state).

## <a name="processing-data-frames"></a>Quadros de dados de processamento

2 de bate-papo jogo não tem sua própria camada de transporte; Isso deve ser fornecido pelo aplicativo. Este plug-in é gerenciado por chamadas de regulares e frequentes do aplicativo para o `chat_manager::start_processing_data_frames()` e `chat_manager::finish_processing_data_frames()` par de métodos. Esses métodos são como o jogo de bate-papo 2 fornece dados de saída para o aplicativo. Eles são projetados para operar rapidamente, de modo que pode ser controlados com frequência em um thread dedicado de rede. Isso fornece um local conveniente para recuperar todos os dados na fila sem se preocupar com a imprevisibilidade de tempo de rede ou complexidade de retorno de chamada com multithread.

Quando `chat_manager::start_processing_data_frames()` é chamado, todos os dados na fila são informados em uma matriz de `game_chat_data_frame` estrutura ponteiros. Aplicativos devem iterar sobre a matriz, inspecione o destino de "pontos de extremidade" e usar a camada de rede do aplicativo para entregar os dados para as instâncias de aplicativo remoto apropriado. Após a conclusão com todos os `game_chat_data_frame` estruturas, a matriz deve ser passada para 2 de bate-papo de jogo para liberar os recursos chamando `chat_manager:finish_processing_data_frames()`. Por exemplo:

```cpp
uint32_t dataFrameCount;
game_chat_data_frame_array dataFrames;
chat_manager::singleton_instance().start_processing_data_frames(&dataFrameCount, &dataFrames);
for (uint32_t dataFrameIndex = 0; dataFrameIndex < dataFrameCount; ++dataFrameIndex)
{
    game_chat_data_frame const* dataFrame = dataFrames[dataFrameIndex];
    HandleOutgoingDataFrame(
        dataFrame->packet_byte_count,
        dataFrame->packet_buffer,
        dataFrame->target_endpoint_identifier_count,
        dataFrame->target_endpoint_identifiers,
        dataFrame->transport_requirement
        );
}
chat_manager::singleton_instance().finish_processing_data_frames(dataFrames);
```

Quanto mais frequentemente os quadros de dados são processados, menor será a latência de áudio aparente para o usuário final. O áudio é Unido em quadros de dados de 40 ms; Esse é o período de sondagem sugeridas.

## <a name="processing-state-changes"></a>Alterações de estado de processamento

2 de bate-papo jogo fornece atualizações para o aplicativo, como mensagens de texto recebidas, por meio de chamadas de regulares e frequentes do aplicativo para o `chat_manager::start_processing_state_changes()` e `chat_manager::finish_processing_state_changes()` par de métodos. Eles são projetados para operar rapidamente, de modo que eles podem ser chamados todos os quadros de gráficos em seu loop de renderização da interface do usuário. Isso fornece um local conveniente para recuperar todas as alterações na fila sem se preocupar com a imprevisibilidade de tempo de rede ou complexidade de retorno de chamada com multithread.

Quando `chat_manager::start_processing_state_changes()` é chamado, todas as atualizações em fila são relatadas em uma matriz de `game_chat_state_change` estrutura ponteiros. Aplicativos devem iterar sobre a matriz, inspecionar a estrutura de base para seu tipo mais específico, converta a estrutura de base para os respectivos mais detalhados de tipo e, em seguida, lidar com essa atualização conforme apropriado. Após a conclusão com todos os `game_chat_state_change` objetos disponíveis no momento, essa matriz deve passada de volta para 2 de bate-papo de jogo para liberar os recursos chamando `chat_manager::finish_processing_state_changes()`. Por exemplo:

```cpp
uint32_t stateChangeCount;
game_chat_state_change_array gameChatStateChanges;
chat_manager::singleton_instance().start_processing_state_changes(&stateChangeCount, &gameChatStateChanges);

for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; ++stateChangeIndex)
{
    switch (gameChatStateChanges[stateChangeIndex]->state_change_type)
    {
        case game_chat_state_change_type::text_chat_received:
        {
            HandleTextChatReceived(static_cast<const game_chat_text_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        case Xs::game_chat_2::game_chat_state_change_type::transcribed_chat_received:
        {
            HandleTranscribedChatReceived(static_cast<const Xs::game_chat_2::game_chat_transcribed_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_state_changes(gameChatStateChanges);
```

Porque `chat_manager::remove_user()` imediatamente invalida a memória associada a um objeto de usuário, e as alterações de estado podem conter os ponteiros para objetos de usuário, `chat_manager::remove_user()` não pode ser chamado durante o processamento de alterações de estado.

## <a name="text-chat"></a>Bate-papo do texto

Para enviar o bate-papo de texto, use `chat_user::chat_user_local::send_chat_text()`. Por exemplo:

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

2 de bate-papo jogo irá gerar um quadro de dados que contém esta mensagem; os pontos de extremidade de destino para o quadro de dados serão aquelas associadas com usuários que foram configurados para receber o texto do usuário local. Quando os dados são processados pelos pontos de extremidade remotos, a mensagem será exposta por meio de um `game_chat_text_chat_received_state_change`. Assim como acontece com bate-papo, restrições de privacidade e privilégio serão respeitadas para bate-papo do texto. Se um par de usuários foram configurados para permitir bate-papo de texto, mas as restrições de privilégio ou de privacidade não permitir que a comunicação, a mensagem de texto será removida.

Suporte a entrada de bate-papo de texto e a exibição é necessária para acessibilidade (consulte [acessibilidade](#access) para obter mais detalhes).

## <a name="accessibility"></a>Acessibilidade

Suporte de bate-papo de texto de entrada e a exibição é necessária. Entrada de texto é necessária porque, mesmo em plataformas ou gêneros de jogos que ainda não tiveram generalizado físico do teclado usar, os usuários podem configurar o sistema para usar tecnologias assistenciais texto em fala. Da mesma forma, a exibição de texto é necessária porque os usuários podem configurar o sistema para usar a fala em texto. Essas preferências podem ser detectadas em usuários locais com a chamada a `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` e `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` métodos, respectivamente, e talvez você queira habilitar condicionalmente os mecanismos de texto.

### <a name="text-to-speech"></a>Conversão de texto em fala

Quando um usuário tiver habilitado, de texto em fala `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` retornará 'true'. Quando esse estado é detectado, o aplicativo deve fornecer um método de entrada de texto. Depois que a entrada de texto fornecida por um teclado real ou virtual, passe a cadeia de caracteres para o `chat_user::chat_user_local::synthesize_text_to_speech()` método. 2 de bate-papo jogo detectará e sintetizador de dados de áudio com base na cadeia de caracteres e a preferência do usuário voice acessível. Por exemplo:

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

O áudio sintetizado como parte dessa operação será transportado para todos os usuários que foram configurados para receber o áudio do usuário local. Se `chat_user::chat_user_local::synthesize_text_to_speech()` é chamado em um usuário que não tem texto em fala será habilitado do jogo bate-papo 2 não executar nenhuma ação.

### <a name="speech-to-text"></a>Conversão de fala em texto

Quando um usuário tiver habilitada, conversão de fala para texto `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` retornará true. Quando esse estado é detectado, o aplicativo deve estar preparado para fornecer a que interface do usuário associado com mensagens de bate-papo transcrita. 2 de bate-papo jogo automaticamente transcrição de áudio de cada usuário remoto e expô-lo por meio de um `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` uma classe Xbox One projetada especificamente para fornecer processamento simples de bate-papo do texto no jogo com foco nas tecnologias assistenciais de fala em texto.

### <a name="speech-to-text-performance-considerations"></a>Considerações sobre desempenho de fala em texto

Quando a fala em texto é habilitado, a instância de 2 de bate-papo do jogo em cada dispositivo remoto inicia uma conexão de soquete da web com o ponto de extremidade de serviços de fala. Cada cliente remoto do jogo bate-papo 2 carrega áudio para o ponto de extremidade de serviços de fala por meio de neste websocket; o ponto de extremidade de serviços de fala, ocasionalmente, retornará uma mensagem de transcrição ao dispositivo remoto. O dispositivo remoto, em seguida, envia a mensagem de transcrição (ou seja, uma mensagem de texto) para o dispositivo local, em que a mensagem transcrita é determinada por 2 de bate-papo de jogo para o aplicativo para processar.

Portanto, o custo de desempenho principal de fala em texto é o uso de rede. A maioria do tráfego de rede é o carregamento de áudio codificado. O websocket carrega áudio que já foi codificado por 2 de bate-papo do jogo no caminho de bate-papo de voz "normal"; o aplicativo tem controle sobre a taxa de bits por meio de `chat_manager::set_audio_encoding_type_and_bitrate`.

## <a name="ui"></a>Interface do Usuário

É recomendável que em qualquer lugar os jogadores são mostrados, especialmente em uma lista de gamertags como um placar também exibem ícones mudo/falando como comentários do usuário. Isso é feito chamando `chat_user::chat_indicator()` para recuperar um `game_chat_user_chat_indicator` que representa o status atual, o instantâneo de bate-papo para esse jogador. O exemplo a seguir demonstra o recuperando o valor do indicador para um `chat_user` objeto apontado pela variável chatUserA para determinar um valor constante do ícone específico para atribuir a uma variável 'iconToShow':

```cpp
switch (chatUserA->chat_indicator())
{
   case game_chat_user_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::local_microphone_muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

O valor relatado pelo `xim_player::chat_indicator()` deve ser alterado com frequência como players de início e parada falando, por exemplo. Ele é projetado para dar suporte a aplicativos sondando cada quadro de interface do usuário como resultado.

## <a name="muting"></a>Um silenciament

O `chat_user::chat_user_local::set_microphone_muted()` método pode ser usado para alternar o estado ativar mudo do microfone do usuário local. Quando o microfone estiver mudo, sem áudio do que microfone será capturado. Se o usuário estiver em um dispositivo compartilhado, como Kinect, o estado sem som se aplica a todos os usuários.

O `chat_user::chat_user_local::microphone_muted()` método pode ser usado para recuperar o estado de sem som do microfone do usuário local. Esse método reflete somente se o microfone do usuário foi desativado no software por meio de uma chamada para `chat_user::chat_user_local::set_microphone_muted()`; esse método não reflete mudo um hardware controlado, por exemplo, por meio de um botão em headset do usuário. Não há nenhum método para recuperar o estado de sem som do hardware de dispositivo de áudio de um usuário por meio de 2 de bate-papo do jogo.

O `chat_user::chat_user_local::set_remote_user_muted()` método pode ser usado para alternar o estado de sem som de um usuário remoto em relação a um determinado usuário local. Quando o usuário remoto estiver mudo, o usuário local não ouvir o áudio ou receber qualquer mensagem de texto do usuário remoto.

## <a name="bad-reputation-auto-mute"></a>Reputação ruim mudo automático

Normalmente, os usuários remotos começará unmuted. 2 de bate-papo jogo iniciará os usuários em um estado mudo quando (1) o usuário remoto não amigos com o usuário local e (2) o usuário remoto tem um sinalizador de Reputação ruim. Quando os usuários são mudo devido a essa operação `chat_user::chat_indicator()` retornará `game_chat_user_chat_indicator::reputation_restricted`. Esse estado será substituído pela primeira chamada para `chat_user::chat_user_local::set_remote_user_muted()` que inclui o usuário remoto como o usuário de destino.

## <a name="privilege-and-privacy"></a>Privilégio e privacidade

Sobre a relação de comunicação configurada pelo jogo, o jogo 2 bate-papo impõe restrições de privilégio e privacidade. 2 de bate-papo jogo executa pesquisas de restrição de privilégio e privacidade quando um usuário é adicionado pela primeira vez; o usuário `chat_user::chat_indicator()` sempre retornará `game_chat_user_chat_indicator::silent` até que conclua essas operações. Se a comunicação com um usuário é afetada por um privilégio ou privacidade restrição, o usuário `chat_user::chat_indicator()` retornará `game_chat_user_chat_indicator::platform_restricted`. Restrições de comunicação de plataforma se aplicam a voz e texto bate-papo; nunca haverá uma instância em que bate-papo do texto está bloqueado por uma restrição de plataforma, mas bate-papo é não, ou vice-versa.

`chat_user::chat_user_local::get_effective_communication_relationship()` pode ser usado para ajudar a distinguir quando os usuários não podem se comunicar devido ao privilégio incompleto e operações de privacidade. Ele retorna a relação de comunicação imposta por 2 bate-papo do jogo na forma de `game_chat_communication_relationship_flags` e o motivo pelo qual a relação não pode ser igual à relação configurada na forma de `game_chat_communication_relationship_adjuster`. Por exemplo, se as operações de pesquisa ainda estão em andamento, o `game_chat_communication_relationship_adjuster` será `game_chat_communication_relationship_adjuster::intializing`. Esse método deve ser usado em cenários de desenvolvimento e depuração; não deve ser usado para influenciar a interface do usuário (consulte [interface do usuário](#UI)).

## <a name="cleanup"></a>Cleanup

Quando o aplicativo não precisa mais comunicações via 2 bate-papo do jogo, você deverá chamar `chat_manager::cleanup()`. Isso permite que o jogo de bate-papo 2 recuperar os recursos que foram alocados para gerenciar as comunicações.

## <a name="failure-model"></a>Modelo de falha

A implementação de 2 de bate-papo do jogo não lança exceções como um meio de relatórios para que você pode consumi-lo facilmente em projetos sem exceções se preferencial de erros não fatais. 2 de bate-papo jogo, no entanto, lançar exceções para informar sobre erros fatais. Esses erros são o resultado do uso incorreto de API, como adicionar um usuário para a instância do serviço de bate-papo de jogo antes de inicializar a instância ou acesso a um objeto de usuário depois que ele tiver sido removido da instância de 2 de bate-papo do jogo. Esses erros devem ser detectadas no início do desenvolvimento e podem ser corrigidos, modificando o padrão usado para interagir com 2 de bate-papo do jogo. Quando ocorre um erro, uma dica sobre o que causou o erro é impresso para o depurador antes da exceção será gerada.

## <a name="how-to-configure-popular-scenarios"></a>Como configurar cenários populares

### <a name="push-to-talk"></a>Envio por push a palestra

Envio por push a conversa deve ser implementada com `chat_user::chat_user_local::set_microphone_muted()`. Chame `set_microphone_muted(false)` para permitir que a conversão de fala e `set_microphone_muted(true)` restringi-la. Esse método fornece a resposta de latência mais baixa de 2 de bate-papo do jogo.

### <a name="teams"></a>Equipes

Suponha que o usuário A e B estão na equipe azul, e o usuário C e D de usuário estão na equipe vermelha. Cada usuário está em uma instância exclusiva do aplicativo.

No dispositivo do usuário:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

No dispositivo do usuário B:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

No dispositivo do usuário C:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

No dispositivo do usuário D:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>Transmissão

Suponha que o usuário A é a líder dando pedidos e usuários B, C e D só podem escutar. Cada jogador está em um dispositivo exclusivo.

No dispositivo do usuário:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

No dispositivo do usuário B:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

No dispositivo do usuário C:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

No dispositivo do usuário D:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```
