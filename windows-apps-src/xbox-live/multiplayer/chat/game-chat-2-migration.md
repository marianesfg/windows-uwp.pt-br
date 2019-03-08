---
title: Migração de jogo bate-papo 2
description: Saiba como migrar o código de jogo bate-papo existente para usar 2 bate-papo do jogo.
ms.date: 05/02/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, jogo bate-papo 2, jogo bate-papo, comunicação de voz
ms.localizationpriority: medium
ms.openlocfilehash: e963210091694a07114f10d5a3dc531a353621df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653581"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>Migração de bate-papo jogo para jogo bate-papo 2

Este documento fornece detalhes sobre as semelhanças entre o jogo de bate-papo e 2 de bate-papo do jogo e como migrar de bate-papo de jogo para 2 de bate-papo do jogo. Como tal, é para títulos que têm uma implementação existente do serviço de bate-papo do jogo que desejarem migrar para 2 de bate-papo do jogo. Se você ainda não tiver uma implementação de bate-papo do jogo, o ponto de partida sugerido é [2 de bate-papo de jogo usando](using-game-chat-2.md). 

## <a name="preface"></a>Prefácio

A API de bate-papo de jogo original é uma API do WinRT que expostos a um conceito de usuários de bate-papo e canais de voz para auxiliar na implementação de cenários de bate-papo com voz do jogo Xbox Live. A API de bate-papo do jogo é criada sobre a API de bate-papo, que por si só é uma API do WinRT que expostos a um conceito de usuários de bate-papo e canais de voz, exigindo o gerenciamento de nível baixo de dispositivos de áudio. 2 de bate-papo jogo é o sucessor as APIs de bate-papo e o bate-papo de jogo original – ele foi projetado para ser uma API mais simples para cenários de bate-papo básicas, como a comunicação da equipe, e fornecer simultaneamente mais flexibilidade para cenários de bate-papo avançadas, como estilo de transmissão a comunicação e manipulação de áudio em tempo real. Jogo de bate-papo e jogos 2 de bate-papo preencher o nicho mesmo: as APIs oferecem um método conveniente para a integração do Xbox Live habilitado, bate-papo no jogo de voz, enquanto o título fornece uma camada de transporte para a transmissão de pacotes de dados de e para instâncias remotas do jogo de bate-papo ou jogo Bate-papo 2.

A API de 2 de bate-papo de jogo tem muitas vantagens em relação a bate-papo de jogo original e APIs de bate-papo. Alguns dos destaques incluem:
* Um flexível orientado ao usuário que não é restrito para o modelo de "canal" API.
* Melhorias de largura de banda devido a filtragem de pacotes por fontes de dados e receptores de dados.
* Uma restrição interna para 2 threads de longa duração com a afinidade configuráveis pelo aplicativo.
* Uma API disponível nos formulários de C++ e o WinRT.
* Modelo de consumo simplificada alinhando com um padrão de "trabalho".
* Ganchos de memória para redirecionar as alocações de 2 de bate-papo do jogo por meio de um alocador personalizado.
* UWP idêntico + cabeçalhos de aplicativo de recurso exclusivo (ERA) para uma experiência de desenvolvimento de multiplataforma mais conveniente.

Este documento fornece detalhes sobre as semelhanças entre o jogo de bate-papo e 2 de bate-papo do jogo e como migrar de bate-papo de jogo para a API de C++ de 2 de bate-papo de jogo. Se você estiver interessado na migração de bate-papo de jogo para a API do WinRT jogo bate-papo 2, é recomendável que você leia este documento para entender como mapear os conceitos de bate-papo do jogo para 2 de bate-papo do jogo e, em seguida, consulte [usando o jogo de bate-papo 2 WinRT as projeções](using-game-chat-2-winrt.md) para o padrões específicos de WinRT. O código de exemplo para o Chat de jogo original neste documento usará C + + c++ /CLI CX.

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

