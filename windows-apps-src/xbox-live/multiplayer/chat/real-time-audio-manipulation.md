---
title: Manipulação de áudio em tempo real
description: Saiba como manipular e processar o áudio de bate-papo capturado por 2 de bate-papo do jogo.
ms.date: 05/10/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, jogo bate-papo 2, jogo bate-papo, comunicação de voz, manipulação de buffer, manipulação de áudio
ms.localizationpriority: medium
ms.openlocfilehash: 7746080ea8a9698993a679b425f41442e4a6d943
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643891"
---
# <a name="real-time-audio-manipulation"></a>Manipulação de áudio em tempo real

2 de bate-papo jogo fornece aos desenvolvedores a opção de inserem no pipeline de áudio de bate-papo para inspecionar e manipular dados de áudio de bate-papo dos jogadores. Isso pode ser útil para a aplicação de efeitos de áudio para vozes de jogadores no jogo de interessante. Pipeline de manipulação de áudio de jogo bate-papo do 2 é interagir por meio de objetos de fluxo de áudio que podem ser monitorados para dados de áudio. Em vez de usar retornos de chamada, esse modelo permite que os desenvolvedores inspecionar ou manipular áudio em qualquer thread de processamento é mais conveniente para eles.

Um breve passo a passo usando a manipulação de áudio em tempo real é apresentada abaixo, que contém os tópicos a seguir:

1. [Inicializando o pipeline de manipulação de áudio](#initializing-the-audio-manipulation-pipeline)
2. [Alterações de estado do processamento de fluxo de áudio](#processing-audio-stream-state-changes)
3. [Pré-manipulando codificar o áudio de bate-papo](#manipulating-pre-encode-chat-audio-data)
4. [Pós-manipulando decodificar o áudio de bate-papo](#manipulating-post-decode-chat-audio-data)
5. [Tempos de vida de usuário de bate-papo](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>Inicializando o pipeline de manipulação de áudio

2 de bate-papo jogo por padrão não habilitará a manipulação de áudio em tempo real. Para habilitar a manipulação de áudio em tempo real, o aplicativo deve especificar quais formulários de manipulação de áudio a ele gostaria de ter habilitado no `chat_manager::initialize()` , definindo o parâmetro audioManipulationMode.

Atualmente, há suporte para os seguintes formulários de manipulação de áudio:

* `game_chat_audio_manipulation_mode_flags::none` -Desabilita a manipulação de áudio. Essa é a configuração padrão. Nesse modo, o áudio de bate-papo fará o fluxo sem interrupções.
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` -Permite codifica previamente manipulação de áudio. Nesse modo, todos os áudio de bate-papo geradas pelos usuários locais serão alimentados por meio do pipeline de manipulação de áudio antes de ser codificada. Mesmo se o aplicativo for apenas inspecionando os dados de áudio de bate-papo e não manipulá-los, ainda é responsabilidade do aplicativo para enviar os buffers de áudio inalterados para 2 de bate-papo de jogo para que possa ser codificadas e transmitidas.
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` -Habilita decodificar posteriores à manipulação de áudio. Nesse modo, todos os áudio de bate-papo recebido de usuários remotos serão alimentados por meio do pipeline de manipulação de áudio depois que ele é decodificado pelo receptor, mas antes de serem processada. Mesmo se o aplicativo for apenas inspecionando os dados de áudio de bate-papo e não manipulá-los, ainda é responsabilidade do aplicativo para combinar e envie os buffers de áudio inalterados para 2 de bate-papo de jogo para que eles podem ser renderizados.

## <a name="processing-audio-stream-state-changes"></a>Alterações de estado do processamento de fluxo de áudio

2 de bate-papo jogo fornece atualizações para o estado dos fluxos de áudio por meio de `game_chat_stream_state_change` estruturas. Essas atualizações armazenam informações sobre quais fluxo foi atualizado e como ele foi atualizado. Essas atualizações podem ser sondadas quanto por meio de chamadas para o `chat_manager::start_processing_stream_state_changes()` e `chat_manager::finish_processing_stream_state_changes()` par de métodos. Esse par de métodos fornece tudo o mais recente, na fila do fluxo de áudio atualizações de estado como uma matriz de `game_chat_stream_state_change` estrutura ponteiros. Os aplicativos devem iterar sobre a matriz e lidar com cada atualização adequadamente. Uma vez disponíveis `game_chat_stream_state_change` as atualizações terem sido manipuladas, essa matriz deve ser passada para 2 de bate-papo do jogo por meio de `chat_manager::finish_processing_stream_state_changes()`. Por exemplo:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>Manipulando previamente codificar os dados de áudio de bate-papo

2 de bate-papo jogo fornece acesso para codificar previamente os dados de áudio de bate-papo de usuários locais por meio de `pre_encode_audio_stream` classe.

### <a name="stream-lifetime"></a>Tempo de vida do Stream
Quando um novo `pre_encode_audio_stream` instância esteja pronta para o aplicativo para usar, ela será entregue por meio de um `game_chat_stream_state_change` estrutura com seus `state_change_type` campo definido como `game_chat_stream_state_change_type::pre_encode_audio_stream_created`. Depois que essa alteração de estado de fluxo é retornada para 2 de bate-papo do jogo, o fluxo de áudio se tornará disponível para codificar previamente a manipulação de áudio.

Quando um existente `pre_encode_audio_stream` torna-se indisponível a ser usado para manipulação de áudio, o aplicativo será notificado por meio de um `game_chat_stream_state_change` estrutura com seus `state_change_type` campo definido como `game_chat_stream_state_change_type::pre_encode_audio_stream_closed`. Esta é a oportunidade do aplicativo para começar a limpar os recursos associados a esse fluxo de áudio. Depois que essa alteração de estado de fluxo é retornada para 2 de bate-papo do jogo, o fluxo de áudio será ficam indisponível para codificar previamente a manipulação de áudio.

Quando um fechados `pre_encode_audio_stream` tem todos os recursos retornados, o fluxo será destruído e o aplicativo será notificado por meio de um `game_chat_stream_state_change` estrutura com seus `state_change_type` campo definido como `game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`. Quaisquer referências ou ponteiros para esse fluxo devem ser limpa. Depois que essa alteração de estado de fluxo é retornada ao jogo de bate-papo 2, a memória de fluxo de áudio se tornarão inválida.

### <a name="stream-users"></a>Usuários do Stream
A lista de usuários associados a um fluxo pode ser inspecionada usando `pre_encode_audio_stream::get_users()`.

### <a name="audio-formats"></a>Formatos de áudio
O formato de áudio dos buffers, o aplicativo recupera do jogo de bate-papo 2 pode ser examinado usando `pre_encode_audio_stream::get_pre_processed_format()`. O formato de áudio pré-processada sempre será mono. O aplicativo deve esperar lidar com os dados representados como inteiros de 32 bits, inteiros de 16 bits e flutuações de 32 bits.

O aplicativo precisa informar o jogo 2 de bate-papo do formato de áudio dos buffers manipulados que estão sendo enviados a ele para codificação e transmissão usando `pre_encode_audio_stream::set_processed_format()`. Formatos processados para codificam previamente áudio fluxos devem atender a essas pré condições:

* O formato deve ser mono.
* O formato deve ser float de 32 bits PCM, PCM de inteiro de 32 bits ou formatos de PCM de inteiro de 16 bits.
* Taxa de amostragem do formato deve seguir as pré-condições com base em sua plataforma. Xbox One ERA dá suporte a taxas de amostragem de 8kHz, kHz 12, 16kHz e 24kHz. Dá suporte a UWP para Xbox One e PC, 8kHz, kHz 12, 16kHz, kHz 24, 32kHz, 44.1 kHz e taxas de amostragem de 48 kHz.

### <a name="retrieving-and-submitting-audio"></a>Recuperar e enviar áudio
Aplicativos podem consultar codificar previamente os fluxos de áudio para o número de buffers disponíveis para processar usando `pre_encode_audio_stream::get_available_buffer_count()`. Essas informações podem ser usadas se o aplicativo deseja adiar o processamento de áudio até que um número mínimo de buffers disponível. Buffers de apenas 10 serão enfileiradas em cada previamente codificar o fluxo de áudio e áudio atrasos introduzirão a latência no pipeline de áudio, portanto, é recomendável que os aplicativos drenar suas previamente codificar fluxos de áudio antes que eles buffers mais de 4 da fila.

Aplicativos podem recuperar os buffers de áudio de um codificar previamente com o fluxo de áudio `pre_encode_audio_stream::get_next_buffer()`. Novos buffers de áudio estarão disponíveis em média, uma vez a cada 40ms. Buffers retornados por esse método devem ser liberados para `pre_encode_audio_stream::return_buffer()` quando eles são feitos que está sendo usado. Um máximo de 10 na fila ou buffers unreturned podem existir em um determinado momento para um previamente codificar o fluxo de áudio. Quando esse limite é atingido, novos buffers capturados de fonte de áudio do player serão removidos até que alguns dos buffers pendentes sejam retornadas.

Aplicativos podem enviar seus buffers inspecionados e manipuladas para 2 de bate-papo de jogo para codificação e transmissão usando `pre_encode_audio_stream::submit_buffer()`. 2 de bate-papo jogo dá suporte à manipulação de áudio no local e fora do local, para que os buffers enviadas ao `pre_encode_audio_stream::submit_buffer()` não precisa necessariamente ser os mesmos buffers recuperados do `pre_encode_audio_stream::get_next_buffer()`. Privacidade/privilégio para esses buffers enviados será aplicado com base no que os usuários associados a este fluxo. Cada 40ms, a próxima 40ms de áudio desse fluxo será codificado e transmitidas. Para evitar interrupções de áudio, buffers de áudio que deve ser ouvido continuamente devem ser enviadas para esse fluxo a uma taxa constante.

### <a name="stream-contexts"></a>Contextos de Stream
Aplicativos podem gerenciar valores de contexto personalizado de tamanho de ponteiro em codificar previamente usando fluxos de áudio `pre_encode_audio_stream::set_custom_stream_context()` e `pre_encode_audio_stream::custom_stream_context()`. Esses contextos de fluxos personalizados são úteis para criar mapeamentos entre os fluxos de áudio e dados de auxiliar jogo bate-papo do 2: fluxo de metadados, o estado do jogo, etc.

### <a name="example"></a>Exemplo
Aqui está um exemplo simplificado de ponta a ponta de como usar codificar previamente os fluxos de áudio no quadro de um processamento de áudio:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            pre_encode_audio_stream* stream = streamStateChanges[streamStateChangeIndex]->pre_encode_audio_stream;
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t preEncodeAudioStreamCount;
pre_encode_audio_stream_array preEncodeAudioStreams;
chat_manager::singleton_instance().get_pre_encode_audio_streams(&preEncodeAudioStreamCount, &preEncodeAudioStreams);
for (uint32_t preEncodeAudioStreamIndex = 0; preEncodeAudioStreamIndex < preEncodeAudioStreamCount; ++preEncodeAudioStreamIndex)
{
    pre_encode_audio_stream* stream = preEncodeAudioStreams[preEncodeAudioStreamIndex];
    StreamContext* context = reinterpret_cast<StreamContext*>(stream->custom_stream_context());

    game_chat_audio_format audio_format = stream->get_pre_processed_format();

    uint32_t preProcessedBufferByteCount;
    void* preProcessedBuffer;
    stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);

    while (preProcessedBuffer != nullptr)
    {
        void* processedBuffer = nullptr;
        switch (audio_format.bits_per_sample)
        {
            case 16:
            {
                assert (audio_format.sample_type == game_chat_sample_type::integer);
                processedBuffer = ManipulateChatBuffer<int16_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                break;
            }

            case 32:
            {
                switch (audio_format.sample_type)
                {
                    case game_chat_sample_type::integer:
                    {
                        processedBuffer = ManipulateChatBuffer<int32_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    case game_chat_sample_type::ieee_float:
                    {
                        processedBuffer = ManipulateChatBuffer<float>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    default:
                    {
                        assert(false);
                        break;
                    }
                }
                break;
            }

            default:
            {
                assert(false);
                break;
            }
        }
        // processedBuffer can be the same as preProcessedBuffer (in-place manipulation) or it can be a buffer of
        // memory not managed by Game Chat 2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from Game Chat 2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>Pós-manipulando decodificar dados de áudio de bate-papo

2 de bate-papo jogo fornece acesso para pós-decodificar dados de áudio de bate-papo por meio de `post_decode_audio_source_stream` e `post_decode_audio_sink_stream` classes, para que os usuários podem manipular o áudio de usuários remotos exclusivamente para cada destinatário local de áudio de bate-papo.

### <a name="sources-and-sinks"></a>Origens e Coletores
Ao contrário do pipeline pre-encode, o modelo para lidar com pós-decodificar dados de áudio é dividido em duas classes: `post_decode_audio_source_stream` e `post_decode_audio_sink_stream`. Áudio decodificado de usuários remotos pode ser recuperado do `post_decode_audio_source_stream` objetos, manipulado e enviado para `post_decode_audio_sink_stream` objetos para renderização. Isso permite a integração entre o jogo de bate-papo 2 pós-decodificar o pipeline de processamento de áudio e middleware úteis de áudio.

### <a name="stream-lifetime"></a>Tempo de vida do Stream
Quando um novo `post_decode_audio_source_stream` ou `post_decode_audio_sink_stream` instância esteja pronta para o aplicativo para usar, ela será entregue por meio de uma `game_chat_stream_state_change` estrutura com seus `state_change_type` campo definido como `game_chat_stream_state_change_type::post_decode_audio_source_stream_created` ou `game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`, respectivamente. Depois que essa alteração de estado de fluxo é retornada para 2 de bate-papo do jogo, o fluxo de áudio se tornará disponível para decodificar posteriores à manipulação de áudio.

Quando um existente `post_decode_audio_source_stream` ou `post_decode_audio_sink_stream` torna-se indisponível a ser usado para manipulação de áudio, o aplicativo será notificado por meio de uma `game_chat_stream_state_change` estrutura com seus `state_change_type` campo definido como `game_chat_stream_state_change_type::post_decode_audio_source_stream_closed` ou `game_chat_stream_state_change_type::post_decode_audio_sink_stream`, respectivamente. Esta é a oportunidade do aplicativo para começar a limpar os recursos associados a esse fluxo de áudio. Depois que essa alteração de estado de fluxo é retornada para 2 de bate-papo do jogo, o fluxo de áudio se tornará não está disponível para decodificar posteriores à manipulação de áudio. Para fluxos de origem, isso significa que não há mais buffers serão ser enfileirados para manipulação. Para fluxos de coletor, isso significa que enviou buffers não serão renderizados.

Quando um fechados `post_decode_audio_source_stream` ou `post_decode_audio_sink_stream` tem todos os recursos retornados, o fluxo será destruído e o aplicativo será notificado por meio de uma `game_chat_stream_state_change` estrutura com seus `state_change_type` campo definido como `game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed` ou `game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`, respectivamente. Quaisquer referências ou ponteiros para esse fluxo devem ser limpa. Depois que essa alteração de estado de fluxo é retornada ao jogo de bate-papo 2, a memória de fluxo de áudio se tornarão inválida.

### <a name="stream-users"></a>Usuários do Stream
A lista de usuários remotos associados a um fluxo de origem post-decode pode ser examinada usando `post_decode_audio_source_stream::get_users()`. A lista de usuários locais associado a um fluxo de coletor post-decode pode ser examinada usando `post_decode_audio_sink_stream::get_users()`.

### <a name="audio-formats"></a>Formatos de áudio
O formato de áudio dos buffers, o aplicativo recupera do jogo de bate-papo 2 pode ser examinado usando `post_decode_audio_source_stream::get_pre_processed_format()`. O formato de áudio pré-processada sempre será inteiros de 16 bits, mono PCM.

O aplicativo precisa informar o jogo 2 de bate-papo do formato de áudio dos buffers manipulados que estão sendo enviados a ele para renderização usando `post_decode_audio_sink_stream::set_processed_format()`. Formatos processados para pós-decodificar áudio fluxos do coletor devem atender a essas pré condições:

* O formato deve ter menos de 64 canais.
* O formato deve ser inteiro de 16 bits PCM (ideal), 20 bits inteiro PCM (em um contêiner de 24 bits), inteiro de 24 bits PCM, PCM de inteiro de 32 bits ou float de 32 bits PCM (formato preferencial depois PCM de inteiro de 16 bits). 
* Taxa de amostragem do formato deve ser entre 1000 e 200000 amostras por segundo.

### <a name="retrieving-and-submitting-audio"></a>Recuperar e enviar áudio
Aplicativos podem consultar pós-decodificar fluxos de áudio de origem para o número de buffers disponíveis para processar usando `post_decode_audio_source_stream::get_available_buffer_count()`. Essas informações podem ser usadas se o aplicativo deseja adiar o processamento de áudio até que um número mínimo de buffers disponível. Buffers de apenas 10 serão enfileiradas em cada pós-decodificar o fluxo de fonte de áudio e áudio atrasos introduzirão a latência no pipeline de áudio, portanto, é recomendável que os aplicativos drenar suas pós-decodificar fluxos de áudio antes que eles buffers mais de 4 da fila.

Aplicativos podem recuperar os buffers de áudio de um pós-decodificar o fluxo de fonte de áudio usando `post_decode_audio_source_stream::get_next_buffer()`. Novos buffers de áudio estarão disponíveis em média, uma vez a cada 40ms. Buffers retornados por esse método devem ser liberados para `post_decode_audio_source_stream::return_buffer()` quando eles são feitos que está sendo usado. Um máximo de 10 na fila ou buffers unreturned podem existir em qualquer dado momento para uma posterior ao decodificar o fluxo de áudio de origem. Quando esse limite é atingido, novos buffers decodificados do player de remoto serão removidos até que alguns dos buffers pendentes sejam retornadas.

Aplicativos podem enviar seus buffers inspecionados e manipuladas para 2 de bate-papo do jogo por meio de pós-decodificar fluxos de áudio do coletor para renderização usando `post_decode_audio_sink_stream::submit_mixed_buffer()`. 2 de bate-papo jogo dá suporte à manipulação de áudio no local e fora do local, para que os buffers enviadas ao `post_decode_audio_sink_stream::submit_mixed_buffer()` não precisa necessariamente ser os mesmos buffers recuperados do `post_decode_audio_source_stream::get_next_buffer()`. Cada 40ms, a próxima 40ms de áudio desse fluxo será renderizado. Para evitar interrupções de áudio, buffers de áudio que deve ser ouvido continuamente devem ser enviadas para esse fluxo a uma taxa constante.

### <a name="privacy-and-mixing"></a>Privacidade e combinação
Devido ao modelo de coletor do código-fonte do pipeline post-decode, é responsabilidade do aplicativo para combinar buffers recuperados do `post_decode_audio_source_stream` objetos e enviar os buffers mistos para `post_decode_audio_sink_stream` objetos para renderização. Isso também significa que é responsabilidade do aplicativo para executar a combinação com a privacidade adequada e imposta de privilégio. 2 de bate-papo jogo fornece `post_decode_audio_sink_stream::can_receive_audio_from_source_stream()` para tornar a consulta para obter essas informações simples e eficientes.

### <a name="chat-indicators"></a>Indicadores de bate-papo

Pós-decodificar áudio manipulação não afetará o estado de indicador de bate-papo para cada usuário. Por exemplo, quando um usuário remoto estiver mudo, o áudio será fornecido para o aplicativo, mas o indicador de bate-papo para que o usuário remoto ainda indicarão mudo. Quando um usuário remoto está falando, o áudio será fornecido, mas o indicador de bate-papo indicará falando, independentemente se o aplicativo fornece uma combinação de áudio que contém o áudio do que o usuário. Para obter mais informações sobre a interface do usuário e o indicador de bate-papo, consulte [2 de bate-papo de jogo usando](using-game-chat-2.md#ui). Se as restrições adicionais específicos do aplicativo são usadas para determinar quais usuários estão presentes em uma combinação de áudio, é responsabilidade do aplicativo a considerar essas mesmas restrições quando está lendo os indicadores de bate-papo fornecidos pelo 2 de bate-papo do jogo.

### <a name="stream-contexts"></a>Contextos de Stream
Aplicativos podem gerenciar valores de contexto personalizado de tamanho de apontador na pós-decodificar fluxos de áudio usando o `set_custom_stream_context()` e `custom_stream_context()` métodos. Esses contextos de fluxos personalizados são úteis para criar mapeamentos entre os fluxos de áudio e dados de auxiliar jogo bate-papo do 2: fluxo de metadados, o estado do jogo, etc.

### <a name="example"></a>Exemplo
Aqui está um exemplo simplificado de ponta a ponta de como usar pós-decodificar fluxos de áudio no quadro de um processamento de áudio:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::post_decode_audio_source_stream_created:
        {
            post_decode_audio_source_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_source_stream;
            stream->set_custom_stream_context(...);
            HandlePostDecodeAudioSourceStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_closed:
        {
            HandlePostDecodeAudioSourceStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed:
        {
            HandlePostDecodeAudioSourceStreamDestroyed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_created:
        {
            post_decode_audio_sink_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_sink_stream;
            stream->set_custom_stream_context(...);
            stream->set_processed_format(...);
            HandlePostDecodeAudioSinkStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_closed:
        {
            HandlePostDecodeAudioSinkStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed:
        {
            HandlePostDecodeAudioSinkStreamDestroyed(stream);
            break;
        }

        ...
    }
}

chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t sourceStreamCount;
post_decode_audio_source_stream_array sourceStreams;
chatManager::singleton_instance().get_post_decode_audio_source_streams(&sourceStreamCount, &sourceStreams);

uint32_t sinkStreamCount;
post_decode_audio_sink_stream_array sinkStreams;
chatManager::singleton_instance().get_post_decode_audio_sink_streams(&sinkStreamCount, &sinkStreams);

//
// MixBuffer is a custom type defined as:
// struct MixBuffer
// {
//     uint32_t bufferByteCount;
//     void* buffer;
// };
//
std::vector<std::pair<post_decode_audio_source_stream*, MixBuffer>> cachedSourceBuffers;

for (uint32_t sourceStreamIndex = 0; sourceStreamIndex < sourceStreamCount; ++sourceStreamIndex)
{
    post_decode_audio_source_stream* sourceStream = sourceStreams[sourceStreamIndex];

    MixBuffer mixBuffer;
    sourceStream->get_next_buffer(&mixBuffer.bufferByteCount, &mixBuffer.buffer);
    if (buffer != nullptr)
    {
        // Stash the buffer to return after we are done with mixing. If this program was using audio middleware, now
        // would be an appropriate time to plumb the buffer through the middleware
        cachedSourceBuffer.push_back(std::pair<post_decode_audio_source_stream*, MixBuffer>{sourceStream, mixBuffer});
    }
}

// Loop over each sink stream, perform mixing, and submit
for (uint32_t sinkStreamIndex = 0; sinkStreamIndex < sinkStreamCount; ++sinkStreamIndex)
{
    post_decode_audio_sink_stream* sinkStream = sinkStreams[sinkStreamIndex];

    if (sinkStream->is_open())
    {
        std::vector<std::pair<MixBuffer, float>> buffersToMixForThisStream;

        for (const std::pair<post_decode_audio_source_stream, MixBuffer>& sourceBufferPair : cachedSourceBuffers)
        {
            float volume;
            if (sinkStream->can_receive_audio_from_source_stream(sourceBufferPair.first, &volume))
            {
                buffersToMixForThisStream.push_back(std::pair<MixBuffer, float>{sourceBufferPair.second, volume});
            }
        }

        if (buffersToMixForThisStream.size() > 0)
        {
            uint32_t mixedBufferByteCount;
            uint8_t* mixedBuffer;
            MixPostDecodeBuffers(buffersToMixForThisStream, &mixedBufferByteCount, &mixedBuffer);
            sinkStream->submit_mixed_buffer(mixedBufferByteCount, mixedBuffer);
        }
    }
}

// Return buffers after mix and submission
for (const std::pair<post_decode_audio_source_stream*, MixBuffer>& cachedSourceBuffer : cachedSourceBuffers)
{
    post_decode_audio_source_stream* sourceStream = cachedSourceBuffer.first;
    void* bufferToReturn = cachedSourceBuffer.second.buffer;
    sourceStream->return_buffer(bufferToReturn);
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="chat-user-lifetimes"></a>Tempos de vida de usuário de bate-papo

Habilitar a manipulação de áudio em tempo real afetará os tempos de vida dos usuários de bate-papo. Se `chat_manager::remove_user(chatUserX)` é chamado, o chat_user objeto apontado pelo chatUserX permanecerá válido até que todos os fluxos de áudio que fazem referência a chatUserX tenham sido destruídos. Considere os seguintes cenários:

```cpp
// At somepoint a chat user, chatUserX, leaves the game session.
chat_manager::singleton_instance().remove_user(chatUserX);

// chatUserX is still valid, but to avoid further synchronization, prevent non-audio-stream use of chatUserX
chatUserX = nullptr;

// On the audio processing thread...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        // All of the streams associated with chatUserX will close.
        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

// The next time the app processes stream state changes...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            uint32_t chatUserCount;
            Xs::game_chat_2::chat_user_array chatUsers;
            streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream->get_users(&chatUserCount, &chatUsers);
            assert(chatUserCount != 0);
            for (uint32_t chatUserIndex = 0; chatUserIndex < chatUserCount; ++chatUserIndex)
            {
                // chat_user objects such as chatUserX will still be valid while the destroyed state change is being processed.
                Log(chatUsers[chatUserIndex]->xbox_user_id());
            }
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
// Once the all destroyed state changes have been processed for all streams associated with chatUserX, it's memory will be invalidated.
// Do not call methods on chatUserX, e.g. chatUserX->xbox_user_id()
```
