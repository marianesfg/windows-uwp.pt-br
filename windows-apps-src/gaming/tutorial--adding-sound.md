---
title: Adicionar som
description: Desenvolva um mecanismo de som simple usando APIs XAudio2 para reprodução jogo música e efeitos sonoros.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, sons
ms.localizationpriority: medium
ms.openlocfilehash: 94044e3d10df15cb1cb256d86ced798395e6af6f
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8196537"
---
# <a name="add-sound"></a>Adicionar som

Neste tópico, podemos criar um mecanismo de som simple usando-se [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) APIs. Se você for iniciante no __XAudio2__, incluímos uma breve introdução em [conceitos de áudio](#audio-concepts).

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Adicione sons para o jogo de exemplo usando-se [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813).

## <a name="define-the-audio-engine"></a>Definir o mecanismo de áudio

No exemplo de jogo, os objetos de áudio e os comportamentos são definidos em três arquivos:

* __[Audio. h](#audioh)/.cpp__: define o objeto de __áudio__ , que contém os recursos __XAudio2__ para reprodução de som. Ele também define o método para suspender e retomar a reprodução de som, caso o jogo seja pausado ou desativado.
* __ [MediaReader.h](#mediareaderh)/.cpp__: define os métodos para ler arquivos. wav de áudio do armazenamento local.
* __ [SoundEffect.h](#soundeffecth)/.cpp__: define um objeto para reprodução de som no jogo.

## <a name="overview"></a>Visão geral

Há três partes principais no Preparando-se para reprodução de áudio em seu jogo.

1. [Crie e inicialize os recursos de áudio](#create-and-initialize-the-audio-resources)
2. [Carregar o arquivo de áudio](#load-audio-file)
3. [Associar o som ao objeto](#associate-sound-to-object)

Eles são definidos no método [simple3dgame:: Initialize](#simple3dgameinitialize-method) . Portanto, vamos primeiro examinar este método e, em seguida, nos aprofundar em mais detalhes em cada uma das seções.

Depois de configurar, aprendemos como disparar os efeitos de som para reproduzir. Para obter mais informações, vá para [reproduzir o som](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Método simple3dgame:: Initialize

__Simple3dgame:: Initialize__, onde __m\_controller__ e __m\_renderer__ também são inicializados, podemos configurar o mecanismo de áudio e prepare-o reproduzir sons.

 * Crie __m\_audioController__, que é uma instância da classe de [áudio](#audioh) .
 * Crie os recursos de áudio necessários usando o método [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) . Aqui, dois objetos de __XAudio2__ &mdash; um objeto do mecanismo de música e um objeto do mecanismo de som e uma voz de masterização para cada uma delas foram criados. O objeto do mecanismo de música pode ser usado para reproduzir música em segundo plano para o seu jogo. O mecanismo de som pode ser usado para reproduzir efeitos sonoros em seu jogo. Para obter mais informações, consulte [criar e inicializar os recursos de áudio](#create-and-initialize-the-audio-resources).
 * Crie __mediaReader__, que é uma instância da classe [MediaReader](#mediareaderh) . [MediaReader](#mediareaderh), que é uma classe auxiliar para a classe [SoundEffect](#soundeffecth) , lê pequenos arquivos de áudio de forma síncrona de local do arquivo e retorna dados de som como uma matriz de bytes.
 * Use [mediareader:: Loadmedia](#mediareaderloadmedia-method) para carregar arquivos de som desde a sua localização e crie uma variável __targetHitSound__ para armazenar os dados de som. wav carregado. Para obter mais informações, consulte o [arquivo de áudio de carga](#load-audio). 

Efeitos sonoros são associados com o objeto do jogo. Portanto, quando ocorre uma colisão com esse objeto de jogo, ele aciona o efeito de som a ser reproduzido. Neste jogo de exemplo, temos efeitos sonoros para a munição (o que usamos para atirar nos alvos com) e para o destino. 
    
* Na classe __GameObject__ , há uma propriedade __HitSound__ que é usada para associar o efeito de som ao objeto.
* Crie uma nova instância da classe [SoundEffect](#soundeffecth) e inicializá-lo. Durante a inicialização, uma voz de origem para o efeito de som é criada. 
* Essa classe reproduzir um som usando uma voz de masterização fornecida a partir da classe de [áudio](#audioh) . Dados de som são lidos no local do arquivo usando a classe [MediaReader](#mediareaderh) . Para obter mais informações, consulte [associar som ao objeto](#associate-sound-to-object).

>[!Note]
>O gatilho real para reproduzir o som é determinado pelo movimento e a colisão desses objetos de jogo. Portanto, a chamada para realmente esses sons são definidos no método [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) . Para obter mais informações, vá para [reproduzir o som](#play-the-sound).

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Crie e inicialize os recursos de áudio

* Use [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212), uma API do XAudio2, para criar dois novos objetos de XAudio2 que definem os mecanismos de efeito música e sons. Esse método retorna um ponteiro para a interface do objeto [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) que gerencia todos os estados de mecanismo de áudio, o áudio processamento thread, o gráfico de voz e muito mais.
* Após os mecanismos de tenham sido criados, use [ixaudio2:: Createmasteringvoice](https://msdn.microsoft.com/library/windows/desktop/hh405048) para criar uma voz de masterização para cada um dos objetos mecanismo som.

Para obter mais informações, acesse [como: inicializar o XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx).

### <a name="audiocreatedeviceindependentresources-method"></a>Método Audio::CreateDeviceIndependentResources

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Carregar o arquivo de áudio

No exemplo de jogo, o código para ler arquivos de formato de áudio é definido em [MediaReader.h](#mediareaderh)/cpp__.  Para ler um arquivo de áudio. wav codificado, chame [mediareader:: Loadmedia](#mediareaderloadmedia-method), passando o nome do arquivo de WAV como o parâmetro de entrada.

### <a name="mediareaderloadmedia-method"></a>Método mediareader:: Loadmedia

Esse método usa as APIs [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) para ler o arquivo de áudio .wav como um buffer de modulação por código de pulso (PCM).

#### <a name="set-up-the-source-reader"></a>Configurar o leitor de origem

1. Use [MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110) para criar uma mídia de leitor de origem ([IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655)).
2. Use [MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861) para criar um objeto de tipo ([IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850)) de mídia (_mediaType_). Ele representa uma descrição de um formato de mídia. 
3. Especifique a saída decodificada do _mediaType_é áudio PCM, que é um tipo de áudio __XAudio2__ pode usar.
4. Conjuntos de tipo a mídia de saída decodificada para o leitor de origem por chamada [imfsourcereader:: Setcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx).

Para obter mais informações sobre por que usamos o leitor de origem, vá para o [Leitor de origem](https://msdn.microsoft.com/library/windows/desktop/dd940436.aspx).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Descrever o formato de dados do fluxo de áudio

1. Use [imfsourcereader:: Getcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374660) para obter o tipo de mídia atual para o fluxo.
2. Use [imfmediatype:: Mfcreatewaveformatexfrommfmediatype](https://msdn.microsoft.com/library/windows/desktop/ms702177) para converter o tipo de mídia de áudio atual em um buffer [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) , usando os resultados da operação anterior como entrada. Essa estrutura Especifica o formato de dados do fluxo de áudio de onda é usado depois que o áudio é carregado. 

O formato __WAVEFORMATEX__ pode ser usado para descrever o buffer PCM. Em comparação com a estrutura [WAVEFORMATEXTENSIBLE](https://msdn.microsoft.com/library/windows/hardware/ff538802) , ele só pode ser usado para descrever um subconjunto dos formatos de onda de áudio. Para obter mais informações sobre as diferenças entre __WAVEFORMATEX__ e __WAVEFORMATEXTENSIBLE__, consulte [Extensível descritores de formato de onda](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Ler o fluxo de áudio

1.  Obter a duração, em segundos, do fluxo de áudio chamando [imfsourcereader:: Getpresentationattribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) e, em seguida, converte a duração para bytes.
2.  Ler o arquivo de áudio como um fluxo chamando [imfsourcereader:: Readsample](https://msdn.microsoft.com/library/windows/desktop/dd374665). __ReadSample__ lê a próxima amostra da fonte de mídia.
3.  Use [IMFSample::ConvertToContiguousBuffer](https://msdn.microsoft.com/library/windows/desktop/ms698917.aspx) para copiar o conteúdo do buffer de amostra de áudio (_exemplo_) em uma matriz (_mediaBuffer_).

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>Associar o som ao objeto

Associar sons para o objeto ocorre quando o jogo é inicializado no método [simple3dgame:: Initialize](#simple3dgameinitialize-method) .

Recapitulação:
* Na classe __GameObject__ , há uma propriedade __HitSound__ que é usada para associar o efeito de som ao objeto.
* Crie uma nova instância do objeto de classe [SoundEffect](#soundeffecth) e associá-lo com o objeto do jogo. Essa classe reproduzir um som usando-se __XAudio2__ APIs.  Ele usa uma voz de masterização fornecida pela classe de [áudio](#audioh) . Os dados de som podem ser lidos do local do arquivo usando a classe [MediaReader](#mediareaderh) .

[SoundEffect:: Initialize](#soundeffectinitialize-method) é usado para inicializar o __SoundEffect__ instância com os seguintes parâmetros de entrada: ponteiro para o objeto do mecanismo de som (IXAudio2 objetos criados no método [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) ), ponteiro para formatar o. wav usando __mediareader:: Getoutputwaveformatex__e os dados de som do arquivo carregado usando o método [mediareader:: Loadmedia](#mediareaderloadmedia-method) . Durante a inicialização, a voz de origem para o efeito de som também é criada.

### <a name="soundeffectinitialize-method"></a>Método SoundEffect:: Initialize

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Reproduzir o som

Gatilhos para reproduzir efeitos sonoros são definidos no método [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) porque é onde o movimento dos objetos são atualizados e colisão entre objetos é determinada.

Desde que a interação de entre objetos difere bastante, dependendo do jogo, não vamos falar sobre a dinâmica dos objetos do jogo aqui. Se você estiver interessado entender sua implementação, vá para o método [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) .

Em princípio, quando ocorre uma colisão, ele aciona o efeito de som para reproduzir chamando [SoundEffect::PlaySound]((soundeffectplaysound-method). Esse método impede que os efeitos de som que está sendo reproduzido e enfileira o buffer de memória com os dados de som desejados. Ele usa a voz de origem para definir o volume, enviar dados de som e iniciar a reprodução.

### <a name="soundeffectplaysound-method"></a>Método SoundEffect:: PlaySound

* Usa o de objeto de voz de origem **m\_sourceVoice** para iniciar a reprodução do buffer de dados de som **m\_soundData**
* Cria um [XAUDIO2\_BUFFER](https://msdn.microsoft.com/library/windows/desktop/ee419228), para que ele fornece uma referência para o buffer de dados de som e, em seguida, o envia com uma chamada para [ixaudio2sourcevoice:: Submitsourcebuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473). 
* Com os dados de som na fila, **SoundEffect::PlaySound** começa a reproduzir chamando [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471).

```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame::UpdateDynamics

O método __Simple3DGame::UpdateDynamics__ se encarrega interação e colisão entre objetos do jogo. Quando objetos colidem (ou interseção), ele aciona o efeito de som associado a reproduzir.

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
           // Start playing the sounds for the impact between the two balls.
              m_ammo[one]->PlaySound(impact, m_player->Position());
              m_ammo[two]->PlaySound(impact, m_player->Position());

       }
    }
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.
    if (impact > 0.0f)
    {
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>Próximas etapas

Abordamos UWP framework, elementos gráficos, controles, interface do usuário e áudio de um jogo do Windows 10. A próxima parte deste tutorial, [Estendendo o exemplo de jogo](tutorial-resources.md), explica outras opções que podem ser usadas ao desenvolver um jogo.

## <a name="audio-concepts"></a>Conceitos de áudio

Para o desenvolvimento de jogos do Windows 10, use o XAudio2 versão 2.9. Esta versão é fornecida com o Windows 10. Para obter mais informações, vá para [Versões de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415802.aspx).

__AudioX2__ é uma API de baixo nível que oferece processamento e mixagem. Para obter mais informações, consulte [XAudio2 Key Concepts](https://msdn.microsoft.com/library/windows/desktop/ee415764.aspx).

### <a name="xaudio2-voices"></a>Vozes XAudio2

Há três tipos de objetos de voz do XAudio2: origem, submixagem e masterização. As vozes são que os objetos XAudio2 usam para processar, manipular e reproduzir os dados de áudio. 
* As vozes de origem operam em dados de áudio fornecidos pelo cliente. 
* As vozes de origem e submixagem enviam sua saída para uma ou mais vozes de submixagem ou masterização. 
* As vozes de submixagem e masterização combinam o áudio de todas as vozes que as alimentam e operam no resultado. 
* As vozes de masterização receba dados de vozes de origem e de vozes de submix e envia esses dados para o hardware de áudio.

Para obter mais informações, vá para [vozes XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415824.aspx).

### <a name="audio-graph"></a>Gráfico de áudio

Gráfico de áudio é uma coleção de [vozes XAudio2](#xaudio2-voice-objects). Áudio começa em um lado de um gráfico de áudio nas vozes de origem, opcionalmente passa por um ou mais vozes de submix e termina em uma voz de masterização. Um gráfico de áudio contém uma voz de origem para cada som atualmente em execução, zero ou mais vozes de submix e uma voz de masterização. O gráfico de áudio mais simples e o mínimo necessário para tornar um ruído no XAudio2, é uma voz de origem única saída diretamente para uma voz de masterização. Para obter mais informações, vá para [gráficos de áudio](https://msdn.microsoft.com/library/windows/desktop/ee415739.aspx).

### <a name="additional-reading"></a>Leituras adicionais

* [Como: Inicializar o XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)
* [Como: Carregar arquivos de dados de áudio no XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [Como: Reproduzir um som com o XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415787.aspx)

## <a name="key-audio-h-files"></a>Arquivos de chave. h áudio

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```