A interface de 2 de bate-papo do jogo não exige um projeto para escolher entre a compilação com C + + c++ /CLI CX versus C++ tradicionais; ele pode ser usado com qualquer um. A implementação também não gera exceções como um meio de relatórios para que você pode consumi-lo facilmente em projetos sem exceções se preferencial de erros não fatais. A implementação, no entanto, lançar exceções como um meio de relatório de erros fatais (consulte [modelo falha](#failure-model-and-debugging) para obter mais detalhes).

## <a name="initialization"></a>Inicialização

### <a name="game-chat"></a>Jogo bate-papo

Interagindo com o Chat de jogo original é feito por meio de `ChatManager` classe. O exemplo a seguir mostra como construir um `ChatManager` usando os parâmetros padrão da instância:

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>Chat de Jogo 2

Toda a interação com 2 de bate-papo do jogo é feita por meio de jogo de bate-papo 2 `chat_manager` singleton. O singleton deve ser inicializado antes que qualquer interação significativa com a biblioteca possa ocorrer. O singleton requer que o número máximo de usuários simultâneos locais e remotos bate-papo ser especificado no momento da inicialização; Isso ocorre porque o jogo de bate-papo 2 previamente aloca memória proporcional ao número esperado de usuários. O exemplo a seguir mostra como inicializar a instância singleton quando o número máximo de usuários de bate-papo locais e remotos simultâneos será quatro:

```cpp
chat_manager::singleton_instance().initialize(4);
```

Há vários parâmetros opcionais que podem ser especificados durante a inicialização, como o padrão renderizar o volume de usuários remotos de bate-papo e se 2 de bate-papo de jogo deve executar a conversão automática de fala em texto. Para obter mais detalhes sobre esses parâmetros, consulte a documentação do `chat_manager::initialize()`.

## <a name="configuring-users"></a>Configurar usuários

### <a name="game-chat"></a>Jogo bate-papo

Adição de usuários locais para a API de bate-papo de jogo original é feita por meio de `ChatManager::AddLocalUserToChatChannelAsync()`. Adicionando o usuário em vários canais de bate-papo requer várias chamadas, cada uma especificando um canal diferente. O exemplo a seguir mostra como adicionar o usuário com xuid "myXuid" para 0 do canal:

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

Os usuários remotos não são adicionados diretamente à instância. Quando um dispositivo remoto aparece na rede do título, o título cria um objeto que representa o dispositivo remoto e informa ao jogo de bate-papo do novo dispositivo via `ChatManager::HandleNewRemoteConsole()`. O título também deve fornecer um método de comparar os objetos que representam os dispositivos remotos jogo conversar com a implementação de `CompareUniqueConsoleIdentifiersHandler`. O exemplo a seguir mostra como fornecer esse delegado ao jogo de bate-papo, supondo `Platform::String` objetos são usados para representar um novo dispositivo, representado pela cadeia de caracteres e dispositivos remotos `L"1"` ingressou em rede do título:

```cpp
auto token = chatManager->OnCompareUniqueConsoleIdentifiers +=
    ref new CompareUniqueConsoleIdentifiersHandler(
        [this](Platform::Object^ obj1, Platform::Object^ obj2)
    {
        return (static_cast<Platform::String^>(obj1)->Equals(static_cast<Platform::String^>(obj2)));
    });

...

Platform::String^ newDeviceIdentifier = ref new Platform::String(L"1");
chatManager->HandleNewRemoteConsole(newDeviceIdentifier);
```

Depois que o jogo de bate-papo foi informado deste dispositivo, ele gerará os pacotes que contêm informações sobre todos os usuários locais e fornecer esses pacotes para o título para a transmissão para a instância de bate-papo do jogo no dispositivo remoto. Da mesma forma, a instância de bate-papo do jogo no dispositivo remoto irá gerar os pacotes que contêm informações sobre os usuários no dispositivo remoto que transmitirá o título para a instância local do Chat do jogo. Quando a instância local recebe pacotes contendo informações sobre novos usuários remotos, os novos usuários remotos são adicionados à lista de locais da instância de usuários, disponíveis por meio do `ChatManager::GetUsers()`.

Removendo usuários da instância do serviço de bate-papo do jogo é realizada por meio de chamadas semelhantes para `ChatManager::RemoveLocalUserFromChatChannelAsync()` e `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>Chat de Jogo 2

Adição de usuários locais para 2 de bate-papo do jogo é feito de forma síncrona por meio `chat_manager::add_local_user()`. Neste exemplo, o usuário A representará um usuário local com a Id de usuário do Xbox `L"myLocalXboxUserId"`:

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

Observe que o usuário não é adicionado para um canal específico - 2 de bate-papo do jogo usa um conceito de "relações de comunicação" em vez de canais para determinar se os usuários poderão conversar com e ouvir um ao outro. O método de configurar relações de comunicação é abordado posteriormente nesta seção.

Usuários semelhantes para o local, os usuários remotos são adicionados forma síncrona no jogo bate-papo local instância 2. Os usuários remotos simultaneamente devem ser associados com os identificadores que serão usados para representar o dispositivo remoto. 2 de bate-papo jogo refere-se a uma instância do aplicativo em execução em um dispositivo remoto como um "ponto de extremidade". Neste exemplo, o usuário B será um usuário com a Id de usuário do Xbox `L"remoteXboxUserId"` em um ponto de extremidade representado pelo número inteiro `1`.

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

Depois que os usuários foram adicionados à instância, você deve configurar a "relação de comunicação" entre cada usuário remoto e cada usuário local. Neste exemplo, suponha que o usuário A e B estão na mesma equipe e a comunicação bidirecional é permitida. `c_communicationRelationshiSendAndReceiveAll` é uma constante definida em GameChat2.h para representar a comunicação bidirecional. A relação pode ser configurada com:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Por fim, suponha que o usuário B saiu do jogo e devem ser removido da instância local do jogo bate-papo. Que é executada de maneira síncrona com a chamada a seguir:

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Consulte a [usando o jogo de bate-papo 2 – Configurando usuários](using-game-chat-2.md#configuring-users) para obter mais exemplos ou a referência para métodos de API individuais para obter mais informações.

## <a name="processing-data"></a>Processando dados

### <a name="game-chat"></a>Jogo bate-papo

Bate-papo jogo não tem sua própria camada de transporte; Isso deve ser fornecido pelo aplicativo. Pacotes de saída é manipulado pelo assinando o `OnOutgoingChatPacketReady` eventos e inspecionando os argumentos para determinar os requisitos de transporte e de destino do pacote. O exemplo a seguir mostra como assinar o evento e encaminhar os argumentos a `HandleOutgoingPacket()` método implementado pelo título:

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

Os pacotes de entrada são enviados ao jogo bate-papo via `ChatManager::ProcessingIncomingChatMessage()`. O buffer de pacote básico e o identificador de dispositivo remoto devem ser fornecidos. O exemplo a seguir mostra como enviar um pacote armazenado no local `packetBuffer` e o identificador de dispositivo remoto armazenado na variável local `remoteIdentifier`.

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>Chat de Jogo 2

Da mesma forma, 2 de bate-papo do jogo não tem sua própria camada de transporte; Isso deve ser fornecido pelo aplicativo. Pacotes de saída é manipulado por chamadas de regulares e frequentes do aplicativo para o `chat_manager::start_processing_data_frames()` e `chat_manager::finish_processing_data_frames()` par de métodos. Esses métodos são como o jogo de bate-papo 2 fornece dados de saída para o aplicativo. Eles são projetados para operar rapidamente, de modo que pode ser controlados com frequência em um thread dedicado de rede. Isso fornece um local conveniente para recuperar todos os dados na fila sem se preocupar com a imprevisibilidade de tempo de rede ou complexidade de retorno de chamada com multithread.

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

Os dados de entrada são enviados para o jogo de bate-papo 2 por meio de `chat_manager::processing_incoming_data()`. O buffer de dados e o identificador de ponto de extremidade remoto devem ser fornecidos. O exemplo a seguir mostra como enviar um pacote de dados que é armazenado na variável local `dataFrame` e o identificador de ponto de extremidade remoto armazenado na variável local `remoteEndpointIdentifier`:

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>Processamento de eventos

### <a name="game-chat"></a>Jogo bate-papo

Bate-papo jogo usa um modelo de eventos para informar o aplicativo quando algo interessante ocorre - como o recebimento de uma mensagem de texto ou a alteração de preferência de acessibilidade de um usuário. O aplicativo deve assinar e implemente um manipulador para cada evento de interesse. Este exemplo mostra como se inscrever para o `OnTextMessageReceived` evento e encaminhar os argumentos para o `OnTextChatReceived()` ou `OnTranscribedChatReceived()` métodos implementados pelo aplicativo:

```cpp
auto token = chatManager->OnTextMessageReceived +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^ args)
    {
        if (args->ChatTextMessageType != ChatTextMessageType::TranscribedSpeechMessage)
        {
            this->OnTextChatReceived(args);
        }
        else
        {
            this->OnTranscribedChatReceived(args);
        }
    });
```

### <a name="game-chat-2"></a>Chat de Jogo 2

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

### <a name="game-chat"></a>Jogo bate-papo

Enviar chat de texto com bate-papo do jogo, `GameChatUser::GenerateTextMessage()` pode ser usado. O exemplo a seguir mostra como enviar uma mensagem de texto de bate-papo com um usuário de bate-papo local representado pelo `chatUser` variável:

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

O segundo parâmetro boolean controla a conversão de texto em fala. Para obter mais detalhes, consulte [acessibilidade](#accessibility). Bate-papo de jogos e em seguida, gera um pacote de bate-papo, que contém esta mensagem. Instâncias remotas do jogo de bate-papo serão notificados sobre a mensagem de texto por meio de `OnTextMessageReceived` eventos.

### <a name="game-chat-2"></a>Chat de Jogo 2

Para enviar o bate-papo de texto com 2 de bate-papo do jogo, use `chat_user::chat_user_local::send_chat_text()`. O exemplo a seguir mostra como enviar uma mensagem de texto de bate-papo com um usuário de bate-papo local representado pelo `chatUser` variável:

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

2 de bate-papo jogo irá gerar um quadro de dados que contém esta mensagem; os pontos de extremidade de destino para o quadro de dados serão aquelas associadas com usuários que foram configurados para receber o texto do usuário local. Quando os dados são processados pelos pontos de extremidade remotos, a mensagem será exposta por meio de um `game_chat_text_chat_received_state_change`. Assim como acontece com bate-papo, restrições de privacidade e privilégio serão respeitadas para bate-papo do texto. Se um par de usuários foram configurados para permitir bate-papo de texto, mas as restrições de privilégio ou de privacidade não permitir que a comunicação, a mensagem de texto será descartada silenciosamente.

Suporte a entrada de bate-papo de texto e a exibição é necessária para acessibilidade (consulte [acessibilidade](#accessibility) para obter mais detalhes).

## <a name="accessibility"></a>Acessibilidade

Suporte de bate-papo de texto de entrada e a exibição é necessária para bate-papo do jogo e 2 de bate-papo do jogo. Entrada de texto é necessária porque, mesmo em plataformas ou gêneros de jogos que ainda não tiveram generalizado físico do teclado usar, os usuários podem configurar o sistema para usar tecnologias assistenciais texto em fala. Da mesma forma, a exibição de texto é necessária porque os usuários podem configurar o sistema para usar a fala em texto. Bate-papo do jogo e o jogo bate-papo 2 fornecem métodos de detecção e respeitar as preferências de acessibilidade de um usuário; Talvez você queira habilitar condicionalmente com base nessas configurações de mecanismos de texto.

### <a name="text-to-speech---game-chat"></a>Texto em fala – jogos de bate-papo

Quando um usuário tiver habilitado, de texto em fala `GameChatUser::HasRequestedSynthesizedAudio()` retornará true. Quando esse estado é detectado, `GameChatUser::GenerateTextMessage()` adicionalmente irá gerar o texto em fala áudio que está inserido no fluxo de áudio associado ao usuário local. O exemplo a seguir mostra como enviar uma mensagem de texto de bate-papo com um usuário local representado pelo `chatUser` variável:

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

O segundo parâmetro boolean é usado para permitir que o aplicativo substituir a preferência de fala em texto - 'true' indica que o bate-papo de jogo deve gerar áudio texto para fala quando a preferência do usuário texto em fala tiver sido habilitada, enquanto 'false' indica que jogo Bate-papo nunca deve gerar um áudio texto em fala da mensagem.

### <a name="text-to-speech---game-chat-2"></a>Texto em fala-jogo bate-papo 2

Quando um usuário tiver habilitado, de texto em fala `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` retornará 'true'. Quando esse estado é detectado, o aplicativo deve fornecer um método de entrada de texto. Depois que a entrada de texto fornecida por um teclado real ou virtual, passe a cadeia de caracteres para o `chat_user::chat_user_local::synthesize_text_to_speech()` método. 2 de bate-papo jogo detectará e sintetizador de dados de áudio com base na cadeia de caracteres e a preferência do usuário voice acessível. Por exemplo:

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

O áudio sintetizado como parte dessa operação será transportado para todos os usuários que foram configurados para receber o áudio do usuário local. Se `chat_user::chat_user_local::synthesize_text_to_speech()` é chamado em um usuário que não tem texto em fala será habilitado do jogo bate-papo 2 não executar nenhuma ação.

### <a name="speech-to-text---game-chat"></a>Conversão de fala em texto - jogo bate-papo

Quando um usuário tiver habilitada, conversão de fala para texto `GameChatUser::HasRequestSynthesizedAudio()` retornará 'true'. Quando esse estado é detectado, bate-papo do jogo automaticamente transcrever o áudio de áudio de cada usuário remoto e expô-lo por meio de `OnTextMessageReceived` eventos. Quando o `OnTextMessageReceived` evento aciona devido ao recebimento de uma mensagem de transcrição, a `TextMessageReceivedEventArgs` indicará um tipo de mensagem de `ChatTextMessageType::TranscribedSpeechMessage`.

### <a name="speech-to-text---game-chat-2"></a>Conversão de fala em texto - jogo bate-papo 2

Quando um usuário tiver habilitada, conversão de fala para texto `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` retornará true. Quando esse estado é detectado, o aplicativo deve estar preparado para fornecer a que interface do usuário associado com mensagens de bate-papo transcrita. 2 de bate-papo jogo automaticamente transcrição de áudio de cada usuário remoto e expô-lo por meio de um `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` uma classe Xbox One projetada especificamente para fornecer processamento simples de bate-papo do texto no jogo com foco nas tecnologias assistenciais de fala em texto.

Consulte a [2 da usando jogo bate-papo - considerações sobre desempenho de fala em texto](using-game-chat-2.md#speech-to-text-performance-considerations) para obter mais detalhes sobre considerações de desempenho de fala em texto.

## <a name="ui"></a>Interface do Usuário

É recomendável que em qualquer lugar os jogadores são mostrados, especialmente em uma lista de gamertags como um placar também exibem ícones mudo/falando como comentários do usuário. 2 de bate-papo jogos apresenta um indicador conciliado que fornece um meio mais simples de determinar os elementos de interface do usuário apropriados para mostrar.

### <a name="game-chat"></a>Jogo bate-papo

Um `GameChatUser` tem três propriedades que normalmente são inspecionadas ao determinar os elementos de interface do usuário apropriados - `GameChatUser::TalkingMode`, `GameChatUser::IsMuted`, e `GameChatUser::RestrictionMode`. O exemplo a seguir demonstra uma possível heurística para determinar um valor constante do ícone específico para atribuir a um `iconToShow` variável de um `GameChatUser` objeto apontado pela variável 'chatUser'.

```cpp
if (chatUser->RestrictionMode != None)
{
    if (!chatUser->IsMuted)
    {
        if (chatUser->TalkingMode != NotTalking)
        {
            iconToShow = Icon_ActiveSpeaker;
        }
        else
        {
            iconToShow = Icon_InactiveSpeaker;
        }
    }
    else
    {
        iconToShow = Icon_MutedSpeaker;
    }
}
else
{
    iconToShow = Icon_RestrictedSpeaker;
}
```

### <a name="game-chat-2"></a>Chat de Jogo 2

2 de bate-papo jogo tem um conciliados `game_chat_user_chat_indicator` usado para representar o status atual, o instantâneo de bate-papo de um player. Esse valor é recuperado por meio da chamada `chat_user::chat_indicator()`. O exemplo a seguir demonstra o recuperando o valor do indicador para um `chat_user` objeto apontado pela variável chatUser para determinar um valor constante do ícone específico para atribuir a uma variável 'iconToShow':

```cpp
switch (chatUser->chat_indicator())
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

## <a name="muting"></a>Um silenciament

## <a name="game-chat"></a>Jogo bate-papo

Um silenciament ou desativação da opção mudo um usuário no jogo de bate-papo é realizada por meio de uma chamada para `ChatManager::MuteUserFromAllChannels()` ou `ChatManager::UnMuteUIserFromAllChannels()`, respectivamente. O estado de sem som de um usuário pode ser recuperado por meio da inspeção `GameChatUser::IsMuted` ou `GameChatUser::IsLocalUserMuted`.

## <a name="game-chat-2"></a>Chat de Jogo 2

O `chat_user::chat_user_local::set_microphone_muted()` método pode ser usado para alternar o estado ativar mudo do microfone do usuário local. Quando o microfone estiver mudo, sem áudio do que microfone será capturado. Se o usuário estiver em um dispositivo compartilhado, como Kinect, o estado sem som se aplica a todos os usuários.

O `chat_user::chat_user_local::microphone_muted()` método pode ser usado para recuperar o estado de sem som do microfone do usuário local. Esse método reflete somente se o microfone do usuário foi desativado no software por meio de uma chamada para `chat_user::chat_user_local::set_microphone_muted()`; esse método não reflete mudo um hardware controlado, por exemplo, por meio de um botão em headset do usuário. Não há nenhum método para recuperar o estado de sem som do hardware de dispositivo de áudio de um usuário por meio de 2 de bate-papo do jogo.

O `chat_user::chat_user_local::set_remote_user_muted()` método pode ser usado para alternar o estado de sem som de um usuário remoto em relação a um determinado usuário local. Quando o usuário remoto estiver mudo, o usuário local não ouvir o áudio ou receber qualquer mensagem de texto do usuário remoto.

## <a name="bad-reputation-auto-mute"></a>Reputação ruim mudo automático

Normalmente, os usuários remotos começará unmuted. Bate-papo do jogo e o jogo bate-papo 2 tem um recurso de "mudo automático de Reputação ruim". Isso significa que os usuários começarão em um estado mudo quando (1) o usuário remoto não amigos com um usuário local, e (2) o usuário remoto tem um sinalizador de Reputação ruim. 2 de bate-papo jogo fornece comentários quando um usuário é desligado devido a esse recurso. Consulte a [2 da usando jogo bate-papo - reputação ruim mudo automático](using-game-chat-2.md#bad-reputation-auto-mute) para obter mais informações.

## <a name="privilege-and-privacy"></a>Privilégio e privacidade

Bate-papo do jogo e o jogo bate-papo 2 impõem restrições de privilégio e privacidade Xbox Live na parte superior de qualquer canal ou comunicação relações gerenciadas pelo aplicativo. Além disso, o jogo 2 bate-papo fornece informações de diagnóstico para determinar exatamente como a restrição está afetando a direção de áudio (por exemplo, se uma restrição de áudio de áudio é uni ou bidirecional).

### <a name="game-chat"></a>Jogo bate-papo

Jogo bate-papo expostas a informações de privilégio e privacidade por meio de `RestrictionMode` propriedade. Ele pode ser recuperado por meio da inspeção `GameChatUser::RestrictionMode`.

### <a name="game-chat-2"></a>Chat de Jogo 2

2 de bate-papo jogo executa pesquisas de restrição de privilégio e privacidade quando um usuário é adicionado pela primeira vez; o usuário `chat_user::chat_indicator()` sempre retornará `game_chat_user_chat_indicator::silent` até que conclua essas operações. Se a comunicação com um usuário é afetada por um privilégio ou privacidade restrição, o usuário `chat_user::chat_indicator()` retornará `game_chat_user_chat_indicator::platform_restricted`. Restrições de comunicação de plataforma se aplicam a voz e texto bate-papo; nunca haverá uma instância em que bate-papo do texto está bloqueado por uma restrição de plataforma, mas bate-papo é não, ou vice-versa.

`chat_user::chat_user_local::get_effective_communication_relationship()` pode ser usado para ajudar a distinguir quando os usuários não podem se comunicar devido ao privilégio incompleto e operações de privacidade. Ele retorna a relação de comunicação imposta por 2 bate-papo do jogo na forma de `game_chat_communication_relationship_flags` e o motivo pelo qual a relação não pode ser igual à relação configurada na forma de `game_chat_communication_relationship_adjuster`. Por exemplo, se as operações de pesquisa ainda estão em andamento, o `game_chat_communication_relationship_adjuster` será `game_chat_communication_relationship_adjuster::intializing`. Esse método deve ser usado em cenários de desenvolvimento e depuração; não deve ser usado para influenciar a interface do usuário (consulte [interface do usuário](#UI)).

## <a name="cleanup"></a>Cleanup

Do original jogo bate-papo `ChatManager` é uma classe de tempo de execução do WinRT - contado de uma referência de interface. Dessa forma, os recursos de memória são recuperados quando a última contagem de referência cai para zero e limpa. Interação com o do C++ API 2 bate-papo jogo é feita por meio de uma instância singleton; Quando o aplicativo não precisa mais comunicações via 2 bate-papo do jogo, você deverá chamar `chat_manager::cleanup()`. Isso permite que o jogo de bate-papo 2 liberar todos os recursos que foram alocados para gerenciar as comunicações imediatamente. Para obter detalhes sobre a API do WinRT jogo bate-papo 2 e o gerenciamento de recursos, consulte [usando o jogo de bate-papo 2 WinRT as projeções](using-game-chat-2-winrt.md#cleanup).

## <a name="failure-model-and-debugging"></a>Modelo de falha e de depuração

O Chat de jogo original é uma API do WinRT; Portanto, ele relatou erros por meio de exceções. Erros não fatais ou avisos são relatados por meio de um retorno de chamada de depuração adicionais. Jogo do C++ API 2 bate-papo não lança exceções como um meio de relatórios para que você pode consumi-lo facilmente em projetos sem exceções se preferencial de erros não fatais. 2 de bate-papo jogo, no entanto, lançar exceções para informar sobre erros fatais. Esses erros são o resultado do uso incorreto de API, como adicionar um usuário para a instância do serviço de bate-papo de jogo antes de inicializar a instância ou acesso a um objeto de usuário depois que ele tiver sido removido da instância do serviço de bate-papo do jogo. Esses erros devem ser detectadas no início do desenvolvimento e podem ser corrigidos, modificando o padrão usado para interagir com 2 de bate-papo do jogo. Quando ocorre um erro, uma dica sobre o que causou o erro é impresso para o depurador antes da exceção será gerada. API do WinRT jogo bate-papo do 2 segue o padrão de WinRT dos relatórios de erros por meio de exceções.

## <a name="how-to-configure-popular-scenarios"></a>Como configurar cenários populares

Consulte a [usando o jogo de bate-papo 2 - como configurar cenários populares](using-game-chat-2.md#how-to-configure-popular-scenarios) para obter exemplos sobre como configurar cenários populares, como o envio por push para falar, equipes e cenários de comunicação de estilo de transmissão.

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>Pré-codificar e decodificar posteriores à manipulação de áudio

O Chat de jogo original permitido para a manipulação de áudio, permitindo o acesso para o áudio do microfone bruto a `OnPreEncodeAudioBuffer` e `OnPostDecodeAudioBuffers` eventos. 2 de bate-papo jogo expõe esse recurso por meio de um modelo de sondagem. Para obter mais informações, consulte [manipulação de áudio em tempo real](real-time-audio-manipulation.md).
